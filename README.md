# what-ive-been-up-to

A simple static personal site served by Nginx. Documents what Noah Nemec has been working on between roles.

- Hand-written HTML + CSS, no JS framework, no trackers
- Single-stage `nginx:alpine` Docker image
- Image is built and published to GHCR by GitHub Actions on every push to `main`
- One-click install on Unraid via the included template

---

## Install on Unraid

There are two ways. Pick one.

### Option A — Add as a Template Repository (recommended)

This makes the template show up in Unraid's "Add Container" list, and Unraid will sync any future template changes automatically.

1. In Unraid, go to **Docker** tab.
2. Scroll to the bottom and find the **Template Repositories** box.
3. Paste this URL into an empty row:
   ```
   https://github.com/swimmingnoah/what-ive-been-up-to
   ```
4. Click **Save**. Unraid clones the repo and discovers any `*.xml` files in `templates/`.
5. Click **Add Container** at the top.
6. Open the **Select a template** dropdown — choose **User templates → WhatIveBeenUpTo**.
7. Adjust the **WebUI Port** if `8080` is already taken, then **Apply**.

### Option B — Paste the template XML URL directly

1. In Unraid, go to **Docker → Add Container**.
2. In the **Template** field at the top of the form, paste:
   ```
   https://raw.githubusercontent.com/swimmingnoah/what-ive-been-up-to/main/templates/WhatIveBeenUpTo.xml
   ```
3. Click off the field — Unraid fetches the XML and fills the form.
4. Adjust the WebUI Port if needed, then **Apply**.

### After install

- Unraid pulls `ghcr.io/swimmingnoah/what-ive-been-up-to:latest` and starts the container.
- Add a proxy host in NPM pointing at `<unraid-ip>:<webui-port>` to give it a real URL + TLS.
- To update later: **Force Update** on the container in Unraid. The Actions workflow rebuilds the image on every push to `main`.

> **First time only:** GHCR packages are private by default. After the first successful Actions run, go to your GitHub profile → **Packages → what-ive-been-up-to → Package settings → Change visibility → Public**, otherwise Unraid can't pull without credentials.

## Run locally with Docker Compose

```sh
docker compose up -d --build
```

Site is then live on `http://localhost:8080`. Stop with `docker compose down`.

## Edit content

All copy lives in `site/index.html`. Photos live in `site/assets/`.

To edit without rebuilding, uncomment the `volumes:` block in `docker-compose.yml`, then `docker compose up -d` once. File edits in `site/` show up on refresh.

## Photos

Drop photos into `site/assets/` with these filenames:

| Section | Filenames |
| --- | --- |
| Working with AI | `claude.png` |
| The solar project | `IMG_9864.jpg` |
| BBQ and smoker | `Smoker-tracker.png`, `IMG_5113.JPG` |
| DIY around the house | `IMG_5973.JPG`, `IMG_5974.JPG` |
| Electric dirt bike | `bike.png` |
| Dog Dad | `IMG_5789.JPG` |

Any of `.jpg`, `.png`, `.webp` works — if you use a different extension, update the matching `<img src>` in `site/index.html`. Recommended size: ~1400px on the long edge.

## Repo layout

```
.
├── Dockerfile
├── docker-compose.yml                    # local dev / non-Unraid hosts
├── templates/
│   └── WhatIveBeenUpTo.xml               # Unraid Community Apps template
├── .github/workflows/build.yml           # builds + pushes image to ghcr.io
├── nginx/default.conf
└── site/
    ├── index.html
    ├── styles.css
    └── assets/
```
