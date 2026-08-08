# UniPOS Website

A static, single-vendor website for UniPOS — no build step, no server, no
database. Every page is a self-contained HTML file with its CSS and JS
inline, plus embedded (base64) logo/favicon images, so nothing external is
required to run it.

## Files in this project

```
index.html                    → Home page (the whole site experience)
privacy-policy.html           → Privacy Policy (separate page)
terms-and-conditions.html     → Terms & Conditions (separate page)
refund-policy.html            → Refund & Cancellation Policy (separate page)
og-image.png                  → Social share image (Open Graph / Twitter card)
CNAME                         → Custom domain config for GitHub Pages
README.md                     → This file
```

All internal links use **relative paths** (e.g. `href="privacy-policy.html"`),
so the site works identically whether it's opened directly from disk, hosted
at a GitHub Pages default URL, or served from `unipos.store`.

## 1. Create the GitHub repository

1. Go to [github.com/new](https://github.com/new) and create a new repository
   (public, since GitHub Pages on the free tier requires a public repo unless
   you're on GitHub Enterprise/Pro).
2. Name it anything you like, e.g. `unipos-site`.
3. Do **not** initialize it with a README (you already have one here) — or if
   you do, just overwrite it with this one afterward.

## 2. Upload the files

Upload every file in this folder to the **root** of the repository:

```
index.html
privacy-policy.html
terms-and-conditions.html
refund-policy.html
og-image.png
CNAME
README.md
```

You can do this either by dragging the files into the GitHub web UI
("Add file → Upload files") or via git:

```bash
git init
git add .
git commit -m "Initial UniPOS site"
git branch -M main
git remote add origin https://github.com/<your-username>/<your-repo>.git
git push -u origin main
```

## 3. Enable GitHub Pages

1. In your repository, go to **Settings → Pages**.
2. Under **Build and deployment → Source**, choose **Deploy from a branch**.
3. Set the branch to `main` and the folder to `/ (root)`.
4. Click **Save**.

GitHub will publish the site at `https://<your-username>.github.io/<your-repo>/`
within a minute or two — that's normal and expected before the custom domain
is configured.

## 4. Configure the custom domain (unipos.store)

The `CNAME` file in this repo already contains `unipos.store`, which tells
GitHub Pages which custom domain to serve. You also need to point your DNS
at GitHub:

**At your domain registrar / DNS provider**, add these records for
`unipos.store` (an apex/root domain):

| Type | Host | Value |
|------|------|-------|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |

If you also want `www.unipos.store` to work, add:

| Type | Host | Value |
|------|------|-------|
| CNAME | www | `<your-username>.github.io` |

(These are GitHub's standard Pages IP addresses as of this writing — GitHub's
official docs at
[docs.github.com/pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)
are the source of truth if they ever change.)

Back in **Settings → Pages** on GitHub, enter `unipos.store` in the **Custom
domain** field and save — GitHub will verify the DNS records automatically
(this can take anywhere from a few minutes to a few hours to propagate).

## 5. HTTPS

Once GitHub verifies your custom domain, a checkbox for **Enforce HTTPS**
will appear under Settings → Pages. Turn it on. GitHub automatically
provisions and renews a free TLS certificate for `unipos.store` via Let's
Encrypt — no extra configuration needed on your end.

## Notes

- This is a fully static site — no PHP, Node, database, or backend of any
  kind. It is compatible with GitHub Pages, Netlify, Vercel, Cloudflare
  Pages, or literally any static file host.
- Assets (logo, favicon) are embedded directly in the HTML as base64 data
  URIs rather than separate files in an `/assets/` folder — this keeps each
  page fully self-contained and avoids any broken-path issues across hosts.
- The "Request Custom Software" form on the homepage opens the visitor's
  email client via a `mailto:` link addressed to `support@unipos.store`,
  pre-filled with a message and the address they entered — there is no
  backend involved, so no server setup is required for it to work.
- Do not create or reference a `basic-demo.unipos.store` CNAME — the only
  domain this project is configured for is `unipos.store`.
