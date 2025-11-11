# 🪦 AWS Amplify Deployment - Complete Summary

## What We've Set Up

Your "Echoes of the Dead Web" project is now ready for AWS deployment with three different approaches:

### 1. **App Runner + Amplify** (Recommended)
- **Backend**: AWS App Runner (auto-scaling Node.js container)
- **Frontend**: AWS Amplify (static hosting with CI/CD)
- **Best for**: Production apps, WebSocket support, easy scaling
- **Files created**: `apprunner.yaml`, `amplify.yml`

### 2. **Lambda + Amplify** (Serverless)
- **Backend**: AWS Lambda + API Gateway (serverless functions)
- **Frontend**: AWS Amplify (static hosting)
- **Best for**: Low traffic, cost optimization, pay-per-use
- **Files created**: `lambda-handler.js`, `serverless.yml`

### 3. **CDK Infrastructure** (Optional)
- **Storage**: S3 bucket for resurrection data
- **Database**: DynamoDB for metadata
- **Processing**: Lambda for analysis
- **Already exists**: `infrastructure/` folder with CDK stack

---

## 📁 New Files Created

### Configuration Files
1. **`apprunner.yaml`** - App Runner service configuration
2. **`amplify.yml`** - Amplify build configuration
3. **`serverless.yml`** - Serverless Framework configuration
4. **`lambda-handler.js`** - Lambda-compatible backend wrapper

### Documentation
5. **`deploy-to-amplify.md`** - Complete step-by-step deployment guide
6. **`AMPLIFY_QUICKSTART.md`** - Quick reference card
7. **`AWS_DEPLOYMENT_SUMMARY.md`** - This file
8. **`.github/workflows/deploy.yml`** - GitHub Actions CI/CD

### Code Updates
9. **`backend-server.js`** - Updated with production CORS settings
10. **`frontend/src/App.jsx`** - Updated to use environment variables
11. **`package.json`** - Added serverless dependencies

---

## 🚀 Deployment Steps (Quick Version)

### Option 1: App Runner + Amplify (20 minutes)

```bash
# 1. Push to GitHub
git add .
git commit -m "Ready for AWS deployment"
git push origin main

# 2. Deploy Backend (AWS Console)
# - Go to App Runner Console
# - Create service from GitHub
# - Use apprunner.yaml
# - Add environment variables
# - Copy service URL

# 3. Deploy Frontend (AWS Console)
# - Go to Amplify Console
# - Connect GitHub repo
# - Use amplify.yml
# - Add VITE_API_URL with backend URL
# - Deploy

# 4. Update CORS
# - Add Amplify URL to App Runner FRONTEND_URL
# - Redeploy App Runner service
```

### Option 2: Serverless (15 minutes)

```bash
# 1. Install Serverless
npm install -g serverless

# 2. Configure AWS credentials
aws configure

# 3. Deploy
serverless deploy --stage prod

# 4. Deploy frontend to Amplify (same as above)
```

---

## 🔑 Environment Variables Needed

### Backend (App Runner or Lambda)
```bash
# Required
OPENAI_API_KEY=sk-...
PINECONE_API_KEY=...
PINECONE_ENVIRONMENT=us-east-1-aws
PINECONE_INDEX_NAME=dead-web-memory
FRONTEND_URL=https://your-app.amplifyapp.com

# Optional (if using CDK infrastructure)
S3_BUCKET_NAME=your-bucket-name
DYNAMODB_TABLE_NAME=your-table-name
AWS_REGION=us-east-1
```

### Frontend (Amplify)
```bash
VITE_API_URL=https://your-backend-url.com
```

---

## 💰 Cost Estimates

### Free Tier (First 12 months)
- **App Runner**: First 2,000 vCPU-hours free
- **Amplify**: 1,000 build minutes + 15GB storage free
- **Lambda**: 1M requests + 400,000 GB-seconds free
- **S3**: 5GB storage + 20,000 GET requests free
- **DynamoDB**: 25GB storage + 25 WCU/RCU free
- **Total**: ~$0/month (within free tier limits)

### After Free Tier (Low Traffic)
- **App Runner**: ~$5-10/month (1 vCPU, 2GB RAM)
- **Amplify**: ~$0-5/month (build minutes + hosting)
- **Lambda**: ~$0-2/month (if using serverless)
- **S3**: ~$0-1/month
- **DynamoDB**: ~$0-1/month
- **Total**: ~$10-20/month

### Plus External Services
- **OpenAI API**: ~$10-100/month (depends on usage)
- **Pinecone**: Free tier (1M vectors) or $70/month (paid)

---

## 🧪 Testing Your Deployment

### 1. Test Backend Health
```bash
curl https://your-backend-url.com/health

# Expected response:
# {"status":"alive","message":"The resurrection engine awakens..."}
```

### 2. Test Resurrection API
```bash
curl -X POST https://your-backend-url.com/api/resurrect \
  -H "Content-Type: application/json" \
  -d '{"url":"myspace.com"}'

# Should return resurrection data with snapshots
```

