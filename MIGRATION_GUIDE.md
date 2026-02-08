# Database Migration Guide for Replit Auth

## Important: Database Schema Update Required

The StudySimplify application has been updated to support individual user authentication using Replit Auth. This requires updating the database schema, which involves changing the user ID type from `serial (integer)` to `varchar (string)`.

## What Changed?

1. **User ID Type**: Changed from `serial` to `varchar` to support Replit Auth's string-based user IDs
2. **User Fields**: Removed `username` and `password`, added `firstName`, `lastName`, and `profileImageUrl`
3. **Sessions Table**: Added new table for storing user sessions
4. **All Foreign Keys**: Updated all `userId` references to use `varchar` instead of `integer`

## Migration Options

Since this is beta testing with shared data (userId = 1), you have two options:

### Option 1: Fresh Start (Recommended for Beta)

This will DELETE all existing data and create new tables:

```sql
-- WARNING: This will delete ALL data!
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

After running the above SQL, run:
```bash
npm run db:push
```

When prompted about `first_name`, select: **"+ first_name create column"** (press Enter)

### Option 2: Manual Migration (Preserve Data)

If you want to preserve existing data, you'll need to manually migrate:

**This is complex and not recommended for beta testing!** The fresh start is easier.

## Post-Migration Steps

1. **Verify Tables**: Check that all tables were created successfully
2. **Test Authentication**: Visit the app and sign in with Replit Auth
3. **Verify User Creation**: Check that your user profile is created automatically

## What to Expect

After the migration:
- Each beta tester will have their own individual account
- Login required to access the application
- Each user has their own library, saved content, and usage limits
- Credits and subscriptions are per-user
- Email addresses are collected automatically for future mailing list
