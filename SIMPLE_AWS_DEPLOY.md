# 🚀 Simple AWS Deployment (No Extra Services!)

## What You Need
- ✅ GitHub account (free)
- ✅ AWS account (free tier)
- ❌ NO Pinecone needed
- ❌ NO OpenAI needed
- ❌ NO other services needed

Your app works 100% with just AWS!

---

## Step 1: Deploy Backend (App Runner) - 10 minutes

### 1.1 Open App Runner
Go to: https://console.aws.amazon.com/apprunner

### 1.2 Create Service
1. Click **"Create service"**
2. **Repository type**: "Source code repository"
3. Click **"Add new"** → Connect GitHub
4. Authorize AWS
5. Select your repo: `aws-resurrection`
6. Branch: `main`
7. **Deployment trigger**: Automatic
8. Click **"Next"**

### 1.3 Build Settings
1. **Configuration file**: "Use a configuration file"
2. File: `apprunner.yaml`
3. Click **"Next"**

### 1.4 Service Settings
1. **Service name**: `echoes-backend`
2. **CPU**: 1 vCPU
3. **Memory**: 2 GB
4. **Port**: 3001

### 1.5 Environment Variables (MINIMAL)
Add only these TWO:
```
PORT = 3001
NODE_ENV = production
```

That's it! No API keys needed!

### 1.6 Deploy
1. Click **"Next"** → **"Create & deploy"**
2. Wait 5-10 minutes
3. Copy your URL: `https://xxxxx.us-east-1.awsapprunner.com`

### 1.7 Test
Open: `https://your-url.us-east-1.awsapprunner.com/health`

Should see: `{"status":"alive",...}`

---

## Step 2: Deploy Frontend (Amplify) - 10 minutes

### 2.1 Open Amplify
Go to: https://console.aws.amazon.com/amplify

### 2.2 Create App
1. Click **"New app"** → **"Host web app"**
2. Select **"GitHub"**
3. Authorize AWS Amplify
4. Select repo: `aws-resurrection`
5. Branch: `main`
6. Click **"Next"**

### 2.3 Build Settings
1. **App name**: `echoes-dead-web`
2. Amplify auto-detects `amplify.yml` ✓
3. Click **"Next"** → **"Save and deploy"**
4. Wait 5-10 minutes

### 2.4 Add Backend URL
1. Click **"App settings"** → **"Environment variables"**
2. Click **"Manage variables"** → **"Add variable"**
3. Add:
   ```
   VITE_API_URL = https://your-apprunner-url.us-east-1.awsapprunner.com
   ```
4. Click **"Save"**
5. Go to **"Hosting"** → **"Redeploy this version"**

### 2.5 Get Your URL
Copy: `https://main.xxxxx.amplifyapp.com`

---

## Step 3: Connect Them (CORS) - 2 minutes

### 3.1 Update Backend
1. Go to App Runner console
2. Select `echoes-backend`
3. **Configuration** → **Edit** environment variables
4. Add:
   ```
   FRONTEND_URL = https://main.xxxxx.amplifyapp.com
   ```
   (Use your actual Amplify URL)
5. Click **"Save"** → Auto-redeploys

---

## Step 4: Test! 🎉

1. Open your Amplify URL
2. Enter: `myspace.com`
3. Click resurrect
4. See dead websites come back to life!

---

## What About AI Chat?

Your app has **built-in ghost responses** that work without any AI service! They're pre-programmed and contextual.

If you want smarter AI later, you can add OpenAI, but it's **completely optional**.

---

## Total Cost

**Free Tier (12 months):**
- App Runner: First 2,000 vCPU hours free
- Amplify: 1,000 build minutes free
- **Total: $0/month**

**After Free Tier:**
- App Runner: ~$5-10/month
- Amplify: ~$0-5/month
- **Total: ~$10/month**

---

## That's It!

No Pinecone. No OpenAI. No complicated setup.

Just GitHub → AWS → Done! 👻

---

## FAQ

**Q: Do I need Pinecone?**
A: No! It's not implemented in your code.

**Q: Do I need OpenAI?**
A: No! Your app has built-in responses.

**Q: Do I need S3/DynamoDB?**
A: No! Your app uses in-memory storage (works fine for demo).

**Q: What does my app actually use?**
A: Just the Wayback Machine API (free, no key needed).

**Q: Can I add AI later?**
A: Yes! Just add OPENAI_API_KEY environment variable when ready.
