# Social Media & API Integration Setup Guide

## Overview

SariJay Traffic Automation integrates with 10+ social media platforms, search engines, and email marketing services to automate traffic generation and content distribution.

## Supported Platforms

### Social Media (7 platforms)
- Facebook (Pages & Groups)
- Instagram (Business Accounts)
- Twitter/X
- TikTok
- LinkedIn
- Pinterest
- YouTube

### Search Engines (5 services)
- Google Search Console
- Google Business Profile
- Bing Webmaster Tools
- SEMrush
- Ahrefs

### Email Marketing (5 services)
- SendGrid
- Mailchimp
- ConvertKit
- ActiveCampaign
- Klaviyo

---

## Installation & Setup

### Prerequisites

```bash
# Install required packages
npm install axios dotenv
```

### Environment Variables

Add to `.env.local`:

```env
# Facebook
FACEBOOK_APP_ID=your_app_id
FACEBOOK_APP_SECRET=your_app_secret
FACEBOOK_PAGE_ACCESS_TOKEN=your_page_token

# Instagram
INSTAGRAM_BUSINESS_ACCOUNT_ID=your_account_id
INSTAGRAM_ACCESS_TOKEN=your_access_token

# Twitter/X
TWITTER_BEARER_TOKEN=your_bearer_token
TWITTER_API_KEY=your_api_key
TWITTER_API_SECRET=your_api_secret

# TikTok
TIKTOK_ACCESS_TOKEN=your_access_token
TIKTOK_CLIENT_KEY=your_client_key

# LinkedIn
LINKEDIN_ACCESS_TOKEN=your_access_token
LINKEDIN_CLIENT_ID=your_client_id
LINKEDIN_CLIENT_SECRET=your_client_secret

# Pinterest
PINTEREST_ACCESS_TOKEN=your_access_token
PINTEREST_APP_ID=your_app_id

# YouTube
YOUTUBE_API_KEY=your_api_key
YOUTUBE_CLIENT_ID=your_client_id
YOUTUBE_CLIENT_SECRET=your_client_secret

# Google Search Console
GOOGLE_SEARCH_CONSOLE_TOKEN=your_token

# SEMrush
SEMRUSH_API_KEY=your_api_key

# Ahrefs
AHREFS_API_KEY=your_api_key

# SendGrid
SENDGRID_API_KEY=your_api_key

# Mailchimp
MAILCHIMP_API_KEY=your_api_key

# ConvertKit
CONVERTKIT_API_KEY=your_api_key

# ActiveCampaign
ACTIVECAMPAIGN_API_URL=your_api_url
ACTIVECAMPAIGN_API_KEY=your_api_key

# Klaviyo
KLAVIYO_API_KEY=your_api_key
```

---

## API Setup Instructions

### Facebook

1. **Create Facebook App**
   - Go to https://developers.facebook.com
   - Create new app → Business type
   - Select "Manage business integrations"

2. **Get Access Token**
   - Navigate to App Settings → Basic
   - Copy App ID and App Secret
   - Use Facebook Graph API Explorer to get page access token

3. **Permissions Required**
   - `pages_manage_posts`
   - `pages_read_engagement`
   - `pages_manage_metadata`

**API Endpoint:**
```bash
POST /api/integrations/social/facebook/post
Body: {
  "pageId": "YOUR_PAGE_ID",
  "accessToken": "YOUR_PAGE_TOKEN",
  "content": {
    "message": "Your post text",
    "link": "https://example.com",
    "image": "https://example.com/image.jpg"
  }
}
```

---

### Instagram

1. **Switch to Business Account**
   - Instagram → Settings → Account Type
   - Switch to Professional Account (Business)

2. **Connect to Facebook**
   - Settings → Apps and Websites → Connected apps
   - Link your Facebook Business Account

3. **Get Business Account ID**
   - Facebook Business Suite → Instagram Accounts
   - Copy Business Account ID

**API Endpoint:**
```bash
POST /api/integrations/social/instagram/post
Body: {
  "igBusinessAccountId": "YOUR_BUSINESS_ACCOUNT_ID",
  "accessToken": "YOUR_ACCESS_TOKEN",
  "content": {
    "imageUrl": "https://example.com/image.jpg",
    "caption": "Your caption with #hashtags"
  }
}
```

---

### Twitter/X

1. **Create Developer Account**
   - Go to https://developer.twitter.com
   - Apply for developer access

2. **Create App**
   - Go to Dashboard → Projects & Apps
   - Create new app
   - Generate API keys and tokens

3. **Enable Required Permissions**
   - Settings → User Authentication
   - Enable OAuth 2.0
   - Set scopes: `tweet.write`, `tweet.read`, `users.read`

**API Endpoint:**
```bash
POST /api/integrations/social/twitter/post
Body: {
  "accessToken": "YOUR_BEARER_TOKEN",
  "content": {
    "text": "Your tweet text",
    "link": "https://example.com"
  }
}
```

---

### TikTok

1. **Get Creator Account**
   - Create TikTok account
   - Switch to Creator Account

2. **Register Developer App**
   - Go to https://developers.tiktok.com
   - Create new app
   - Verify email

3. **Get Access Token**
   - Use OAuth flow to get user access token

**API Endpoint:**
```bash
POST /api/integrations/social/tiktok/post
Body: {
  "accessToken": "YOUR_ACCESS_TOKEN",
  "content": {
    "videoUrl": "https://example.com/video.mp4",
    "caption": "Your caption",
    "hashtags": ["#sarijah", "#traffic"]
  }
}
```

---

### LinkedIn

1. **Create LinkedIn App**
   - Go to https://www.linkedin.com/developers/apps
   - Create app
   - Get Client ID and Secret

