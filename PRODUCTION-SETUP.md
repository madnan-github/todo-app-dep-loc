# Production Deployment Setup Guide

This guide walks you through configuring your TaskFlow application for production deployment on Vercel (frontend) and Railway (backend).

## Deployed URLs

- **Frontend (Vercel):** https://web-taskflow.vercel.app/
- **Backend (Railway):** https://tasklow-web-production.up.railway.app/

---

## 1. Railway Backend Configuration

### Required Environment Variables

Set these environment variables in your Railway dashboard:

```bash
# Database (Neon PostgreSQL - Pooled Connection)
DATABASE_URL=postgresql://neondb_owner:npg_sTVtWHiXo15L@ep-damp-wildflower-ada8vtmu-pooler.c-2.us-east-1.aws.neon.tech/neondb?sslmode=require&channel_binding=require

# JWT Authentication
JWT_SECRET_KEY=BeYWoWvyT1eFr0HFiDeVU1rCJ6EpRZcw9IB/dcZ9by8=

# Better Auth
BETTER_AUTH_SECRET=pgkePkCIxaibL3qlPYRyihR73KUx7kKt

# CORS Configuration (CRITICAL - include both localhost and production)
CORS_ORIGINS=http://localhost:3000,https://web-taskflow.vercel.app

# Application Settings
ENVIRONMENT=production
DEBUG=false
API_HOST=0.0.0.0

# Railway automatically provides PORT - do NOT set it manually
```

### How to Set Environment Variables in Railway

1. Go to your Railway project dashboard
2. Select your backend service
3. Click on the "Variables" tab
4. Add each environment variable:
   - Click "+ New Variable"
   - Enter the variable name (e.g., `CORS_ORIGINS`)
   - Enter the value (e.g., `http://localhost:3000,https://web-taskflow.vercel.app`)
   - Click "Add"
5. After adding all variables, Railway will automatically redeploy

### Important Notes

- **PORT variable:** Railway automatically injects this - do NOT set it manually
- **DATABASE_URL:** Must use the "Pooled" connection string from Neon (with `-pooler` suffix)
- **CORS_ORIGINS:** Must include your Vercel URL for API access
- **Leading Space Bug:** Railway sometimes adds a leading space to env var names - the backend config has a workaround for this

---

## 2. Vercel Frontend Configuration

### Required Environment Variables

Set these environment variables in your Vercel dashboard:

```bash
# API Configuration (Railway Backend)
NEXT_PUBLIC_API_URL=https://tasklow-web-production.up.railway.app
NEXT_PUBLIC_AUTH_URL=https://tasklow-web-production.up.railway.app

# JWT Configuration (must match backend)
JWT_SECRET=BeYWoWvyT1eFr0HFiDeVU1rCJ6EpRZcw9IB/dcZ9by8=
JWT_EXPIRY=7d

# Database URL for Better Auth (Neon PostgreSQL)
DATABASE_URL=postgresql://neondb_owner:npg_sTVtWHiXo15L@ep-damp-wildflower-ada8vtmu-pooler.c-2.us-east-1.aws.neon.tech/neondb?sslmode=require&channel_binding=require

# Better Auth Configuration
BETTER_AUTH_SECRET=pgkePkCIxaibL3qlPYRyihR73KUx7kKt
BETTER_AUTH_URL=https://web-taskflow.vercel.app

# Environment Settings
NODE_ENV=production
```

### How to Set Environment Variables in Vercel

1. Go to your Vercel project dashboard: https://vercel.com/dashboard
2. Select your project (web-taskflow)
3. Go to "Settings" → "Environment Variables"
4. For each variable:
   - Enter the "Key" (e.g., `NEXT_PUBLIC_API_URL`)
   - Enter the "Value" (e.g., `https://tasklow-web-production.up.railway.app`)
   - Select environments: **Production**, **Preview**, and **Development** (all three)
   - Click "Save"
