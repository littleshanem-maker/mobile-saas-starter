# 📱 Mobile Micro-SaaS Starter Kit

A complete, production-ready starter kit for launching mobile micro-SaaS products. **Fork, customize, and deploy in minutes.** Built on the same principles: minimal complexity, low costs, high profit margins.

## ✨ Features

- **React Native** - Cross-platform (iOS & Android) with single codebase
- **Expo** - Streamlined development and deployment
- **TypeScript** - Type-safe code
- **Supabase** - Auth & PostgreSQL database (shared backend)
- **Stripe** - Payment processing with webhooks
- **NativeWind** - TailwindCSS for React Native
- **Pre-built Screens** - Onboarding, auth, dashboard, billing
- **API Integration** - Shared backend with web version
- **Push Notifications** - Expo Notifications setup
- **Offline Support** - Local caching with AsyncStorage
- **Dark Mode** - Fully supported

## 🚀 Quick Start

```bash
# 1. Install dependencies
npm install

# 2. Install Expo CLI globally (if not already)
npm install -g expo-cli

# 3. Copy environment template
cp .env.example .env.local

# 4. Add your API keys (Supabase, Stripe)

# 5. Run on iOS simulator or Android emulator
npm run ios
# or
npm run android

# 6. Or run on physical device with Expo Go
expo start
```

## 📖 Full Setup Guide

See **[MOBILE_SETUP_GUIDE.md](./MOBILE_SETUP_GUIDE.md)** for complete setup instructions including:
- Expo configuration
- Supabase connection
- Stripe mobile integration
- iOS & Android build setup
- App Store & Play Store deployment

## 🗂️ Project Structure

```
├── app/                      # React Native screens (Expo Router)
│   ├── (auth)/              # Login/signup screens
│   ├── (app)/               # Protected screens
│   │   ├── dashboard.tsx    # Home screen
│   │   ├── billing.tsx      # Subscription management
│   │   └── settings.tsx     # User settings
│   ├── onboarding.tsx       # Onboarding flow
│   └── _layout.tsx          # Navigation setup
├── lib/                     # Utilities
│   ├── supabase.ts         # Supabase client
│   ├── stripe.ts           # Stripe integration
│   ├── storage.ts          # AsyncStorage helpers
│   └── api.ts              # API clients
├── components/             # Reusable React Native components
│   ├── Button.tsx
│   ├── Card.tsx
│   ├── Input.tsx
│   └── ...
├── types/                  # TypeScript definitions
├── hooks/                  # Custom React hooks
├── app.json               # Expo configuration
├── eas.json               # EAS Build configuration
├── .env.example           # Environment template
└── MOBILE_SETUP_GUIDE.md  # Detailed setup instructions
```

## 🎯 What's Pre-Built

### Screens
- ✅ Onboarding carousel with feature highlights
- ✅ Login/signup forms with email validation
- ✅ Dashboard with bottom tab navigation
- ✅ Billing & subscription management
- ✅ User settings & profile management
- ✅ Splash screen

### Backend Integration
- ✅ Supabase auth (email, social login ready)
- ✅ Secure token storage in device keychain
- ✅ Stripe integration for in-app purchases
- ✅ Webhook handlers (same as web version)
- ✅ Offline data persistence

### Native Features
- ✅ Push notifications (Expo Notifications)
- ✅ Local storage (AsyncStorage)
- ✅ Device camera access
- ✅ Share functionality
- ✅ Deep linking support

### UI/UX
- ✅ Dark mode by default
- ✅ Responsive design (all device sizes)
- ✅ NativeWind components
- ✅ Customizable brand colors
- ✅ Loading states & error handling
- ✅ Toast notifications

## 🛠️ Customization

### Change Brand Colors
Edit `app/globals.css` and `tailwind.config.js` in `nativewind.config.js`

### Add New Screens
Create a file in `/app` folder with Expo Router conventions

### Connect Supabase
Use the same database as your web version for shared data

### Enable Mobile Payments
Configure Stripe for mobile subscriptions with TestFlight & sandbox

### Push Notifications
Update Firebase credentials in `eas.json`

## 📦 Tech Stack

- **Frontend:** React Native, Expo, TypeScript
- **Styling:** NativeWind, TailwindCSS
- **Backend:** Supabase (PostgreSQL, Auth) - **shared with web**
- **Payments:** Stripe (with mobile SDKs)
- **Build & Deploy:** EAS Build, App Store & Play Store
- **Package Manager:** npm/yarn
- **State Management:** React Context or Zustand

