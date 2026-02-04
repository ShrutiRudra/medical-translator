# Deployment Guide

This guide covers deploying the Medical Translator application to production.

## Quick Deploy Options

### Option 1: Vercel (Frontend) + Railway (Backend)

#### Backend on Railway

1. **Create Railway Account**
   - Go to [railway.app](https://railway.app)
   - Sign up with GitHub

2. **Deploy Backend**
   ```bash
   cd backend
   
   # Install Railway CLI
   npm install -g @railway/cli
   
   # Login
   railway login
   
   # Initialize project
   railway init
   
   # Add environment variables
   railway variables set ANTHROPIC_API_KEY=your_api_key_here
   railway variables set NODE_ENV=production
   
   # Deploy
   railway up
   ```

3. **Get Backend URL**
   - Railway will provide a URL like `https://your-app.railway.app`
   - Copy this URL for frontend configuration

#### Frontend on Vercel

1. **Update API URL**
   ```bash
   cd frontend
   echo "VITE_API_URL=https://your-app.railway.app" > .env.production
   ```

2. **Deploy to Vercel**
   ```bash
   # Install Vercel CLI
   npm install -g vercel
   
   # Login
   vercel login
   
   # Deploy
   vercel
   
   # For production
   vercel --prod
   ```

### Option 2: Render (Full Stack)

#### Backend on Render

1. **Create Render Account**
   - Go to [render.com](https://render.com)
   - Connect your GitHub repository

2. **Create Web Service**
   - Click "New +" → "Web Service"
   - Connect your repository
   - Set root directory: `backend`
   - Build command: `npm install`
   - Start command: `npm start`

3. **Add Environment Variables**
   - In Render dashboard, add:
     - `ANTHROPIC_API_KEY`: Your API key
     - `NODE_ENV`: production

4. **Deploy**
   - Render will auto-deploy on every push to main

#### Frontend on Render

1. **Create Static Site**
   - Click "New +" → "Static Site"
   - Connect same repository
   - Set root directory: `frontend`
   - Build command: `npm install && npm run build`
   - Publish directory: `dist`

2. **Add Environment Variable**
   - `VITE_API_URL`: Your backend URL from step above

### Option 3: Single Server (VPS/EC2)

For deploying to a VPS or EC2 instance:

```bash
# Install Node.js and npm
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Install PM2 for process management
sudo npm install -g pm2

# Clone repository
git clone <your-repo-url>
cd medical-translator

# Setup Backend
cd backend
npm install
cp .env.example .env
# Edit .env and add your ANTHROPIC_API_KEY
pm2 start src/index.js --name medical-translator-api

# Setup Frontend
cd ../frontend
npm install
npm run build

# Serve frontend with nginx
sudo apt install nginx
sudo cp -r dist/* /var/www/html/

# Configure nginx
sudo nano /etc/nginx/sites-available/default
# Add proxy settings for /api to localhost:3001

sudo systemctl restart nginx
```

## Environment Variables

### Backend (.env)
```env
ANTHROPIC_API_KEY=sk-ant-xxxxx
PORT=3001
NODE_ENV=production
```

### Frontend (.env.production)
```env
VITE_API_URL=https://your-backend-url.com
```

## Post-Deployment Checklist

- [ ] Backend is accessible at /health endpoint
- [ ] Frontend connects to backend successfully
- [ ] API key is working (test translation)
- [ ] Database persists between restarts
- [ ] Audio uploads work correctly
- [ ] CORS is configured properly
- [ ] HTTPS is enabled (use Let's Encrypt)
- [ ] Error logging is set up
- [ ] Backups are configured for database

## Monitoring

### Backend Health Check
```bash
curl https://your-backend-url.com/health
```

Expected response:
```json
{
  "status": "healthy",
  "timestamp": "2024-01-15T10:30:00.000Z",
  "uptime": 12345
}
```

### Frontend Check
Visit your frontend URL and:
1. Create a new conversation
2. Send a test message
3. Verify translation works
4. Test audio recording
5. Generate a summary

## Troubleshooting

### Backend Issues

**Problem**: 500 errors on API calls
- Check logs for API key issues
- Verify ANTHROPIC_API_KEY is set correctly
- Check database file permissions

**Problem**: Audio uploads fail
- Ensure uploads directory exists and is writable
- Check file size limits
- Verify CORS settings

### Frontend Issues

**Problem**: Can't connect to backend
- Verify VITE_API_URL is correct
- Check CORS configuration in backend
- Ensure backend is accessible from frontend domain

**Problem**: Build fails
- Run `npm install` to ensure all dependencies are installed
- Check for any TypeScript/ESLint errors
- Verify all imports are correct

## Scaling Considerations

### For Production Use:

1. **Database**: Migrate from SQLite to PostgreSQL
2. **File Storage**: Use S3 or similar for audio files
3. **Caching**: Add Redis for conversation caching
4. **Load Balancing**: Use multiple backend instances
5. **CDN**: Serve frontend assets via CDN
6. **Monitoring**: Set up application monitoring (Sentry, DataDog)
7. **Authentication**: Add user authentication system
8. **Rate Limiting**: Implement API rate limiting
9. **Backup**: Automate database backups
10. **HIPAA Compliance**: If handling real patient data

## Cost Estimates

### Hobby/Demo (Free Tier):
- Railway: Free tier (500 hours/month)
- Vercel: Free tier (unlimited deployments)
- Anthropic API: Pay per use (~$0.003/1K tokens)
- **Total**: ~$5-20/month for light usage

### Production:
- Railway/Render: $7-20/month
- Vercel: Free - $20/month
- Anthropic API: $50-500/month (depends on usage)
- Database hosting: $10-50/month
- **Total**: $70-600/month

## Security Notes

1. **Never commit** `.env` files
2. **Rotate** API keys regularly
3. **Use HTTPS** in production
4. **Sanitize** all user inputs
5. **Implement** rate limiting
6. **Add** user authentication before production use
7. **Comply** with HIPAA if handling real medical data

---

For questions or issues, refer to the main README.md or contact support.
