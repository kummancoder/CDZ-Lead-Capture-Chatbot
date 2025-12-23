# Deployment Guide

This guide explains how to deploy the chatbot frontend to Cloudflare Pages and backend to Render.

## Backend Deployment (Render)

### Prerequisites
1. GitHub account with your code repository
2. Render account (sign up at https://render.com)

### Steps

1. **Prepare your repository**
   - Make sure your code is pushed to GitHub
   - Ensure `src/package.json` has the start script: `"start": "node server.js"`

2. **Create a new Web Service on Render**
   - Go to https://dashboard.render.com
   - Click "New +" → "Web Service"
   - Connect your GitHub repository
   - Configure the service:
     - **Name**: `chatbot-backend` (or your preferred name)
     - **Root Directory**: `src`
     - **Environment**: `Node`
     - **Build Command**: `npm install`
     - **Start Command**: `npm start`
     - **Plan**: Choose your plan (Free tier available)

3. **Configure Environment Variables**
   In Render dashboard, go to your service → Environment tab, add:
   ```
   CHATBOT_NAME=chatbot
   CLOSINGDEALZ_API_KEY=your_closingdealz_api_key_here
   OPENAI_API_KEY=your_openai_api_key_here
   OPENAI_MODEL=gpt-4-turbo
   OPENAI_TEMPERATURE=1
   OPENAI_TOP_P=1
   ENABLE_API_KEY=true
   API_KEY=303205a3-1968-4e0a-855c-5c224e1f2a75
   HOSTNAME=0.0.0.0
   PORT=10000
   ALLOWED_ORIGINS=*
   ```
   
   **Important**: 
   - Use your actual API keys
   - Render sets `PORT` automatically, but you can override it
   - `ALLOWED_ORIGINS` can be set to `*` or specific Cloudflare Pages URLs (comma-separated)

4. **Deploy**
   - Click "Create Web Service"
   - Render will build and deploy your backend
   - Note your service URL (e.g., `https://chatbot-backend.onrender.com`)

### Render Notes
- Free tier services sleep after 15 minutes of inactivity
- First request after sleep may take longer
- Consider upgrading for production use

---

## Frontend Deployment (Cloudflare Pages)

### Prerequisites
1. Cloudflare account (sign up at https://dash.cloudflare.com)
2. Your Render backend URL

### Steps

1. **Update Frontend Configuration**
   - Open `chatbot.html`
   - Find the API_URL configuration (around line 260)
   - Change it to your Render backend URL:
     ```javascript
     const API_URL = 'https://your-app-name.onrender.com'; // Your Render URL
     ```

2. **Deploy via Cloudflare Dashboard**
   - Go to https://dash.cloudflare.com → Pages
   - Click "Create a project" → "Upload assets"
   - Drag and drop `chatbot.html` (or upload it)
   - Click "Deploy site"
   - Your site will be live at: `https://your-project-name.pages.dev`

3. **Deploy via Git (Recommended)**
   - In Cloudflare Pages, click "Create a project" → "Connect to Git"
   - Connect your GitHub repository
   - Configure build:
     - **Project name**: `chatbot-frontend`
     - **Production branch**: `main` (or your default branch)
     - **Build command**: Leave empty (static HTML)
     - **Build output directory**: `/` (root)
   - In "Environment variables" (if needed for build process)
   - Click "Save and Deploy"

4. **Update API URL for Production**
   - If using Git deployment, update `chatbot.html` with your Render URL
   - Commit and push changes
   - Cloudflare will auto-deploy

### Alternative: Manual Configuration via URL Parameter
Instead of editing the file, you can use URL parameters:
```
https://your-site.pages.dev/?apiUrl=https://your-backend.onrender.com
```

---

## Quick Deployment Checklist

### Backend (Render)
- [ ] Code pushed to GitHub
- [ ] Render service created
- [ ] All environment variables configured
- [ ] Service URL noted (e.g., `https://chatbot-backend.onrender.com`)

### Frontend (Cloudflare Pages)
- [ ] `chatbot.html` updated with Render backend URL
- [ ] Deployed to Cloudflare Pages
- [ ] Frontend URL noted (e.g., `https://chatbot-frontend.pages.dev`)

### Testing
- [ ] Test backend: `curl https://your-backend.onrender.com/start-thread -H "X-API-Key: your-api-key"`
- [ ] Test frontend: Open Cloudflare Pages URL and try chatting
- [ ] Verify API key authentication works
- [ ] Test full conversation flow

---

## Environment Variables Reference

### Backend (.env for local, Environment Variables in Render)

| Variable | Description | Example |
|----------|-------------|---------|
| `CHATBOT_NAME` | Name of the chatbot | `chatbot` |
| `CLOSINGDEALZ_API_KEY` | ClosingDealz CRM API key | `your_key_here` |
| `OPENAI_API_KEY` | OpenAI API key | `sk-...` |
| `OPENAI_MODEL` | OpenAI model to use | `gpt-4-turbo` |
| `OPENAI_TEMPERATURE` | Model temperature | `1` |
| `OPENAI_TOP_P` | Model top_p | `1` |
| `ENABLE_API_KEY` | Enable API key protection | `true` |
| `API_KEY` | API key for requests | `303205a3-1968-4e0a-855c-5c224e1f2a75` |
| `HOSTNAME` | Server hostname | `0.0.0.0` (Render) |
| `PORT` | Server port | `10000` (or let Render set it) |
| `ALLOWED_ORIGINS` | CORS allowed origins | `*` or `https://your-site.pages.dev` |

---

## Troubleshooting

### Backend Issues

**Service not starting**
- Check Render logs for errors
- Verify all environment variables are set
- Ensure `package.json` has start script

**CORS errors**
- Set `ALLOWED_ORIGINS` environment variable
- Include your Cloudflare Pages URL
- Format: `https://your-site.pages.dev,https://another-site.pages.dev`

**API key errors**
- Verify `API_KEY` matches in backend and frontend
- Check `ENABLE_API_KEY` is set correctly

### Frontend Issues

**Cannot connect to backend**
- Verify API_URL in `chatbot.html` matches Render URL
- Check browser console for CORS errors
- Ensure backend is running (not sleeping on free tier)

**CORS errors**
- Update `ALLOWED_ORIGINS` in Render environment variables
- Include your Cloudflare Pages domain

---

## Production Recommendations

1. **Custom Domain**: Set up custom domains on both Render and Cloudflare
2. **API Key Security**: Use environment-specific API keys
3. **Rate Limiting**: Consider adding rate limiting to backend
4. **Monitoring**: Set up logging and monitoring
5. **Error Handling**: Ensure proper error handling in production
6. **HTTPS**: Both Render and Cloudflare provide HTTPS by default

---

## Support

For issues:
- Render docs: https://render.com/docs
- Cloudflare Pages docs: https://developers.cloudflare.com/pages/

