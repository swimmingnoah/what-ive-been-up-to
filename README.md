# what-ive-been-up-to

A simple static personal site served by Nginx. Documents what Noah Nemec has been working on between roles.

- Hand-written HTML + CSS, no JS framework, no trackers
- Single-stage `nginx:alpine` Docker image
- Image is built and published to GHCR by GitHub Actions on every push to `main`
- One-click install on Unraid via the included template

---

## Install on Unraid (recommended)

1. In Unraid, go to **Docker → Add Container**.
2. In the **Template** dropdown at the top, paste this URL and click the floppy disk to save:
   ```
   https://raw.githubusercontent.com/swimmingnoah/what-ive-been-up-to/main/unraid-template.xml
   ```
3. Adjust the **WebUI Port** if `8080` is already taken.
4. Click **Apply**. Unraid pulls `ghcr.io/swimmingnoah/what-ive-been-up-to:latest` and starts the container.
5. Add a proxy host in NPM pointing at `<unraid-ip>:<webui-port>` to give it a real URL + TLS.

To update later: hit **Force Update** on the container in Unraid. The Actions workflow rebuilds the image on every push to `main`, so the latest version is always one click away.

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
├── docker-compose.yml          # local dev / non-Unraid hosts
├── unraid-template.xml         # Unraid Community Apps template
├── .github/workflows/build.yml # builds + pushes image to ghcr.io
├── nginx/default.conf
└── site/
    ├── index.html
    ├── styles.css
    └── assets/
```
