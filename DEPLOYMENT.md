# Deployment Guide for Render

## Prerequisites
- GitHub account
- Render account (free)
- Your code pushed to GitHub

## Steps to Deploy on Render

### Method 1: Using render.yaml (Recommended)

1. **Push your code to GitHub** including the `render.yaml` file in the root directory

2. **Connect to Render**:
   - Go to [render.com](https://render.com)
   - Sign up/Login with GitHub
   - Click "New" -> "Blueprint"
   - Connect your GitHub repository
   - Render will automatically detect the `render.yaml` file

3. **Environment Variables** (will be set automatically via render.yaml):
   - `DATABASE_URL` - PostgreSQL connection string (auto-generated)
   - `SECRET_KEY` - Django secret key (auto-generated)
   - `WEB_CONCURRENCY` - Number of workers

### Method 2: Manual Setup

1. **Create a Web Service**:
   - Go to Render Dashboard
   - Click "New" -> "Web Service"
   - Connect your GitHub repository
   - Choose the repository and branch

2. **Configure the Service**:
   - **Name**: `batnotes-backend`
   - **Runtime**: `Python 3`
   - **Build Command**: `./build.sh`
   - **Start Command**: `gunicorn backend.wsgi:application`
   - **Root Directory**: `backend`

3. **Add Environment Variables**:
   - `SECRET_KEY`: Generate a secure secret key
   - `DEBUG`: `False`
   - `ALLOWED_HOSTS`: `your-app-name.onrender.com`
   - `DATABASE_URL`: Will be provided by Render PostgreSQL

4. **Create PostgreSQL Database**:
   - Click "New" -> "PostgreSQL"
   - Name: `batnotes`
   - Copy the connection string to `DATABASE_URL` environment variable

## Post-Deployment

1. **Update CORS Settings**:
   - After deployment, update `CORS_ALLOWED_ORIGINS` in settings.py
   - Add your Render app URL: `https://your-app-name.onrender.com`

2. **Test the API**:
   - Visit `https://your-app-name.onrender.com/api/`
   - Test authentication endpoints

3. **Frontend Deployment**:
   - Deploy your React frontend separately
   - Update API URLs in your frontend to point to your Render backend

## Important Notes

- **Free Tier Limitations**:
  - Apps spin down after 15 minutes of inactivity
  - First request after spin-down may be slow (cold start)
  - 512MB RAM limit

- **Database**:
  - PostgreSQL database will be created automatically
  - Data persists across deployments

- **Static Files**:
  - Handled by WhiteNoise middleware
  - Collected during build process

## Troubleshooting

- Check build logs in Render dashboard
- Ensure all environment variables are set
- Verify database connection
- Check Django settings for production

## Environment Variables Reference

| Variable | Description | Example |
|----------|-------------|---------|
| `SECRET_KEY` | Django secret key | Auto-generated |
| `DEBUG` | Debug mode | `False` |
| `ALLOWED_HOSTS` | Allowed hostnames | `your-app.onrender.com` |
| `DATABASE_URL` | PostgreSQL connection | Auto-provided by Render |
| `WEB_CONCURRENCY` | Number of workers | `4` |