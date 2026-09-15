# flutter-docker-image

This repo automatically builds and publishes a Docker image to **GitHub Container Registry (ghcr.io)** every time code is pushed to the `main` branch.

## How it works

A GitHub Actions workflow, defined in [`.github/workflows/docker-publish.yml`](.github/workflows/docker-publish.yml), runs on every push to `main`:

1. **Checkout** — the workflow checks out the repository code.
2. **Log in to ghcr.io** — it authenticates to GitHub Container Registry using the automatically generated `GITHUB_TOKEN`. No manual login or personal access token is required.
3. **Build and push** — it builds the Docker image from the `Dockerfile` at the repo root and pushes it to:

   ```
   ghcr.io/phiiter/flutter-docker-image:latest
   ```

No manual steps are needed — as soon as you push to `main`, a fresh image is built and published automatically.

## Pulling the image

Once published, anyone with access can pull the image with:

```bash
docker pull ghcr.io/phiiter/flutter-docker-image:latest
```

If the package is private, you'll need to log in first:

```bash
docker login ghcr.io -u YOUR_GITHUB_USERNAME
```

## Checking build status

- **Actions tab** — view workflow runs, logs, and build status at `Actions` in this repository.
- **Packages tab** — view published image versions and tags under this repo's sidebar, or on the [GitHub profile/org Packages page](https://github.com/phiiter?tab=packages).

## Manual build and push (optional)

If you ever need to build and push manually instead of relying on the workflow:

```bash
docker login ghcr.io -u YOUR_GITHUB_USERNAME
docker build -t ghcr.io/phiiter/flutter-docker-image:latest .
docker push ghcr.io/phiiter/flutter-docker-image:latest
```

## Notes

- The workflow uses the repository's built-in `GITHUB_TOKEN`, which is automatically created for each run and scoped with `packages: write` permission (configured in the workflow's `permissions` block).
- If pushes fail with a `permission_denied: write_package` error, check **Settings → Actions → General → Workflow permissions** and ensure "Read and write permissions" is enabled (at both the repo and, if applicable, organization level).