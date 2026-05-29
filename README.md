# Just Say No to `curl | bash`

A small, dependency-free static site that security advocates can point to when
making the case that installing software via `curl … | bash` is a bad idea.

It's written for two audiences:

1. **Software distributors** — to persuade them to stop *asking* users to pipe a
   server straight into a shell.
2. **Software users** — to persuade them to stop *doing* it.

Tone: a vaguely humorous public-service announcement, with a light "Just Say No"
/ D.A.R.E. callback (**D**on't **A**uto-**R**un **E**choed-from-the-internet
scripts).

## The argument, in brief

1. **Untraceable & unauditable.** The script executes straight off the wire and
   is never saved. After the fact you have no way to know what actually ran.
2. **The server can tell you're piping into a shell** (a documented timing
   side-channel) and serve different content when you are.
3. **Every request is a fresh roll.** A malicious endpoint can serve the real
   installer to everyone and slip in a payload only a small fraction of the
   time, only to live shells — low enough to dodge reports and inspection.

It also rebuts the common "but package managers run arbitrary code too"
objection: the distinction isn't *whether* code runs, it's the accountability of
the channel around it (inspection, immutable versions, checksums/signing, audit
trails, central revocation) — none of which a raw HTTP endpoint provides.

## Pages

| File | What it covers |
| --- | --- |
| `index.html` | The landing page: hook, the three reasons, user & distributor guides, the package-manager comparison, FAQ, and the pledge. |
| `how-it-works.html` | Technical deep dive: the timing side-channel, the "1% attack," defenses, and sources. |
| `assets/css/style.css` | All styling. No frameworks. |
| `assets/js/main.js` | Copy-to-clipboard buttons (progressive enhancement; site works without JS). |

## Running locally

It's plain static HTML — open `index.html` in a browser, or serve the folder:

```sh
python3 -m http.server 8000   # then visit http://localhost:8000
```

## Deploying

Designed for **GitHub Pages** (or any static host). A `.nojekyll` file is
included so the `assets/` directory is served as-is. To publish via Pages:
Settings → Pages → deploy from the `main` branch, root (`/`).

## A note on accuracy

This is an advocacy/reference site, so it tries hard to stay honest:

- The server-side detection technique is a real, documented proof of concept.
  The canonical write-up ("Detecting the use of `curl | bash` server side",
  idontplaydarts.com, 2016) is linked from `how-it-works.html#sources` via the
  Wayback Machine, as the original is offline.
- Exact pipe/socket buffer sizes and timing thresholds are platform-dependent;
  the site says so rather than over-claiming.
- Claims about per-registry log retention and revocation policies are flagged in
  the copy as **worth verifying against each registry's current documentation**
  before being cited in a specific argument. (This corresponds to the
  fact-check note in the original brief.)

Contributions that add primary-source citations are especially welcome.

## License

MIT — see [`LICENSE`](./LICENSE).
