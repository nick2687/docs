# Email/SMS Communications Setup Guide

This guide will walk you through setting up email and SMS communication capabilities in HomeLens.

## Prerequisites

- Firebase project already set up
- Firebase CLI installed (`firebase --version`)
- Node.js 18+ installed

## Step 1: Create Mailgun Account

1. Go to [https://www.mailgun.com](https://www.mailgun.com)
2. Sign up for a free account
3. Complete email verification

### Get API Key
1. Navigate to Settings → API Keys
2. Copy your **Private API key**
3. Save it somewhere secure

### Set Up Domain
1. Navigate to Sending → Domains
2. Choose to use the sandbox domain (for testing) or add your own domain
3. If using your own domain:
   - Click "Add New Domain"
   - Enter your domain (e.g., `mg.yourcompany.com`)
   - Follow DNS configuration instructions
   - Wait for verification (can take up to 48 hours)

**Sandbox Domain**: Good for testing, limited to 300 emails/day to authorized recipients only
**Custom Domain**: Required for production, no sending limits with free tier (10k/month)

### Authorize Recipients (Sandbox Only)
If using sandbox domain:
1. Navigate to Sending → Sending Domains → [Your Sandbox]
2. Click "Authorized Recipients"
3. Add your test email addresses

## Step 2: Create Plivo Account

1. Go to [https://www.plivo.com](https://www.plivo.com)
2. Sign up for a free trial account
3. Complete phone verification

### Get Credentials
1. Navigate to Overview dashboard
2. Note your **Auth ID** and **Auth Token**
3. Save them somewhere secure

### Get a Phone Number
1. Navigate to Phone Numbers → Buy Numbers
2. Select your country (USA recommended)
3. Choose a number type:
   - **Standard** ($0.50/month) - Best for most uses
   - **Toll-free** ($2/month) - Professional appearance
4. Purchase the number
5. Note your **phone number** (e.g., +14155551234)

## Step 3: Configure Cloud Functions

### Option A: Using .env File (Development)

1. Navigate to functions directory:
   ```bash
   cd web/functions
   ```

2. Create `.env` file:
   ```bash
   touch .env
   ```

3. Add your credentials:
   ```bash
   # Mailgun Configuration
   MAILGUN_API_KEY=key-1234567890abcdef1234567890abcdef
   MAILGUN_DOMAIN=sandboxXXXXXXXX.mailgun.org

   # Plivo Configuration
   PLIVO_AUTH_ID=MAXXXXXXXXXXXXXXXXXX
   PLIVO_AUTH_TOKEN=YourAuthTokenHere123456789
   PLIVO_PHONE_NUMBER=+14155551234
   ```

4. **Important**: `.env` is already in `.gitignore` - never commit this file!

### Option B: Using Firebase Config (Production)

Set config via Firebase CLI:
```bash
firebase functions:config:set \
  mailgun.api_key="key-1234567890abcdef1234567890abcdef" \
  mailgun.domain="sandboxXXXXXXXX.mailgun.org" \
  plivo.auth_id="MAXXXXXXXXXXXXXXXXXX" \
  plivo.auth_token="YourAuthTokenHere123456789" \
  plivo.phone_number="+14155551234"
```

View current config:
```bash
firebase functions:config:get
```

## Step 4: Install Dependencies

```bash
cd web/functions
npm install
```

This will install:
- `firebase-functions` - Cloud Functions SDK
- `firebase-admin` - Admin SDK for Firestore
- `mailgun.js` - Mailgun SDK
- `form-data` - Required by Mailgun
- `plivo` - Plivo SDK

## Step 5: Build and Test Functions

### Build TypeScript
```bash
npm run build
```

Expected output:
```
> build
> tsc
```

No errors means success!

### Test Locally (Optional)

1. Start Firebase emulators:
   ```bash
   firebase emulators:start --only functions
   ```

2. Functions will be available at:
   ```
   http://localhost:5001/swiftinspect-4b885/us-central1/sendEmail
   http://localhost:5001/swiftinspect-4b885/us-central1/sendSms
   ```

## Step 6: Deploy to Firebase

### Deploy Functions
```bash
cd web
firebase deploy --only functions
```

This will:
1. Build TypeScript code
2. Upload functions to Firebase
3. Configure Cloud Functions

Expected output:
```
✔  functions[sendEmail(us-central1)] Successful create operation.
✔  functions[sendSms(us-central1)] Successful create operation.
✔  functions[sendBulkEmail(us-central1)] Successful create operation.
```

### Deploy Firestore Indexes
```bash
firebase deploy --only firestore:indexes
```

This creates indexes for:
- Communications by user and contact
- Communications by type and status
- Email/SMS templates by category

## Step 7: Update Firestore Security Rules

Edit `web/firestore.rules` and add:

```javascript
match /communications/{commId} {
  allow read, write: if request.auth != null && 
    request.auth.uid == resource.data.userId;
}

match /emailTemplates/{templateId} {
  allow read, write: if request.auth != null && 
    request.auth.uid == resource.data.userId;
}

match /smsTemplates/{templateId} {
  allow read, write: if request.auth != null && 
    request.auth.uid == resource.data.userId;
}
```

Deploy rules:
```bash
firebase deploy --only firestore:rules
```

## Step 8: Test in Production

### Test Email Sending

1. Log into your app
2. Go to Contacts
3. Create or edit a contact
4. Check "Email Consent"
5. Add a valid email address
6. Save the contact
7. Use the send email feature (when UI is complete)

### Test SMS Sending

1. Create or edit a contact
2. Check "SMS Consent"
3. Add a phone number in E.164 format (e.g., +14155551234)
4. Save the contact
5. Use the send SMS feature (when UI is complete)

### Verify in Dashboards

**Mailgun**:
1. Go to Sending → Logs
2. See all sent emails
3. Check delivery status

**Plivo**:
1. Go to Messaging → SMS History
2. See all sent SMS
3. Check delivery status

**Firestore**:
1. Open Firebase Console
2. Navigate to Firestore Database
3. Check `communications` collection
4. Verify records are created

## Troubleshooting

### "Configuration missing" Error

**Problem**: Cloud Functions can't find API keys

**Solution**:
1. Verify environment variables are set:
   ```bash
   firebase functions:config:get
   ```
2. Redeploy functions:
   ```bash
   firebase deploy --only functions
   ```

### Mailgun "Unauthorized" Error

**Problem**: Invalid API key or domain

**Solutions**:
1. Verify API key is correct (should start with `key-`)
2. Verify domain is correct
3. Check domain is verified in Mailgun dashboard
4. Ensure using **Private API key**, not public

### Plivo "Authentication Failed"

**Problem**: Invalid Auth ID or Token

**Solutions**:
1. Verify Auth ID (starts with `MA`)
2. Verify Auth Token
3. Check credentials in Plivo dashboard (Overview)

### SMS "Invalid phone number"

**Problem**: Phone number format is wrong

**Solution**: Use E.164 format:
- ✅ Correct: `+14155551234`
- ❌ Wrong: `4155551234`, `(415) 555-1234`, `415-555-1234`

### Email to Sandbox Domain Rejected

**Problem**: Email address not authorized

**Solution**: 
1. Go to Mailgun → Sending → Domains → Sandbox
2. Add recipient to "Authorized Recipients"
3. Recipient must confirm via email
4. Or use custom verified domain

### Functions Deployment Fails

**Problem**: Build errors or permission issues

**Solutions**:
1. Check for TypeScript errors:
   ```bash
   cd web/functions
   npm run build
   ```
2. Verify you're authenticated:
   ```bash
   firebase login
   ```
3. Check you're using correct project:
   ```bash
   firebase use swiftinspect-4b885
   ```

### "Index not found" Error

**Problem**: Firestore indexes not deployed

**Solution**:
```bash
firebase deploy --only firestore:indexes
```

Wait 5-10 minutes for indexes to build.

## Cost Management

### Free Tier Limits

**Mailgun**:
- 10,000 emails/month FREE
- After: $35/month for up to 50k emails

**Plivo**:
- No free tier (pay-as-you-go)
- $0.0055 per SMS
- $0.50-$2/month per phone number

**Firebase Cloud Functions**:
- 2 million invocations/month FREE
- 400,000 GB-seconds compute time FREE
- After: $0.40 per million invocations

### Monitoring Usage

**Mailgun**:
- Dashboard → Analytics
- Set up usage alerts

**Plivo**:
- Dashboard → Account → Usage
- Set spending limits

**Firebase**:
- Console → Usage tab
- Set budget alerts

## Security Best Practices

1. **Never commit API keys** - Already in `.gitignore`
2. **Use different keys for dev/prod** - Create separate Mailgun/Plivo accounts
3. **Rotate keys regularly** - Change keys every 90 days
4. **Monitor logs** - Check for suspicious activity
5. **Limit permissions** - Use least-privilege principle

## Next Steps

1. Complete frontend UI components (SendEmailModal, SendSmsModal)
2. Create template management interface
3. Seed default email/SMS templates
4. Add communication history view
5. Test end-to-end workflow

## Support Resources

- **Mailgun Docs**: https://documentation.mailgun.com
- **Plivo Docs**: https://www.plivo.com/docs/
- **Firebase Functions**: https://firebase.google.com/docs/functions
- **HomeLens Implementation**: See `web/COMMUNICATIONS_IMPLEMENTATION.md`

## Quick Reference

### Required Values
- Mailgun API Key (starts with `key-`)
- Mailgun Domain (e.g., `sandboxXXXXXX.mailgun.org`)
- Plivo Auth ID (starts with `MA`)
- Plivo Auth Token (20+ characters)
- Plivo Phone Number (E.164 format: `+14155551234`)

### Key Commands
```bash
# Install dependencies
cd web/functions && npm install

# Build functions
npm run build

# Deploy functions
firebase deploy --only functions

# Deploy indexes
firebase deploy --only firestore:indexes

# View logs
firebase functions:log

# Check config
firebase functions:config:get
```

