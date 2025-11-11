# 🪦 Echoes of the Dead Web - Deployment Guide

## Quick Start (Local Development)

### Prerequisites
- Node.js 18+ and npm
- Python 3.11+ (for AWS CDK infrastructure)
- Chrome browser (for extension)
- OpenAI or Anthropic API key
- Pinecone account (free tier works)

### Step 1: Install Dependencies

```bash
# Root dependencies
npm install

# Frontend dependencies
cd frontend
npm install
cd ..
```

### Step 2: Configure Environment

```bash
# Copy environment template
cp .env.example .env

# Edit .env and add your API keys:
# - OPENAI_API_KEY or ANTHROPIC_API_KEY
# - PINECONE_API_KEY
# - PINECONE_ENVIRONMENT
# - PINECONE_INDEX_NAME
```

### Step 3: Start the Resurrection Engine

```bash
# Terminal 1: Start backend server
npm run backend

# Terminal 2: Start frontend (in new terminal)
cd frontend
npm run dev
```

The séance chamber will be available at: http://localhost:5173

### Step 4: Load Chrome Extension (Optional)

1. Open Chrome and navigate to `chrome://extensions/`
2. Enable "Developer mode" (toggle in top right)
3. Click "Load unpacked"
4. Select the `chrome_extension/` folder from this project
5. The extension icon will appear in your toolbar

## AWS Amplify Deployment (Recommended)

### Prerequisites
- AWS Account with appropriate permissions
- GitHub repository (or GitLab/Bitbucket)
- AWS CLI configured: `aws configure`

### Architecture Overview
```
Frontend (React/Vite) → AWS Amplify Hosting
Backend (Express API) → AWS App Runner or Lambda
Infrastructure → AWS CDK (S3, DynamoDB)
```

### Step 1: Prepare Your Repository

```bash
# Ensure your code is committed and pushed to GitHub
git add .
git commit -m "Prepare for Amplify deployment"
git push origin main
```

### Step 2: Deploy Backend to AWS App Runner

Create `apprunner.yaml` in project root:

```yaml
version: 1.0
runtime: nodejs18
build:
  commands:
    build:
      - npm install
run:
  command: node backend-server.js
  network:
    port: 3001
    env: PORT
  env:
    - name: PORT
      value: "3001"
```

Deploy via AWS Console:
1. Go to AWS App Runner console
2. Click "Create service"
3. Source: "Source code repository" → Connect GitHub
4. Select your repository and branch
5. Build settings: Use `apprunner.yaml`
6. Service name: `echoes-backend`
7. Add environment variables (see Step 4)
8. Click "Create & deploy"
9. Copy the service URL (e.g., `https://xxx.us-east-1.awsapprunner.com`)

### Step 3: Deploy Frontend to AWS Amplify

#### Option A: Via AWS Console (Easiest)

1. Go to AWS Amplify console
2. Click "New app" → "Host web app"
3. Connect your GitHub repository
4. Select repository and branch (main)
5. Configure build settings:

```yaml
version: 1
frontend:
  phases:
    preBuild:
      commands:
        - cd frontend
        - npm ci
    build:
      commands:
        - npm run build
  artifacts:
    baseDirectory: frontend/dist
    files:
      - '**/*'
  cache:
    paths:
      - frontend/node_modules/**/*
```

6. Add environment variable:
   - Key: `VITE_API_URL`
   - Value: Your App Runner URL from Step 2
7. Click "Save and deploy"

#### Option B: Via Amplify CLI

```bash
# Install Amplify CLI
npm install -g @aws-amplify/cli

# Initialize Amplify in your project
amplify init

# Add hosting
amplify add hosting

# Select: "Hosting with Amplify Console"
# Select: "Continuous deployment"

# Publish
amplify publish
```

### Step 4: Configure Environment Variables

#### Backend (App Runner)
Add these in App Runner console → Configuration → Environment variables:

```bash
OPENAI_API_KEY=your_key_here
# OR
ANTHROPIC_API_KEY=your_key_here

PINECONE_API_KEY=your_key_here
PINECONE_ENVIRONMENT=us-east-1-aws
PINECONE_INDEX_NAME=dead-web-memory

AWS_REGION=us-east-1
FRONTEND_URL=https://your-amplify-domain.amplifyapp.com

# From CDK deployment (Step 5)
S3_BUCKET_NAME=your-bucket-name
DYNAMODB_TABLE_NAME=your-table-name
```

