# Calculator

A small calculator web application built with Node.js and Express. It provides a responsive browser interface for basic arithmetic and a JSON API for the same operations.

## Features

- Add, subtract, multiply, and divide two numbers
- Browser UI served as static files by Express
- JSON API at `POST /calculate`
- Validation for unsupported operations, non-finite numbers, and division by zero
- Docker and Docker Compose support
- Render deployment hook workflow for pushes to `master`

## Requirements

- Node.js 22 or later
- npm
- Docker and Docker Compose, if running with containers

## Run Locally

1. Install dependencies:

   ```bash
   npm install
   ```

2. Start the server:

   ```bash
   npm start
   ```

3. Open [http://localhost:8080](http://localhost:8080) in a browser.

The server uses the `PORT` environment variable when it is provided and defaults to `8080`.

For example:

```bash
PORT=3000 npm start
```

## Docker

Build and run the image directly:

```bash
docker build -t calculator .
docker run --rm -p 8080:8080 calculator
```

Or use Docker Compose:

```bash
docker compose up --build
```

Then open [http://localhost:8080](http://localhost:8080).

To stop Compose:

```bash
docker compose down
```

## API

### `POST /calculate`

Request body:

```json
{
  "operation": "add",
  "firstNumber": 12,
  "secondNumber": 5
}
```

Supported operation values are `add`, `subtract`, `multiply`, and `divide`.

Successful response, HTTP `200`:

```json
{
  "operation": "add",
  "firstNumber": 12,
  "secondNumber": 5,
  "result": 17
}
```

Example with `curl`:

```bash
curl -X POST http://localhost:8080/calculate \
  -H "Content-Type: application/json" \
  -d '{"operation":"divide","firstNumber":20,"secondNumber":4}'
```

Validation errors return HTTP `400` with an `error` field. Examples include:

- `Operation must be add, subtract, multiply, or divide`
- `Both firstNumber and secondNumber must be finite numbers`
- `Cannot divide by zero`

## Project Structure

```text
.
├── controllers/              # HTTP request validation and response handling
├── public/                   # Browser calculator interface
├── routes/                   # Express route definitions
├── services/                 # Arithmetic calculation logic
├── .github/workflows/        # Render deploy-hook workflow
├── Dockerfile                # Node 22 Alpine image
├── docker-compose.yml        # Local container configuration
├── index.js                  # Express application entrypoint
├── package.json              # Scripts and dependencies
└── package-lock.json         # Locked dependency versions
```

## Scripts

- `npm start` starts the Express server with `node index.js`.
- `npm test` is currently a placeholder and exits with an error because automated tests have not been configured yet.

## Deployment

The workflow in `.github/workflows/deploy.yml` runs on pushes to the `master` branch and sends a request to the Render deploy hook stored in the GitHub Actions secret `RENDER_HOST`. Configure that secret in the repository before relying on the workflow.

## Environment

The application reads `PORT` from the process environment and defaults to `8080`. The repository ignores `.env` files so local environment values remain uncommitted.