### 3. Test Frontend
```bash
# Open browser to your Amplify URL
# Try resurrecting a dead site:
# - myspace.com
# - geocities.com
# - Any 404 URL
```

---

## 📊 Architecture Diagram

```
┌─────────────────────────────────────────────────────┐
│                    User's Browser                    │
│  - Chrome Extension (optional)                       │
│  - React Frontend (Amplify hosted)                   │
└────────────────────┬────────────────────────────────┘
                     │ HTTPS
                     ▼
┌─────────────────────────────────────────────────────┐
│              AWS Amplify (Frontend)                  │
│  - Static hosting (S3 + CloudFront)                  │
│  - CI/CD from GitHub                                 │
│  - Custom domain support                             │
└────────────────────┬────────────────────────────────┘
                     │ API Calls
                     ▼
┌─────────────────────────────────────────────────────┐
│         AWS App Runner (Backend)                     │
│  - Node.js Express server                            │
│  - Auto-scaling containers                           │
│  - WebSocket support                                 │
└────────────────────┬────────────────────────────────┘
                     │
        ┌────────────┼────────────┐
        ▼            ▼            ▼
┌──────────┐  ┌──────────┐  ┌──────────┐
│    S3    │  │ DynamoDB │  │  Lambda  │
│ Storage  │  │ Metadata │  │ Analysis │
└──────────┘  └──────────┘  └──────────┘
        │            │            │
        └────────────┴────────────┘
                     │
        ┌────────────┼────────────┐
        ▼            ▼            ▼
┌──────────┐  ┌──────────┐  ┌──────────┐
│ Wayback  │  │  OpenAI  │  │ Pinecone │
│ Machine  │  │   API    │  │  Vector  │
└──────────┘  └──────────┘  └──────────┘
```

---

## 🐛 Troubleshooting

### Backend won't start
```bash
# Check App Runner logs
AWS Console → App Runner → Your service → Logs

# Common issues:
# - Missing environment variables
# - npm install failed
# - Port binding issues
```

### Frontend can't connect to backend
```bash
# Check browser console for errors
# Verify VITE_API_URL is set correctly
# Check CORS settings in backend
# Ensure FRONTEND_URL includes your Amplify domain
```

### CORS errors
```javascript
// backend-server.js already updated with:
app.use(cors({
    origin: (origin, callback) => {
        if (origin.includes('.amplifyapp.com')) {
            callback(null, true);
        }
    }
}));
```

### Build fails
```bash
# Check Amplify build logs
# Verify amplify.yml baseDirectory: frontend/dist
# Ensure package-lock.json is committed
# Check Node.js version compatibility
```

---

## 🔄 Continuous Deployment

### Automatic Deployment
Both App Runner and Amplify auto-deploy on git push:

```bash
# Make changes
git add .
git commit -m "Update resurrection algorithm"
git push origin main

# App Runner and Amplify will automatically:
# 1. Pull latest code
# 2. Run build
# 3. Deploy new version
# 4. Health check
# 5. Switch traffic
```

### Manual Deployment
```bash
# Redeploy backend
AWS Console → App Runner → Deploy → Redeploy

# Redeploy frontend
AWS Console → Amplify → Redeploy this version
```

### GitHub Actions (Optional)
The `.github/workflows/deploy.yml` file enables:
- Automatic testing on push
- Infrastructure deployment with `[deploy-infra]` in commit message
- Serverless deployment with `[deploy-serverless]` in commit message

---

## 📚 Next Steps

1. **Deploy to AWS** using one of the three options
2. **Set up custom domain** (optional)
3. **Configure monitoring** (CloudWatch, X-Ray)
4. **Add authentication** (Cognito, Auth0)
5. **Implement caching** (CloudFront, Redis)
6. **Scale up** as traffic grows

---

## 📖 Documentation Reference

- **Quick Start**: `AMPLIFY_QUICKSTART.md`
- **Step-by-Step**: `deploy-to-amplify.md`
- **Full Guide**: `DEPLOYMENT.md`
- **Architecture**: `ARCHITECTURE.md`
- **Demo Script**: `HACKATHON_DEMO.md`

---

## 🆘 Getting Help

1. **Check logs** in AWS Console
2. **Review documentation** in this repo
3. **Test locally** first with `npm run backend` and `npm run dev`
4. **Verify environment variables** are set correctly
5. **Check AWS service quotas** if deployment fails

---

## ✅ Deployment Checklist

Before deploying:
- [ ] Code pushed to GitHub
- [ ] `.env` configured (don't commit!)
- [ ] API keys obtained (OpenAI, Pinecone)
- [ ] AWS account created
- [ ] AWS CLI configured
- [ ] Billing alerts set up

After deploying:
- [ ] Backend health check passes
- [ ] Frontend loads correctly
- [ ] API calls work
- [ ] CORS configured
- [ ] Environment variables set
- [ ] Custom domain configured (optional)
- [ ] Monitoring enabled

---

**The spirits await in the cloud... 👻☁️**

Your resurrection engine is ready to summon the dead web from AWS!