#### Frontend (Amplify)
Add in Amplify console → App settings → Environment variables:

```bash
VITE_API_URL=https://your-apprunner-url.us-east-1.awsapprunner.com
```

### Step 5: Deploy Infrastructure (Optional - for S3/DynamoDB)

```bash
cd infrastructure

# Create Python virtual environment
python -m venv venv

# Activate virtual environment
# Windows:
venv\Scripts\activate
# Mac/Linux:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Bootstrap CDK (first time only)
cdk bootstrap

# Deploy infrastructure
cdk deploy

# Note the outputs: bucket name and table name
# Add these to App Runner environment variables
```

### Step 6: Update CORS Settings

Update `backend-server.js` to allow your Amplify domain:

```javascript
const cors = require('cors');

app.use(cors({
    origin: [
        'http://localhost:5173',
        'http://localhost:3000',
        'https://your-app.amplifyapp.com'  // Add your Amplify domain
    ],
    credentials: true
}));
```

Commit and push - App Runner will auto-deploy.

### Step 7: Test Your Deployment

```bash
# Test backend
curl https://your-apprunner-url.us-east-1.awsapprunner.com/api/resurrections

# Visit frontend
# Open https://your-app.amplifyapp.com in browser
```

## Alternative: Serverless Backend with Lambda

If you prefer Lambda over App Runner:

### Create Lambda Function

1. Package backend:
```bash
# Create deployment package
npm install --production
zip -r backend.zip . -x "*.git*" "frontend/*" "infrastructure/*"
```

2. Create Lambda via AWS Console:
   - Runtime: Node.js 18.x
   - Upload `backend.zip`
   - Handler: `backend-server.handler`
   - Timeout: 30 seconds
   - Memory: 512 MB

3. Add API Gateway HTTP API trigger

4. Update environment variables in Lambda console

5. Modify `backend-server.js` for Lambda:

```javascript
// Add at the end of backend-server.js
exports.handler = async (event, context) => {
    // Lambda handler wrapper
    const serverless = require('serverless-http');
    return serverless(app)(event, context);
};
```

## AWS Amplify Gen 2 (Modern Approach)

For a fully integrated experience:

```bash
# Install Amplify Gen 2
npm create amplify@latest

# Follow prompts to set up:
# - Authentication (optional)
# - API (REST or GraphQL)
# - Storage (S3)
# - Database (DynamoDB)

# Deploy everything
npx ampx sandbox
```

This creates a complete backend with type-safe APIs.

## Monitoring Your Deployment

### Amplify Frontend
- View build logs: Amplify Console → Your App → Build history
- Monitor traffic: CloudWatch → Amplify metrics
- Custom domain: Amplify Console → Domain management

### App Runner Backend
- View logs: App Runner Console → Logs tab
- Monitor health: App Runner Console → Metrics
- Auto-scaling: Automatically handled

### CDK Infrastructure
- CloudFormation: View stack status and resources
- CloudWatch: Monitor Lambda, DynamoDB, S3 metrics

## Environment Variables Reference

### Required
- `OPENAI_API_KEY` or `ANTHROPIC_API_KEY` - For ghost conversations
- `PINECONE_API_KEY` - For vector memory storage
- `PINECONE_ENVIRONMENT` - Your Pinecone environment (e.g., us-east-1-aws)
- `PINECONE_INDEX_NAME` - Name of your Pinecone index

