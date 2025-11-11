# 🪦 AWS Amplify Deployment Guide - Echoes of the Dead Web

## Quick Deploy Checklist

- [ ] GitHub repository created and code pushed
- [ ] AWS account with billing enabled
- [ ] API keys ready (OpenAI/Anthropic, Pinecone)
- [ ] AWS CLI installed and configured

## Step-by-Step Deployment

### 1️⃣ Deploy Backend (AWS App Runner)

**Via AWS Console:**

1. Navigate to [AWS App Runner Console](https://console.aws.amazon.com/apprunner)
2. Click **"Create service"**
3. **Source:**
   - Repository type: **Source code repository**
   - Connect to GitHub (authorize AWS)
   - Select your repository: `echoes-of-the-dead-web`
   - Branch: `main`
4. **Deployment settings:**
   - Deployment trigger: **Automatic**
   - Configuration file: **Use configuration file** (`apprunner.yaml`)
5. **Service settings:**
   - Service name: `echoes-backend`
   - Virtual CPU: **1 vCPU**
   - Memory: **2 GB**
6. **Environment variables** (click "Add environment variable"):
   ```
   PORT=3001
   NODE_ENV=production
   OPENAI_API_KEY=your_key_here
   PINECONE_API_KEY=your_key_here
   PINECONE_ENVIRONMENT=us-east-1-aws
   PINECONE_INDEX_NAME=dead-web-memory
   FRONTEND_URL=https://main.xxxxx.amplifyapp.com
   ```
   *(You'll update FRONTEND_URL after Step 2)*
7. Click **"Create & deploy"**
8. Wait 5-10 minutes for deployment
9. **Copy the service URL** (e.g., `https://abc123.us-east-1.awsapprunner.com`)

### 2️⃣ Deploy Frontend (AWS Amplify)

**Via AWS Console:**

1. Navigate to [AWS Amplify Console](https://console.aws.amazon.com/amplify)
2. Click **"New app"** → **"Host web app"**
3. **Connect repository:**
   - Select **GitHub**
   - Authorize AWS Amplify
   - Select repository: `echoes-of-the-dead-web`
   - Branch: `main`
4. **Build settings:**
   - Amplify will auto-detect `amplify.yml`
   - Verify it shows:
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
     ```
5. **App name:** `echoes-of-the-dead-web`
6. Click **"Save and deploy"**
7. Wait 5-10 minutes for build
8. **Copy your Amplify URL** (e.g., `https://main.d1234abcd.amplifyapp.com`)

### 3️⃣ Update Environment Variables

**Update Backend (App Runner):**

1. Go back to App Runner console
2. Select your service → **Configuration** → **Environment variables**
3. Edit `FRONTEND_URL` to your Amplify URL
4. Click **"Deploy"** to restart with new config

**Add Frontend Environment Variable:**

1. Go to Amplify console
2. Select your app → **App settings** → **Environment variables**
3. Add variable:
   - Key: `VITE_API_URL`
   - Value: Your App Runner URL (from Step 1)
4. Go to **Build settings** → **Redeploy this version**

### 4️⃣ Deploy Infrastructure (Optional - for S3/DynamoDB)

```bash
# Navigate to infrastructure folder
cd infrastructure

# Create virtual environment
python -m venv venv

# Activate (Windows)
venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Bootstrap CDK (first time only)
cdk bootstrap

# Deploy
cdk deploy

# Note the outputs
# Example:
# ProjectStitchStack.IngestionBucketName = projectstitch-ingestionbucket-abc123
# ProjectStitchStack.LineageTableName = projectstitch-lineagetable-xyz789
```

**Add to App Runner environment variables:**
```
S3_BUCKET_NAME=projectstitch-ingestionbucket-abc123
DYNAMODB_TABLE_NAME=projectstitch-lineagetable-xyz789
AWS_REGION=us-east-1
```

### 5️⃣ Set Up Pinecone

1. Go to [Pinecone Console](https://app.pinecone.io/)
2. Create new index:
   - Name: `dead-web-memory`
   - Dimensions: `1536`
   - Metric: `cosine`
   - Cloud: `AWS`
   - Region: `us-east-1`
3. Copy API key from dashboard
4. Already added to App Runner in Step 1 ✓

### 6️⃣ Test Your Deployment

```bash
# Test backend API
curl https://your-apprunner-url.us-east-1.awsapprunner.com/api/resurrections

# Expected response:
# {"resurrections":[]}

# Test frontend
# Open your Amplify URL in browser
# Try resurrecting a dead site (e.g., myspace.com)
```

### 7️⃣ Set Up Custom Domain (Optional)

**For Frontend (Amplify):**
1. Amplify Console → **Domain management**
2. Add domain → Enter your domain
3. Follow DNS configuration steps
4. SSL certificate auto-provisioned

**For Backend (App Runner):**
1. App Runner Console → **Custom domains**
2. Add domain → Enter API subdomain (e.g., `api.yourdomain.com`)
3. Add CNAME record to your DNS
4. SSL certificate auto-provisioned

## Monitoring & Logs

### View Backend Logs
```bash
# Via AWS Console
App Runner → Your service → Logs tab

# Via AWS CLI
aws logs tail /aws/apprunner/echoes-backend/application --follow
```

### View Frontend Build Logs
```bash
# Via AWS Console
Amplify → Your app → Build history → Click on build

# View deployment
Amplify → Your app → Hosting → Deployed URL
```

### Monitor Costs
```bash
# AWS Cost Explorer
https://console.aws.amazon.com/cost-management/home

# Expected costs (first month):
# - App Runner: ~$5-10/month (1 vCPU, 2GB)
# - Amplify: ~$0-5/month (build minutes + hosting)
# - S3: ~$0-1/month
# - DynamoDB: ~$0-1/month (on-demand)
# - Total: ~$5-20/month
```

## Troubleshooting

### Backend won't start
```bash
# Check App Runner logs
# Common issues:
# - Missing environment variables
# - npm install failed
# - Port binding issues (ensure PORT=3001)
```

### Frontend can't connect to backend
```bash
# Check browser console for CORS errors
# Verify VITE_API_URL is set correctly
# Ensure FRONTEND_URL is set in backend
# Check App Runner security group allows HTTPS
```

### Build fails
```bash
# Check Amplify build logs
# Common issues:
# - npm ci failed (check package-lock.json)
# - Build command failed (check amplify.yml)
# - Wrong baseDirectory (should be frontend/dist)
```

### WebSocket connection fails
```bash
# App Runner doesn't support WebSocket by default
# Options:
# 1. Use API Gateway WebSocket API
# 2. Use polling instead of WebSocket
# 3. Deploy backend to ECS/EC2 with ALB
```

## Alternative: Serverless Backend (Lambda + API Gateway)

If you prefer fully serverless:

```bash
# Install Serverless Framework
npm install -g serverless

# Create serverless.yml
# Deploy
serverless deploy

# This creates:
# - Lambda function for backend
# - API Gateway for HTTP endpoints
# - API Gateway WebSocket API
```

See `serverless.yml` template in project root.

## Continuous Deployment

Both Amplify and App Runner auto-deploy on git push:

```bash
# Make changes
git add .
git commit -m "Update resurrection algorithm"
git push origin main

# Amplify and App Runner will auto-deploy
# Check status in AWS Console
```

## Rollback

**Amplify:**
- Console → Build history → Select previous build → Redeploy

**App Runner:**
- Console → Deployments → Select previous deployment → Redeploy

---

**The spirits await in the cloud... 👻☁️**

Need help? Check the main DEPLOYMENT.md or open an issue on GitHub.
