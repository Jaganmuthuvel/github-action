# GitHub Actions CI/CD Demo

A simple Express.js API used to demonstrate a GitHub Actions CI/CD pipeline that deploys to a self-hosted runner.

## Features

- Express server with health check endpoint
- Unit tests using Node.js built-in test runner
- Docker support
- Automated deploy on push to `master`

## Prerequisites

- Node.js 20+
- npm
- (Optional) Docker
- (For deploy) PM2 and a self-hosted GitHub Actions runner

## Getting Started

```bash
npm install
npm start
```

Server runs on [http://localhost:3000](http://localhost:3000) by default.

## API Endpoints

| Method | Path      | Description              |
|--------|-----------|--------------------------|
| GET    | `/`       | Welcome message          |
| GET    | `/health` | Health check             |

### Examples

```bash
curl http://localhost:3000/
# {"status":"ok","message":"Hello Github Action"}

curl http://localhost:3000/health
# {"status":"ok","message":"Server is healthy"}
```

## Scripts

| Command       | Description                |
|---------------|----------------------------|
| `npm start`   | Start the Express server   |
| `npm test`    | Run tests                  |

## Docker

```bash
docker build -t github-action-ci-cd .
docker run -p 3000:3000 github-action-ci-cd
```

## CI/CD Pipeline

On every push to `master`, the workflow in `.github/workflows/main.yml`:

1. Checks out the repository on a self-hosted runner
2. Syncs the app to `/home/ubuntu/github-action-ci-cd/`
3. Installs dependencies (`npm i`)
4. Runs tests (`npm run test`)
5. Restarts the app with PM2 (`pm2 restart backend-prod`)

## Project Structure

```
.
├── .github/workflows/main.yml  # CI/CD pipeline
├── Dockerfile
├── index.js                    # Express app
├── index.test.js               # Tests
├── package.json
└── README.md
```

## License

ISC
