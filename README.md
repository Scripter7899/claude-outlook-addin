# Claude AI — Outlook Add-in v2
## GitHub Pages Hosting Guide

---

## Files in this package

```
claude-outlook-addin/
├── taskpane.html      ← The full add-in UI
├── manifest.xml       ← Tells Outlook about the add-in
├── commands.html      ← Required Outlook placeholder
├── assets/
│   ├── icon-16.png    ← Add your own icons here
│   ├── icon-32.png
│   └── icon-80.png
└── README.md
```

---

## Step 1 — Create a GitHub repository

1. Go to **github.com** and sign in (create a free account if needed)
2. Click the **"+"** button (top right) → **"New repository"**
3. Name it exactly: `claude-outlook-addin`
4. Set it to **Public** (required for GitHub Pages free tier)
5. Click **"Create repository"**

---

## Step 2 — Upload your files

**Option A — Via browser (easiest):**
1. On your new repo page, click **"uploading an existing file"**
2. Drag and drop ALL files from this folder (taskpane.html, manifest.xml, commands.html)
3. Also create an `assets` folder and upload your 3 icon PNG files
4. Click **"Commit changes"**

**Option B — Via Git (if you have Git installed):**
```bash
git clone https://github.com/YOUR-USERNAME/claude-outlook-addin.git
# Copy all files into this folder
git add .
git commit -m "Add Claude Outlook add-in files"
git push
```

---

## Step 3 — Enable GitHub Pages

1. In your repo, click **Settings** (top tab)
2. Scroll down to **"Pages"** in the left sidebar
3. Under **"Branch"**, select `main` and folder `/root`
4. Click **Save**
5. Wait ~2 minutes — GitHub will show you your URL:
   `https://YOUR-USERNAME.github.io/claude-outlook-addin/`

---

## Step 4 — Update the manifest with your URL

Open `manifest.xml` and replace **every occurrence** of:
```
YOUR-GITHUB-USERNAME
```
with your actual GitHub username. There are **6 places** to update.

Example — if your username is `rajesh-sharma`:
```
https://rajesh-sharma.github.io/claude-outlook-addin/taskpane.html
```

After editing, re-upload the manifest.xml to your GitHub repo.

---

## Step 5 — Install the add-in in Outlook

### Outlook on the web (easiest):
1. Go to **outlook.office.com** or **outlook.live.com**
2. Open any email
3. Click **"..."** (More actions) → **"Get Add-ins"**
4. Click **"My add-ins"** in the left sidebar
5. Scroll to **"Custom Connectors"** → **"+ Add a custom add-in"** → **"Add from file"**
6. Upload your `manifest.xml`
7. Click **Install**

### Outlook Desktop (Windows):
1. Open Outlook
2. Click **File** → **Manage Add-ins** (opens browser)
3. Follow the same steps as above

### Deploy for your whole organization (IT Admin):
1. Go to **admin.microsoft.com**
2. **Settings** → **Integrated Apps** → **Upload custom apps**
3. Upload manifest.xml
4. Assign to specific users or everyone

---

## Step 6 — First use

1. Open any email in Outlook
2. Click **"Ask Claude"** in the email toolbar
3. The side panel opens — choose your connection mode:
   - **Direct**: enter your Anthropic API key
   - **On-Prem**: enter your gateway URL + key
4. Start asking questions!

---

## Features

| Feature | Description |
|---|---|
| Mode selector | Switch between Direct (Anthropic) and On-Prem gateway |
| Quick actions | 6 one-click actions: summarize, reply, action items, tone check, sender profile, translate |
| Templates | 8 ready-made prompts (rejection, follow-up, legal flags, exec summary, etc.) |
| Chat history | Last 20 conversations saved locally, viewable in History tab |
| Copy button | Copy any Claude response to clipboard |
| Settings panel | Toggle behaviour, clear history, view connection info |
| Email context bar | See which email is loaded, expand for sender/date details |

---

## Connecting to an On-Premise Server

Your gateway (LiteLLM or similar) must:
1. Accept POST requests to `/v1/messages`
2. Accept an `x-api-key` header with your internal key
3. Forward requests to Anthropic (or your chosen model)
4. Return the same response format as the Anthropic Messages API

The add-in will send requests to:
```
YOUR-GATEWAY-URL/v1/messages
```

Your gateway can enrich the prompt before forwarding — adding CRM data,
internal knowledge base content, or any other context.

**LiteLLM quick setup:**
```bash
pip install litellm[proxy]
litellm --model anthropic/claude-sonnet-4-6 --port 4000
```
Then use `http://your-server:4000` as the gateway URL.

---

## Troubleshooting

| Problem | Solution |
|---|---|
| Add-in button doesn't appear | Restart Outlook after installing manifest |
| "API key invalid" | Check key starts with `sk-ant-` with no extra spaces |
| On-prem connection fails | Make sure gateway URL is HTTPS if on a real domain |
| Email not loading | Add-in needs ReadItem permission (already set in manifest) |
| GitHub Pages not working | Make sure repo is Public, not Private |

---

## Getting an Anthropic API Key

1. Go to **https://console.anthropic.com**
2. Sign in or create account
3. Click **API Keys** → **Create Key**
4. Copy key (starts with `sk-ant-api03-...`)
