# Getting Started

## Prerequisites

- Node.js 20+
- npm 9+
- Docker (for local container testing)
- GCP access (for deployment)

## Local Development

```bash
# Clone the repo
git clone https://github.com/${{ values.repoUrl }}.git
cd ${{ values.name }}

# Install dependencies
npm install

# Start development server
npm run dev
```

The app will start on `http://localhost:8080`.

## Running Tests

```bash
# Run all tests
npm test

# Run with coverage
npm run test:coverage

# Run lint
npm run lint
```

## Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `PORT` | Server port (default: 8080) | No |
| `NODE_ENV` | Environment (`development`/`production`) | No |

## Docker (Local)

```bash
# Build image
docker build -t ${{ values.name }} .

# Run container
docker run -p 8080:8080 ${{ values.name }}
```