5. After adding all variables, go to "Deployments" tab
6. Click the three dots menu on your latest deployment
7. Select "Redeploy" to trigger a new deployment with updated environment variables

### Important Notes

- **NEXT_PUBLIC_* variables:** These are exposed to the browser and used by API client
- **DATABASE_URL:** Required for Better Auth to connect to Neon database
- **JWT_SECRET:** Must exactly match the backend's `JWT_SECRET_KEY`
- **BETTER_AUTH_URL:** Must be your Vercel production URL
- **.env.local:** This file is NOT deployed to Vercel - you must set variables in the dashboard

---

## 3. Verification Checklist

After configuring both platforms, verify your deployment:

### Backend Health Check
```bash
curl https://tasklow-web-production.up.railway.app/health
# Expected: {"status":"healthy"}
```

### Frontend Access
1. Open https://web-taskflow.vercel.app/
2. You should see the TaskFlow homepage
3. Try signing up with a new account
4. Verify you can create tasks

### Common Issues

#### Issue: CORS errors in browser console
**Solution:** Check Railway's `CORS_ORIGINS` includes `https://web-taskflow.vercel.app`

#### Issue: "Not authenticated" errors
**Solution:** Verify `JWT_SECRET` in Vercel matches `JWT_SECRET_KEY` in Railway exactly

#### Issue: Database connection errors
**Solution:**
- Check `DATABASE_URL` is using the "Pooled" connection string
- Verify both Railway and Vercel have the same database URL
- Ensure `sslmode=require` is in the connection string

#### Issue: Better Auth not working
**Solution:**
- Verify `BETTER_AUTH_SECRET` matches in both platforms
- Check `BETTER_AUTH_URL` in Vercel is set to `https://web-taskflow.vercel.app`
- Ensure `DATABASE_URL` is accessible from Vercel

#### Issue: Environment variables not taking effect
**Solution:**
- Railway: Changes trigger automatic redeployment
- Vercel: You must manually redeploy after adding/changing variables

---

## 4. Local Development

To switch back to local development, update your local `.env.local`:

```bash
# Comment out production URLs
# NEXT_PUBLIC_API_URL=https://tasklow-web-production.up.railway.app
# NEXT_PUBLIC_AUTH_URL=https://tasklow-web-production.up.railway.app

# Uncomment local URLs
NEXT_PUBLIC_API_URL=http://localhost:8000
NEXT_PUBLIC_AUTH_URL=http://localhost:8000

# Update Better Auth URL
BETTER_AUTH_URL=http://localhost:3000

# Update environment
NODE_ENV=development
```

---

## 5. Security Recommendations

1. **Rotate Secrets:** The secrets in this guide are examples. In production:
   - Generate new `JWT_SECRET_KEY` using: `python3 -c "import secrets; print(secrets.token_urlsafe(32))"`
   - Generate new `BETTER_AUTH_SECRET` using the same command
   - Update both Railway and Vercel with the new secrets

2. **Database Access:** Your Neon database credentials are in the `DATABASE_URL`. Keep this secret.

3. **Environment Variables:** Never commit `.env` or `.env.local` files to Git (they're in `.gitignore`).

4. **CORS:** Only include trusted domains in `CORS_ORIGINS`. Do not use `*` wildcard in production.

---

## 6. Monitoring

### Railway Logs
- Go to Railway dashboard → Your service → "Deployments" tab
- Click on latest deployment to view logs
- Check for startup errors or configuration issues

### Vercel Logs
- Go to Vercel dashboard → Your project → "Deployments" tab
- Click on latest deployment
- View "Functions" logs for API route errors
- View "Build Logs" for compilation issues

---

## Support

If you encounter issues:
1. Check Railway logs for backend errors
2. Check Vercel logs for frontend errors
3. Use browser DevTools Console to see client-side errors
4. Verify all environment variables are set correctly in both platforms

