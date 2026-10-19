# example-site

An example container for testing deployments. It's just an nginx container (`nginx:1.27-alpine`) serving one static HTML page.

## Files

| File | Purpose |
|------|---------|
| `index.html` | The static page that gets served |
| `Dockerfile` | Copies `index.html` into the nginx image |
| `compose.dev.yaml` | Builds the image locally and runs it |
| `compose.prod.yaml` | Runs the prebuilt `example-site:latest` image, restarts unless stopped |

## Run locally

```sh
docker compose -f compose.dev.yaml up --build
```

Open http://localhost:8080.

## Deploy

Build and tag the image:

```sh
docker build -t example-site:latest .
```

Then on the target host:

```sh
docker compose -f compose.prod.yaml up -d
```

`compose.prod.yaml` sets `pull_policy: always`, so `example-site:latest` has to be in a registry the host can pull from. Push it there first, and change the `image:` name to the full registry path (for example `ghcr.io/<user>/example-site:latest`).

Both compose files publish the site on port **8080**.
