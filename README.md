# CI/CD Lab

A small Express and TypeScript application for demonstrating a simple CI/CD pipeline. GitHub Actions installs the dependencies and runs type checking, tests, and a production build. The app is deployed on Render.

## Features

- Home page that displays the running app and its version
- Health check at `/api/health`
- Version endpoint at `/api/version`
- GitHub Actions verification on pushes and pull requests targeting `main`
- Production build and start scripts suitable for Render

## Tech stack

- Node.js 24
- pnpm 12.9.1
- Express 5
- TypeScript
- Node.js test runner

## Project structure

```text
.
├── .github/workflows/ci.yml  # CI verification workflow
├── src/
│   ├── app.ts                # Express app and routes
│   └── server.ts             # HTTP server entry point
├── test/
│   └── app.test.ts           # Endpoint tests
├── package.json              # Scripts, dependencies, and tool versions
├── pnpm-lock.yaml            # Locked dependency versions
└── tsconfig.json             # TypeScript compiler configuration
```

## Run locally

Install the pinned pnpm version (12.9.1) and use Node.js 24. Then install the locked dependencies:

```bash
pnpm install --frozen-lockfile
```

Start the development server:

```bash
pnpm run dev
```

By default, the app listens on port `3000`:

- Home page: `http://localhost:3000/`
- Health check: `http://localhost:3000/api/health`
- Version: `http://localhost:3000/api/version`

The health endpoint returns:

```json
{"status":"ok"}
```

The version endpoint returns the value of `APP_VERSION`, or `"development"` if it is unset:

```json
{"version":"development"}
```

## Scripts

| Command | Description |
| --- | --- |
| `pnpm run dev` | Run the server in watch mode for development. |
| `pnpm run typecheck` | Check TypeScript without emitting files. |
| `pnpm test` | Run the endpoint tests. |
| `pnpm run build` | Compile the app into `dist/`. |
| `pnpm start` | Run the compiled app from `dist/`. |
| `pnpm run verify` | Run type checking, tests, and the production build. |

Before opening a pull request or deploying, run the complete local verification:

```bash
pnpm install --frozen-lockfile
pnpm run verify
```

## CI workflow

The workflow in `.github/workflows/ci.yml` runs for pushes and pull requests targeting `main`. It sets up Node.js 24 and pnpm 12.9.1, installs dependencies with the frozen lockfile, and runs `pnpm run verify`. A failed type check, test, or build fails the workflow.

The GitHub Actions workflow verifies the code; it does not itself deploy the app. Deployment is handled by the Render service configuration.

## Deploy on Render

Connect this GitHub repository to a Render **Web Service**. Configure the service with:

- **Runtime:** Node
- **Build command:** `pnpm install --frozen-lockfile && pnpm run build`
- **Start command:** `pnpm start`

The `packageManager` field in `package.json` pins pnpm to `12.9.1`, and the `engines` field specifies Node.js `24.x`. Render provides the `PORT` environment variable; the server listens on that port and binds to `0.0.0.0`, as required for the hosted service.

You can optionally set `APP_VERSION` in the Render service's environment variables. Its value appears on the home page and in the `/api/version` response. Without it, the app reports `"development"`.

After deployment, open the public URL provided by Render and check `/`, `/api/health`, and `/api/version`.

## Configuration

- `PORT` — HTTP port; defaults to `3000` locally. Set automatically by Render.
- `APP_VERSION` — version displayed by the app; defaults to `development`.

For example, on macOS or Linux:

```bash
PORT=4000 APP_VERSION=v1.2.0 pnpm run dev
```
