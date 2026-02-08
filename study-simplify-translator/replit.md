# StudySimplify Application

## Overview

StudySimplify is a full-stack web application designed to transform complex content from web pages, uploaded files, and direct text input into professional, comprehensive analyses through AI-powered information synthesis. It leverages AI to generate detailed explanations, structured key points, red/green flag analysis, and provide focused insights, with a primary focus on crypto whitepapers, investment reports, and legal documents. The application aims to make complex information accessible and actionable, particularly for cryptocurrency research and investment analysis.

## User Preferences

Preferred communication style: Friendly but highly intelligent content analyst - smart, approachable, not condescending. Think "brilliant friend explaining over coffee" with sharp insights, structure, and humor where appropriate. Avoid generic AI summarizer tone.

## Pricing Structure

**Subscription Plan:**
- **Pro Tier** ($15/month): Unlimited analyses with max 10 analyses/day limit, all complexity levels, Study Buddy AI chat for deeper analysis, priority processing, and crypto payment options (USDC/USDT). Positioned as the most popular option for serious researchers and investors.

**Free Tier:**
- 3 analyses per day with no monthly cap

**Credit Packs (one-time purchase):**
- 25 credits for $5.00 ($0.20 per analysis)
- 100 credits for $15.00 ($0.15 per analysis)
- 250 credits for $30.00 ($0.12 per analysis)
- Max 10 analyses/day limit applies to all credit packs

Credits never expire and can be used anytime. The Pro subscription offers unlimited analyses (within daily limits) and exclusive features like Study Buddy AI chat, making it the preferred choice for power users who need regular access to content analysis.

## System Architecture

### Full-Stack Monorepo Structure
- **Frontend**: React + TypeScript with Vite
- **Backend**: Node.js + Express
- **Database**: PostgreSQL with Drizzle ORM
- **UI Framework**: Shadcn/UI components with Tailwind CSS
- **AI Integration**: OpenAI GPT-4o
- **File Processing**: Tesseract.js for OCR, pdf2pic for PDF-to-image conversion (requires poppler_utils), Cheerio for web scraping

### Technical Implementation
- **Runtime**: Node.js with ESM modules
- **Frontend**: React 18, TypeScript, Vite, Wouter (routing), TanStack Query (server state), React hooks (local state)
- **Backend**: Express.js, Multer (file uploads)
- **Styling**: Tailwind CSS with Radix UI primitives
- **API Design**: RESTful endpoints
- **Content Processing**: Pipeline for URL scraping, OCR, and AI synthesis. Includes specific handling for JavaScript-heavy websites by guiding users to copy/paste text. PDF processing pipeline converts each page to images using pdf2pic, validates PNG output, and extracts text via Tesseract.js OCR with comprehensive error handling and validation.
- **Error Handling**: Centralized error handling and user-friendly error messages (e.g., bot-themed JavaScript alerts).
- **Data Flow**: Content is extracted, processed by AI, stored in PostgreSQL, and returned to the frontend.
- **Data Storage**: Stores processed content with source information, analysis results (summary, explanation, key points, red/green flags, human/environmental impact), and metadata (reading time, complexity level, processing status).
- **Legal Document Features**: Specializes in analyzing legal documents with Red Flag Detection, Green Flag Highlighting, Friendly Explanations, Real-world Implications, and Visual Indicators.
- **Investigative Journalism Approach**: Enhanced AI prompts to capture critical issues, concerns, and policy changes with detailed WHO, WHAT, WHY information.
- **Content Formatting**: Intelligent content formatter for structured JSON and Markdown, ensuring visual hierarchy and distinct content sections (e.g., "Why This Matters" vs "Learn The Concepts").
- **Vitalik Mode**: Sophisticated "VitalikMode" prompts for deep intellectual analysis, including multi-disciplinary frameworks and relevant technical analogies. Visual effects (floating Vitalik head) are implemented to indicate this mode.
- **Saved Content**: Users can save analyzed content to a personal library with full reading, downloading (JSON, Text, Markdown), and sharing capabilities. Includes notes, tags, favorites, and search/filter functionality.
- **Mobile UI**: Improved mobile experience with visible comprehension slider icons, mobile-friendly library access, and clear instructions for mobile workflows.
- **Enhanced Navigation**: "Simplify Another Page" button now smoothly scrolls users directly to the "Get Started" section, eliminating confusion and extra scrolling steps.
- **Study Buddy Improvements**: Fixed z-index layering issues ensuring AI responses appear above all UI elements including Reference Content boxes. Added mobile-accessible conversation history with "Chats" overlay. Text highlighting popup now reliably displays with proper Study Buddy integration.
- **Rate Limiting System**: Implemented 3 free analyses per day per user to manage OpenAI API costs. Users see real-time usage status with progress bars and clear messaging about daily limits.
- **Content Caching**: Smart caching system that reuses AI-processed content for identical URL and text inputs, reducing API costs while maintaining fast performance. File uploads are not cached for security.
- **Usage Status Display**: Real-time usage indicator showing current progress toward daily limit, remaining analyses, and reset time with user-friendly messaging.
- **Michael Meter™**: Custom Bitcoin-themed usage bar featuring Michael Saylor's real photo with animated laser eyes, rocket progress indicator, and dynamic milestone messages. Includes full Bitcoin orange color progression (#f7931a) and rotating collection of 16 authentic Saylor quotes at usage limits (including "Forever, Laura." and "People who use fiat currency as a store of value - we call them poor."). Quote rotation system changes every minute during testing for variety. Scaled to complement the complexity slider with compact, balanced proportions.
- **Comprehensive Legal Protection**: Iron-clad privacy policy and disclaimer system with clear "use at your own risk" messaging. Includes dedicated privacy page (/privacy) with detailed liability disclaimers, user responsibilities, AI limitations warnings, and third-party service information. Compact disclaimer notices appear prominently on all analysis result pages. Legal notices cover AI content accuracy warnings, user verification responsibilities, and complete liability disclaimers to protect from lawsuits.
- **Test Mode Feature**: Added dedicated test mode that allows processing new documents without consuming OpenAI API credits. Generates realistic analysis structures with content previews, document type detection, and simulated key points extraction. Includes three-way mode toggle (AI/Test/Demo) for flexible testing workflows. Test mode provides comprehensive analysis templates showing what full AI mode would include while preserving rate limits for production use.
- **PDF Upload Support**: Full PDF processing capability with robust error handling. System validates PDF format (magic number check), converts pages to images via pdf2pic (requires poppler_utils system dependency), validates PNG conversion output, and extracts text using Tesseract.js OCR. Includes consecutive failure tracking, graceful degradation for partially-readable PDFs, and clear error messages for password-protected or unreadable documents. Processing pipeline prevents crashes and provides detailed logging for debugging.