2. **Request Access**
   - Set redirect URI
   - Request access to needed products

3. **Get Access Token**
   - Use OAuth 2.0 flow
   - Scope: `w_member_social`

**API Endpoint:**
```bash
POST /api/integrations/social/linkedin/post
Body: {
  "accessToken": "YOUR_ACCESS_TOKEN",
  "content": {
    "text": "Your LinkedIn post",
    "linkedArticleUrl": "https://example.com/article"
  }
}
```

---

### Pinterest

1. **Create Pinterest App**
   - Go to https://developers.pinterest.com
   - Create app
   - Get App ID

2. **Verify Website**
   - Add Pinterest meta tag to website
   - Or use HTML file upload

3. **Get Access Token**
   - Use OAuth flow
   - Scope: `boards:read,boards:write,pins:read,pins:write`

**API Endpoint:**
```bash
POST /api/integrations/social/pinterest/post
Body: {
  "accessToken": "YOUR_ACCESS_TOKEN",
  "content": {
    "boardId": "YOUR_BOARD_ID",
    "imageUrl": "https://example.com/image.jpg",
    "title": "Pin title",
    "description": "Pin description",
    "link": "https://example.com"
  }
}
```

---

### YouTube

1. **Create Google Project**
   - Go to https://console.cloud.google.com
   - Create new project
   - Enable YouTube API v3

2. **Create OAuth Credentials**
   - Credentials → Create OAuth 2.0 Client ID
   - Application type: Desktop app
   - Download JSON credentials

3. **Get Access Token**
   - Use OAuth flow with scopes:
   - `https://www.googleapis.com/auth/youtube.upload`

**API Endpoint:**
```bash
POST /api/integrations/social/youtube/post
Body: {
  "accessToken": "YOUR_ACCESS_TOKEN",
  "content": {
    "title": "Video title",
    "description": "Video description",
    "videoUrl": "https://example.com/video.mp4",
    "tags": ["traffic", "marketing"],
    "privacyStatus": "public"
  }
}
```

---

## Search Engine Integrations

### Google Search Console

```bash
GET /api/integrations/search/google/analytics
Query: ?siteUrl=https://example.com&accessToken=TOKEN&startDate=2024-01-01&endDate=2024-12-31
```

### SEMrush

```bash
GET /api/integrations/search/semrush/overview
Query: ?domain=example.com&apiKey=YOUR_KEY
```

### Ahrefs

```bash
GET /api/integrations/search/ahrefs/overview
Query: ?domain=example.com&apiKey=YOUR_KEY
```

---

## Email Marketing Integrations

### SendGrid

```bash
POST /api/integrations/email/sendgrid/send
Body: {
  "apiKey": "YOUR_API_KEY",
  "content": {
    "to": "recipient@example.com",
    "subject": "Email subject",
    "htmlContent": "<h1>Email content</h1>",
    "fromEmail": "sender@example.com",
    "fromName": "SariJay"
  }
}
```

### Mailchimp

```bash
POST /api/integrations/email/mailchimp/subscriber
Body: {
  "apiKey": "YOUR_API_KEY",
  "dataCenter": "us1",
  "listId": "YOUR_LIST_ID",
  "subscriber": {
    "email": "subscriber@example.com",
    "firstName": "John",
    "lastName": "Doe"
  }
}
```

### ConvertKit

```bash
POST /api/integrations/email/convertkit/subscribe
Body: {
  "apiKey": "YOUR_API_KEY",
  "sequenceId": "SEQUENCE_ID",
  "subscriber": {
    "email": "subscriber@example.com",
    "firstName": "John"
  }
}
```

---

## Usage Examples

### Schedule Daily Posts

```typescript
// Schedule Facebook post at 9 AM daily
import cron from 'node-cron';
import { ContentGenerator } from './ai-engine';
import SocialMediaService from './backend/src/services/SocialMediaService';

cron.schedule('0 9 * * *', async () => {
  const content = await ContentGenerator.generateSocialContent(
    'Digital marketing tips',
    ['marketing', 'business']
  );

  await SocialMediaService.facebook.postToPage(
    pageId,
    accessToken,
    { message: content }
  );
});
```

### Monitor Analytics

```typescript
// Get daily insights
const insights = await SocialMediaService.facebook.getPageInsights(
  pageId,
  accessToken
);

console.log('Daily visitors:', insights.data[0].values);
```

### Automated Email Campaigns

```typescript
// Send follow-up email
await EmailMarketingService.sendgrid.sendEmail(
  apiKey,
  {
    to: 'customer@example.com',
    subject: 'Check out our latest offer!',
    htmlContent: '<h1>Special 50% off today</h1>',
  }
);
```

---

## Best Practices

✅ **Rate Limiting**
- Respect API rate limits
- Implement exponential backoff for retries
- Cache responses when possible

✅ **Error Handling**
- Log all API errors
- Implement fallback mechanisms
- Alert on critical failures

✅ **Security**
- Never commit API keys to version control
- Use environment variables
- Rotate tokens regularly
- Implement request signing

✅ **Monitoring**
- Track API usage and costs
- Monitor posting success rates
- Set up alerts for failures

---

## Troubleshooting

### "Invalid Access Token"
- Token may have expired
- Regenerate fresh token
- Verify permissions

### "Rate Limit Exceeded"
- Wait before next request
- Implement queue system
- Upgrade API tier

### "Permission Denied"
- Check OAuth scopes
- Verify app is approved
- Review account permissions

---

## Support

For API-specific issues:
- Facebook: https://developers.facebook.com/docs
- Twitter: https://developer.twitter.com/en/docs
- Google: https://developers.google.com/docs
- TikTok: https://developers.tiktok.com/doc

Contact support: support@sarijah.com
