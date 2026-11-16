# Ashgrove Medical Group staff apps

The front door to the practice's apps, served at
<https://ashgrovemedicalgroup.app>. One static page with a link to each app.
It holds no data and nobody signs in to it; each app keeps its own sign-in.

| App | Address | Runs on |
|---|---|---|
| Shift Central | `shiftcentral.ashgrovemedicalgroup.app` (not open yet) | the VPS |
| GP Rota | `rota.ashgrovemedicalgroup.app` | the VPS |
| Practice HR | `hr.ashgrovemedicalgroup.app` | the VPS |
| Care navigation simulator | `carenav.ashgrovemedicalgroup.app` | Vercel |

## What is here

    public/            everything that is served
      index.html       the page: one block per app
      styles.css       all of the styling, colours at the top
      fonts/           Atkinson Hyperlegible Next, self-hosted (OFL)
      img/             the practice logo, as an SVG
    vercel.json        the response headers (see below)

There is no build step and no JavaScript.

## Changing a card

Each app is one `<li>` in `public/index.html`: a name, a line saying what it is
for, the address shown in small type, and the `href` the card links to. The
colour comes from the class: `sign-navy`, `sign-teal`, `sign-green` or
`sign-blue`.

### Opening Shift Central

A card with no `href` is drawn as a dashed outline with a "Coming soon" tag.
When Shift Central is live, in its block:

1. add `href="https://shiftcentral.ashgrovemedicalgroup.app/"` to the `<a>`;
2. replace the `<span class="sign-tag">Coming soon</span>` line with the
   `<svg class="sign-arrow" …>` line from any other card.

## Preview

    python3 -m http.server 4173 --directory public

then open <http://localhost:4173>.

## Deploy

The page is the Vercel project `practice-apps`. Vercel serves `public/` as it
is. The DNS for `ashgrovemedicalgroup.app` is at Cloudflare: the bare domain
and `www` point at Vercel, **DNS only** (grey cloud), the same way `carenav`
does.

`vercel.json` sets the headers on every response:

- a Content-Security-Policy that allows this site's own stylesheet, images
  and fonts and nothing else, so no script and no inline style will run;
- `X-Robots-Tag: noindex, nofollow`, with the matching `<meta>` in the page,
  so search engines do not list it. There is deliberately no `robots.txt`
  that blocks crawling: a crawler that may not fetch the page never sees the
  noindex.