## 💰 Pricing Philosophy (Same as Web)

Keep costs under **$200/month** with 90%+ profit margins:
- **Vercel:** ~$20/month (web backend & API routes)
- **Supabase:** ~$50/month (shared database)
- **Stripe:** 2.9% + $0.30 per transaction
- **EAS Build:** Free tier available (~$1-5/month for additional builds)
- **App Store/Play Store:** One-time fees ($99 Apple, $25 Google)
- **Domain:** ~$10/year
- **Push Notifications:** Free (Expo's free tier)

**Total:** ~$80-120/month ongoing (after initial app store fees)

## 📱 Build & Deploy

### iOS (TestFlight)
```bash
eas build --platform ios
# Then upload to TestFlight via EAS dashboard
```

### Android (Play Store)
```bash
eas build --platform android
# Submit to Play Store via Google Play Console
```

### Development
```bash
# Preview builds for testing
eas build --platform all --profile preview

# Preview on physical device
expo start --dev-client
```

See **[MOBILE_SETUP_GUIDE.md](./MOBILE_SETUP_GUIDE.md)** for detailed deployment instructions.

## 📚 Learning Resources

- [Expo Docs](https://docs.expo.dev)
- [React Native Docs](https://reactnative.dev/docs/getting-started)
- [Supabase Docs](https://supabase.com/docs)
- [Stripe Mobile Docs](https://stripe.com/docs/mobile)
- [NativeWind Docs](https://www.nativewind.dev)
- [EAS Build Docs](https://docs.expo.dev/build/introduction/)

## 🆘 Troubleshooting

1. **Can't connect to Supabase?** → Check `.env.local` keys and network
2. **Stripe not working?** → Verify Stripe keys in .env and device has internet
3. **Build errors?** → Run `npm install` and `expo prebuild` again
4. **TypeScript errors?** → Run `npm run type-check`
5. **Simulator not loading?** → Try `expo start -c` (clear cache)

## ✅ Pre-Launch Checklist

- [ ] Update app name and colors in `app.json`
- [ ] Configure Supabase (same as web version)
- [ ] Configure Stripe (add mobile-specific product IDs)
- [ ] Test login/signup flow on device
- [ ] Test in-app purchases on TestFlight/Play Store Internal Test
- [ ] Update app description and screenshots
- [ ] Set up App Store Connect & Google Play Console accounts
- [ ] Build & submit to App Store (1-3 days approval)
- [ ] Build & submit to Play Store (2-4 hours approval)
- [ ] Enable push notifications
- [ ] Set up analytics

## 📦 What's NOT Included (But Easy to Add)

- **Email sending** - Resend API (same backend as web)
- **Analytics** - PostHog or Mixpanel SDK
- **In-app chat** - Firebase Cloud Messaging + UI
- **File uploads** - Supabase Storage
- **Maps** - React Native Maps
- **Video** - Expo Video or react-native-video
- **Biometric auth** - Expo Local Authentication

## 🌐 Sync With Web Version

This kit is designed to share the **same Supabase backend** as your web version:
- Single database
- Unified user management
- Shared API routes
- Cross-platform data sync
- Cost efficient

Mobile users can seamlessly switch between web and mobile with same account.

## 💬 Support

- 📖 Read the [MOBILE_SETUP_GUIDE.md](./MOBILE_SETUP_GUIDE.md)
- 📋 Check the docs for each tool above
- 🛠 Debug using Expo DevTools and console

## 📄 License

This project is open source and available under the MIT license.

---

**Ready to launch your mobile SaaS? Let's go! 🚀**

Questions? Read [MOBILE_SETUP_GUIDE.md](./MOBILE_SETUP_GUIDE.md) for complete documentation.

---

## 🎯 Key Differences from Web Version

| Aspect | Web | Mobile |
|--------|-----|--------|
| Framework | Next.js 14 | React Native + Expo |
| Styling | TailwindCSS | NativeWind |
| Deploy | Vercel | App Store & Play Store |
| Build | npm run build | eas build |
| Authentication | Supabase Auth | Supabase Auth (same) |
| Database | PostgreSQL | PostgreSQL (same) |
| Payments | Stripe Web | Stripe Mobile SDKs |
| Push Notifications | Not included | Expo Notifications |
| Offline | Not included | AsyncStorage |
| Cost | ~$50-70/month | ~$30-50/month extra |

---

**Shared Infrastructure = Lower Costs & Faster Development**
