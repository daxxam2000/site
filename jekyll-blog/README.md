# Personal site — Jekyll + GitHub Pages + iA Writer

A brutalist personal site with categories (philosophy, fiction, essays, updates), an about page, a photography section with a page per project, and a TTRPG page listing your games with buttons to microsites and storefronts.

This README covers four things:

1. What's in here
2. Local preview (optional but useful)
3. Deploying to GitHub Pages
4. **Publishing from iA Writer — desktop and iPhone**

---

## 1. What's in here

```
.
├── _config.yml            # Site settings, nav, collections, plugins
├── Gemfile                # Ruby gems for local preview
├── index.html             # Homepage (latest posts + sections)
├── about.md               # About page (with profile image)
├── photography.html       # Photo project index
├── ttrpg.html             # TTRPG game list
├── category/
│   ├── philosophy.html    # Filtered post archives
│   ├── fiction.html
│   ├── essays.html
│   └── updates.html
├── _layouts/              # default, page, post, photo-project, ttrpg
├── _includes/             # header, footer, post-list partial
├── _posts/                # Blog posts. ONE post per file. iA Writer writes here.
├── _photo_projects/       # One file per photo project
├── _ttrpg/                # One file per game
└── assets/
    ├── css/main.css       # All styles (no build step)
    └── images/            # Profile picture, project covers, post images
```

### How posts work

Each post lives in `_posts/` and is named `YYYY-MM-DD-slug.md`. The front matter looks like:

```yaml
---
layout: post
title: "Post title"
date: 2026-04-12 09:30:00 +0100
category: philosophy   # philosophy | fiction | essays | updates
excerpt: "Optional one-line preview shown on listing pages."
---
```

The `category:` field is what puts it on the right archive page in the nav.

### How photo projects work

Each project is a file in `_photo_projects/`, e.g. `concrete-light.md`:

```yaml
---
title: "Concrete Light"
year: 2025
location: "London, UK"
subtitle: "On the way the late sun finds post-war architecture."
cover: /assets/images/your-cover.jpg
images:
  - { src: /assets/images/01.jpg, alt: "Frame 01", caption: "01 — Hayward Gallery, 17:42" }
  - { src: /assets/images/02.jpg, alt: "Frame 02" }
---

Project notes / statement go here as Markdown.
```

The project gets its own page automatically at `/photography/concrete-light/`.

### How TTRPG entries work

Each game is a file in `_ttrpg/`, e.g. `dust-and-iron.md`:

```yaml
---
title: "Dust & Iron"
order: 1                  # controls list order, lower = first
year: 2025
status: "Released"        # "Released" or "In progress" — styles the badge
players: "2–5"
tagline: "A rules-light western about debts that cannot be paid."
blurb: "Short paragraph shown on the list page."
cover: /assets/images/dust-and-iron.jpg
links:
  - { label: "Microsite",    url: "https://example.com/dust-and-iron", featured: true }
  - { label: "DriveThruRPG", url: "https://www.drivethrurpg.com/..." }
  - { label: "itch.io",       url: "https://itch.io/..." }
---

Long-form description goes here.
```

`featured: true` makes the button red (use it for the primary CTA — usually the microsite).

### Footnotes

