# Business Bodhisattva

Marketing site for Business Bodhisattva — *Never Let A Crisis Go To Waste*.

Static site, no build step. Everything served lives in `public/`.

```
public/
  index.html    the site
  404.html      not-found page
  favicon.svg   brand mark
  robots.txt
  sitemap.xml
  _headers      Cloudflare Pages security + cache headers
wrangler.toml   Cloudflare Pages project config
```

## Local preview

Any static server works:

```bash
python3 -m http.server 8000 --directory public
# then open http://localhost:8000
```

## Deploy to Cloudflare Pages

### Option A — Git integration (recommended; auto-deploys on push)

1. Sign in at <https://dash.cloudflare.com> → **Workers & Pages** → **Create** → **Pages** → **Connect to Git**.
2. Authorize GitHub and pick this repository.
3. Build settings:
   - **Framework preset:** `None`
   - **Build command:** *(leave empty)*
   - **Build output directory:** `public`
   - **Root directory:** *(leave empty)*
4. **Save and Deploy.** The site goes live at `https://<project>.pages.dev` in about a minute.
5. Set the **Production branch** under *Settings → Builds & deployments* to whichever branch you want live (`main` once this work is merged). Every push to it redeploys; other branches get preview URLs.

### Option B — Wrangler CLI (one-off deploy)

```bash
npm install -g wrangler
wrangler login
wrangler pages deploy public --project-name=business-bodhisattva
```

## Custom domain

1. In the Pages project → **Custom domains** → **Set up a custom domain**.
2. Enter `businessbodhisattva.com`, then repeat for `www.businessbodhisattva.com`.
3. If the domain's nameservers are already on Cloudflare, the DNS records are created for you. If not, either move the domain to Cloudflare or add the `CNAME` record Cloudflare shows you at your current registrar.
4. TLS is issued automatically. Under **SSL/TLS** set the mode to **Full (strict)**.
5. To pick one canonical host, add a redirect rule (e.g. `www` → apex) under **Rules → Redirect Rules**.

Update the absolute URLs in `public/index.html` (`canonical`, `og:url`), `public/robots.txt`, and `public/sitemap.xml` if the final domain differs from `businessbodhisattva.com`.

## Editing content

The page is a single self-contained HTML file with its CSS in a `<style>` block. Colors are CSS custom properties on `:root` in `public/index.html` — change them there and the whole page follows.

Contact currently points at `mailto:hello@businessbodhisattva.com`. Swap that `href` for a scheduling link (Calendly, Cal.com, etc.) when one exists — there are two: the "Book a free intro call" button jumps to `#contact`, and the "Schedule your intro call" button holds the mailto.
