# Daylance website

Static marketing site, privacy policy, and terms of use for [daylance.farnood.tech](https://daylance.farnood.tech).

Source of truth for site content lives in this private repo under `site/`. GitHub Pages cannot host directly from a private repo on the free plan, so CI publishes `site/` to the public [`FarnoodF/daylance-site`](https://github.com/FarnoodF/daylance-site) repository.

## Pages

- `/` — product landing page
- `/policy/` — privacy policy (also linked from the mobile app)
- `/terms/` — terms of use
- `/contact/` — contact form and email (`me@farnood.tech`); form submissions are emailed via [FormSubmit](https://formsubmit.co)

The first real form submission triggers a one-time FormSubmit activation email to `me@farnood.tech`. Confirm that email so later messages arrive in your inbox.

## Local preview

From the repository root:

```sh
npx --yes serve site
```

Then open the URL printed in the terminal (typically `http://localhost:3000`).

## Deployment

Pushes to `main` that touch `site/` run [`.github/workflows/deploy-site.yml`](../.github/workflows/deploy-site.yml), which copies the `site/` folder to `main` on the public Pages repo.

### One-time setup (outside this repo)

#### 1. Create the public Pages repo

1. Create a new **public** GitHub repository named `daylance-site`.
2. Leave it empty or with a placeholder README.

#### 2. Add a write deploy key to this private repo

1. Generate an SSH key pair used only for this deploy (`ssh-keygen -t ed25519 -f daylance-site-deploy -N ""`).
2. In `FarnoodF/daylance-site` → **Settings → Deploy keys**, add the public key with **Allow write access**.
3. In `FarnoodF/dailance` → **Settings → Secrets and variables → Actions**, add:
   - Name: `SITE_DEPLOY_SSH_KEY`
   - Value: the private key, including the `BEGIN/END OPENSSH PRIVATE KEY` lines

#### 3. Enable GitHub Pages on the public repo

In `FarnoodF/daylance-site` → **Settings → Pages**:

1. **Source**: Deploy from branch
2. **Branch**: `main` / `/ (root)`
3. After the first deploy, set **Custom domain** to `daylance.farnood.tech`
4. Enable **Enforce HTTPS** once the certificate is issued

The `site/CNAME` file is copied on each deploy.

#### 4. DNS

Replace any URL-forward for `daylance.farnood.tech` with:

| Type  | Host       | Answer               |
| ----- | ---------- | -------------------- |
| CNAME | `daylance` | `farnoodf.github.io` |

Remove any URL-forwarding rule for the same host so GitHub Pages can serve `/`, `/policy/`, `/terms/`, and `/contact/`.

DNS can take a few minutes to propagate. GitHub may need up to 24 hours to issue HTTPS for the custom domain.

#### 5. Ship content

Merge/push site changes to `main` in `dailance`, or run **Actions → Deploy site → Run workflow** manually.

### Store listings

Use these public URLs in App Store Connect:

- Privacy: `https://daylance.farnood.tech/policy`
- Terms: `https://daylance.farnood.tech/terms`

Set the same values in the app as `EXPO_PUBLIC_PRIVACY_URL` and `EXPO_PUBLIC_TERMS_URL`.