The site uses [littlefoot.js](https://littlefoot.js.org/) to turn standard kramdown footnotes into popovers. You write footnotes the normal Markdown way — iA Writer supports the same syntax — and they automatically become click-to-expand boxes inline.

Syntax:

```markdown
The metaphor is economic, and like all economic metaphors it makes
certain things visible by hiding others.[^stock]

What if attention is not a stock but a posture?[^crary]

[^stock]: William James already saw this in 1890.
[^crary]: Cf. Jonathan Crary, *Suspensions of Perception* (1999).
```

The reference label (`stock`, `crary`) can be anything — letters, numbers, hyphens. They don't appear on the page; littlefoot renders them as sequential numbers. Markdown auto-numbers `[^1]`, `[^2]` etc. work too.

**No-JS fallback:** if a reader has JavaScript disabled, the footnotes simply render at the bottom of the post as a numbered list — the standard kramdown behaviour. You don't lose anything.

**Where it's configured:**
- `_layouts/default.html` — loads littlefoot from unpkg and initialises it
- `assets/css/main.css` (bottom of file) — brutalist override styles for the button and popover

To pin a specific version (rather than the latest v4), change both URLs in `_layouts/default.html` from `littlefoot@4` to e.g. `littlefoot@4.0.1`.

---

## 2. Local preview (optional)

You don't need this to publish, but it's nice to see changes before pushing.

```bash
# one-time: install Ruby + Bundler if you don't have them
# (on macOS, `brew install ruby` works fine)

cd path/to/this/folder
bundle install
bundle exec jekyll serve
# open http://127.0.0.1:4000
```

---

## 3. Deploy to GitHub Pages

### One-time setup

1. **Create a GitHub repo.** For a site at `https://<you>.github.io`, name the repo exactly `<you>.github.io`. For a project site at `https://<you>.github.io/blog`, name it whatever you like (e.g. `blog`).
2. **Push this folder up.**
   ```bash
   cd path/to/this/folder
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin git@github.com:<you>/<repo>.git
   git push -u origin main
   ```
3. **Enable Pages.** On GitHub: *Settings → Pages → Build and deployment → Source: Deploy from a branch → main / (root) → Save*.
4. **If it's a project site (not `<you>.github.io`)**, edit `_config.yml`:
   ```yaml
   url: "https://<you>.github.io"
   baseurl: "/<repo>"
   ```
   Commit and push.

GitHub will build the site within a minute or two. You'll see a green check on your latest commit when it's live.

### Custom domain (optional)

Add a file named `CNAME` at the repo root containing only your domain (e.g. `yourname.com`), then point the domain's DNS at GitHub Pages per [their docs](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site).

---

## 4. Publishing from iA Writer

iA Writer doesn't talk to GitHub directly — but it doesn't need to. The site is just a folder of Markdown files, and you can keep iA Writer pointed at that folder on both Mac and iPhone.

### Mac: simplest workflow

1. **Clone the repo somewhere convenient**, e.g. `~/Sites/blog`.
2. **In iA Writer, add the folder as a Library location.**
   `Preferences → Library → +` and pick `~/Sites/blog/_posts`. (You can add `_photo_projects` and `_ttrpg` too.)
3. **Write a new post.** In iA Writer, *File → New*, save into `_posts` with the filename pattern `YYYY-MM-DD-your-slug.md`. Paste in the front-matter template (snippet below). Write.
4. **Publish** — open Terminal:
   ```bash
   cd ~/Sites/blog
   git add .
   git commit -m "New post: your-slug"
   git push
   ```
   GitHub Pages rebuilds automatically. Allow ~1 minute.

If even that feels like too much, install [GitHub Desktop](https://desktop.github.com/) — point it at the folder and you can commit/push with two clicks. Or set up an alias:

```bash
# add to ~/.zshrc
publish() {
  ( cd ~/Sites/blog && git add . && git commit -m "${1:-update}" && git push )
}
# then: publish "new philosophy post"
```

### iPhone / iPad: iA Writer + Working Copy

This is the standard combo and it's solid.

1. **Install [Working Copy](https://workingcopy.app/)** from the App Store. Free for read-only; Pro (one-time purchase) for push. You need Pro.
2. **Clone your repo into Working Copy.** Open Working Copy → *+* → Clone repository → paste the SSH or HTTPS URL of your repo → authenticate (SSH key or GitHub OAuth).
3. **Link the repo into the Files app.** Working Copy → tap the repo → *⋯* → *Set up Folder*. This makes the repo show up under Files → Working Copy → your-repo. iA Writer can read and write directly to Files.
4. **In iA Writer, add the repo's `_posts` folder as a Library location:**
   Settings → Library → *+* → Files → Working Copy → your-repo → `_posts`.
5. **Write a post in iA Writer.** Save with the filename pattern `YYYY-MM-DD-your-slug.md` and paste the front-matter snippet below.
6. **Commit and push in Working Copy.** Open Working Copy, tap your repo — you'll see the new file under *Working Tree*. Tap *Commit*, write a message, tap *Push*. Done. GitHub Pages rebuilds.

There's also a Working Copy *share extension* — once your post is saved in iA Writer, you can long-press the file in Files, tap *Share → Working Copy → Commit and Push*, and it does the whole thing in one pop-up.

### Front-matter snippet to keep handy

Put this in an iA Writer template (or a TextExpander snippet, or just keep it in your notes):

```markdown
---
layout: post
title: ""
date: 2026-05-06 09:00:00 +0100
category: essays
excerpt: ""
---

```

Update the date/category each time. The categories that show up in the nav are: `philosophy`, `fiction`, `essays`, `updates`.

### Drafts

Want to write something without publishing it? Two options:

- Save it in a folder called `_drafts/` (Jekyll won't publish files there).
- Or set `published: false` in the front matter.

Either way, when you're ready to ship, move the file (or remove the flag) and push.

---

## Customisation cheat sheet

| Want to change… | Edit |
|---|---|
| Site title / tagline / your name / email | `_config.yml` |
| Nav links | `_config.yml` → `nav:` |
| Categories | `_config.yml` → `categories_list:` (and add a matching file under `category/`) |
| Colours / fonts / spacing | `assets/css/main.css` (the `:root` block at the top) |
| Profile photo | Replace `assets/images/profile.svg` with `profile.jpg` (or `.png`) and update the path in `about.md` |
| Footer links | `_includes/footer.html` |
| Footnote popover styles | `assets/css/main.css` (FOOTNOTES section, near bottom) |
| littlefoot version / options | `_layouts/default.html` (script tags + init block) |

---

## Troubleshooting

- **Site builds but pages 404.** Double-check `url:` and `baseurl:` in `_config.yml` if it's a project site.
- **Local preview throws Ruby errors.** Run `bundle update github-pages` to refresh gem versions.
- **Working Copy can't push.** You probably need a deploy key or SSH key set up — Working Copy has a built-in walkthrough under Settings → SSH Keys.
- **iA Writer doesn't see the repo on iPhone.** Make sure Working Copy → repo → *⋯* → *Set up Folder* is enabled. Without that step the repo isn't visible to other apps.

That's the whole system. Happy writing.
