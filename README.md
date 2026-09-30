# Rx Label Lookup

A single-page app that searches the live [openFDA](https://open.fda.gov/) API for any drug and shows its
approved indications, brand names, generic names and Drugs@FDA applications. It's one static file
(`index.html`) with no build step and no server. openFDA allows cross-origin requests, so the browser calls it directly.

Share a search with `?q=`, e.g. `https://your-site/?q=metformin` (the **Copy share link** button does this).

## Run locally

    python3 -m http.server 8080   # then open http://localhost:8080

## Publish (pick one)

| Host | Steps |
|---|---|
| **GitHub Pages** | `gh repo create rx-label-lookup --public --source . --push`, then repo Settings → Pages → Source: **GitHub Actions**. The workflow in `.github/workflows/pages.yml` deploys on every push to `main`. |
| **Netlify** | Drag this folder onto <https://app.netlify.com/drop>, or run `npx netlify-cli deploy --prod`. Uses `netlify.toml`. |
| **Vercel** | `npx vercel --prod`. Uses `vercel.json`. |

## Config

At the top of the `<script>` in `index.html`:

- `OPENFDA_API_KEY`: optional. Without a key, openFDA allows 1,000 requests/day per IP (one search makes about 5).
  A free key ([get one here](https://open.fda.gov/apis/authentication/)) raises this to 120,000/day. Note that the key is visible in page source.
- `DEFAULT_QUERY`: the example search shown on first load.
- `LABEL_PAGE`: labels fetched per "Load more".

openFDA data is unvalidated and is not medical advice.

## Clinical review

`review.html` lists every condition mapping (everyday name, medical term, ICD-10-CM code) from `conditions.js`
for a pharmacist or physician to approve or correct. Reviewers copy their feedback and send it back.
Both pages read `conditions.js`, so edits there update the app and the review page together.
