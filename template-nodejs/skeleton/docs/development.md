# Development Guide

## Prerequisites
- Node.js 18+
- PostgreSQL 14+
- npm or yarn

## Setup
```bash
git clone https://github.com/udhayakiran-stack/${{ values.name }}
cd your-nodejs-service
npm install
cp .env.example .env
npm run dev
```

## Environment Variables
| Variable | Description | Required |
|----------|-------------|----------|
| PORT | Server port | Yes |
| DATABASE_URL | PostgreSQL connection | Yes |
| JWT_SECRET | JWT signing key | Yes |

## Running Tests
```bash
npm test
npm run test:coverage
```

## Deployment
```bash
npm run build
npm start
```