# CI/CD Lab

This project is a lightweight Express + TypeScript application built to demonstrate a simple CI/CD workflow. The app exposes a home page and a health-check endpoint, and it is designed to be validated automatically with tests, type checking, and a production build before deployment.

## Overview

The CI/CD lab focuses on the core automation steps commonly used in modern software delivery:

- Install dependencies
- Run static validation
- Execute automated tests
- Build the production bundle
- Deploy the app after successful verification

The goal is to keep the example small and easy to understand while illustrating real-world release automation.

## Tech Stack

- Node.js 24
- TypeScript
- Express
- Node.js test runner

## Project Structure

```text
.
├── src/
│   ├── app.ts         # Express app and routes
│   └── server.ts      # Starts the HTTP server
├── test/
│   └── app.test.ts    # Health endpoint test
├── dist/              # Generated production build
├── package.json       # Scripts and dependency configuration
├── tsconfig.json      # TypeScript compiler settings
├── .gitignore         # Ignore build and dependency files
└── README.md
```

## Getting Started

Install dependencies:

```bash
npm install
```

Start the app in development mode:

```bash
npm run dev
```

Then open:

- http://localhost:3000/
- http://localhost:3000/api/health

The health endpoint returns:

```json
{ "status": "ok" }
```

## Available Scripts

```bash
npm run dev
```
Runs the server with automatic restarts during development.

```bash
npm run typecheck
```
Checks the TypeScript code without generating a build output.

```bash
npm test
```
Runs the automated health test against the app.

```bash
npm run build
```
Compiles the TypeScript project into the `dist` directory.

```bash
npm start
```
Starts the compiled application from `dist`.

```bash
npm run verify
```
Performs the full validation pipeline locally:

1. Type-check the project
2. Run tests
3. Build the app

This is the same logic typically used in CI before deployment.

## Local CI/CD Flow

A simple pipeline for this project looks like this:

1. Code is pushed to the repository.
2. CI installs dependencies.
3. TypeScript is checked for errors.
4. Automated tests run.
5. The application is compiled.
6. If all checks pass, the app is deployed.

This keeps the release process fast, repeatable, and less error-prone.

## Configuration

The server listens on port `3000` by default, unless you set the `PORT` environment variable. The home page also reads `APP_VERSION`, which defaults to `development` when it is not provided.

Example:

```bash
PORT=4000 APP_VERSION=v1.2.0 npm run dev
```

## Purpose

This repository is intentionally minimal so it can be used as a practical training project for learning CI/CD fundamentals, including:

- automated validation
- build verification
- deploy-ready project structure
- repeatable delivery pipelines

