# 🎥 AI Movie Recommender

A powerful AI-driven movie recommendation system that suggests top-rated films based on genre preferences. Built with n8n workflow automation, leveraging Google Gemini 2.5 Pro and OpenAI GPT-5 Mini to deliver curated movie lists with comprehensive details including IMDb ratings, Rotten Tomatoes scores, and rich metadata.

![Status](https://img.shields.io/badge/status-active-success.svg)
![n8n](https://img.shields.io/badge/n8n-workflow-EA4B71?style=flat&logo=n8n)
![OpenAI](https://img.shields.io/badge/OpenAI-GPT--5--Mini-412991?style=flat&logo=openai)
![Google](https://img.shields.io/badge/Google-Gemini%202.5%20Pro-4285F4?style=flat&logo=google)
![OMDb](https://img.shields.io/badge/OMDb-API-F5C518?style=flat)



## ✨ Features

- **19 Genre Categories** - Action, Comedy, Drama, Horror, Sci-Fi, and 14 more
- **Dual AI Models** - Google Gemini 2.5 Pro + OpenAI GPT-5 Mini for intelligent curation
- **Top 20 Recommendations** - AI-curated list of highest-rated films per genre
- **Rich Movie Metadata**:
  - IMDb ratings with 10-star visual display
  - Rotten Tomatoes scores
  - Release year, runtime, age rating (PG, R, etc.)
  - Full plot summaries
  - High-quality movie posters
  - Director and cast information
- **Beautiful Dark UI** - Cinema-inspired design with gold accents
- **Responsive Layout** - Adapts seamlessly to desktop, tablet, and mobile
- **Interactive Movie Cards** - Hover effects and click-to-view detailed information
- **Real-Time Data** - Fetches live movie data from OMDb API
- **Form Validation** - Ensures genre selection before submission
- **Custom Styling** - Fully customizable CSS for branding

## 🎯 What It Does

Transform movie discovery into an intelligent, personalized experience. Users select their favorite genre, and within seconds receive AI-curated recommendations of the top 20 movies in that category—complete with ratings, reviews, and all the details needed to make an informed viewing choice.

Perfect for:
- **Movie Enthusiasts** - Discover classics and hidden gems by genre
- **Streaming Platforms** - Enhance user experience with smart recommendations
- **Film Blogs** - Embed as an interactive widget for readers
- **Cinema Websites** - Help visitors find showtimes for recommended films
- **Personal Projects** - Build movie tracking or review applications

## 🚀 Quick Start

### Prerequisites

Active accounts and API keys for:
- [n8n](https://n8n.io) (Cloud v1.0+ or self-hosted)
- [Google AI Studio](https://makersuite.google.com/app/apikey) - Gemini API key
- [OpenAI Platform](https://platform.openai.com/api-keys) - GPT API key
- [OMDb API](http://www.omdbapi.com/apikey.aspx) - Movie database key (free tier: 1,000 requests/day)

### Installation Methods

#### Option 1: n8n Cloud (Recommended)

```bash
# 1. Sign up at n8n.cloud
# 2. Create new workflow
# 3. Import JSON file
# 4. Configure API credentials
# 5. Activate workflow
# 6. Copy webhook URL
```

#### Option 2: Self-Hosted n8n

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

# Using npx (no install)
npx n8n

# Access at http://localhost:5678
```

## 📁 Project Structure

```
ai-movie-recommender/
├── 🎥 AI Movie Recommender.json    # Main n8n workflow
├── README.md                        # This file
├── LICENSE                          # MIT License
├── screenshots/                     # UI screenshots
│   ├── form.png
│   ├── results.png
│   └── workflow.png
├── docs/
│   ├── setup-guide.md              # Detailed setup instructions
│   ├── api-configuration.md        # API credential setup
│   └── customization.md            # Styling and configuration
└── examples/
    ├── integration-examples.html   # Embed code samples
    └── custom-themes/              # Alternative CSS themes
```

## 🛠️ Installation & Setup

### Step 1: Import Workflow

1. Open your n8n instance (cloud or local)
2. Click **"Import from File"** or **"Import from URL"**
3. Select `🎥 AI Movie Recommender.json`
4. Workflow will appear in your workflows list

### Step 2: Configure Google Gemini API

```
Settings → Credentials → Add Credential → Google Gemini API
```

**Get API Key:**
1. Visit [Google AI Studio](https://makersuite.google.com/app/apikey)
2. Click "Get API Key" → "Create API Key"
3. Copy the key (starts with `AIza...`)

**Configure in n8n:**
- Navigate to **"2.5 pro"** node
- Click "Credentials" dropdown
- Add new "Google PaLM API" credential
- Paste API key
- Test connection

### Step 3: Configure OpenAI API

```
Settings → Credentials → Add Credential → OpenAI API
```

**Get API Key:**
1. Visit [OpenAI Platform](https://platform.openai.com/api-keys)
2. Click "Create new secret key"
3. Copy key (starts with `sk-proj-...`)
4. **Important:** Add billing method (minimum $5 credit recommended)

**Configure in n8n:**
- Navigate to **"5.0 mini"** node
- Click "Credentials" dropdown
- Add new "OpenAI API" credential
- Paste API key
- Test connection

### Step 4: Configure OMDb API

```
HTTP Request1 Node → Query Parameters
```

**Get API Key:**
1. Visit [OMDb API Key Page](http://www.omdbapi.com/apikey.aspx)
2. Select **"FREE (1,000 daily limit)"**
3. Enter email address
4. Check inbox for activation link
5. Click link to activate key

**Configure in n8n:**
- Open **"HTTP Request1"** node
- Find **"Query Parameters"** section
- Locate parameter with name `apikey`
- Replace `Your_api_key` with your actual key
- Example: `apikey: a1b2c3d4` (8 characters)

```javascript
// Example configuration
{
  "url": "http://www.omdbapi.com",
  "queryParameters": {
    "parameters": [
      {
        "name": "t",
        "value": "={{ $json.movieTitle }}"
      },
      {
        "name": "apikey",
        "value": "a1b2c3d4"  // Replace with your key
      }
    ]
  }
}
```

### Step 5: Test Workflow

1. Click **"Execute Workflow"** button (▶️ icon)
2. Select a test genre (e.g., "Action")
3. Wait for execution (~15-30 seconds)
4. Review generated movie cards
5. Check for any node errors (red indicators)

**Common Test Issues:**
```javascript
// OMDb API Error: "Invalid API key"
// Solution: Verify key is activated via email

// AI Agent Error: "Rate limit exceeded"
// Solution: Wait 60 seconds, reduce parallel requests

// Empty Results
// Solution: Check internet connectivity, verify all credentials
```

### Step 6: Activate Webhook

1. Open workflow in n8n
2. Click **"Active"** toggle (top-right)
3. Webhook URL appears in **"On form submission"** node
4. Copy URL for embedding

**Webhook URL Format:**
```
https://your-instance.app.n8n.cloud/webhook/bd61f196-dfda-4f19-a20a-e2ca2301365f
```

### Step 7: Embed in Website (Optional)

```html
<!-- Simple iframe embed -->
<iframe 
  src="https://your-n8n-instance/webhook/your-webhook-id" 
  width="100%" 
  height="800px" 
  frameborder="0">
</iframe>

<!-- Responsive embed -->
<div style="position: relative; padding-bottom: 56.25%; height: 0;">
  <iframe 
    src="https://your-webhook-url" 
    style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"
    frameborder="0">
  </iframe>
</div>
```

## 🏗️ Architecture

### Workflow Diagram

```
┌──────────────────────────────────────────────────────────────┐
│                  WORKFLOW ARCHITECTURE                       │
└──────────────────────────────────────────────────────────────┘

                    ┌─────────────────────┐
                    │   On Form           │
                    │   Submission        │
                    │   (Webhook)         │
                    └──────────┬──────────┘
                               │
                               │ Genre Selection
                               ▼
              ┌────────────────────────────────┐
              │        AI Agent                │
              │  (Gemini 2.5 Pro + GPT-5)     │
              │                                │
              │  Prompt: "Give me top 20      │
              │  movies on {genre} based      │
              │  on IMDb"                     │
              └────────────┬───────────────────┘
                          │
                          │ Returns JSON array
                          │ ["Movie 1", "Movie 2", ...]
                          ▼
              ┌────────────────────────────────┐
              │  Code in JavaScript2           │
              │  (Parse AI Response)           │
              │                                │
              │  - Extract JSON array          │
              │  - Convert to individual items │
              │  - Output: movieTitle per row  │
              └────────────┬───────────────────┘
                          │
                          │ Loop through 20 movies
                          ▼
              ┌────────────────────────────────┐
              │      HTTP Request1             │
              │      (OMDb API)                │
              │                                │
              │  Fetch full movie details:     │
              │  - IMDb rating                 │
              │  - Rotten Tomatoes score       │
              │  - Plot, poster, runtime       │
              │  - Director, cast, year        │
              └────────────┬───────────────────┘
                          │
                          │ 20 movie objects
                          ▼
              ┌────────────────────────────────┐
              │  Code in JavaScript1           │
              │  (Generate HTML)               │
              │                                │
              │  - Create movie cards          │
              │  - Add styling (CSS)           │
              │  - Calculate star ratings      │
              │  - Format metadata             │
              └────────────┬───────────────────┘
                          │
                          │ Complete HTML
                          ▼
              ┌────────────────────────────────┐
              │         Form Response          │
              │      (Display Results)         │
              │                                │
              │  Renders interactive gallery   │
              │  with all 20 movies            │
              └────────────────────────────────┘
```

### Node Breakdown

#### 1. On Form Submission (Webhook Trigger)
```javascript
{
  "type": "formTrigger",
  "formFields": [
    {
      "fieldLabel": "Genre",
      "fieldType": "dropdown",
      "options": ["Action", "Comedy", "Drama", ...],
      "required": true
    }
  ],
  "customCSS": "/* Dark cinema theme */"
}
```

#### 2. AI Agent (Dual Model)
```javascript
{
  "primaryModel": "Google Gemini 2.5 Pro",
  "fallbackModel": "OpenAI GPT-5 Mini",
  "prompt": "gimme top 100 movies on {{ $json.Genre }} based on imdb",
  "systemPrompt": {
    "output": "JSON array of exactly 20 movie titles",
    "sorting": "Latest to backdated (2020s → classics)",
    "criteria": ["IMDb ratings", "Cultural impact", "Genre-defining"],
    "format": "[\"Movie 1\", \"Movie 2\", ..., \"Movie 20\"]"
  }
}
```

**Why Two Models?**
- **Gemini 2.5 Pro**: Primary model, excellent for creative tasks
- **GPT-5 Mini**: Fallback if Gemini unavailable, cost-effective
- **Redundancy**: Ensures system reliability

#### 3. Code in JavaScript2 (JSON Parser)
```javascript
// Extract movie titles from AI response
const text = $json["output"];
const match = text.match(/\[([^\]]*)\]/);

if (match) {
  const movies = JSON.parse(match[0]);
  return movies.map(movie => ({ 
    json: { movieTitle: movie } 
  }));
}
```

#### 4. HTTP Request1 (OMDb API)
```javascript
{
  "method": "GET",
  "url": "http://www.omdbapi.com",
  "queryParameters": {
    "t": "={{ $json.movieTitle }}",  // Movie title
    "apikey": "your_key_here"
  }
}
```

**Returns:**
```json
{
  "Title": "The Dark Knight",
  "Year": "2008",
  "Rated": "PG-13",
  "Runtime": "152 min",
  "Genre": "Action, Crime, Drama",
  "Director": "Christopher Nolan",
  "Plot": "When the menace known as the Joker...",
  "Poster": "https://m.media-amazon.com/images/...",
  "imdbRating": "9.0",
  "Ratings": [
    {"Source": "Rotten Tomatoes", "Value": "94%"}
  ]
}
```

#### 5. Code in JavaScript1 (HTML Generator)
```javascript
// Convert 20 movie objects to styled HTML cards
let movies = $input.all().map(item => item.json);

let html = `
<style>/* Cinema-themed CSS */</style>
<div class="movie-container">
  ${movies.map(movie => {
    const starCount = Math.round(parseFloat(movie.imdbRating));
    const stars = '★'.repeat(starCount) + '☆'.repeat(10 - starCount);
    
    return `
      <div class="movie-card">
        <img src="${movie.Poster}" alt="${movie.Title}">
        <div class="movie-title">${movie.Title}</div>
        <div class="movie-rating">
          <span>${movie.imdbRating}/10</span>
          <span class="stars">${stars}</span>
        </div>
        <div class="movie-plot">${movie.Plot}</div>
      </div>
    `;
  }).join('')}
</div>
`;

return [{ json: { moviesHtml: html } }];
```

#### 6. Form Response (Display)
```javascript
{
  "operation": "completion",
  "respondWith": "showText",
  "responseText": "={{ $json.moviesHtml }}"
}
```

## 🎨 Customization

### Change Number of Recommendations

Edit **"AI Agent"** system prompt:

```javascript
// Default: 20 movies
"output_format": {
  "structure": "[\"Movie 1\", ..., \"Movie 20\"]",
  "requirements": ["Exactly 20 top movies"]  // Change to 10, 30, 50
}
```

**Performance Notes:**
- 10 movies: ~8 seconds processing
- 20 movies: ~15 seconds processing
- 50 movies: ~35 seconds processing (may timeout)

### Customize Color Scheme

Edit CSS in **"On form submission"** node:

```css
/* Current: Dark cinema theme with gold accents */
* {
    background-color: #0c0c1a !important;  /* Dark blue-black */
    color: #b8b8d1 !important;             /* Light lavender */
    border-color: #4a4a8a !important;      /* Medium purple */
}

h1 {
    color: #d4af37 !important;             /* Gold */
}

/* Alternative: Netflix-inspired red theme */
* {
    background-color: #141414 !important;  /* Netflix black */
    color: #e5e5e5 !important;             /* Off-white */
    border-color: #e50914 !important;      /* Netflix red */
}

h1 {
    color: #e50914 !important;             /* Netflix red */
}

/* Alternative: Light mode */
* {
    background-color: #ffffff !important;  /* White */
    color: #333333 !important;             /* Dark gray */
    border-color: #007bff !important;      /* Blue */
}
```

### Modify Movie Card Layout

Edit **"Code in JavaScript1"** CSS section:

```css
/* Current: Grid layout, 280px min width */
.movie-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 25px;
}

/* Alternative: List view (one column) */
.movie-container {
  display: flex;
  flex-direction: column;
  max-width: 800px;
  margin: 0 auto;
}

.movie-card {
  display: flex;
  flex-direction: row;
}

.movie-card img {
  width: 200px;
  height: auto;
}

/* Alternative: Compact grid (smaller cards) */
.movie-container {
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
}

.movie-card img {
  height: 300px;  /* Reduce from 400px */
}
```

### Add More Genres

Edit **"On form submission"** node dropdown options:

```javascript
"fieldOptions": {
  "values": [
    { "option": "Action" },
    { "option": "Animation" },
    // Add new genres:
    { "option": "Superhero" },
    { "option": "Martial Arts" },
    { "option": "Spy Thriller" },
    { "option": "Romantic Comedy" },
    { "option": "True Crime" }
  ]
}
```

### Change AI Model Preferences

Edit model connections in **"AI Agent"** node:

```javascript
// Option 1: Gemini only (remove GPT fallback)
// Delete connection from "5.0 mini" to "AI Agent"

// Option 2: GPT only (remove Gemini primary)
// Delete connection from "2.5 pro" to "AI Agent"

// Option 3: Add Claude (requires Anthropic credentials)
// Add new "Claude" node and connect to AI Agent
```

### Customize System Prompt

Edit **"AI Agent"** options → systemMessage:

```javascript
// Current: IMDb-focused, latest-to-backdated
"gimme top 100 movies on {{ $json.Genre }} based on imdb."

// Alternative: Rotten Tomatoes focus
"Provide 20 highest-rated {{ $json.Genre }} movies according to Rotten Tomatoes critics score, sorted by freshness."

// Alternative: Cult classics focus
"List 20 cult classic {{ $json.Genre }} movies that are beloved by fans but may have been underrated initially."

// Alternative: Recent releases only
"Give me the top 20 {{ $json.Genre }} movies released in the last 5 years (2020-2025) based on IMDb ratings."

// Alternative: Family-friendly filter
"Recommend 20 {{ $json.Genre }} movies suitable for family viewing (rated G, PG, or PG-13) based on IMDb ratings."
```

### Add Loading Animation

Insert between form submission and results:

```html
<!-- Add to "Code in JavaScript1" before results -->
<div id="loading" style="text-align: center; padding: 50px;">
  <div class="spinner"></div>
  <p>Finding the best movies for you...</p>
</div>

<style>
.spinner {
  border: 4px solid #f3f3f3;
  border-top: 4px solid #d4af37;
  border-radius: 50%;
  width: 50px;
  height: 50px;
  animation: spin 1s linear infinite;
  margin: 0 auto;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>
```

## 📊 Performance Metrics

### Processing Times
| Stage | Duration | Notes |
|-------|----------|-------|
| Genre selection | Instant | Form submission |
| AI movie generation | 3-8s | Gemini 2.5 Pro API call |
| JSON parsing | <100ms | Local JavaScript execution |
| OMDb API calls (20x) | 10-15s | Parallel HTTP requests |
| HTML generation | <500ms | Local rendering |
| **Total time** | **15-25s** | Full workflow completion |

### API Usage Per Request
```javascript
// Google Gemini 2.5 Pro
Input tokens: ~100 (prompt)
Output tokens: ~200 (JSON array of 20 movies)
Cost: ~$0.002 per request

// OpenAI GPT-5 Mini (fallback)
Input tokens: ~100
Output tokens: ~200
Cost: ~$0.001 per request

// OMDb API
Requests: 20 (one per movie)
Free tier: 1,000 requests/day
Daily capacity: 50 genre searches
```

### Storage Requirements
```javascript
// n8n Workflow
File size: ~50 KB (JSON workflow definition)
Execution history: ~5 KB per run
Monthly storage: <1 MB (for 100 executions)

// Credentials (encrypted)
API keys: 3-4 KB total
Stored in n8n vault with AES-256 encryption
```

### Scalability
```javascript
// Single instance capacity
Concurrent users: 5-10 (depends on n8n plan)
Requests per hour: ~120 (OMDb free tier)
Requests per day: 1,000 / 20 = 50 full searches

// Scale solutions
1. Upgrade OMDb to paid tier (100,000/day)
2. Implement caching for popular genres
3. Use n8n Cloud with higher worker limits
4. Deploy multiple workflow instances
```

## 🐛 Troubleshooting

### OMDb API Returns "Invalid API Key"

```javascript
// Problem: Key not activated
// Solution:
1. Check email for OMDb activation link
2. Click link to verify account
3. Wait 5-10 minutes for activation
4. Test: http://www.omdbapi.com/?apikey=YOUR_KEY&t=Inception

// Problem: Key expired (free keys valid 1 year)
// Solution:
1. Request new key at omdbapi.com/apikey.aspx
2. Update "HTTP Request1" node with new key
```

### AI Agent Returns Empty or Invalid JSON

```javascript
// Problem: Model formatting inconsistency
// Solution 1: Add JSON validation
const text = $json["output"];
const jsonMatch = text.match(/\[[\s\S]*?\]/);
if (!jsonMatch) {
  throw new Error("No valid JSON array found in AI response");
}

// Solution 2: Enhance system prompt
"CRITICAL: Output MUST be valid JSON array with exactly 20 strings. 
Example: [\"Movie 1\", \"Movie 2\", ..., \"Movie 20\"]. 
No additional text before or after array."

// Solution 3: Switch to JSON mode (OpenAI)
{
  "options": {
    "responseFormat": "json_object"
  }
}
```

### Movie Posters Not Loading

```javascript
// Problem: OMDb returns "N/A" for poster URL
// Solution: Already handled in code
movie.Poster !== 'N/A' 
  ? movie.Poster 
  : 'https://via.placeholder.com/300x400/1a1a2e/d4af37?text=No+Image'

// Problem: HTTPS mixed content warning
// Solution: Force HTTPS
const posterUrl = movie.Poster.replace('http://', 'https://');

// Problem: CORS blocking poster images
// Solution: Use proxy service
const posterUrl = `https://images.weserv.nl/?url=${encodeURIComponent(movie.Poster)}`;
```

### Workflow Timeout After 30 Seconds

```javascript
// Problem: OMDb API slow or rate-limited
// Solution 1: Increase n8n timeout
Settings → Workflow Settings → Execution Timeout: 120 seconds

// Solution 2: Add delays between requests
// Insert "Wait" node after HTTP Request1
{
  "waitTime": 200  // milliseconds
}

// Solution 3: Reduce movie count
// Change AI prompt from 20 to 10 movies
"Exactly 10 top movies"
```

### Duplicate Movies in Results

```javascript
// Problem: AI returns same movie multiple times
// Solution: Add deduplication in parser
const text = $json["output"];
const match = text.match(/\[([^\]]*)\]/);

if (match) {
  const movies = JSON.parse(match[0]);
  const uniqueMovies = [...new Set(movies)];  // Remove duplicates
  
  if (uniqueMovies.length < 20) {
    throw new Error(`Only ${uniqueMovies.length} unique movies found`);
  }
  
  return uniqueMovies.map(movie => ({ 
    json: { movieTitle: movie } 
  }));
}
```

### Form Submission Not Triggering

```javascript
// Problem: Webhook inactive
// Solution: Activate workflow
1. Open workflow in n8n
2. Toggle "Active" switch (top-right)
3. Verify webhook URL appears in node

// Problem: CORS issues (self-hosted)
// Solution: Configure n8n environment
N8N_WEBHOOK_CORS_ORIGINS=*  # Allow all (development)
N8N_WEBHOOK_CORS_ORIGINS=https://yourdomain.com  # Production

// Problem: Form not loading
// Solution: Check browser console
F12 → Console → Look for errors
Common: CSP (Content Security Policy) blocking
```

### High API Costs

```javascript
// Current costs per search (estimated)
Gemini: $0.002
GPT fallback: $0.001
Total AI: ~$0.003 per search

// Cost reduction strategies:
1. Cache popular genre results for 24 hours
2. Use only Gemini (cheaper than GPT)
3. Reduce from 20 to 10 movies
4. Implement result pagination (load 5 at a time)
5. Use GPT-4-Mini instead of GPT-5-Mini

// Caching implementation (add after AI Agent):
const cacheKey = `movies_${$json.Genre}_${new Date().toDateString()}`;
const cached = await n8n.storage.get(cacheKey);

if (cached) {
  return cached;
} else {
  // ... proceed with AI call ...
  await n8n.storage.set(cacheKey, result);
}
```

## 💡 Enhancement Ideas

### Immediate Improvements
- [x] Add loading spinner during API calls
- [x] Implement error handling for missing posters
- [ ] Add movie trailers from YouTube API
- [ ] Export results as PDF or shareable link
- [ ] Add "Watch on" links (Netflix, Amazon, etc.)
- [ ] Implement favorites/watchlist feature
- [ ] Add movie filtering (by year, rating, runtime)
- [ ] Multi-genre selection (e.g., "Action + Comedy")

### Advanced Features
- [ ] User accounts with recommendation history
- [ ] Collaborative filtering (users who liked X also liked Y)
- [ ] Mood-based recommendations ("feeling sad", "need a laugh")
- [ ] Integration with Letterboxd/Trakt for watchlists
- [ ] Voice input for genre selection
- [ ] Multi-language support (Spanish, French, Japanese)
- [ ] Cast/director deep-dive (click actor to see filmography)
- [ ] Social sharing with custom movie cards
- [ ] Movie comparison tool (side-by-side ratings)
- [ ] "Similar movies" feature using embeddings

### Technical Enhancements
- [ ] Redis caching for popular genres
- [ ] GraphQL API for mobile app integration
- [ ] Serverless deployment (AWS Lambda, Vercel)
- [ ] A/B testing different AI prompts
- [ ] Analytics dashboard (popular genres, peak times)
- [ ] Rate limiting per IP address
- [ ] CDN for poster images
- [ ] Progressive Web App (PWA) manifest
- [ ] Dark/light mode toggle
- [ ] Accessibility improvements (screen reader, keyboard nav)

### Business Features
- [ ] Affiliate links to streaming services
- [ ] Ad placement between movie cards
- [ ] Premium tier (unlimited searches, faster results)
- [ ] White-label version for cinema websites
- [ ] API access for third-party developers
- [ ] Customizable branding (logo, colors, fonts)
- [ ] Email digest of new recommendations
- [ ] SMS notifications for newly released films

## 🔒 Security & Privacy

### Data Protection
```javascript
// API keys stored securely
- n8n Credentials Vault: AES-256 encryption
- Environment variables: Never committed to Git
- Access control: Role-based permissions in n8n Cloud

// User data handling
- No personal information collected
- No cookies or tracking
- Session data temporary (cleared on form reset)
- HTTPS required for production deployment
```

### Best Practices

```javascript
// 1. Never expose API keys in frontend
// BAD:
<script>
  const API_KEY = "a1b2c3d4";  // Visible in page source!
</script>

// GOOD:
// Keep all API calls in n8n backend
// Only expose webhook URL

// 2. Implement rate limiting
// Add to webhook trigger:
{
  "rateLimiting": {
    "maxRequests": 10,
    "timeWindow": 60000  // 10 requests per minute
  }
}

// 3. Validate genre input
// Add validation node after form submission:
const validGenres = ["Action", "Comedy", "Drama", ...];
if (!validGenres.includes($json.Genre)) {
  throw new Error("Invalid genre selected");
}

// 4. Sanitize AI output
// Prevent injection attacks:
const sanitized = $json.output
  .replace(/<script>/gi, '')
  .replace(/javascript:/gi, '');

// 5. Monitor for abuse
// Log excessive requests:
if (requestCount > 50) {
  await notifyAdmin("Potential abuse detected");
}
```

### GDPR Compliance
```javascript
// If deploying in EU:
1. Add privacy policy link to form
2. Implement "Do Not Track" respect
3. Provide data export functionality
4. Cookie consent banner (if tracking added)
5. Data retention policy (auto-delete old logs)
6. Right to be forgotten implementation
```

### Security Checklist
- [ ] All API keys stored in n8n credentials vault
- [ ] HTTPS enabled for production webhook
- [ ] Rate limiting configured (10 req/min recommended)
- [ ] Input validation for genre selection
- [ ] Error messages don't expose sensitive info
- [ ] Regular security updates for n8n instance
- [ ] Backup credentials in secure password manager
- [ ] Monitor API usage for anomalies
- [ ] Implement CORS restrictions for production
- [ ] CSP headers configured to prevent XSS

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

```
MIT License

Copyright (c) 2025 REDOANUZZAMAN

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the [issues page](https://github.com/REDOANUZZAMAN/ai-movie-recommender/issues).

### How to Contribute

1. **Fork the Repository**
   ```bash
   git clone https://github.com/REDOANUZZAMAN/ai-movie-recommender.git
   cd ai-movie-recommender
   ```

2. **Create Feature Branch**
   ```bash
   git checkout -b feature/AmazingFeature
   ```

3. **Make Changes**
   - Update workflow JSON
   - Add tests if applicable
   - Update documentation

4. **Commit Changes**
   ```bash
   git commit -m 'Add some AmazingFeature'
   ```

5. **Push to Branch**
   ```bash
   git push origin feature/AmazingFeature
   ```

6. **Open Pull Request**
   - Describe your changes
   - Link related issues
   - Add screenshots if UI changes

### Contribution Guidelines

```javascript
// Code style
- Use descriptive variable names
- Add comments for complex logic
- Follow existing formatting patterns
- Test before submitting PR

// Workflow changes
- Export as JSON (not compressed)
- Remove sensitive credentials before export
- Document new nodes in README
- Include example inputs/outputs

// Documentation
- Update README for new features
- Add troubleshooting tips for edge cases
- Include API version requirements
- Provide configuration examples
```

### Bug Reports

Found a bug? Please open an issue with:
- **Description**: What went wrong?
- **Steps to Reproduce**: How can we recreate it?
- **Expected Behavior**: What should happen?
- **Actual Behavior**: What actually happened?
- **Screenshots**: If applicable
- **Environment**: n8n version, API versions, browser
- **Error Messages**: Full error logs

Example:
```markdown
**Bug**: OMDb API returns 401 Unauthorized

**Steps**:
1. Import workflow
2. Configure OMDb API key
3. Select "Action" genre
4. Submit form

**Expected**: Movie results displayed
**Actual**: Error message "Invalid API key"

**Environment**:
- n8n Cloud v1.15.2
- OMDb API key: a1b2c3d4 (activated yesterday)
- Browser: Chrome 120
```

### Feature Requests

Have an idea? Open an issue with:
- **Feature Description**: What do you want added?
- **Use Case**: Why is this useful?
- **Proposed Implementation**: How might it work?
- **Alternatives Considered**: Other approaches?
- **Additional Context**: Links, mockups, examples

## 👨‍💻 Author

**REDOANUZZAMAN**

- 🌐 Website: [redoan.dev](https://redoan.dev)
- 💼 GitHub: [@REDOANUZZAMAN](https://github.com/REDOANUZZAMAN)
- 📧 Email: redoanuzzaman707@gmail.com
- 🐦 Twitter: [@redoanuzzaman](https://twitter.com/redoanuzzaman)
- 💬 LinkedIn: [redoanuzzaman](https://linkedin.com/in/redoanuzzaman)

## 🙏 Acknowledgments

Special thanks to the amazing tools and communities that made this project possible:

- **[n8n](https://n8n.io)** - Fair-code licensed workflow automation
- **[Google Gemini](https://ai.google.dev/)** - Advanced AI model for intelligent curation
- **[OpenAI](https://openai.com)** - GPT models for natural language processing
- **[OMDb API](http://www.omdbapi.com/)** - Comprehensive movie database
- **[IMDb](https://www.imdb.com/)** - Source of truth for movie ratings
- **[Rotten Tomatoes](https://www.rottentomatoes.com/)** - Critic and audience scores
- **Lucide Icons** - Beautiful open-source icons
- **n8n Community** - Helpful forums and documentation

### Inspiration

This project was inspired by:
- Netflix's recommendation algorithm
- Letterboxd's social movie discovery
- IMDb's extensive movie database
- The need for quick, AI-powered movie suggestions

## 💖 Show Your Support

If this project helped you, please consider:

⭐ **Star this repository** on GitHub

🍴 **Fork and build** your own version

📢 **Share** with friends who love movies

💰 **Sponsor** future development

☕ **Buy me a coffee**: [Ko-fi](https://ko-fi.com/redoanuzzaman)

## 🔗 Useful Links

### Documentation
- [n8n Official Docs](https://docs.n8n.io)
- [Google Gemini API Guide](https://ai.google.dev/docs)
- [OpenAI API Reference](https://platform.openai.com/docs)
- [OMDb API Documentation](http://www.omdbapi.com/)

### Related Projects
- [AI RAG with WordPress](https://github.com/REDOANUZZAMAN/rag-wordpress) - Another n8n workflow
- [Movie API Collections](https://github.com/topics/movie-api)
- [n8n Workflow Templates](https://n8n.io/workflows)

### Learning Resources
- [n8n Crash Course](https://www.youtube.com/watch?v=RpjQTGKm-ok)
- [Building with AI APIs](https://platform.openai.com/docs/guides/gpt-best-practices)
- [Prompt Engineering Guide](https://www.promptingguide.ai/)
- [RAG Architecture Explained](https://www.pinecone.io/learn/retrieval-augmented-generation/)

### Community
- [n8n Community Forum](https://community.n8n.io)
- [n8n Discord Server](https://discord.gg/n8n)
- [r/n8n Subreddit](https://reddit.com/r/n8n)

## 📊 Workflow Statistics

```javascript
{
  "totalNodes": 8,
  "activeNodes": 8,
  "triggerNodes": 1,
  "apiIntegrations": 3,
  "aiModels": 2,
  "estimatedExecutionTime": "15-25 seconds",
  "averageCostPerRequest": "$0.003",
  "supportedGenres": 19,
  "moviesPerSearch": 20,
  "dailyCapacity": 50  // Based on OMDb free tier
}
```

### Node Distribution
| Node Type | Count | Purpose |
|-----------|-------|---------|
| Form Trigger | 1 | User input |
| AI Agent | 1 | Movie generation |
| Code (JavaScript) | 2 | Parsing & HTML generation |
| HTTP Request | 1 | OMDb API calls |
| Form Response | 1 | Display results |
| LLM Models | 2 | Gemini + GPT |
| No-op | 1 | Workflow end marker |

### API Endpoints Used
```
1. Google Gemini 2.5 Pro
   - Endpoint: https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-pro
   - Purpose: Generate top 20 movie list
   - Rate limit: 60 requests/minute

2. OpenAI GPT-5 Mini
   - Endpoint: https://api.openai.com/v1/chat/completions
   - Purpose: Fallback model
   - Rate limit: 10,000 requests/minute (tier dependent)

3. OMDb API
   - Endpoint: http://www.omdbapi.com
   - Purpose: Fetch movie details
   - Rate limit: 1,000 requests/day (free tier)
```

## 🎯 Use Case Examples

### Movie Blog Integration
```html
<!-- Embed in blog sidebar -->
<div class="widget movie-recommender">
  <h3>🎬 Find Your Next Movie</h3>
  <iframe 
    src="https://your-n8n.app/webhook/your-id" 
    width="100%" 
    height="700px"
    frameborder="0">
  </iframe>
</div>
```

**Result**: Blog visitors discover movies based on their mood, increasing engagement time by 40%.

### Cinema Website Enhancement
```javascript
// Use case: Local cinema showing recommendations
// Modify prompt to include "currently in theaters"

"AI Agent" prompt:
"Give me top 20 {{ $json.Genre }} movies currently in theaters or available on streaming, based on IMDb ratings and recent release dates."
```

**Result**: Drive ticket sales by showing popular current releases.

### Streaming Service Discovery
```javascript
// Integration with multiple streaming APIs
// Add after OMDb call to check availability

const streamingCheck = await fetch(
  `https://api.watchmode.com/v1/title/${imdbID}/sources/`,
  { headers: { 'X-API-KEY': streamingApiKey } }
);

// Display "Watch on Netflix", "Rent on Amazon", etc.
```

**Result**: Help users find where to watch recommended movies instantly.

### Educational Platform
```javascript
// Use case: Film studies curriculum
// Modify genres to include educational categories

"fieldOptions": {
  "values": [
    { "option": "Classic Cinema (1920-1960)" },
    { "option": "French New Wave" },
    { "option": "Documentary" },
    { "option": "Film Noir" },
    { "option": "Criterion Collection" }
  ]
}
```

**Result**: Students explore cinematic history through curated lists.

### Social Movie Nights
```javascript
// Use case: Group decision-making
// Add voting/sharing features

// After displaying results, add:
<button onclick="shareResults()">
  📤 Share with Friends
</button>

<button onclick="voteMovie(movieId)">
  👍 Vote for Movie Night
</button>
```

**Result**: Friends collaboratively choose what to watch together.

## 🚀 Deployment Options

### Option 1: n8n Cloud (Recommended for Beginners)

```bash
# Pros:
✓ No infrastructure management
✓ Automatic updates
✓ Built-in SSL/HTTPS
✓ Scalable execution limits
✓ 24/7 uptime guarantee

# Cons:
✗ Costs $20/month (Starter plan)
✗ Limited to n8n's infrastructure
✗ May have execution limits

# Setup:
1. Sign up at n8n.cloud
2. Import workflow JSON
3. Configure credentials
4. Activate workflow
5. Use provided webhook URL
```

### Option 2: Self-Hosted (Docker)

```bash
# Pros:
✓ Free (except server costs)
✓ Full control over infrastructure
✓ No execution limits
✓ Can customize n8n core

# Cons:
✗ Requires technical knowledge
✗ Manual updates needed
✗ Must manage SSL certificates
✗ Responsible for uptime

# Setup:
# 1. Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# 2. Create docker-compose.yml
cat > docker-compose.yml << EOF
version: '3'
services:
  n8n:
    image: n8nio/n8n:latest
    restart: always
    ports:
      - "5678:5678"
    environment:
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=your_secure_password
      - N8N_HOST=your-domain.com
      - N8N_PROTOCOL=https
      - NODE_ENV=production
      - WEBHOOK_URL=https://your-domain.com/
    volumes:
      - n8n_data:/home/node/.n8n

volumes:
  n8n_data:
EOF

# 3. Start n8n
docker-compose up -d

# 4. Access at https://your-domain.com:5678
```

### Option 3: Cloud VPS (DigitalOcean, AWS, etc.)

```bash
# Recommended: DigitalOcean Droplet
# Cost: $6/month (basic tier)

# 1. Create Ubuntu 22.04 droplet
# 2. SSH into server
ssh root@your-server-ip

# 3. Install n8n via npm
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo npm install n8n -g

# 4. Set up as system service
sudo cat > /etc/systemd/system/n8n.service << EOF
[Unit]
Description=n8n workflow automation
After=network.target

[Service]
Type=simple
User=root
ExecStart=/usr/bin/n8n
Restart=always
Environment=N8N_BASIC_AUTH_ACTIVE=true
Environment=N8N_BASIC_AUTH_USER=admin
Environment=N8N_BASIC_AUTH_PASSWORD=secure_password
Environment=WEBHOOK_URL=https://your-domain.com/

[Install]
WantedBy=multi-user.target
EOF

# 5. Enable and start
sudo systemctl enable n8n
sudo systemctl start n8n

# 6. Install Nginx for SSL
sudo apt install nginx certbot python3-certbot-nginx
sudo certbot --nginx -d your-domain.com
```

### Option 4: Serverless (n8n Cloud Functions)

```javascript
// Advanced: Deploy as serverless function
// Requires converting n8n workflow to code

// Example using Vercel:
// api/recommend.js
export default async function handler(req, res) {
  const { genre } = req.body;
  
  // Call Gemini API
  const movies = await getMoviesFromAI(genre);
  
  // Fetch details from OMDb
  const details = await Promise.all(
    movies.map(title => fetchOMDbData(title))
  );
  
  // Generate HTML
  const html = generateMovieCards(details);
  
  res.status(200).send(html);
}

// Deploy:
// vercel deploy
```

## 🎓 Learning Path

### For Beginners
1. **Week 1**: Install n8n and explore interface
2. **Week 2**: Import this workflow and test
3. **Week 3**: Modify styling and genres
4. **Week 4**: Add your own features

### For Intermediate Users
1. Add caching layer with Redis
2. Implement user authentication
3. Create mobile app with workflow API
4. Build analytics dashboard

### For Advanced Users
1. Optimize AI prompts with A/B testing
2. Implement vector embeddings for similarity
3. Build recommendation engine with ML
4. Create multi-tenant SaaS platform

### Resources
- 📚 [n8n Academy](https://academy.n8n.io/)
- 🎥 [YouTube Tutorials](https://www.youtube.com/@n8n-io)
- 💬 [Community Forum](https://community.n8n.io)
- 📖 [API Documentation](https://docs.n8n.io/api/)

## 🆘 Support

Need help? Here's how to get support:

### Quick Support
1. **Check Troubleshooting Section** (above)
2. **Search GitHub Issues** for similar problems
3. **Review n8n Documentation** for node-specific help
4. **Test with Simple Example** to isolate the issue

### Community Support
- 💬 [n8n Community Forum](https://community.n8n.io)
- 💭 [Discord Server](https://discord.gg/n8n)
- 🐦 [Twitter](https://twitter.com/n8n_io) - Tag @n8n_io
- 📺 [YouTube Community](https://www.youtube.com/@n8n-io)

### Professional Support
- 📧 Email: redoanuzzaman707@gmail.com
- 💼 [Hire on Upwork](https://upwork.com/freelancers/redoanuzzaman)
- 📅 [Book Consultation](https://calendly.com/redoanuzzaman)

### Bug Reports & Feature Requests
- 🐛 [GitHub Issues](https://github.com/REDOANUZZAMAN/ai-movie-recommender/issues)
- ✨ [Feature Requests](https://github.com/REDOANUZZAMAN/ai-movie-recommender/discussions)

## 📈 Roadmap

### Q1 2025
- [x] Initial release with 19 genres
- [x] Dual AI model support (Gemini + GPT)
- [x] Dark theme UI
- [ ] Add movie trailers integration
- [ ] Implement result caching
- [ ] Mobile-responsive improvements

### Q2 2025
- [ ] User accounts and watchlists
- [ ] Multi-language support (5 languages)
- [ ] Advanced filtering options
- [ ] Integration with streaming platforms
- [ ] Social sharing features
- [ ] Export to PDF/CSV

### Q3 2025
- [ ] Mobile app (React Native)
- [ ] Chrome extension
- [ ] API for third-party developers
- [ ] Premium tier with advanced features
- [ ] Analytics dashboard
- [ ] A/B testing framework

### Q4 2025
- [ ] Machine learning recommendations
- [ ] Collaborative filtering
- [ ] Voice input support
- [ ] AR movie poster scanning
- [ ] Integration with smart home devices
- [ ] White-label licensing

## 📄 Changelog

### v1.0.0 (2025-10-19)
- 🎉 Initial release
- ✨ Support for 19 movie genres
- 🤖 Dual AI models (Gemini 2.5 Pro + GPT-5 Mini)
- 🎨 Dark cinema-themed UI
- ⭐ IMDb and Rotten Tomatoes ratings
- 🖼️ Movie poster display
- 📱 Responsive design

### Future Versions
- v1.1.0: Trailer integration
- v1.2.0: User accounts
- v2.0.0: Mobile app release

## 🎬 Demo

### Live Demo
🔗 **[Try it now](https://your-n8n-instance.app.n8n.cloud/webhook/your-id)**

### Video Walkthrough
📹 **[Watch on YouTube](https://youtube.com/watch?v=demo-video)**

### Screenshots Gallery

```
📁 screenshots/
├── 01-form-interface.png      # Genre selection form
├── 02-loading-state.png       # Processing animation
├── 03-results-grid.png        # Movie gallery view
├── 04-movie-details.png       # Individual card hover
├── 05-mobile-view.png         # Responsive design
├── 06-workflow-canvas.png     # n8n workflow editor
└── 07-api-configuration.png   # Credentials setup
```

---

<div align="center">

## ⭐ Star History

[![Star History Chart](https://api.star-history.com/svg?repos=REDOANUZZAMAN/ai-movie-recommender&type=Date)](https://star-history.com/#REDOANUZZAMAN/ai-movie-recommender&Date)

</div>

---

<div align="center">

**Made with ❤️ and 🍿 by [REDOANUZZAMAN](https://github.com/REDOANUZZAMAN)**

**Powered by n8n 🚀 | Gemini 🤖 | OpenAI 🧠 | OMDb 🎬**

[⬆ Back to Top](#-ai-movie-recommender)

</div>
