# 🤖 RAG & GenAI App With WordPress Content

A powerful retrieval-augmented generation (RAG) system that enables intelligent chat conversations powered by your WordPress website content. Built with n8n, OpenAI, and Supabase vector database for semantic search and AI-driven responses.

![Status](https://img.shields.io/badge/status-active-success.svg)
![n8n](https://img.shields.io/badge/n8n-workflow-EA4B71?style=flat&logo=n8n)
![OpenAI](https://img.shields.io/badge/OpenAI-Embeddings-412991?style=flat&logo=openai)
![Supabase](https://img.shields.io/badge/Supabase-pgvector-3ECF8E?style=flat)
![WordPress](https://img.shields.io/badge/WordPress-API-21759B?style=flat&logo=wordpress)

## ✨ Features

- **Automated Content Indexing** - Continuously syncs WordPress posts and pages to vector database
- **Semantic Search** - Find relevant content using OpenAI embeddings (text-embedding-3-small)
- **RAG Architecture** - Combines document retrieval with generative AI for accurate answers
- **Multi-Workflow System** - Three specialized workflows for initial setup, updates, and chat
- **Intelligent Caching** - Tracks last execution to only process modified content
- **Conversation Memory** - PostgreSQL-backed chat history for context-aware responses
- **HTML to Markdown** - Automatically converts WordPress HTML content to clean text
- **Metadata Enrichment** - Preserves URL, publication date, content type in responses
- **Scalable Vector Store** - pgvector integration for efficient similarity search
- **Webhook Chat Interface** - Ready-to-embed conversational interface
- **Filter Controls** - Exclude protected or unpublished content automatically

## 🎯 What It Does

Transform your WordPress website into an AI-powered knowledge base where visitors can ask questions and receive accurate, sourced answers directly from your content. Perfect for:

- **Customer Support** - Answer FAQs automatically from your help documentation
- **Blog Engagement** - Enable readers to query articles in natural language
- **Product Information** - Let customers discover features through conversation
- **Documentation** - Make technical docs searchable in conversational format
- **Content Discovery** - Help users find relevant pages they'd otherwise miss

## 🚀 Quick Start

### Prerequisites

Active accounts with:
- [n8n](https://n8n.io) (Cloud or self-hosted v1.0+)
- [WordPress](https://wordpress.org) with REST API enabled
- [OpenAI](https://platform.openai.com) API key
- [Supabase](https://supabase.com) PostgreSQL database
- [PostgreSQL](https://www.postgresql.org/) 13+ with pgvector extension

### Option 1: n8n Cloud Deploy

```bash
# 1. Sign up for n8n Cloud
# 2. Import the workflow JSON file
# 3. Configure credentials (see Configuration)
# 4. Run initial embedding workflow
# 5. Activate chat workflow
# 6. Deploy chat interface
```

### Option 2: Self-Hosted n8n

```bash
# Using Docker
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n

# Using npm
npm install n8n -g
n8n start

# Access at http://localhost:5678
```

## 📁 Project Structure

```
rag-genai-wordpress/
├── RAG & GenAI App With WordPress Content.json
├── README.md
├── LICENSE
├── docs/
│   ├── setup-guide.md
│   ├── wordpress-setup.md
│   ├── supabase-setup.md
│   └── customization.md
└── examples/
    ├── sample-queries.txt
    └── integration-examples/
```

## 🛠️ Installation & Setup

### Step 1: Import Workflow

1. Open your n8n instance
2. Click **"Import from File"**
3. Select `RAG & GenAI App With WordPress Content.json`
4. Three workflows will be created (Initial, Upsert, Chat)

### Step 2: Configure WordPress Connection

```
Settings → Credentials → Add Credential → WordPress API
- Website URL: https://yourwebsite.com
- Username: wp_api_user (create in WordPress)
- Password: (app-specific password from WordPress)
```

Enable WordPress REST API:
1. WordPress Dashboard → Settings → Permalinks
2. Ensure "Post name" or custom structure is selected
3. Test: `https://yourwebsite.com/wp-json/wp/v2/posts`

### Step 3: Set Up Supabase

Create PostgreSQL tables:

```sql
-- Run "Postgres - Create documents table" node
-- Creates 'documents' table with vector column
-- Creates 'match_documents' function for similarity search

-- Run "Postgres - Create workflow execution history table" node
-- Creates 'n8n_website_embedding_histories' table
```

Or manually:

```sql
-- Enable pgvector
CREATE EXTENSION IF NOT EXISTS vector;

-- Create documents table
CREATE TABLE documents (
  id BIGINT PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
  content TEXT,
  metadata jsonb,
  embedding vector(1536)
);

-- Enable RLS
ALTER TABLE documents ENABLE ROW LEVEL SECURITY;

-- Create search function
CREATE FUNCTION match_documents (
  query_embedding vector(1536),
  match_count INT DEFAULT NULL,
  filter jsonb DEFAULT '{}'
) RETURNS TABLE (
  id BIGINT,
  content TEXT,
  metadata jsonb,
  similarity FLOAT
) LANGUAGE plpgsql AS $$
BEGIN
  RETURN QUERY
  SELECT
    documents.id,
    documents.content,
    documents.metadata,
    1 - (documents.embedding <=> query_embedding) AS similarity
  FROM documents
  WHERE metadata @> filter
  ORDER BY documents.embedding <=> query_embedding
  LIMIT match_count;
END;
$$;

-- Create execution history table
CREATE TABLE n8n_website_embedding_histories (
  id BIGINT PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

### Step 4: Configure Credentials

#### OpenAI API
```
Settings → Credentials → OpenAI API
- API Key: sk-proj-...
- Organization: (optional)
```

#### Supabase Connection
```
Settings → Credentials → Supabase
- Project URL: https://xxx.supabase.co
- API Key: (from Project Settings → API)
```

#### PostgreSQL (for execution tracking)
```
Settings → Credentials → PostgreSQL
- Host: db.xxx.supabase.co
- Port: 5432
- Database: postgres
- User: postgres
- Password: (from Supabase)
```

### Step 5: Customize Workflows

#### Update WordPress URL

Edit nodes "Wordpress - Get posts modified after...":

```javascript
{
  "url": "https://your-site.com/wp-json/wp/v2/posts"
}
```

#### Adjust Sync Schedule

Edit **"Every 30 seconds"** node:

```javascript
// For testing: every 30 seconds
// For production: hourly or daily
{
  "rule": {
    "interval": [
      { "field": "hours", "value": 1 }  // Change frequency
    ]
  }
}
```

#### Customize AI System Message

Edit **"AI Agent"** node in Chat workflow:

```javascript
{
  "systemMessage": "You are an assistant for {{your_website_url}}.\n\nYour role: Answer visitor questions using website content.\n\nMUSTDO:\n- Always include source metadata (URL, date, type)\n- Reply in visitor's language\n- Cite all sources naturally\n- If no relevant content found, say so clearly\n\nTone: Professional, helpful, friendly"
}
```

### Step 6: Test Initial Workflow

1. Open "Workflow 1: Initial Embedding" workflow
2. Click "Execute Workflow" on "When clicking 'Test workflow'" node
3. Monitor execution for errors
4. Verify Supabase table populated: `SELECT COUNT(*) FROM documents;`

### Step 7: Activate Chat Workflow

1. Go to "Workflow 3: Chat" workflow
2. Click **"Active"** toggle
3. Copy webhook URL from "When chat message received" node
4. Test with curl:

```bash
curl -X POST https://your-n8n-instance.com/webhook/your-webhook-id \
  -H "Content-Type: application/json" \
  -d '{
    "sessionId": "test-session-123",
    "chatInput": "What is your return policy?"
  }'
```

## 🏗️ Architecture

### Three-Workflow System

#### Workflow 1: Initial Embedding
- Triggers: Manual test or initial setup
- Purpose: Create embeddings for all WordPress content
- Process: Fetch posts/pages → Filter → Convert HTML → Chunk → Embed → Store

#### Workflow 2: Upsert (Continuous Sync)
- Triggers: Scheduled (every 30 seconds by default)
- Purpose: Keep embeddings updated with new/modified content
- Process: Check last execution → Fetch modified → Filter → Convert → Chunk → Embed → Upsert

#### Workflow 3: Chat (User Interface)
- Triggers: Webhook when user sends message
- Purpose: Answer visitor questions using RAG
- Process: Retrieve docs → Format → Generate → Respond

### Data Flow Diagram

```
┌──────────────────────────────────────────────────────────────┐
│         WORKFLOW 1: INITIAL EMBEDDING                        │
└──────────────────────────────────────────────────────────────┘
                          │
                          ▼
            ┌──────────────────────────┐
            │ Fetch WordPress Posts    │
            │ and Pages                │
            └──────────────┬───────────┘
                          │
                          ▼
            ┌──────────────────────────┐
            │ Filter: Published +      │
            │ Unprotected Content      │
            └──────────────┬───────────┘
                          │
        ┌─────────────────┴─────────────────┐
        ▼                                   ▼
   ┌─────────────┐               ┌─────────────────┐
   │ HTML to     │               │ HTML to         │
   │ Markdown    │               │ Markdown        │
   └─────────────┘               └─────────────────┘
        │                               │
        └─────────────────┬─────────────┘
                          ▼
            ┌──────────────────────────┐
            │ Split into 300-token     │
            │ Chunks (30 overlap)      │
            └──────────────┬───────────┘
                          │
                          ▼
            ┌──────────────────────────┐
            │ Create Embeddings        │
            │ (text-embedding-3-small) │
            └──────────────┬───────────┘
                          │
                          ▼
            ┌──────────────────────────┐
            │ Store in Supabase        │
            │ Vector Database          │
            └──────────────┬───────────┘
                          │
                          ▼
            ┌──────────────────────────┐
            │ Record Execution Time    │
            │ in History Table         │
            └──────────────────────────┘

┌──────────────────────────────────────────────────────────────┐
│         WORKFLOW 2: UPSERT (CONTINUOUS SYNC)                │
└──────────────────────────────────────────────────────────────┘
                          │
          ┌───────────────┴───────────────┐
          ▼                               ▼
    ┌──────────────────┐        ┌──────────────────┐
    │ Check Last       │        │ Schedule Trigger │
    │ Execution Time   │        │ (every 30 sec)   │
    └─────────┬────────┘        └──────┬───────────┘
              │                        │
              └────────────┬───────────┘
                          ▼
            ┌──────────────────────────┐
            │ Fetch Modified Posts/    │
            │ Pages Since Last Run      │
            └──────────────┬───────────┘
                          │
                    [Similar to Workflow 1]
                          │
                          ▼
            ┌──────────────────────────┐
            │ Check if Document ID     │
            │ Exists in Database       │
            └──────────────┬───────────┘
                          │
          ┌───────────────┴───────────────┐
          ▼                               ▼
    ┌──────────────┐          ┌──────────────────┐
    │ Exists:      │          │ New:             │
    │ Delete Old   │          │ Insert Embedding │
    │ + Insert     │          │                  │
    └──────────────┘          └──────────────────┘

┌──────────────────────────────────────────────────────────────┐
│              WORKFLOW 3: CHAT (USER INTERFACE)               │
└──────────────────────────────────────────────────────────────┘
                          │
                ┌─────────┴─────────┐
                ▼                   ▼
        ┌──────────────┐    ┌──────────────────┐
        │ Receive Chat │    │ User's Question  │
        │ Message      │    │                  │
        └──────────────┘    └──────────────────┘
                │                   │
                └─────────────┬─────┘
                              ▼
                ┌──────────────────────────┐
                │ Embed User Question      │
                │ Using Same Model         │
                └──────────────┬───────────┘
                              │
                              ▼
                ┌──────────────────────────┐
                │ Search Supabase Vector   │
                │ for Similar Documents    │
                └──────────────┬───────────┘
                              │
                              ▼
                ┌──────────────────────────┐
                │ Retrieve Top Matches +   │
                │ Metadata                 │
                └──────────────┬───────────┘
                              │
                              ▼
                ┌──────────────────────────┐
                │ Load Chat History from   │
                │ PostgreSQL               │
                └──────────────┬───────────┘
                              │
                ┌─────────────┴─────────────┐
                ▼                           ▼
        ┌──────────────────┐      ┌──────────────┐
        │ Format Documents │      │ OpenAI Chat  │
        │ with Metadata    │      │ Model (GPT)  │
        └─────────┬────────┘      └──────┬───────┘
                  │                      │
                  └──────────┬───────────┘
                             ▼
                ┌──────────────────────────┐
                │ AI Agent Generates       │
                │ Answer with Sources      │
                └──────────────┬───────────┘
                              │
                              ▼
                ┌──────────────────────────┐
                │ Save Response to Chat    │
                │ History                  │
                └──────────────┬───────────┘
                              │
                              ▼
                ┌──────────────────────────┐
                │ Return Response to User  │
                │ via Webhook              │
                └──────────────────────────┘
```

## 🎨 Customization

### Filter Content by Type

Edit filter nodes to include/exclude post types:

```javascript
// Only index blog posts
{
  "conditions": [
    {
      "leftValue": "={{ $json.type }}",
      "operation": "equals",
      "rightValue": "post"
    }
  ]
}
```

### Adjust Chunk Size

Edit **"Token Splitter"** node:

```javascript
{
  "chunkSize": 300,      // Smaller = more chunks, more accurate
  "chunkOverlap": 30     // Overlap helps context
}
```

### Change Embedding Model

Edit **"Embeddings OpenAI"** nodes:

```javascript
{
  "model": "text-embedding-3-small"  // or "text-embedding-3-large"
}
```

### Customize Response Format

Edit **"AI Agent"** system message:

```javascript
{
  "systemMessage": "You are a helpful assistant for {{domain}}.\n\nWhen answering:\n1. Use ONLY information from provided documents\n2. Always cite source with [Title](URL)\n3. Include publication date\n4. Reply in user's language\n5. If uncertain, say so"
}
```

### Add Content Exclusions

Add more filter conditions:

```javascript
// Exclude specific categories
{
  "conditions": [
    {
      "leftValue": "={{ $json.categories }}",
      "operation": "notContains",
      "rightValue": "Internal"
    }
  ]
}
```

## 📊 Performance Metrics

### Processing Speed
- **Initial indexing**: ~5 seconds per 10 pages (WordPress retrieval)
- **Embedding generation**: ~100ms per document (OpenAI API)
- **Vector similarity search**: <50ms (Supabase)
- **Chat response time**: 2-5 seconds (OpenAI generation)

### Storage Requirements
- **Per page**: ~2-5 KB vector data + metadata
- **Example**: 100 pages ≈ 300-500 KB in database
- **Supabase**: Included in free tier for most sites

### API Costs (Approximate)
- **OpenAI Embeddings**: $0.02 per 1M tokens
- **OpenAI Chat**: $0.0005 per 1K tokens (gpt-3.5) or $0.03 per 1K (gpt-4)
- **Supabase**: Free tier includes 500MB storage + 1M API calls

## 🐛 Troubleshooting

### WordPress Content Not Syncing

```javascript
// Check:
1. WordPress REST API enabled
2. Credentials configured correctly
3. Posts are published (not draft/private)
4. Content not protected
5. Test: curl https://yoursite.com/wp-json/wp/v2/posts
```

### Embeddings Not Storing in Supabase

```javascript
// Verify:
1. pgvector extension enabled: CREATE EXTENSION vector;
2. documents table exists with vector column
3. Supabase credentials correct
4. API key has INSERT permissions
5. Network access allowed (check Supabase firewall)
```

### Chat Returns No Relevant Results

```javascript
// Solutions:
1. Run initial embedding workflow to populate database
2. Check if question matches your content topics
3. Try simpler, more specific questions
4. Increase topK value in retrieval (default: 5)
5. Verify embeddings stored: SELECT COUNT(*) FROM documents;
```

### OpenAI API Errors

```javascript
// Common issues:
1. API key expired/invalid
2. Quota exceeded - check https://platform.openai.com/account/usage
3. Rate limiting - add delay between requests
4. Wrong model name - verify in OpenAI docs
```

### Chat Memory Not Working

```javascript
// Check PostgreSQL table:
SELECT * FROM website_chat_histories;

// Ensure:
1. PostgreSQL credentials configured
2. Table exists and accessible
3. sessionId passed in chat request
4. Sufficient storage space
```

### Slow Vector Search

```javascript
// Optimize:
1. Reduce chunk size (faster but less context)
2. Reduce topK results (default 5)
3. Add vector index: CREATE INDEX ON documents USING ivfflat (embedding vector_cosine_ops);
4. Consider Supabase Pro plan for better performance
```

## 💡 Enhancement Ideas

- [ ] Add vector index for faster searches
- [ ] Implement response caching for common questions
- [ ] Add feedback collection (thumbs up/down)
- [ ] Multi-language support with translations
- [ ] Export chat conversations as PDF
- [ ] Analytics dashboard (popular questions, response quality)
- [ ] Custom website theme for chat widget
- [ ] Integration with Slack/Teams for internal Q&A
- [ ] Batch processing for large websites (100+ pages)
- [ ] Add image/document uploads to RAG system
- [ ] Implement cost tracking per conversation
- [ ] Create automated FAQ generation
- [ ] Add reranking for better result ordering
- [ ] Support for other CMS platforms (Webflow, Contentful)
- [ ] Implement guardrails for sensitive content

## 🔒 Security & Privacy

### Data Protection
- All content transmitted over HTTPS
- Supabase encryption at rest
- OpenAI API usage logged
- PostgreSQL password-protected

### Privacy Considerations
- **Data Retention**: Configure Supabase backup policies
- **User Data**: Chat history stored with sessionId
- **API Keys**: Stored securely in n8n vault
- **PII**: Monitor if website contains sensitive information

### Best Practices

```javascript
// Add content filtering
"systemMessage": "Do not share: passwords, API keys, personal info, emails"

// Implement rate limiting
Add delay in loop to prevent API abuse

// Monitor costs
Track OpenAI API usage in project dashboard
```

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

1. Fork the project
2. Create your feature branch (`git checkout -b feature/VectorIndexing`)
3. Commit your changes (`git commit -m 'Add vector indexing for performance'`)
4. Push to the branch (`git push origin feature/VectorIndexing`)
5. Open a Pull Request

### Development Guidelines

- Test with WordPress test environment first
- Verify Supabase connectivity before production
- Document any new environment variables
- Include error handling for edge cases
- Update system prompts if changing AI behavior

## 👨‍💻 Author

**Redoanuzzaman**
- GitHub: [@redoanuzzaman](https://github.com/redoanuzzaman)
- Email: redoanuzzaman707@gmail.com
- Website: [redoan.dev](https://redoan.dev)

## 🙏 Acknowledgments

- n8n community for workflow automation
- OpenAI for embeddings and GPT models
- Supabase for pgvector vector database
- WordPress REST API documentation
- LangChain for RAG patterns

## 💖 Show Your Support

Give a ⭐️ if you found this RAG system useful!

## 🔗 Useful Links

- [n8n Documentation](https://docs.n8n.io)
- [OpenAI Embeddings](https://platform.openai.com/docs/guides/embeddings)
- [Supabase pgvector](https://supabase.com/docs/guides/database/extensions/pgvector)
- [WordPress REST API](https://developer.wordpress.org/rest-api/)
- [RAG Best Practices](https://python.langchain.com/docs/use_cases/question_answering)
- [Vector Search Guide](https://supabase.com/docs/guides/ai)

## 📊 Workflow Statistics

- **Total Nodes**: 40+ (across 3 workflows)
- **API Integrations**: WordPress, OpenAI, Supabase, PostgreSQL
- **Embedding Dimension**: 1536 (text-embedding-3-small)
- **Max Chunk Size**: 300 tokens
- **Chat History**: PostgreSQL with sessionId tracking
- **Supported Content**: Posts, Pages (customizable)
- **Update Frequency**: Configurable (default: 30 seconds)

## 🎯 Use Case Examples

### E-commerce Product Support
```
User: "Does this product come in blue?"
AI: (Searches product pages) "Yes, according to our 
product page (updated Dec 2024), this item is available 
in blue. See details at https://..."
```

### Blog Reader Assistance
```
User: "What techniques does the author recommend?"
AI: (Retrieves blog post) "In the article 'Advanced SEO 
Techniques' published March 2024, the author recommends..."
```

### Documentation Search
```
User: "How do I set up the API?"
AI: (Finds relevant docs) "According to our setup guide 
(last updated July 2024), here are the steps..."
```

---

Made with 🤖 and RAG Architecture

**Last Updated:** October 2025
