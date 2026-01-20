# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a personal portfolio and professional blog for Ryan Hagan (Executive Technology Leader) built with Hugo static site generator. The site uses the PaperMod theme and is deployed to https://ryanhagan.net/.

**Repository:** https://github.com/hokiecsgrad/resume-website.git


## 1. Stack & Infrastructure
* **Static Site Generator:** Hugo (Extended version required for some CSS features).
* **Theme:** [PaperMod](https://github.com/adityatelange/hugo-PaperMod) (cloned, git tracking removed).
* **Hosting:** Netlify (via GitHub integration).
* **Source Control:** GitHub (Private/Public Repo).
* **Authentication:** SSH (`git@github.com:...`) is required for pushing code.
* **Analytics:** Google Analytics 4 (Client-side) + Netlify Analytics (Server-side).

## 2. Key Commands

### Development
* **Start Local Server:**
    ```bash
    hugo server
    ```
    * *Add `-D` to see Drafts.*
    * *Add `-F` to see Future-dated posts.*
    * *Server runs at `http://localhost:1313`*

### Deployment (GitOps)
* **Push to Deploy:**
    ```bash
    git add .
    git commit -m "Update content"
    git push origin main
    ```
    * *Note: Run git commands from the **Host Machine** (Mac Terminal), not inside the Docker container, to utilize local SSH keys.*

### Build Settings (Netlify)
* **Build Command:** `hugo --gc --minify`
* **Publish Directory:** `public`
* **Environment Var:** `HUGO_VERSION` = `0.121.2` (or current local version).

## 3. Architecture

### Content Organization

**Three main content sections:**
1. **Resume** (`/content/resume.md`) - Single-page executive CV
2. **Leadership** (`/content/leadership/`) - Leadership philosophy articles
3. **Engineering** (`/content/engineering/`) - Technical articles

Each section directory has an `_index.md` that serves as the section landing page.

## 4. Important File Locations

### Configuration
* **`hugo.yaml`**: Main control file.
    * Controls: `baseURL`, Menus, Profile Mode, Analytics, Share Buttons.
    * **Critical Setting:** `markup.goldmark.renderer.unsafe: true` (Allows raw HTML links/attributes in Markdown).

### Content Structure
* **`content/`**: All site pages.
    * **`content/leadership/`**: Leadership articles.
    * **`content/engineering/`**: Engineering articles.
* **`_index.md`**: REQUIRED in every section folder (e.g., `content/leadership/_index.md`) to define the list page title and sort order.

### Static Assets (Raw Files)
* **`static/`**: Files referenced by URL (Images, PDFs, Favicons).
    * `headshot.jpg`: Profile picture (Square crop recommended).
    * `favicon.png`, `site.webmanifest`, `android-chrome-*.png`: Browser icons.

### Theme Overrides (Do NOT edit themes/PaperMod directy)
* **`assets/`**: Source files processed by Hugo pipes.
    * `bluesky.svg`: Custom SVG for social sharing (processed to remove hardcoded fill colors).
* **`layouts/partials/`**:
    * `extend_head.html`: Adds `<link rel="manifest">` and other head tags.
    * `share_icons.html`: Custom logic to inject Bluesky button into the standard share list.
* **`assets/css/extended/`**:
    * `custom.css`: Custom CSS rules (e.g., rounding profile image).

## 5. Workflows & Gotchas

### Creating New Sections
1.  Create folder: `content/new-section/`
2.  Create definition: `content/new-section/_index.md` (Set `draft: false`, `title: "..."`).
3.  Add to Menu: Update `hugo.yaml` under `menu.main`.

### Content Management
- **Create new article:** `hugo new content/leadership/article-name.md` or `hugo new content/engineering/article-name.md`
- Articles use YAML frontmatter with these key fields:
  - `title`: Article title
  - `weight`: Lower numbers appear first in listings
  - `date`: Publication date
  - `draft`: Set to `false` to publish
  - `summary`: Description used in listings and SEO

### Internal Links and References

Articles use Hugo's `ref` shortcode for internal links:
```markdown
[link text]({{< ref "article-name.md" >}})
[link to engineering section]({{< ref "/engineering/agile-processes.md" >}})
```

### External Links

Use raw HTML with target="_blank" for external links (unsafe HTML is enabled):
```markdown
<a href="https://example.com" target="_blank">Link Text</a>
```

### Ordering Content
* **Articles:** Use `weight: 10`, `weight: 20` in Front Matter to force order (Lower numbers = Top).
* **Sections:** Use `weight` in `_index.md` to order them in lists.

### Analytics Behavior
* **Google Analytics:** Configured in `hugo.yaml`.
    * *Behavior:* Automatically suppresses itself if "Do Not Track" is enabled in browser or if an Ad Blocker is present.
    * *Verification:* Use Browser DevTools -> Network Tab -> Filter "collect".

### Git Troubleshooting
* If `git push` fails from Docker: Run it from the Mac terminal instead.
* If `Permission denied (publickey)`: Ensure remote URL is SSH:
    `git remote set-url origin git@github.com:hokiecsgrad/resume-website.git`
