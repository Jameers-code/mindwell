# 🚀 Deployment Guide - MindWell

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Local Development Setup](#local-development-setup)
3. [Deployment to Vercel](#deployment-to-vercel)
4. [AWS Deployment](#aws-deployment)
5. [Database Setup](#database-setup)
6. [Environment Variables](#environment-variables)
7. [Production Checklist](#production-checklist)
8. [Monitoring & Maintenance](#monitoring--maintenance)

---

## Prerequisites

### Required Tools
- Node.js 18+ 
- npm or pnpm
- Git
- Docker (optional, for containerization)
- PostgreSQL 14+ (local development) or AWS RDS

### Accounts Required
- [GitHub](https://github.com) - Source control
- [Vercel](https://vercel.com) - Frontend hosting
- [Clerk](https://clerk.com) - Authentication
- [Google Cloud](https://cloud.google.com) - AI API
- [Stripe](https://stripe.com) - Payments (optional)
- [AWS](https://aws.amazon.com) - Database & backend (optional)

---

## Local Development Setup

### 1. Clone Repository
```bash
git clone https://github.com/Jameers-code/mindwell.git
cd mindwell
```

### 2. Install Dependencies
```bash
npm install
# or
pnpm install
```

### 3. Setup Environment Variables
```bash
# Copy the example file
cp .env.example .env.local

# Edit with your actual values
nano .env.local
```

### 4. Setup Database Locally

#### Option A: PostgreSQL (Local)
```bash
# Install PostgreSQL (macOS)
brew install postgresql@14

# Start PostgreSQL service
brew services start postgresql@14

# Create database
createdb mindwell_db

# Update .env.local
DATABASE_URL="postgresql://localhost:5432/mindwell_db"
```

#### Option B: Using Docker
```bash
# Create docker-compose.yml in project root
cat > docker-compose.yml << 'EOF'
version: '3.8'
services:
  postgres:
    image: postgres:14-alpine
    environment:
      POSTGRES_DB: mindwell_db
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
EOF

# Start database
docker-compose up -d

# Update .env.local
DATABASE_URL="postgresql://postgres:password@localhost:5432/mindwell_db"
```

### 5. Setup Prisma
```bash
# Generate Prisma client
npx prisma generate

# Create database schema
npx prisma db push

# Seed database (if seed file exists)
npx prisma db seed
```

### 6. Get API Keys

#### Google Generative AI (Gemini)
1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create new project
3. Enable "Google Generative AI API"
4. Create API key
5. Add to `.env.local`:
```
NEXT_PUBLIC_GOOGLE_API_KEY=your_key_here
```

#### Clerk Authentication
1. Go to [Clerk Dashboard](https://dashboard.clerk.com)
2. Create new application
3. Get publishable and secret keys
4. Add to `.env.local`:
```
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_test_...
CLERK_SECRET_KEY=sk_test_...
```

#### YouTube API
1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Enable "YouTube Data API v3"
3. Create API key
4. Add to `.env.local`:
```
NEXT_PUBLIC_YOUTUBE_API_KEY=your_key_here
```

### 7. Run Development Server
```bash
npm run dev
# or
pnpm dev
```

Visit `http://localhost:3000` in your browser.

---

## Deployment to Vercel

### Recommended for Production

### Step 1: Prepare Repository
```bash
# Ensure all changes are committed
git add .
git commit -m "Ready for production deployment"
git push origin main
```

### Step 2: Connect to Vercel
1. Go to [Vercel Dashboard](https://vercel.com/dashboard)
2. Click "Add New Project"
3. Import your GitHub repository
4. Select "mindwell" repository

### Step 3: Configure Environment Variables
In Vercel project settings:

1. Go to "Settings" → "Environment Variables"
2. Add all variables from `.env.example`:

```
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_live_...
CLERK_SECRET_KEY=sk_live_...
DATABASE_URL=postgresql://... (AWS RDS connection string)
NEXT_PUBLIC_GOOGLE_API_KEY=your_key_here
NEXT_PUBLIC_YOUTUBE_API_KEY=your_key_here
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_live_...
STRIPE_SECRET_KEY=sk_live_...
```

### Step 4: Database Setup on AWS RDS
```bash
# Create PostgreSQL RDS instance on AWS
# Get the connection string
# Update DATABASE_URL in Vercel

# Run migrations
npx prisma migrate deploy
```

### Step 5: Deploy
```bash
# Deploy main branch
git push origin main

# Vercel automatically deploys
# Check deployment at: https://mindwell.vercel.app
```

### Step 6: Post-Deployment
1. Verify all environment variables are set
2. Test authentication (Clerk)
3. Test AI chat functionality
4. Check database connectivity

---

## AWS Deployment

### For Full-Stack Deployment

### Step 1: Setup RDS PostgreSQL
```bash
# Create RDS instance
# Type: PostgreSQL
# Instance class: db.t3.micro (free tier)
# Storage: 20 GB
# Backup: 7 days

# Get connection string from AWS Console
# Format: postgresql://username:password@endpoint:5432/dbname

# Update .env
DATABASE_URL="your_aws_rds_connection_string"
```

### Step 2: Setup EC2 (Alternative to Vercel)
```bash
# Launch EC2 instance (Ubuntu 22.04)
# Instance type: t3.micro (free tier)

# SSH into instance
ssh -i your-key.pem ec2-user@your-instance

# Install Node.js
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Install PM2 for process management
sudo npm install -g pm2

# Clone repository
git clone https://github.com/Jameers-code/mindwell.git
cd mindwell

# Install dependencies
npm install

# Create .env file with production variables
nano .env.local

# Build application
npm run build

# Start with PM2
pm2 start "npm run start" --name "mindwell"
pm2 save
pm2 startup
```

### Step 3: Setup CloudFront (CDN)
```bash
# Distribute static assets globally
# Reduce latency for users worldwide
# Cache invalidation for new deployments
```

### Step 4: Setup S3 for Media Storage
```bash
# Store user-generated content
# Profile pictures, therapy resources
# Configure CORS for upload
```

---

## Database Setup

### Initial Setup
```bash
# Generate Prisma client
npx prisma generate

# Create database tables
npx prisma db push

# View database in Prisma Studio
npx prisma studio
```

### Migrations
```bash
# Create new migration
npx prisma migrate dev --name add_new_field

# Apply migrations in production
npx prisma migrate deploy

# Check migration status
npx prisma migrate status
```

### Backup Database
```bash
# PostgreSQL backup
pg_dump -U username -h localhost -d mindwell_db > backup.sql

# Restore from backup
psql -U username -h localhost -d mindwell_db < backup.sql
```

---

## Environment Variables

### Development (.env.local)
```bash
# Authentication
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_test_...
CLERK_SECRET_KEY=sk_test_...

# Database
DATABASE_URL=postgresql://localhost:5432/mindwell_db

# AI
NEXT_PUBLIC_GOOGLE_API_KEY=...

# YouTube
NEXT_PUBLIC_YOUTUBE_API_KEY=...

# Stripe
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_...
STRIPE_SECRET_KEY=sk_test_...

# Environment
NODE_ENV=development
```

### Production
```bash
# Use live/production API keys
# Never commit .env files
# Use Vercel/AWS secrets management
# Rotate keys regularly
```

---

## Production Checklist

### Code Quality
- [ ] Remove all `console.log()` statements
- [ ] Fix all TypeScript errors
- [ ] Run linter: `npm run lint`
- [ ] Check security vulnerabilities: `npm audit`

### Performance
- [ ] Build application: `npm run build`
- [ ] Test build locally: `npm start`
- [ ] Optimize images
- [ ] Minify CSS/JavaScript
- [ ] Test Core Web Vitals

### Security
- [ ] Update all dependencies: `npm update`
- [ ] Audit packages: `npm audit fix`
- [ ] Enable HTTPS
- [ ] Setup CORS properly
- [ ] Review environment variables

### Database
- [ ] Backup database
- [ ] Run migrations: `npx prisma migrate deploy`
- [ ] Test database connectivity
- [ ] Setup automatic backups
- [ ] Monitor database performance

### Authentication
- [ ] Test Clerk authentication
- [ ] Setup production Clerk instance
- [ ] Test protected routes
- [ ] Verify logout functionality

### API Integration
- [ ] Test Google AI API
- [ ] Test YouTube API
- [ ] Test Stripe integration (if using)
- [ ] Verify error handling

### Monitoring
- [ ] Setup error tracking (Sentry)
- [ ] Setup logging
- [ ] Setup performance monitoring
- [ ] Configure alerts

### Testing
- [ ] Run all tests
- [ ] Test critical user flows
- [ ] Test on multiple devices
- [ ] Test in different browsers

---

## Monitoring & Maintenance

### Uptime Monitoring
```bash
# Setup Uptime Robot or similar
# Monitor: https://your-domain.com
# Alert: Email/Slack if down
```

### Error Tracking (Sentry)
```bash
# Install Sentry
npm install @sentry/nextjs

# Configure in next.config.js
# Automatic error reporting
```

### Database Monitoring
```bash
# Monitor with AWS CloudWatch
# Track query performance
# Monitor connections
# Set up alerts
```

### Security Updates
```bash
# Regular dependency updates
npm update

# Check for vulnerabilities
npm audit

# Keep Node.js updated
```

### Regular Tasks
- [ ] Weekly: Check error logs
- [ ] Weekly: Monitor performance
- [ ] Monthly: Update dependencies
- [ ] Monthly: Database backup verification
- [ ] Quarterly: Security audit

---

## Troubleshooting

### Database Connection Issues
```bash
# Check connection string
echo $DATABASE_URL

# Test connection
psql $DATABASE_URL

# Verify Prisma
npx prisma db execute --stdin < test.sql
```

### Build Errors
```bash
# Clear cache
rm -rf .next
rm -rf node_modules

# Reinstall
npm install

# Rebuild
npm run build
```

### Environment Variable Issues
```bash
# Check if loaded
console.log(process.env.NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY)

# Rebuild after changing variables
npm run build
npm run dev
```

---

## Support

For deployment issues:
- Check [Vercel Docs](https://vercel.com/docs)
- Check [AWS Documentation](https://docs.aws.amazon.com)
- Check [Prisma Docs](https://www.prisma.io/docs)
- Open [GitHub Issues](https://github.com/Jameers-code/mindwell/issues)

