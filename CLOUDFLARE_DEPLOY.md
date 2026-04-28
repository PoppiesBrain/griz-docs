# Deploy Griz Legal Pages to Cloudflare Pages

## Why Cloudflare Pages?

Cloudflare Pages offers:
- **Free hosting** — no credit card or monthly bill
- **Instant *.pages.dev subdomain** — deploy once, get a live URL immediately
- **Global CDN** — pages served fast from servers worldwide
- **Custom domain later** — point your own domain once you buy one
- **Zero config** — drag-and-drop HTML files, they just work

## Option 1: Drag & Drop (Easiest)

If you prefer a GUI with no command line:

1. Go to [dash.cloudflare.com/login](https://dash.cloudflare.com/login) and log in (or sign up free).
2. Click **Pages** in the left sidebar.
3. Click **Create a project** → **Upload assets**.
4. Select the folder `/Users/dubbi/Desktop/BRAIN\ CLAUDE/AI\ DATING\ COACH/Griz/docs/` on your computer.
5. Give the project a name (e.g., `griz-legal`).
6. Click **Deploy site**.
7. Cloudflare generates a URL like `https://griz-legal.pages.dev/`.

Done. Your pages are live.

## Option 2: Command Line (Faster for Iteration)

If you prefer the CLI and want to update pages later without logging into the dashboard:

### Step 1: Install Wrangler (Cloudflare's CLI)

```bash
npm install -g wrangler
```

If you don't have Node.js, install it first:
```bash
brew install node
```

### Step 2: Log In

```bash
wrangler login
```

This opens your browser to authenticate. Once done, you're logged in locally.

### Step 3: Deploy

```bash
wrangler pages deploy /Users/dubbi/Desktop/BRAIN\ CLAUDE/AI\ DATING\ COACH/Griz/docs --project-name=griz-legal
```

Wrangler scans the `docs/` folder, uploads all HTML files, and deploys them. On first deploy, it creates the project `griz-legal` automatically.

**Output:**
```
✓ Uploaded 5 files (index.html, terms.html, privacy.html, eula.html, support.html)
✓ Project deployed successfully to https://griz-legal.pages.dev/
```

### Step 4: Update in the Future

If you edit a legal page later, redeploy with the same command:

```bash
wrangler pages deploy /Users/dubbi/Desktop/BRAIN\ CLAUDE/AI\ DATING\ COACH/Griz/docs --project-name=griz-legal
```

It detects changes and updates in seconds.

## Step 5: Update iOS App URLs

Once deployed, update the URLs in the Griz iOS app.

Open `Griz/Griz/Views/Settings/SettingsView.swift` and update the footer links:

**Find:**
```swift
// Privacy & Terms links
Link(destination: URL(string: "https://privacy-policy.html")!) {
    Text("Privacy Policy")
}
```

**Replace with:**
```swift
Link(destination: URL(string: "https://griz-legal.pages.dev/privacy.html")!) {
    Text("Privacy Policy")
}
```

Update these URLs:
- `terms.html` → `https://griz-legal.pages.dev/terms.html`
- `privacy.html` → `https://griz-legal.pages.dev/privacy.html`
- `eula.html` → `https://griz-legal.pages.dev/eula.html`
- `support.html` → `https://griz-legal.pages.dev/support.html`

Also update the footer link in `index.html` (if it links to privacy-policy):
```html
<!-- Change from -->
<a href="privacy-policy.html">Privacy Policy</a>

<!-- To -->
<a href="https://griz-legal.pages.dev/privacy.html">Privacy Policy</a>
```

## Step 6: Add a Custom Domain Later (Optional)

Once your legal pages are live, you can point a custom domain (e.g., `legal.griz.app` or `privacy.griz.app`) to Cloudflare Pages:

1. Go to [dash.cloudflare.com](https://dash.cloudflare.com) → **Pages** → your project.
2. Click **Settings** → **Custom domains**.
3. Enter your domain (e.g., `legal.griz.app`).
4. Follow the prompt to add a DNS CNAME record pointing to the Pages project.
5. Done — your pages are now at `https://legal.griz.app/terms.html`.

(You'll need to own the domain and have it pointed to Cloudflare's nameservers.)

## Troubleshooting

**"Module not found" error?**
Make sure all CSS and HTML are in the same folder and file names match exactly. HTML is case-sensitive on Linux; if you wrote `TERMS.html` but the file is `terms.html`, links will break.

**Pages are blank?**
Check the browser console (F12) for errors. Most common: a CSS or image link is broken. Make sure all links are relative (e.g., `href="privacy.html"`, not `href="/privacy.html"`).

**Want to unpublish?**
Go to the Pages dashboard, find your project, and click **Delete**. The pages go offline immediately.

---

**Questions?** Email brain@poppiesstudios.com.
