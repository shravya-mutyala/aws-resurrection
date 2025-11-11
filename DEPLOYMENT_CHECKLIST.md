# ✅ AWS Deployment Checklist

Use this checklist to ensure a smooth deployment to AWS Amplify.

## 📋 Pre-Deployment

### Code Preparation
- [ ] All code committed to git
- [ ] Code pushed to GitHub (or GitLab/Bitbucket)
- [ ] `.env` file configured locally (but NOT committed)
- [ ] `.gitignore` includes `.env`
- [ ] All dependencies in `package.json`
- [ ] Frontend dependencies in `frontend/package.json`

### AWS Account Setup
- [ ] AWS account created
- [ ] Billing information added
- [ ] AWS CLI installed: `aws --version`
- [ ] AWS credentials configured: `aws configure`
- [ ] IAM user has necessary permissions (or using root)

### API Keys & Services
- [ ] OpenAI API key obtained (or Anthropic)
- [ ] Pinecone account created
- [ ] Pinecone index created:
  - Name: `dead-web-memory`
  - Dimensions: `1536`
  - Metric: `cosine`
- [ ] Pinecone API key copied
- [ ] All API keys stored securely (password manager)

### Configuration Files
- [ ] `apprunner.yaml` exists in project root
- [ ] `amplify.yml` exists in project root
- [ ] `lambda-handler.js` exists (if using serverless)
- [ ] `serverless.yml` exists (if using serverless)

---

## 🚀 Backend Deployment (App Runner)