### Optional
- `PORT` - Backend server port (default: 3001)
- `AWS_REGION` - AWS region for services (default: us-east-1)
- `AWS_PROFILE` - AWS CLI profile name
- `FRONTEND_URL` - Frontend URL for CORS (default: http://localhost:5173)

## Pinecone Setup

1. Create free account at https://www.pinecone.io/
2. Create new index:
   - Name: `dead-web-memory`
   - Dimensions: `1536` (for OpenAI embeddings)
   - Metric: `cosine`
3. Copy API key and environment from dashboard
4. Add to `.env` file

## Troubleshooting

### Backend won't start
```bash
# Check if port 3001 is already in use
# Windows:
netstat -ano | findstr :3001
# Mac/Linux:
lsof -i :3001

# Kill the process or change PORT in .env
```

### Frontend can't connect to backend
```bash
# Verify backend is running on http://localhost:3001
# Check browser console for CORS errors
# Ensure CORS is enabled in backend-server.js
```

### Wayback Machine API errors
```bash
# The Wayback Machine API is public and free
# Rate limits: ~15 requests per second
# If you hit rate limits, add delay between requests
# No API key needed
```

### Chrome Extension not working
```bash
# Ensure extension is loaded in chrome://extensions/
# Check that "Developer mode" is enabled
# Reload extension after code changes
# Check browser console for errors
```

### AWS CDK deployment fails
```bash
# Ensure AWS credentials are configured
aws configure --profile your-aws-profile

# Verify CDK is bootstrapped
cdk bootstrap --profile your-aws-profile

# Check CloudFormation console for detailed errors
```

## Architecture Overview

```
┌─────────────────────────────────────────────┐
│  Chrome Extension (Dead Link Detector)      │
│  - Detects 404s                             │
│  - One-click resurrection                   │
└──────────────────┬──────────────────────────┘
                   │
┌──────────────────▼──────────────────────────┐
│  React Frontend (Gothic CRT UI)             │
│  - Vite dev server (local)                  │
│  - S3 + CloudFront (production)             │
│  - WebSocket connection to backend          │
└──────────────────┬──────────────────────────┘
                   │
┌──────────────────▼──────────────────────────┐
│  Express Backend (Resurrection Engine)      │
│  - Node.js server (local)                   │
│  - Lambda + API Gateway (production)        │
│  - WebSocket for real-time updates          │
└──────────────────┬──────────────────────────┘
                   │
┌──────────────────▼──────────────────────────┐
│  External Services                          │
│  ├─ Wayback Machine API (free)              │
│  ├─ OpenAI/Anthropic (paid)                 │
│  ├─ Pinecone (free tier available)          │
│  ├─ AWS S3 (storage)                        │
│  └─ AWS DynamoDB (metadata)                 │
└─────────────────────────────────────────────┘
```

## Cost Estimates (Production)

### Free Tier (Suitable for Demo)
- Wayback Machine: Free
- Pinecone: Free tier (1M vectors)
- AWS Free Tier: S3, Lambda, DynamoDB (12 months)
- **Total: $0/month**

### Low Traffic (~1000 resurrections/month)
- OpenAI API: ~$10/month
- Pinecone: Free tier
- AWS: ~$5/month
- **Total: ~$15/month**

### Production (~10,000 resurrections/month)
- OpenAI API: ~$100/month
- Pinecone: ~$70/month (paid tier)
- AWS: ~$50/month
- **Total: ~$220/month**

## Security Considerations

1. **API Keys**: Never commit `.env` file to git
2. **CORS**: Configure strict CORS policies in production
3. **Rate Limiting**: Implement rate limiting on API endpoints
4. **Input Validation**: Sanitize all user inputs (URLs)
5. **AWS IAM**: Follow least-privilege principles (see `.kiro/steering/aws-standards.md`)

## Monitoring

### Local Development
```bash
# Backend logs
npm run backend

# Frontend logs
cd frontend && npm run dev

# Check WebSocket connection in browser DevTools
```

### Production
```bash
# CloudWatch Logs
aws logs tail /aws/lambda/resurrection-engine --follow --profile your-aws-profile

# API Gateway metrics
# View in AWS Console → API Gateway → Your API → Dashboard

# DynamoDB metrics
# View in AWS Console → DynamoDB → Your Table → Metrics
```

## Scaling Considerations

### Backend
- Use AWS Lambda for automatic scaling
- Implement caching for frequently resurrected sites
- Add Redis for session management

### Vector Store
- Upgrade Pinecone tier for more vectors
- Implement vector caching for popular sites
- Consider self-hosted alternatives (Weaviate, Milvus)

### Frontend
- Use CloudFront CDN for global distribution
- Implement service worker for offline support
- Add lazy loading for resurrection gallery

---

**The séance chamber awaits... 👻**

For issues or questions, check the GitHub repository or contact the maintainers.
