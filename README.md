# michaelmaben.com

Personal site for [Michael Maben](https://michaelmaben.com) — detection engineer, Splunk builder, federal SOC. Built with Jekyll, deployed via GitHub Pages.

---

## Local Development

**Prerequisites:** Ruby 3.x, Bundler

```bash
# Install dependencies (first time only)
bundle install

# Serve locally at http://localhost:4000
bundle exec jekyll serve --livereload
```

GitHub Pages builds the site automatically on every push to `main` — no CI/CD setup required.

---

## Adding a New Blog Post

1. Create a new file in `_posts/` named with this format:

   ```
   _posts/YYYY-MM-DD-your-post-slug.md
   ```

2. Add frontmatter at the top:

   ```yaml
   ---
   layout: post
   title: "Your Post Title"
   date: 2026-05-01
   description: "One or two sentences shown in the blog index and link previews."
   tags:
     - Splunk
     - MITRE ATT&CK
   ---
   ```

3. Write the post body in Markdown below the frontmatter.

4. Push to `main`. GitHub Pages builds and deploys within ~1–2 minutes.

### Available Tags

Use consistent tags so filtering works correctly:

- `Splunk` — SPL queries and Splunk-specific content
- `MITRE ATT&CK` — ATT&CK mapping and coverage analysis
- `Threat Hunting` — hunting methodology and write-ups
- `Incident Response` — IR playbooks and automation
- `PowerShell` — PowerShell tooling and automation
- `Detection Engineering` — meta-level detection engineering topics
- `Career` — job search, certs, breaking in from SOC
- `VetSec` — veterans in cybersecurity

### Code Blocks

Wrap code in fenced blocks with a language hint. The post layout loads highlight.js automatically.

```
```spl
index=windows EventCode=4624 ...
```

```powershell
Get-Process | Where-Object { ... }
```

```yaml
# Sigma rule
title: ...
```
```

Supported language hints: `spl`, `splunk`, `powershell`, `yaml`, `bash`, `python`, `json`, `xml`

### Read Time

Read time is calculated automatically from word count. To override it, add `read_time: 8` to your frontmatter.

---

## Site Structure

```
/
├── _config.yml          # Jekyll config
├── _layouts/
│   ├── default.html     # Base layout (nav + footer)
│   ├── page.html        # Static pages
│   └── post.html        # Blog post layout
├── _posts/              # Blog posts (Markdown)
├── assets/
│   └── css/style.css    # All styles
├── index.html           # Home page
├── blog/index.html      # Blog index with tag filter
├── about/index.html     # About page
├── work/index.html      # Detection work / portfolio
├── CNAME                # michaelmaben.com — do not modify
└── Gemfile
```

---

## Deployment

Push to `main`. GitHub Pages handles the rest.

**Settings → Pages:** Source should be set to "Deploy from a branch" → `main` → `/ (root)`.

The `CNAME` file already contains `michaelmaben.com` — leave it alone.

---

## Analytics

No analytics are loaded by default. To add privacy-respecting analytics:

1. Sign up for [Plausible](https://plausible.io) or [Fathom](https://usefathom.com)
2. Uncomment and update the analytics snippet in `_layouts/default.html` (near the `</head>` tag)
