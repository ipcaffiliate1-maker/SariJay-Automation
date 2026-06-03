# Development Guide

## Project Structure Overview

### Frontend (`/frontend`)
React/Next.js application with:
- Dashboard UI
- Campaign management
- Analytics visualization
- Chat assistant interface
- Mobile responsive design

### Backend (`/backend`)
Node.js/Express API with:
- User authentication
- Campaign management
- Traffic analytics
- AI orchestration
- Content generation

### AI Engine (`/ai-engine`)
AI and machine learning services:
- Traffic generation logic
- Content creation
- SEO optimization
- Viral content prediction
- Campaign optimization

## Setup Instructions

### 1. Clone Repository
```bash
git clone https://github.com/ipcaffiliate1-maker/SariJay-Automation.git
cd SariJay-Automation
```

### 2. Environment Setup
```bash
cp .env.example .env.local
# Edit .env.local with your credentials
```

### 3. Install Dependencies
```bash
npm run install:all
```

### 4. Start Development

**Option A: Using Docker (Recommended)**
```bash
docker-compose up
```

**Option B: Local Setup**
```bash
# Terminal 1: Backend
cd backend
npm run dev

# Terminal 2: Frontend
cd frontend
npm run dev

# Terminal 3: AI Engine
cd ai-engine
npm run dev
```

## API Endpoints

### Authentication
- `POST /api/auth/register` - User registration
- `POST /api/auth/login` - User login
- `POST /api/auth/refresh` - Refresh token
- `POST /api/auth/logout` - Logout

### Campaigns
- `GET /api/campaigns` - List campaigns
- `POST /api/campaigns` - Create campaign
- `GET /api/campaigns/:id` - Get campaign details
- `PUT /api/campaigns/:id` - Update campaign
- `DELETE /api/campaigns/:id` - Delete campaign

### Analytics
- `GET /api/analytics/traffic` - Traffic data
- `GET /api/analytics/conversions` - Conversion data
- `GET /api/analytics/sources` - Traffic sources

### AI Services
- `POST /api/ai/generate-content` - Generate content
- `POST /api/ai/optimize-campaign` - Optimize campaign
- `GET /api/ai/suggestions` - Get AI suggestions

## Testing

```bash
# Run tests
npm run test

# Run with coverage
npm run test:coverage

# Watch mode
npm run test:watch
```

## Deployment

See [DEPLOYMENT.md](./DEPLOYMENT.md) for deployment instructions.
