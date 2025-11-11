# 🚀 Deployment Fix Guide - Vercel & Render

## ✅ Fixed Issues

1. **Vercel Configuration**: Created `vercel.json` in root directory pointing to `xcel/server.js`
2. **Render Configuration**: Updated `render.yaml` with proper `rootDir: xcel`
3. **Both platforms**: Added proper timeout and memory settings

---

## 📋 Vercel Deployment Steps

### Option 1: Using Vercel Dashboard (Recommended)

1. **Go to Vercel Dashboard**
   - Visit: https://vercel.com/new
   - Sign in with GitHub

2. **Import Repository**
   - Click "Import Git Repository"
   - Select: `ashishpimple94/repo-node-js`
   - Click "Import"

3. **Configure Project Settings**
   - **Framework Preset**: Other
   - **Root Directory**: `xcel` ⚠️ **IMPORTANT!**
   - **Build Command**: (leave empty)
   - **Output Directory**: (leave empty)
   - **Install Command**: `npm install`

4. **Add Environment Variables**
   - Go to: **Settings → Environment Variables**
   - Add these variables:
     ```
     MONGODB_URI = mongodb+srv://Voterlist2:Test123@cluster0.ezzkjmw.mongodb.net/voterdata?retryWrites=true&w=majority
     MAX_FILE_SIZE_MB = 25
     NODE_ENV = production
     ```
   - Select all environments: **Production**, **Preview**, **Development**
   - Click **Save**

5. **Deploy**
   - Click **Deploy**
   - Wait for deployment to complete

### Option 2: Using Vercel CLI

```bash
# Install Vercel CLI
npm i -g vercel

# Login
vercel login

# Deploy (from root directory)
vercel

# For production
vercel --prod
```

**Note**: When using CLI, make sure to set **Root Directory** to `xcel` in Vercel Dashboard after first deployment.

---

## 📋 Render Deployment Steps

### Step 1: Create New Web Service

1. **Go to Render Dashboard**
   - Visit: https://dashboard.render.com
   - Click "New +" → "Web Service"

2. **Connect Repository**
   - Connect your GitHub account
   - Select repository: `ashishpimple94/repo-node-js`
   - Click "Connect"

3. **Configure Service**
   - **Name**: `voterlist-api` (or your choice)
   - **Environment**: `Node`
   - **Region**: Choose closest to you
   - **Branch**: `master` (or `main`)
   - **Root Directory**: `xcel` ⚠️ **IMPORTANT!**
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
   - **Plan**: Free (or your preferred plan)

4. **Add Environment Variables**
   - Scroll to "Environment Variables"
   - Click "Add Environment Variable"
   - Add these:
     ```
     MONGODB_URI = mongodb+srv://Voterlist2:Test123@cluster0.ezzkjmw.mongodb.net/voterdata?retryWrites=true&w=majority
     PORT = 3000
     MAX_FILE_SIZE_MB = 25
     NODE_ENV = production
     ```

5. **Deploy**
   - Click "Create Web Service"
   - Wait for deployment (5-10 minutes first time)

### Step 2: Verify Deployment

After deployment, test these endpoints:

```bash
# Health check
curl https://your-app.onrender.com/health

# Root endpoint
curl https://your-app.onrender.com/

# Search (example)
curl "https://your-app.onrender.com/api/voters/search?query=test"
```

---

## 🔧 Configuration Files

### Root `vercel.json` (for Vercel)
- Points to `xcel/server.js`
- Configured for serverless functions
- 60s timeout, 1024MB memory

### Root `render.yaml` (for Render)
- Uses `rootDir: xcel`
- Proper build and start commands
- Environment variables configured

### `xcel/vercel.json` (backup)
- For direct deployment from xcel folder
- Same configuration as root

---

## ⚠️ Common Issues & Solutions

### Issue 1: "Cannot find module server.js"

**Solution**:
- **Vercel**: Set Root Directory to `xcel` in project settings
- **Render**: Make sure `rootDir: xcel` is in `render.yaml` OR set Root Directory in dashboard

### Issue 2: "MONGODB_URI is not set"

**Solution**:
1. Go to platform dashboard (Vercel/Render)
2. Settings → Environment Variables
3. Add `MONGODB_URI` with your connection string
4. **IMPORTANT**: Redeploy after adding variables

### Issue 3: "Connection timeout"

**Solution**:
1. Check MongoDB Atlas Network Access
2. Add IP: `0.0.0.0/0` (allow from anywhere)
3. Wait 2-3 minutes for changes to propagate
4. Redeploy your app

### Issue 4: "Authentication failed"

**Solution**:
1. Verify MongoDB username and password
2. Check if password has special characters (need URL encoding)
3. Reset database user password if needed
4. Update `MONGODB_URI` in environment variables

---

## 📝 Environment Variables Checklist

### Required for Both Platforms:
- ✅ `MONGODB_URI` - MongoDB connection string
- ✅ `NODE_ENV` - Set to `production`

### Optional:
- `MAX_FILE_SIZE_MB` - Default: 25
- `PORT` - Auto-set by platforms (3000 for Render)

---

## 🎯 Quick Test After Deployment

1. **Health Check**:
   ```bash
   curl https://your-app-url/health
   ```
   Should return:
   ```json
   {
     "status": "ok",
     "mongodb": {
       "state": "connected"
     }
   }
   ```

2. **Root Endpoint**:
   ```bash
   curl https://your-app-url/
   ```
   Should return API information

3. **Search Test**:
   ```bash
   curl "https://your-app-url/api/voters/search?query=test"
   ```

---

## 📞 Support

If deployment still fails:

1. Check platform logs (Vercel/Render dashboard)
2. Verify all environment variables are set
3. Check MongoDB Atlas network access
4. Ensure Root Directory is set to `xcel`

---

## ✅ Success Checklist

- [ ] Repository pushed to GitHub
- [ ] Vercel/Render project created
- [ ] Root Directory set to `xcel`
- [ ] Environment variables added
- [ ] MongoDB Atlas network access configured
- [ ] Deployment successful
- [ ] Health check endpoint working
- [ ] API endpoints responding