## External Dependencies

### AI Services
- **OpenAI API**: Utilizes GPT-4o for all content analysis, simplification, and educational enhancements. Configured via environment variables.

### Database
- **Neon Database**: PostgreSQL hosting service. Connection managed via `DATABASE_URL` environment variable, with schema management via Drizzle Kit.

### Third-Party Libraries
- **Tesseract.js**: Used for OCR processing to extract text from images and documents.
- **pdf2pic**: Converts PDF pages to PNG images for OCR processing. Requires poppler_utils system dependency.
- **Cheerio**: Utilized for server-side HTML parsing and web scraping.
- **Axios**: Employed as an HTTP client for making external API requests.

### System Dependencies
- **poppler_utils**: Required for PDF-to-image conversion via pdf2pic. Provides pdftoppm and related utilities for rendering PDF pages as images.

## Anonymous Access & Authentication System

### Anonymous User Support
The application now supports **anonymous access**, allowing users to try the platform before signing in:

**Anonymous User Features:**
- 3 free daily analyses without requiring sign-in
- Session-based usage tracking (resets daily)
- Full access to content processing (URL, file, text)
- Can browse pricing plans
- Redirected to login when attempting to purchase

**Protected Features (Require Authentication):**
- Library (saved content)
- Account management
- Study Buddy AI chat
- Checkout/payment flow
- Content saving functionality

**Technical Implementation:**
- `optionalAuthMiddleware`: Allows endpoints to accept both anonymous and authenticated requests
- Session-based rate limiting for anonymous users via `rateLimitService`
- Database-backed rate limiting for authenticated users
- Anonymous usage tracked via `req.session.anonymousUsage` with daily reset
- Authentication required at checkout - users redirected to login before payment

**Key Files:**
- `server/replitAuth.ts`: Optional authentication middleware
- `server/services/rateLimitService.ts`: Dual rate limiting (session + database)
- `client/src/pages/checkout.tsx`: Authentication gating for payment
- `client/src/App.tsx`: Route-level access control
- `client/src/components/SaylorUsageBar.tsx`: Anonymous user messaging

### User Authentication
Implemented via **Replit Auth** with Google OAuth integration:
- Login endpoint: `/api/login`
- Logout endpoint: `/api/logout`
- Current user endpoint: `/api/user`
- Session-based authentication with PostgreSQL session store

### Security Considerations
- API keys are properly managed through environment variables
- Session secrets should be randomly generated
- Stripe webhook secrets must be configured for production
- All sensitive credentials should never be committed to version control