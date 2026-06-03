# Deployment Guide

## Prerequisites

- AWS Account or deployment platform of choice
- Docker and Docker Compose installed locally
- PostgreSQL and MongoDB instances
- OpenAI API key

## Local Development

### Using Docker Compose

```bash
# Clone repository
git clone https://github.com/ipcaffiliate1-maker/SariJay-Automation.git
cd SariJay-Automation

# Setup environment
cp .env.example .env.local

# Start services
docker-compose up
```

Services will be available at:
- Frontend: http://localhost:3000
- Backend: http://localhost:5000
- PostgreSQL: localhost:5432
- MongoDB: localhost:27017
- Redis: localhost:6379

## Production Deployment

### Option 1: AWS Deployment

#### EC2 Setup

```bash
# SSH into EC2 instance
ssh -i your-key.pem ubuntu@your-instance-ip

# Install dependencies
sudo apt update
sudo apt install docker.io docker-compose nodejs npm

# Clone repository
git clone https://github.com/ipcaffiliate1-maker/SariJay-Automation.git
cd SariJay-Automation

# Configure environment
nano .env.production

# Build and deploy
docker-compose -f docker-compose.prod.yml up -d
```

#### RDS Database Setup

```bash
# Create PostgreSQL RDS instance
aws rds create-db-instance \
  --db-instance-identifier sarijah-db \
  --db-instance-class db.t3.micro \
  --engine postgres \
  --master-username admin \
  --master-user-password your-password \
  --allocated-storage 20
```

#### S3 Configuration

```bash
# Create S3 bucket for uploads
aws s3 mb s3://sarijah-uploads-$(date +%s)

# Configure bucket policy for public access if needed
```

### Option 2: Heroku Deployment

```bash
# Login to Heroku
heroku login

# Create app
heroku create sarijah-app

# Add PostgreSQL addon
heroku addons:create heroku-postgresql:hobby-dev

# Set environment variables
heroku config:set OPENAI_API_KEY=your_key
heroku config:set JWT_SECRET=your_secret

# Deploy
git push heroku main
```

### Option 3: Vercel (Frontend Only)

```bash
# Login to Vercel
npm install -g vercel
vercel login

# Deploy frontend
cd frontend
vercel --prod
```

## Environment Variables

Create `.env.production`:

```env
# Database
DATABASE_URL=postgresql://user:password@rds-instance:5432/sarijah_db
MONGODB_URI=mongodb+srv://user:password@cluster.mongodb.net/sarijah_analytics

# API Keys
OPENAI_API_KEY=sk_prod_xxxxx
JWT_SECRET=your_production_secret
JWT_REFRESH_SECRET=your_refresh_secret

# Application
NODE_ENV=production
PORT=5000
NEXT_PUBLIC_API_URL=https://api.yourdomain.com

# AWS
AWS_REGION=us-east-1
AWS_ACCESS_KEY_ID=xxxxx
AWS_SECRET_ACCESS_KEY=xxxxx
AWS_S3_BUCKET=sarijah-uploads
```

## SSL/TLS Setup

### Using Let's Encrypt with Nginx

```bash
# Install Nginx
sudo apt install nginx

# Install Certbot
sudo apt install certbot python3-certbot-nginx

# Generate certificate
sudo certbot certonly --nginx -d yourdomain.com

# Configure Nginx proxy
sudo nano /etc/nginx/sites-available/sarijah
```

Nginx configuration:

```nginx
server {
    listen 443 ssl;
    server_name yourdomain.com;

    ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
    }

    location /api {
        proxy_pass http://localhost:5000;
    }
}
```

## Monitoring & Logging

### Setup CloudWatch (AWS)

```bash
# Install CloudWatch agent
wget https://s3.amazonaws.com/amazoncloudwatch-agent/ubuntu/amd64/latest/amazon-cloudwatch-agent.deb
sudo dpkg -i -E ./amazon-cloudwatch-agent.deb
```

### Application Logging

```typescript
// backend/src/utils/logger.ts
import fs from 'fs';
import path from 'path';

const logDir = path.join(process.cwd(), 'logs');

if (!fs.existsSync(logDir)) {
  fs.mkdirSync(logDir);
}

export const log = (level: string, message: string, data?: any) => {
  const timestamp = new Date().toISOString();
  const logMessage = `[${timestamp}] [${level}] ${message}`;
  
  console.log(logMessage);
  
  fs.appendFileSync(
    path.join(logDir, `${level.toLowerCase()}.log`),
    `${logMessage} ${data ? JSON.stringify(data) : ''}\n`
  );
};
```

## Scaling Considerations

### Load Balancing

Use AWS Application Load Balancer or Nginx for distribution.

### Database Optimization

- Add indexes on frequently queried columns
- Enable read replicas for read-heavy operations
- Use connection pooling (PgBouncer for PostgreSQL)

### Caching

- Use Redis for session storage
- Cache API responses with appropriate TTLs
- Implement CDN for static assets

## CI/CD Pipeline

### GitHub Actions Workflow

Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Build Docker image
        run: docker build -t sarijah:latest .
      
      - name: Push to registry
        run: |
          echo ${{ secrets.DOCKER_PASSWORD }} | docker login -u ${{ secrets.DOCKER_USERNAME }} --password-stdin
          docker push sarijah:latest
      
      - name: Deploy to EC2
        run: |
          mkdir -p ~/.ssh
          echo "${{ secrets.SSH_PRIVATE_KEY }}" > ~/.ssh/id_rsa
          chmod 600 ~/.ssh/id_rsa
          ssh -i ~/.ssh/id_rsa ubuntu@${{ secrets.EC2_HOST }} 'cd /app && docker-compose pull && docker-compose up -d'
```

## Backup Strategy

### Daily Database Backups

```bash
# Create backup script
#!/bin/bash
TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
pg_dump -h $DB_HOST -U $DB_USER -d $DB_NAME > backup_$TIMESTAMP.sql
aws s3 cp backup_$TIMESTAMP.sql s3://sarijah-backups/
```

## Troubleshooting

### Check Container Logs

```bash
docker-compose logs -f backend
docker-compose logs -f frontend
```

### Database Connection Issues

```bash
# Test PostgreSQL connection
psql -h $DATABASE_HOST -U $DATABASE_USER -d $DATABASE_NAME

# Test MongoDB connection
mongosh $MONGODB_URI
```

### Performance Issues

```bash
# Check CPU/Memory usage
docker stats

# View database slow queries
# PostgreSQL: Enable log_min_duration_statement
```

## Support

For deployment issues, contact support@sarijah.com or create an issue on GitHub.