### Create Service
- [ ] Navigate to [AWS App Runner Console](https://console.aws.amazon.com/apprunner)
- [ ] Click "Create service"
- [ ] Select "Source code repository"
- [ ] Connect to GitHub (authorize AWS)
- [ ] Select repository: `echoes-of-the-dead-web`
- [ ] Select branch: `main`
- [ ] Deployment trigger: Automatic
- [ ] Configuration: Use `apprunner.yaml`

### Configure Service
- [ ] Service name: `echoes-backend`
- [ ] Virtual CPU: 1 vCPU
- [ ] Memory: 2 GB
- [ ] Port: 3001

### Environment Variables
- [ ] `PORT=3001`
- [ ] `NODE_ENV=production`
- [ ] `OPENAI_API_KEY=your_key`
- [ ] `PINECONE_API_KEY=your_key`
- [ ] `PINECONE_ENVIRONMENT=us-east-1-aws`
- [ ] `PINECONE_INDEX_NAME=dead-web-memory`
- [ ] `FRONTEND_URL=https://main.xxxxx.amplifyapp.com` (update after frontend deploy)

### Deploy & Verify
- [ ] Click "Create & deploy"
- [ ] Wait for deployment (5-10 minutes)
- [ ] Service status shows "Running"
- [ ] Copy service URL
- [ ] Test health endpoint: `curl https://your-url.com/health`

---

## 🎨 Frontend Deployment (Amplify)

### Create App
- [ ] Navigate to [AWS Amplify Console](https://console.aws.amazon.com/amplify)
- [ ] Click "New app" → "Host web app"
- [ ] Select "GitHub"
- [ ] Authorize AWS Amplify
- [ ] Select repository: `echoes-of-the-dead-web`
- [ ] Select branch: `main`

### Build Configuration
- [ ] Amplify auto-detects `amplify.yml`
- [ ] Verify `baseDirectory: frontend/dist`
- [ ] Verify build command: `npm run build`
- [ ] App name: `echoes-of-the-dead-web`

### Environment Variables
- [ ] Add `VITE_API_URL` with App Runner URL from above

### Deploy & Verify
- [ ] Click "Save and deploy"
- [ ] Wait for build (5-10 minutes)
- [ ] Build status shows "Deployed"
- [ ] Copy Amplify URL
- [ ] Open URL in browser
- [ ] Verify frontend loads

---

## 🔄 Update CORS Settings

### Update Backend
- [ ] Go to App Runner console
- [ ] Select your service
- [ ] Configuration → Environment variables
- [ ] Update `FRONTEND_URL` with Amplify URL
- [ ] Click "Deploy" to restart

### Verify Connection
- [ ] Open Amplify URL in browser
- [ ] Open browser DevTools (F12)
- [ ] Try resurrecting a site (e.g., myspace.com)
- [ ] Check Console for errors
- [ ] Verify API calls succeed

---

## 🏗️ Infrastructure Deployment (Optional)

### CDK Setup
- [ ] Navigate to `infrastructure/` folder
- [ ] Create virtual environment: `python -m venv venv`
- [ ] Activate: `venv\Scripts\activate` (Windows)
- [ ] Install dependencies: `pip install -r requirements.txt`
- [ ] Install CDK globally: `npm install -g aws-cdk`

### Deploy Stack
- [ ] Bootstrap CDK: `cdk bootstrap`
- [ ] Review changes: `cdk diff`
- [ ] Deploy: `cdk deploy`
- [ ] Confirm deployment
- [ ] Wait for completion (5-10 minutes)

### Update Backend Variables
- [ ] Copy S3 bucket name from CDK output
- [ ] Copy DynamoDB table name from CDK output
- [ ] Add to App Runner environment variables:
  - `S3_BUCKET_NAME=...`
  - `DYNAMODB_TABLE_NAME=...`
  - `AWS_REGION=us-east-1`
- [ ] Redeploy App Runner service

---

## 🧪 Testing

### Backend Tests
- [ ] Health check: `curl https://backend-url/health`
- [ ] List resurrections: `curl https://backend-url/api/resurrections`
- [ ] Resurrect test: 
  ```bash
  curl -X POST https://backend-url/api/resurrect \
    -H "Content-Type: application/json" \
    -d '{"url":"myspace.com"}'
  ```

### Frontend Tests
- [ ] Open Amplify URL in browser
- [ ] UI loads correctly
- [ ] Gothic theme displays
- [ ] Try resurrecting: `myspace.com`
- [ ] Verify snapshots appear
- [ ] Try chat feature
- [ ] Check browser console for errors

### Integration Tests
- [ ] Frontend can reach backend
- [ ] No CORS errors
- [ ] WebSocket connects (if applicable)
- [ ] Wayback Machine API works
- [ ] AI responses generate
- [ ] Vector storage works (Pinecone)

---

## 📊 Monitoring Setup

### CloudWatch
- [ ] Enable CloudWatch logs for App Runner
- [ ] Enable CloudWatch logs for Lambda (if using)
- [ ] Set up log retention (7 days recommended)
- [ ] Create CloudWatch dashboard

### Alarms
- [ ] Create alarm for App Runner errors
- [ ] Create alarm for high latency
- [ ] Create alarm for failed requests
- [ ] Set up SNS topic for notifications
- [ ] Subscribe email to SNS topic

### Cost Monitoring
- [ ] Enable AWS Cost Explorer
- [ ] Set up billing alerts
- [ ] Create budget: $20/month (adjust as needed)
- [ ] Enable cost allocation tags

---

## 🔒 Security

### API Keys
- [ ] All API keys in environment variables (not code)
- [ ] `.env` file in `.gitignore`
- [ ] No secrets committed to git
- [ ] API keys rotated if exposed

### AWS Security
- [ ] IAM roles follow least privilege
- [ ] S3 bucket not publicly accessible
- [ ] DynamoDB encryption enabled
- [ ] App Runner uses HTTPS only
- [ ] Amplify uses HTTPS only

### Application Security
- [ ] CORS properly configured
- [ ] Input validation enabled
- [ ] Rate limiting considered
- [ ] Error messages don't leak info

---

## 🌐 Custom Domain (Optional)

### Frontend Domain
- [ ] Domain purchased (Route 53, GoDaddy, etc.)
- [ ] Go to Amplify → Domain management
- [ ] Add domain
- [ ] Configure DNS records
- [ ] Wait for SSL certificate (15-30 min)
- [ ] Verify domain works

### Backend Domain
- [ ] Subdomain for API (e.g., api.yourdomain.com)
- [ ] Go to App Runner → Custom domains
- [ ] Add domain
- [ ] Configure CNAME record
- [ ] Wait for SSL certificate
- [ ] Update frontend `VITE_API_URL`

---

## 📱 Chrome Extension (Optional)

### Update Extension
- [ ] Update `chrome_extension/popup.js`
- [ ] Change API URL to production
- [ ] Test extension locally
- [ ] Package extension: `zip -r extension.zip chrome_extension/`

### Publish to Chrome Web Store
- [ ] Create Chrome Web Store developer account ($5 fee)
- [ ] Upload extension package
- [ ] Fill out store listing
- [ ] Submit for review
- [ ] Wait for approval (1-3 days)

---

## 🎉 Post-Deployment

### Documentation
- [ ] Update README with live URLs
- [ ] Update DEPLOYMENT.md with actual values
- [ ] Document any issues encountered
- [ ] Create runbook for common tasks

### Monitoring
- [ ] Check logs daily for first week
- [ ] Monitor costs in AWS Console
- [ ] Watch for errors in CloudWatch
- [ ] Test functionality regularly

### Optimization
- [ ] Review CloudWatch metrics
- [ ] Optimize slow endpoints
- [ ] Add caching if needed
- [ ] Scale resources if needed

### Backup
- [ ] Export DynamoDB data
- [ ] Backup S3 bucket
- [ ] Document configuration
- [ ] Store credentials securely

---

## 🐛 Troubleshooting

### If Backend Won't Start
- [ ] Check App Runner logs
- [ ] Verify environment variables
- [ ] Check npm install succeeded
- [ ] Verify port 3001 configured

### If Frontend Won't Build
- [ ] Check Amplify build logs
- [ ] Verify `amplify.yml` correct
- [ ] Check `package-lock.json` committed
- [ ] Verify Node.js version

### If CORS Errors
- [ ] Verify `FRONTEND_URL` in backend
- [ ] Check CORS configuration in `backend-server.js`
- [ ] Ensure Amplify domain included
- [ ] Redeploy backend after changes

### If API Calls Fail
- [ ] Verify `VITE_API_URL` in frontend
- [ ] Check backend is running
- [ ] Test backend directly with curl
- [ ] Check browser console for errors

---

## 📞 Support Resources

- **AWS Documentation**: https://docs.aws.amazon.com/
- **Amplify Docs**: https://docs.amplify.aws/
- **App Runner Docs**: https://docs.aws.amazon.com/apprunner/
- **Project Docs**: See `DEPLOYMENT.md`, `deploy-to-amplify.md`

---

## ✅ Final Verification

- [ ] Backend is running and accessible
- [ ] Frontend is deployed and loads
- [ ] API calls work end-to-end
- [ ] No CORS errors
- [ ] Resurrection works (test with myspace.com)
- [ ] Chat feature works
- [ ] Monitoring is set up
- [ ] Costs are within budget
- [ ] Documentation is updated
- [ ] Team knows how to access/manage

---

**Deployment Status**: ⬜ Not Started | 🟡 In Progress | ✅ Complete

**Deployment Date**: _______________

**Deployed By**: _______________

**Production URLs**:
- Frontend: _______________
- Backend: _______________

---

**The spirits await in the cloud... 👻☁️**
