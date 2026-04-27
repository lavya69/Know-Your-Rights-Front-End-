# Know Your Rights — Frontend

AI-powered legal awareness platform for Indian citizens.

## Folder Structure
```
know-your-rights/
├── index.html          ← ENTIRE frontend (single file, zero build step)
└── README.md
```

## Setup — 3 Steps

### Step 1: Fix Backend (Required)
Your `blj/src/index.js` is missing two lines. Add them:

```js
const express = require("express");
const cors = require("cors");          // ← ADD THIS
const app = express();

app.use(express.json());               // ← ADD THIS (parses POST body)
app.use(cors());                       // ← ADD THIS (allows frontend to call API)

const routes = require("./routes/chatRoutes");
app.use('/api', routes);

app.listen(process.env.PORT || 3000, () => {
  console.log("server started on port 3000");
});
```

Install cors:
```bash
cd blj
npm install cors
```

### Step 2: Set Backend URL in Frontend
Open `index.html`, find line ~280:
```js
const API_URL = 'http://localhost:3000/api/chat';
```
Change the URL if your backend runs on a different port.

### Step 3: Run

**Backend:**
```bash
cd blj
GEMINI_API_KEY=your_key_here node src/index.js
```

**Frontend:**
```bash
# Option A — Just open in browser (simplest)
open index.html

# Option B — Serve with a local server
npx serve .
# or
python3 -m http.server 5500
```

Visit: http://localhost:5500

## Features

| Feature | Description |
|---|---|
| Landing Page | Hero section + situation cards |
| Chat Interface | ChatGPT-style chat with structured response cards |
| Response Cards | Color-coded sections: Situation, Rights, Law, Actions, Disclaimer |
| Help Links | Smart helplines based on keywords (police, women, cyber, etc.) |
| Action Panel | File Complaint, Contact Authority, Learn More links |
| Emergency Mode | Red banner + priority helplines toggle |
| Explain Simply | Re-queries AI for plain language explanation |
| Copy Response | Clipboard copy with toast notification |
| Clear Chat | Confirmation modal before clearing |
| Mobile Responsive | Works on all screen sizes |

## API Contract

**POST** `/api/chat`

Request:
```json
{ "message": "Police detained me without reason" }
```

Response:
```json
{ "reply": "Situation:\n...\nRights:\n...\nLaw:\n...\nActions:\n...\nDisclaimer:\n..." }
```

## Smart Keyword Mapping

| Keyword in message | Helplines shown |
|---|---|
| police, arrest, fir, detain | Police 100, NHRC |
| women, domestic, harassment | Women 1091, NCW |
| cyber, online, hack, fraud | Cyber Portal, 1930 |
| work, employer, salary | Labour Grievance Portal |
