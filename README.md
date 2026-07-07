# Operation: Restore — Website (v1)

Static site for **operationrestore.net**. No build step, no dependencies — it's plain HTML/CSS, ready to deploy as-is.

## What's in here

```
.
├── index.html              ← the whole site (single page)
├── site.webmanifest         ← app icon config
├── robots.txt
├── sitemap.xml
├── _headers                 ← Cloudflare Pages: caching + security headers
├── _redirects                ← Cloudflare Pages: shortlink routes (empty for now)
├── assets/
│   ├── seal-web.jpg          ← circular badge/seal logo
│   └── banner-web.jpg        ← wide banner art (used in "partner" section + social preview)
├── icons/                    ← favicons + apple touch icon, generated from the seal
└── downloads/                ← printable PDFs linked from "Get Involved"
    ├── Needs_Assessment.pdf
    ├── Volunteer_SignUp.pdf
    ├── Volunteer_Waiver.pdf
    ├── Photo_Testimony_Release.pdf
    └── Ministry_Partnership.pdf
```

## Deploy to Cloudflare Pages

**Option A — dashboard, no git (fastest)**
1. Cloudflare dashboard → Workers & Pages → Create → Pages → **Upload assets**
2. Drag this whole folder in, deploy
3. Pages → your project → Custom domains → add `operationrestore.net`

**Option B — connect a git repo (recommended long-term, gives you deploy history + easy edits)**
```bash
cd operationrestore-net
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin <your-empty-github-repo-url>
git push -u origin main
```
Then in Cloudflare: Workers & Pages → Create → Pages → **Connect to Git** → pick the repo.
- Build command: *(leave blank)*
- Build output directory: `/`

Either way, once it's live, add the custom domain under the project's **Custom domains** tab and Cloudflare handles the DNS if the domain's already on your account (it is, since you bought it through Cloudflare).

## Editing content later

Everything lives in `index.html` — sections are commented by ID (`#what-we-do`, `#saturday`, `#impact`, `#involved`, `#testimonies`, `#about`, `#contact`). Colors and fonts are defined once at the top in `:root` CSS variables, so a palette or type change only needs editing in one place.

### Things to update as they change
- **`#impact` numbers** — currently hand-entered (5 volunteers / 3 requests / 1 scheduled / 0 completed) because the tracker spreadsheet's dashboard has a formula bug (`-997`). Fix that first, then keep these in sync manually, or wait for v2 to pull live data.
- **`#testimonies`** — swap the empty-state block for real testimony cards once the first Restore Saturday happens. Each card just needs a quote, first name (per the Photo & Testimony Release), and optionally a photo.
- **Phone/social links** — in `#contact` and the footer.

## v2 — live forms

`_redirects` is stubbed out for shortlinks (e.g. `operationrestore.net/volunteer`) once real form submission exists. To make the "Request Help" / "Volunteer" buttons submit directly instead of linking to a PDF, the simplest path on Cloudflare is:
1. A Cloudflare Pages Function (`/functions/api/submit.js`) that writes to a KV store, D1 database, or forwards to email
2. Swap the PDF links in `#involved` for real `<form>` elements posting to that function

Not built yet — flagged here so it's a clean next step rather than a rebuild.

## Known gaps (carried over from planning)

- No 501(c)(3) yet — don't add "tax-deductible" language near any future donate button until that's filed
- No online giving link yet (Give Send Go / PayPal / Cash App) — add a `#give` section when one's chosen
- Tracker dashboard formula bug (`-997`) should be fixed at the source spreadsheet
