[LTP_Platform_README.md](https://github.com/user-attachments/files/29438068/LTP_Platform_README.md)
# Learning Through Play — Interactive Training Platform
## SPMO · Addis Ababa City Administration · 2024

A complete, self-contained web-based training platform for the Learning Through Play playwork qualification programme. Zero server required — runs from a single HTML file.

---

## Quick start

1. **Open the file** — Double-click `index.html` in any modern web browser (Chrome, Firefox, Edge, Safari)
2. **Deploy online** — Upload `index.html` to any web host (Netlify, GitHub Pages, etc.)
3. **No installation needed** — All styles, scripts, and content are embedded

---

## What's inside

### 6 main sections

| Section | What it does |
|---|---|
| **Dashboard** | Overview with stats, progress, qualification pathway, current modules |
| **Modules** | Interactive content for Play Types, 8 Principles, Safeguarding, Ethiopian Games, Play Environment |
| **Assessment** | Quiz engine with 15 questions across 3 tracks (80% pass mark for safeguarding) |
| **AI Tutor** | Knowledge-base assistant covering all SPMO training material topics |
| **Reflective Journal** | Gibbs' Cycle journal with save and display |
| **Progress** | Module tracking, certificates, practicum log, document library |

### Content built in
- All **16 Hughes' Play Types** with Ethiopian examples
- All **8 Playwork Principles**
- **5-Step Safeguarding Reporting Procedure** (RECEIVE → RECORD → REPORT → REFER → REVIEW)
- **8 Ethiopian cultural games** (Gebeta, Woye, Tiru Atit, Dama, Alchichibo, Gena, Suq play, Coffee Ceremony)
- **6 play environment quality dimensions**
- Loose parts theory (Nicholson, 1971)
- **Intervention spectrum** (9 levels)
- **Play Cycle** (Sturrock & Else, 1998)
- **Risk-Benefit Assessment** equation and guidance
- All **5 qualification levels** (AACPAC → AACP → AASP → AAPCC → AAUPSD)
- Gibbs' Reflective Cycle

---

## Customisation guide

All content is in clearly labelled JavaScript data arrays near the top of the `<script>` section. Customise without touching the layout code.

### Change the logged-in user
Search for `Abebe Bikila` and `AB` (initials) — replace with your user's name/initials.

### Update progress statistics
Find the `.stat-value` elements in the Dashboard section and the `MOD_PROGRESS` / `QUIZ_PROGRESS` arrays.

### Add or edit quiz questions
Find the `const QUIZZES` object. Each quiz track has an array of question objects:
```js
{
  q: "Question text",
  opts: ["Option A", "Option B", "Option C", "Option D"],
  ans: 0,   // Index of correct answer (0 = A, 1 = B, 2 = C, 3 = D)
  exp: "Explanation shown after answering"
}
```

### Add Ethiopian games
Find the `const CULTURE_GAMES` array and add an object:
```js
{
  icon: "fa-NAME",   // Font Awesome 6 icon name
  name: "Game name",
  types: "Play type · Play type",
  age: "5",
  desc: "Description of the game",
  mat: "Materials needed"
}
```

### Add topics to the AI Tutor
Find the `const KB` object and add a new key:
```js
my_topic: `Your knowledge base text here.
Multi-line text supported.`
```
Then add a detection condition in the `getAIResponse` function:
```js
if(m.includes('keyword')) return KB.my_topic;
```

### Connect to Anthropic API (real AI responses)
Replace the `getAIResponse` function with a fetch call to the Anthropic API:
```js
async function getAIResponse(msg) {
  const response = await fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'x-api-key': 'YOUR_API_KEY',
      'anthropic-version': '2023-06-01'
    },
    body: JSON.stringify({
      model: 'claude-sonnet-4-6',
      max_tokens: 1000,
      system: 'You are an AI Tutor for the SPMO Learning Through Play programme in Addis Ababa...',
      messages: [{ role: 'user', content: msg }]
    })
  });
  const data = await response.json();
  return data.content[0].text;
}
```
Note: For production, never expose API keys in client-side code. Use a backend proxy.

### Add video support
Replace the `video-card` placeholder divs with embedded YouTube or Vimeo iframes:
```html
<iframe width="100%" style="aspect-ratio:16/9;border-radius:12px"
  src="https://www.youtube.com/embed/YOUR_VIDEO_ID"
  frameborder="0" allowfullscreen></iframe>
```

---

## Deploying online

### Option 1: Netlify (free, recommended)
1. Go to [netlify.com](https://netlify.com)
2. Drag the `ltp-platform` folder onto the Netlify dashboard
3. Done — you get a public URL instantly

### Option 2: GitHub Pages (free)
1. Create a GitHub repository
2. Upload `index.html`
3. Go to Settings → Pages → Deploy from branch → main
4. Your platform is live at `https://YOUR-USERNAME.github.io/REPO-NAME`

### Option 3: Any web server
Upload `index.html` to any folder on any web server. No server-side language needed — it's pure HTML/CSS/JavaScript.

### Option 4: Local network (intranet)
Run a simple local server:
```bash
python3 -m http.server 8080
# Then open http://localhost:8080 in any browser on the network
```

---

## Browser support
Tested and working in:
- Chrome 90+
- Firefox 88+
- Edge 90+
- Safari 14+
- Mobile Chrome and Safari (responsive design included)

---

## Adding user accounts and data persistence

The current version is a single-user demo. To add multi-user support:

1. **Backend options**: Node.js + Express, Python + FastAPI, or any serverless platform
2. **Database**: Store progress, quiz scores, journal entries in PostgreSQL or MongoDB
3. **Authentication**: Add login with email/password or Google OAuth
4. **API**: Replace the static JS data with `fetch()` calls to your backend

This is a straightforward extension — the platform's modular JavaScript structure makes it easy to replace static data with API calls.

---

## Qualification certificates

Certificate templates are included in the training documents. For digital certificates, consider:
- **Accredify** or **Credly** for verifiable digital badges
- Generating PDF certificates using the included Word/PDF templates
- Linking to the SPMO's own certification registry

---

## Technical notes

- **File size**: ~108KB (single file, no dependencies to download)
- **External dependencies** (loaded from CDN):
  - Google Fonts: Inter + Plus Jakarta Sans
  - Font Awesome 6.5.0 (icons)
- **Works offline** once fonts/icons are cached by the browser
- **Print-friendly**: The journal entries section prints cleanly

---

## Contact & support

Learning Through Play — Playground Expansion Initiative  
Strategic Programmes Management Office (SPMO)  
Addis Ababa City Administration  
Federal Democratic Republic of Ethiopia  
Version 1.0 · 2024
