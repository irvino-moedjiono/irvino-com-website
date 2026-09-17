# irvino.com

Static personal site (HTML/CSS). Time of day is chosen in the browser from the visitor’s local clock.

Live: [https://irvino.com](https://irvino.com)

Hosting: **GitHub Pages**. DNS: **Namecheap**.

## Preview (before DNS)

After Pages is on, the site is also at:

[https://irvino-moedjiono.github.io/irvino-com-website/](https://irvino-moedjiono.github.io/irvino-com-website/)

## Deploy

Push to `main`. GitHub Pages publishes the repo root (`index.html` + `assets/`). No build step.

The `CNAME` file in this repo tells Pages the custom domain is `irvino.com`.

## Custom domain: GitHub + Namecheap

Do these in order. Keep the TXT record after verification.

### 1. Verify `irvino.com` with GitHub

1. GitHub → your **profile picture** → **Settings**.
2. Sidebar: **Pages** (under Code, planning, and automation).
3. **Add a domain** → enter `irvino.com` → **Add domain**.
4. Copy the **TXT** host and value GitHub shows. They look like:
   - Host: `_github-pages-challenge-irvino-moedjiono`
   - Value: a long token (copy from the page, do not guess)

### 2. Add that TXT in Namecheap

1. [Namecheap Domain List](https://ap.www.namecheap.com/domains/list/) → **Manage** next to `irvino.com`.
2. **Advanced DNS** → **Add New Record** → **TXT Record**.
3. **Host:** only `_github-pages-challenge-irvino-moedjiono`  
   Namecheap already appends `irvino.com`. Do **not** put the full `_github-pages-challenge-….irvino.com` in Host.
4. **Value:** the token from GitHub.
5. **TTL:** Automatic. Save.

Wait a few minutes (sometimes longer). Check:

```bash
dig _github-pages-challenge-irvino-moedjiono.irvino.com TXT +short
```

You should see GitHub’s token. Then in GitHub Pages domain settings click **Verify**. Leave the TXT record in place.

### 3. Enable Pages on this repo

1. Open [repo Settings → Pages](https://github.com/irvino-moedjiono/irvino-com-website/settings/pages).
2. **Build and deployment** → Source: **Deploy from a branch**.
3. Branch: **`main`**, folder: **`/ (root)`** → **Save**.
4. Confirm the preview URL loads.

### 4. Point Namecheap at GitHub

**Advanced DNS.** Remove old **A** / **CNAME** records that still send `irvino.com` or `www` to a previous host (parking page, old CDN, etc.).

Add **A** records, Host `@`, values:

- `185.199.108.153`
- `185.199.109.153`
- `185.199.110.153`
- `185.199.111.153`

Add **AAAA** records, Host `@`, values:

- `2606:50c0:8000::153`
- `2606:50c0:8001::153`
- `2606:50c0:8002::153`
- `2606:50c0:8003::153`

Add **CNAME**, Host `www`, Value `irvino-moedjiono.github.io`.

In the **repo** Pages settings, Custom domain: `irvino.com` (save). GitHub can redirect `www` to the apex.

Check:

```bash
dig irvino.com A +short
dig irvino.com AAAA +short
dig www.irvino.com CNAME +short
```

### 5. HTTPS

When DNS looks right, wait for GitHub’s certificate (often under an hour). In repo Pages settings, enable **Enforce HTTPS**.

Then open [https://irvino.com](https://irvino.com) and [https://www.irvino.com](https://www.irvino.com).

## Local preview

```bash
python3 -m http.server 8765
```

Then [http://127.0.0.1:8765/](http://127.0.0.1:8765/) and `?tod=morning|day|sunset|night`.
