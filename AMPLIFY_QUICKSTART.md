# 🚀 Amplify Deployment - Quick Reference

## 🎯 Three Deployment Options

### Option 1: App Runner + Amplify (Recommended)
**Best for:** Simple deployment, auto-scaling, WebSocket support
**Cost:** ~$10-20/month
**Time:** 20 minutes

```bash
1. Push code to GitHub
2. Deploy backend to App Runner (AWS Console)
3. Deploy frontend to Amplify (AWS Console)
4. Update environment variables
```

### Option 2: Lambda + Amplify (Serverless)
**Best for:** Low traffic, cost optimization
**Cost:** ~$5-10/month (mostly free tier)
**Time:** 15 minutes

```bash
npm install -g serverless
serverless deploy
# Then deploy frontend to Amplify
```

### Option 3: Amplify Gen 2 (All-in-One)
**Best for:** Full-stack integration, type safety
**Cost:** ~$10-15/month
**Time:** 30 minutes

```bash
npm create amplify@latest
npx ampx sandbox
```

---

## 📋 Pre-Deployment Checklist

- [ ] Code pushed to GitHub
- [ ] `.env` file configured (don't commit!)
- [ ] OpenAI or Anthropic API key
- [ ] Pinecone account created
- [ ] AWS account with billing enabled
- [ ] AWS CLI installed: `aws --version`

---

## ⚡ Quick Deploy Commands

### Backend (App Runner)
```bash
# No commands needed - deploy via AWS Console
# Or use AWS CLI:
aws apprunner create-service \
  --service-name echoes-backend \
  --source-configuration file://apprunner-config.json
```

### Frontend (Amplify)
```bash
# Via Console (recommended)
# Or via CLI:
amplify init
amplify add hosting
amplify publish
```

### Infrastructure (CDK)
```bash
cd infrastructure
python -m venv venv
venv\Scripts\activate  # Windows
pip install -r requirements.txt
cdk bootstrap
cdk deploy
```

### Serverless (Lambda)
```bash
npm install
serverless deploy --stage prod
```

---

## 🔑 Environment Variables

### Backend (Required)
```bash
OPENAI_API_KEY=sk-...
PINECONE_API_KEY=...
PINECONE_ENVIRONMENT=us-east-1-aws
PINECONE_INDEX_NAME=dead-web-memory
FRONTEND_URL=https://your-app.amplifyapp.com
```

### Frontend (Required)
```bash
VITE_API_URL=https://your-backend-url.com
```

---

## 🧪 Testing Deployment

```bash
# Test backend
curl https://your-backend-url/api/resurrections

# Test frontend
# Open browser to your Amplify URL
# Try resurrecting: myspace.com
```

---

## 📊 Cost Breakdown

| Service | Free Tier | Paid (Low Traffic) | Paid (High Traffic) |
|---------|-----------|-------------------|---------------------|
| App Runner | - | $5-10/mo | $20-50/mo |
| Amplify | 1000 build min | $0-5/mo | $10-20/mo |
| Lambda | 1M requests | $0-2/mo | $5-10/mo |
| S3 | 5GB | $0-1/mo | $2-5/mo |
| DynamoDB | 25GB | $0-1/mo | $5-10/mo |
| **Total** | **~$0** | **~$10-20/mo** | **~$40-100/mo** |

*Plus API costs: OpenAI (~$10-100/mo), Pinecone (free-$70/mo)*

---

## 🐛 Common Issues

### CORS Error
```javascript
// Add to backend-server.js
app.use(cors({
    origin: 'https://your-app.amplifyapp.com'
}));
```

### Build Fails
```bash
# Check amplify.yml baseDirectory
baseDirectory: frontend/dist  # Not just 'dist'
```

### WebSocket Not Working
```bash
# App Runner doesn't support WebSocket
# Use API Gateway WebSocket or polling
```

### Environment Variables Not Loading
```bash
# Frontend: Must start with VITE_
VITE_API_URL=...

# Backend: Restart service after adding vars
```

---

## 📚 Useful Links

- [App Runner Console](https://console.aws.amazon.com/apprunner)
- [Amplify Console](https://console.aws.amazon.com/amplify)
- [Pinecone Dashboard](https://app.pinecone.io/)
- [OpenAI API Keys](https://platform.openai.com/api-keys)
- [Full Deployment Guide](./deploy-to-amplify.md)

---

## 🆘 Need Help?

1. Check logs:
   - App Runner: Console → Logs tab
   - Amplify: Console → Build history
   - Lambda: CloudWatch Logs

2. Common fixes:
   ```bash
   # Redeploy backend
   # App Runner: Console → Deploy → Redeploy
   
   # Redeploy frontend
   # Amplify: Console → Redeploy this version
   ```

3. Still stuck? Check `DEPLOYMENT.md` for detailed troubleshooting

---

**The spirits await in the cloud... 👻☁️**
