# StudySimplify Beta Testing Guide

Welcome to the StudySimplify beta testing program! This guide will help you set up and test the application with your colleagues.

## Prerequisites

Before you begin, ensure you have:
- Node.js 18+ installed
- Access to a PostgreSQL database (Neon recommended)
- OpenAI API key
- Stripe API keys (for testing payment features)

## Setup Instructions

### 1. Environment Configuration

Copy the `.env.example` file to `.env`:
```bash
cp .env.example .env
```

Fill in the required environment variables in `.env`:

#### Required Variables:
- `DATABASE_URL` - Your PostgreSQL connection string
- `OPENAI_API_KEY` - Your OpenAI API key ([Get it here](https://platform.openai.com/api-keys))
  - **IMPORTANT**: Without this key, the application will operate in demo/test mode only
  - Content analysis and chat features require a valid OpenAI API key
  - You'll see clear error messages if the key is missing
- `STRIPE_SECRET_KEY` - Your Stripe secret key ([Get it here](https://dashboard.stripe.com/apikeys))
- `VITE_STRIPE_PUBLIC_KEY` - Your Stripe publishable key
- `STRIPE_WEBHOOK_SECRET` - Your Stripe webhook secret ([Configure here](https://dashboard.stripe.com/webhooks))
- `SESSION_SECRET` - A random string for session encryption

#### Stripe Product IDs:
Configure your Stripe products and add their price IDs:
- `STRIPE_PRICE_ID_PRO_MONTHLY` - Pro subscription ($15/month)
- `STRIPE_PRICE_ID_25_CREDITS` - 25 credits pack ($5)
- `STRIPE_PRICE_ID_100_CREDITS` - 100 credits pack ($15)
- `STRIPE_PRICE_ID_250_CREDITS` - 250 credits pack ($30)

### 2. Database Setup

Run the database migrations:
```bash
npm run db:push
```

### 3. Install Dependencies

```bash
npm install
```

### 4. Start the Application

```bash
npm run dev
```

The application will be available at `http://localhost:5000`

**Note**: On first visit, you'll see a landing page. Click "Get Started - Sign In" to authenticate with Replit Auth.

## ✨ Authentication System

### Individual User Accounts
StudySimplify now includes full authentication using Replit Auth! Each beta tester gets their own account with:

- **Personal login** using Google, GitHub, email, or other supported providers
- **Individual rate limits** - Each user gets their own 3 free analyses/day
- **Private library** - Your saved content and notes are yours alone
- **Separate usage tracking** - Your usage statistics are tracked individually
- **Email collection** - Emails are automatically collected for future updates (with opt-out)

### First-Time Setup: Database Migration Required

⚠️ **IMPORTANT**: Before anyone can use the app, you need to update the database schema.

**Option 1: Fresh Start (Recommended)**

This approach deletes all existing data and creates new tables for individual user accounts:

1. Open your database console (e.g., Neon dashboard)
2. Run this SQL to drop existing tables:
```sql
-- WARNING: This deletes all data!
DROP TABLE IF EXISTS payment_history CASCADE;
DROP TABLE IF EXISTS credit_transactions CASCADE;
DROP TABLE IF EXISTS learning_recommendations CASCADE;
DROP TABLE IF EXISTS user_learning_profile CASCADE;
DROP TABLE IF EXISTS chat_messages CASCADE;
DROP TABLE IF EXISTS chat_conversations CASCADE;
DROP TABLE IF EXISTS content_cache CASCADE;
DROP TABLE IF EXISTS monthly_usage CASCADE;
DROP TABLE IF EXISTS user_usage CASCADE;
DROP TABLE IF EXISTS saved_searches CASCADE;
DROP TABLE IF EXISTS processed_content CASCADE;
DROP TABLE IF EXISTS sessions CASCADE;
DROP TABLE IF EXISTS users CASCADE;
```

3. Run the migration:
```bash
npm run db:push
```

4. When prompted about `first_name`, select: **"+ first_name create column"** (press Enter)

For detailed migration instructions, see [MIGRATION_GUIDE.md](./MIGRATION_GUIDE.md)

## Testing Checklist

### Core Features to Test

- [ ] **Content Analysis**
  - [ ] URL analysis (try crypto whitepapers, news articles)
  - [ ] File upload (PDF, images, documents)
  - [ ] Direct text paste
  - [ ] Different complexity levels (ELI5, High School, College Grad, Vitalik Mode)

- [ ] **Legal Document Analysis**
  - [ ] Terms of Service analysis
  - [ ] Privacy Policy analysis
  - [ ] Red flag detection
  - [ ] Green flag highlighting

- [ ] **Rate Limiting**
  - [ ] Free tier (3 analyses/day)
  - [ ] Usage meter display (Michael Meter™)
  - [ ] Rate limit messages

- [ ] **Content Library**
  - [ ] Save analyzed content
  - [ ] Add notes and tags
  - [ ] Search and filter
  - [ ] Download (JSON, Text, Markdown)
  - [ ] Delete content

- [ ] **Study Buddy Chat** (Premium Feature)
  - [ ] Chat with AI about analyzed content
  - [ ] Conversation history
  - [ ] Different complexity levels

- [ ] **Payment Processing**
  - [ ] Credit pack purchases (use Stripe test cards)
  - [ ] Pro subscription purchase
  - [ ] Subscription cancellation
  - [ ] Billing info display

- [ ] **UI/UX**
  - [ ] Mobile responsiveness
  - [ ] Dark mode (if implemented)
  - [ ] Loading states
  - [ ] Error handling
  - [ ] Navigation

### Test Cards (Stripe Test Mode)

Use these test card numbers:
- **Success**: `4242 4242 4242 4242`
- **Decline**: `4000 0000 0000 0002`
- **Requires authentication**: `4000 0025 0000 3155`

Use any future expiry date, any 3-digit CVC, and any ZIP code.

## Known Issues & Workarounds

1. **JavaScript-Heavy Websites**: Some websites with heavy JavaScript may not scrape properly. Use the "Paste Text" option instead.

2. **PDF Processing**: Large PDFs may take longer to process. The system converts each page to an image and runs OCR.

3. **Rate Limiting**: Remember that the 3 free analyses/day limit is shared among ALL beta testers.

## Reporting Issues

When reporting issues, please include:
- Steps to reproduce
- Expected behavior
- Actual behavior
- Screenshots (if applicable)
- Browser and device information
- Any error messages from the console

## Feedback Categories

Please provide feedback on:
1. **Usability** - Is the interface intuitive?
2. **Analysis Quality** - Are the AI explanations helpful and accurate?
3. **Performance** - Are response times acceptable?
4. **Features** - What's missing? What would you like to see?
5. **Bugs** - Any errors or unexpected behavior?

## Privacy & Data

- All processed content is stored in the shared database
- Your OpenAI API usage will be charged to your account
- Test data can be cleared at any time
- Do not upload sensitive or confidential documents during beta testing

## Support

For questions or issues during beta testing:
- Check the application logs for errors
- Review this guide for common solutions
- Contact the development team with detailed issue reports

## Sharing with Beta Testers

When inviting your beta testers, share:
1. The application URL (your deployment URL or localhost if testing locally)
2. This beta testing guide
3. That they'll need to sign in with Replit Auth (Google, GitHub, or email)
4. Their email will be collected for future updates (they can opt out anytime)

Each tester will automatically get:
- 3 free content analyses per day
- Their own private library
- Individual usage tracking
- The ability to purchase credits or subscribe for unlimited use

## Next Steps After Beta

Based on your feedback, the following improvements are planned:
- Enhanced security features
- Performance optimizations
- Additional payment options (crypto payments)
- Mobile app versions

Thank you for participating in the StudySimplify beta program!
