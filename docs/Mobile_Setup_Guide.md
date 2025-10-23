# 📱 Mobile Micro-SaaS Setup Guide

Complete step-by-step instructions for setting up your mobile SaaS starter kit.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Initial Setup](#initial-setup)
3. [Supabase Configuration](#supabase-configuration)
4. [Stripe Setup](#stripe-setup)
5. [Expo Configuration](#expo-configuration)
6. [Development](#development)
7. [Building for Production](#building-for-production)
8. [App Store & Play Store Submission](#app-store--play-store-submission)
9. [Troubleshooting](#troubleshooting)

---

## Prerequisites

Before you start, ensure you have:

- **Node.js** 18+ and npm/yarn installed
- **Expo CLI** - Install globally: `npm install -g expo-cli`
- **Xcode** (for iOS testing) - Download from App Store or [developer.apple.com](https://developer.apple.com)
- **Android Studio** (for Android testing) - Download from [developer.android.com](https://developer.android.com)
- **Supabase account** - Free at [supabase.com](https://supabase.com)
- **Stripe account** - Free at [stripe.com](https://stripe.com)
- **App Store Connect account** - $99/year Apple Developer Program
- **Google Play Console account** - $25 one-time Google Developer fee

---

## Initial Setup

### 1. Install Dependencies

```bash
npm install
```

This installs all required packages including:
- `expo` - React Native framework
- `react-native` - Core library
- `nativewind` - Tailwind for React Native
- `@supabase/supabase-js` - Database client
- `@stripe/stripe-react-native` - Stripe integration
- `expo-notifications` - Push notifications
- `expo-local-authentication` - Biometric auth
- Other utilities

### 2. Set Up Environment Variables

```bash
cp .env.example .env.local
```

Your `.env.local` should contain:

```
# Supabase
EXPO_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
EXPO_PUBLIC_SUPABASE_ANON_KEY=your-anon-key

# Stripe
EXPO_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_...
STRIPE_SECRET_KEY=sk_test_...

# App Config
EXPO_PUBLIC_APP_NAME=YourAppName
EXPO_PUBLIC_APP_ENV=development
```

**Note:** In Expo, environment variables must start with `EXPO_PUBLIC_` to be accessible in the app.

### 3. Update app.json

```json
{
  "expo": {
    "name": "Your App Name",
    "slug": "your-app-slug",
    "version": "1.0.0",
    "orientation": "portrait",
    "icon": "./assets/icon.png",
    "splash": {
      "image": "./assets/splash.png",
      "resizeMode": "contain",
      "backgroundColor": "#ffffff"
    },
    "scheme": "com.yourcompany.yourapp",
    "owner": "your-expo-username",
    "runtimeVersion": "1.0.0",
    "updates": {
      "url": "https://u.expo.dev/your-update-group-id"
    }
  }
}
```

---

## Supabase Configuration

### 1. Create a Supabase Project

1. Go to [supabase.com](https://supabase.com) and sign up
2. Click "New Project"
3. Choose your region (pick closest to your users)
4. Generate a strong password
5. Wait for provisioning (1-2 minutes)

### 2. Get Your Credentials

In Supabase Dashboard:
1. Go to **Settings** → **API**
2. Copy:
   - **Project URL** → `EXPO_PUBLIC_SUPABASE_URL`
   - **anon key** → `EXPO_PUBLIC_SUPABASE_ANON_KEY`
   - **service_role key** (keep secret, for backend only)

### 3. Create Database Schema

In Supabase, go to **SQL Editor** and run:

```sql
-- Users table (auto-created by Supabase Auth, but we extend it)
CREATE TABLE IF NOT EXISTS public.user_profiles (
  id UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,
  email TEXT UNIQUE NOT NULL,
  full_name TEXT,
  avatar_url TEXT,
  subscription_status TEXT DEFAULT 'free' -- free, active, cancelled, past_due
  subscription_tier TEXT DEFAULT 'free', -- free, pro, enterprise
  billing_cycle_start DATE,
  billing_cycle_end DATE,
  stripe_customer_id TEXT UNIQUE,
  stripe_subscription_id TEXT,
  created_at TIMESTAMP DEFAULT now(),
  updated_at TIMESTAMP DEFAULT now()
);

-- Products/Features table
CREATE TABLE IF NOT EXISTS public.products (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name TEXT NOT NULL,
  description TEXT,
  price_monthly INT, -- in cents
  price_annually INT,
  stripe_product_id TEXT,
  stripe_price_id_monthly TEXT,
  stripe_price_id_annual TEXT,
  features JSONB DEFAULT '[]'::jsonb,
  is_active BOOLEAN DEFAULT true,
  created_at TIMESTAMP DEFAULT now()
);

-- Subscriptions table
CREATE TABLE IF NOT EXISTS public.subscriptions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES public.user_profiles(id) ON DELETE CASCADE,
  product_id UUID NOT NULL REFERENCES public.products(id),
  stripe_subscription_id TEXT UNIQUE,
  status TEXT NOT NULL, -- active, past_due, cancelled, trialing
  current_period_start TIMESTAMP,
  current_period_end TIMESTAMP,
  cancel_at_period_end BOOLEAN DEFAULT false,
  canceled_at TIMESTAMP,
  created_at TIMESTAMP DEFAULT now(),
  updated_at TIMESTAMP DEFAULT now()
);

-- App data table (example for storing user-generated content)
CREATE TABLE IF NOT EXISTS public.user_data (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES public.user_profiles(id) ON DELETE CASCADE,
  data JSONB DEFAULT '{}'::jsonb,
  created_at TIMESTAMP DEFAULT now(),
  updated_at TIMESTAMP DEFAULT now()
);

-- Enable Row Level Security
ALTER TABLE public.user_profiles ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.subscriptions ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.user_data ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.products ENABLE ROW LEVEL SECURITY;

-- RLS Policies
CREATE POLICY "Users can view own profile" ON public.user_profiles
  FOR SELECT USING (auth.uid() = id);

CREATE POLICY "Users can update own profile" ON public.user_profiles
  FOR UPDATE USING (auth.uid() = id);

CREATE POLICY "Users can view own subscriptions" ON public.subscriptions
  FOR SELECT USING (auth.uid() = user_id);

CREATE POLICY "Users can view own data" ON public.user_data
  FOR SELECT USING (auth.uid() = user_id);

CREATE POLICY "Users can create own data" ON public.user_data
  FOR INSERT WITH CHECK (auth.uid() = user_id);

CREATE POLICY "Users can update own data" ON public.user_data
  FOR UPDATE USING (auth.uid() = user_id);

CREATE POLICY "Users can delete own data" ON public.user_data
  FOR DELETE USING (auth.uid() = user_id);

CREATE POLICY "Anyone can view products" ON public.products
  FOR SELECT USING (is_active = true);

-- Create indexes for performance
CREATE INDEX idx_user_profiles_email ON public.user_profiles(email);
CREATE INDEX idx_user_profiles_stripe_id ON public.user_profiles(stripe_customer_id);
CREATE INDEX idx_subscriptions_user_id ON public.subscriptions(user_id);
CREATE INDEX idx_user_data_user_id ON public.user_data(user_id);
```

### 4. Set Up Authentication

In Supabase Dashboard → **Authentication**:

1. Go to **Providers** and enable:
   - Email (enabled by default)
   - Google (optional - add OAuth credentials)
   - Apple (required for iOS App Store)

2. Go to **URL Configuration**:
   - Add your app's deep link: `com.yourcompany.yourapp://`
   - Add your web domain (for magic links)

3. Go to **Email Templates** and customize if desired

---

## Stripe Setup

### 1. Get Your API Keys

1. Go to [dashboard.stripe.com](https://dashboard.stripe.com)
2. Toggle "Test mode" (top right)
3. Go to **Developers** → **API keys**
4. Copy:
   - **Publishable key** → `EXPO_PUBLIC_STRIPE_PUBLISHABLE_KEY`
   - **Secret key** → `STRIPE_SECRET_KEY`

### 2. Create Products

In Stripe Dashboard → **Products**:

Create your pricing tiers. Example:

**Product: Pro Plan**
- Monthly: $9.99/month (stripe price id: `price_xxx`)
- Annual: $99.99/year (stripe price id: `price_yyy`)

**Product: Enterprise**
- Custom pricing (contact)

### 3. Set Up Webhooks

For production, set up webhook endpoints in Stripe:

1. Go to **Developers** → **Webhooks**
2. Add endpoint: `https://your-domain.com/api/webhooks/stripe`
3. Select events:
   - `customer.subscription.created`
   - `customer.subscription.updated`
   - `customer.subscription.deleted`
   - `invoice.payment_succeeded`
   - `invoice.payment_failed`

4. Copy webhook signing secret → `STRIPE_WEBHOOK_SECRET`

### 4. Add Products to Database

Insert your Stripe products into Supabase:

```sql
INSERT INTO public.products (name, description, price_monthly, price_annually, stripe_product_id, stripe_price_id_monthly, stripe_price_id_annual, features)
VALUES (
  'Pro',
  'Perfect for growing businesses',
  999, -- $9.99 in cents
  9999, -- $99.99 in cents
  'prod_xxx', -- from Stripe
  'price_monthly_xxx',
  'price_annual_xxx',
  '["Feature 1", "Feature 2", "Feature 3"]'::jsonb
);
```

---

## Expo Configuration

### 1. Create Expo Account

```bash
expo register
# or
expo login
```

### 2. Configure EAS Build

```bash
eas build:configure
```

This creates `eas.json`. Update it:

```json
{
  "build": {
    "preview": {
      "android": {
        "buildType": "apk"
      },
      "ios": {
        "buildType": "simulator"
      }
    },
    "preview2": {
      "android": {
        "buildType": "apk"
      },
      "ios": {
        "buildType": "simulator"
      }
    },
    "production": {},
    "development": {
      "developmentClient": true,
      "distribution": "internal"
    }
  },
  "submit": {
    "production": {}
  }
}
```

### 3. Set Up Local Keystore (Android)

Generate a keystore for signing Android apps:

```bash
eas credentials
# Follow prompts to generate keystore
```

---

## Development

### 1. Start Development Server

```bash
npm run dev
# or
expo start
```

This opens the Expo CLI menu.

### 2. Run on iOS Simulator

```bash
# Press 'i' in Expo CLI
# or
npm run ios
```

Requires Xcode and iOS simulator running.

### 3. Run on Android Emulator

```bash
# Press 'a' in Expo CLI
# or
npm run android
```

Requires Android Studio and emulator running.

### 4. Run on Physical Device

1. Download Expo Go app from App Store or Play Store
2. In Expo CLI, scan QR code with device camera
3. App opens in Expo Go

**For production testing**, use development client:

```bash
eas build --platform all --profile development
```

---

## Building for Production

### iOS Build

```bash
eas build --platform ios
```

Steps:
1. Choose "Release" profile
2. EAS builds your app (5-10 minutes)
3. Download `.ipa` file to test on TestFlight
4. Or submit directly to App Store

### Android Build

```bash
eas build --platform android
```

Steps:
1. Choose "Release" profile
2. EAS builds your app (5-10 minutes)
3. Download `.aab` (Android App Bundle)
4. Submit to Play Store

### Both Platforms

```bash
eas build --platform all
```

---

## App Store & Play Store Submission

### iOS App Store

1. **Create App Store Connect Record**
   - Go to [appstoreconnect.apple.com](https://appstoreconnect.apple.com)
   - Click "My Apps"
   - Click "+" → "New App"
   - Select "iOS"
   - Fill in bundle ID (`com.yourcompany.yourapp`)

2. **Fill in App Information**
   - App name
   - Subtitle
   - Description
   - Keywords
   - Support URL
   - Privacy Policy URL

3. **Add Screenshots**
   - iOS requires 3 screenshots minimum
   - iPad screenshots optional but recommended

4. **Set Pricing**
   - Choose tier or custom price
   - Set availability regions

5. **Build Version**
   - Go to "Builds"
   - Select your production build
   - Submit for review

6. **Wait for Review** (1-3 days typically)

### Google Play Store

1. **Create Google Play Console Record**
   - Go to [play.google.com/console](https://play.google.com/console)
   - Click "Create app"
   - Fill in app name and language
   - Accept declarations and save

2. **Fill in App Details**
   - App category
   - App rating questionnaire
   - Content rating form

3. **Add Screenshots**
   - Phone: 2-8 screenshots required
   - Tablet: Optional
   - Watch: Optional

4. **Set Pricing**
   - Free or paid
   - Regions & pricing tiers

5. **Upload Build**
   - Go to "Release" → "Production"
   - Upload your `.aab` file
   - Fill in release notes
   - Review content rating

6. **Publish**
   - Review final checklist
   - Click "Publish to Production"

7. **Wait for Review** (typically 2-4 hours)

---

## Troubleshooting

### "Can't connect to Supabase"

**Solution:**
- Check internet connection
- Verify `EXPO_PUBLIC_SUPABASE_URL` and `EXPO_PUBLIC_SUPABASE_ANON_KEY` in `.env.local`
- Ensure Supabase project is active
- Try `expo start -c` to clear cache

### "Stripe integration not working"

**Solution:**
- Verify `EXPO_PUBLIC_STRIPE_PUBLISHABLE_KEY` is correct
- Make sure you're using Test keys in development
- Check that Stripe SDK is properly initialized
- Use Stripe test card: `4242 4242 4242 4242`

### "Build failed on EAS"

**Solution:**
- Run `npm install` locally first
- Try `expo prebuild --clean` 
- Check build logs in EAS dashboard
- Ensure all dependencies are compatible

### "iOS simulator won't start"

**Solution:**
- Check Xcode is installed: `xcode-select --install`
- Try `xcrun simctl erase all` to reset simulator
- Restart Xcode
- Check iOS SDK version in Xcode preferences

### "Android emulator won't start"

**Solution:**
- Check Android Studio is installed properly
- Check virtualization is enabled in BIOS
- Try `emulator -list-avds` to see available emulators
- Create new emulator: **Tools → AVD Manager → Create Virtual Device**

### "App crashes on startup"

**Solution:**
- Check console: `npx expo-dev-client`
- Verify all environment variables are set
- Try building with `--profile development`
- Check for TypeScript errors: `npm run type-check`

---

## Cost Breakdown

| Service | Free Tier | Paid | Notes |
|---------|-----------|------|-------|
| **Expo** | Yes (50 builds/month) | $29/mo | Pay-as-you-go above free tier |
| **EAS Build** | 30min/month | $$ per min | Usually $1-5/month for indie |
| **Supabase** | 500k API calls | $25/mo | Shared with web version |
| **Stripe** | N/A | 2.9% + $0.30 | Per successful charge |
| **App Store** | N/A | $99/year | One-time, then annual |
| **Play Store** | N/A | $25 | One-time fee |
| **Push Notifications** | Free (Expo) | N/A | Included with Expo |

**Monthly Total: ~$50-80** (after app store fees paid)

---

## Next Steps

1. ✅ Set up Supabase and run schema
2. ✅ Configure Stripe and add products
3. ✅ Update `.env.local` with API keys
4. ✅ Run `npm run dev` and test locally
5. ✅ Build for iOS and Android
6. ✅ Submit to App Store and Play Store
7. ✅ Set up analytics and monitoring
8. ✅ Launch! 🚀

---

## Resources

- [Expo Documentation](https://docs.expo.dev)
- [Supabase React Native Guide](https://supabase.com/docs/guides/getting-started/quickstarts/react-native)
- [Stripe React Native Guide](https://stripe.com/docs/mobile/react-native)
- [EAS Build Guide](https://docs.expo.dev/build/introduction/)
- [React Native Docs](https://reactnative.dev/docs/getting-started)

---

**Questions? Reach out or check the Expo community forums! 🎉**
