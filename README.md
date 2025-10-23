# 📱 Mobile Micro-SaaS Starter Kit

> A complete, production-ready starter kit for launching mobile micro-SaaS products in days, not weeks. Built on React Native + Expo with shared Supabase backend. Ship faster, launch cheaper.

![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-Production%20Ready-brightgreen)
![Version](https://img.shields.io/badge/version-1.0.0-blue)

## ✨ Features

- **React Native + Expo** - Cross-platform iOS & Android with single codebase
- **TypeScript** - 100% type-safe code
- **Supabase** - Shared PostgreSQL database + authentication
- **Stripe** - Pre-configured payment processing
- **NativeWind** - TailwindCSS for React Native
- **Dark Mode** - Fully supported
- **Push Notifications** - Expo Notifications setup
- **Offline Support** - AsyncStorage + Secure Store
- **Pre-built Screens** - Auth, dashboard, billing, settings
- **Production Ready** - Everything configured and tested

## 🚀 Quick Start

```bash
# 1. Clone this repository
git clone https://github.com/yourusername/mobile-saas-starter.git
cd mobile-saas-starter

# 2. Install dependencies
npm install

# 3. Setup environment variables
cp .env.example .env.local
# Add your Supabase and Stripe keys

# 4. Run on simulator
npm run ios    # for iOS
npm run android # for Android

# Or run on physical device with Expo Go
expo start
```

## 📖 Documentation

All documentation is included in the `/docs` folder:

| Document | Purpose | Read Time |
|----------|---------|-----------|
| [README_START_HERE.md](./docs/README_START_HERE.md) | Orientation & executive summary | 5 min |
| [Mobile_SaaS_Starter_Kit.md](./docs/Mobile_SaaS_Starter_Kit.md) | Features & overview | 10 min |
| [MOBILE_SETUP_GUIDE.md](./docs/MOBILE_SETUP_GUIDE.md) | Complete step-by-step setup | 30 min |
| [Mobile_Project_Structure.md](./docs/Mobile_Project_Structure.md) | File structure & code examples | 20 min |
| [WEB_vs_MOBILE_COMPARISON.md](./docs/WEB_vs_MOBILE_COMPARISON.md) | Platform strategy guide | 15 min |
| [QUICK_REFERENCE.md](./docs/QUICK_REFERENCE.md) | Cheat sheet & quick commands | 5 min |
| [DOCUMENTATION_MAP.md](./docs/DOCUMENTATION_MAP.md) | Navigation guide for docs | 5 min |

**Start here:** [README_START_HERE.md](./docs/README_START_HERE.md)

## 🎯 What's Included

### Pre-Built Screens
- ✅ Onboarding carousel
- ✅ Email/password authentication
- ✅ Dashboard with navigation
- ✅ Subscription management
- ✅ User settings & profile
- ✅ Billing management

### Backend Integration
- ✅ Supabase authentication
- ✅ PostgreSQL database (with schema)
- ✅ Row-level security policies
- ✅ Stripe payment processing
- ✅ Webhook handlers
- ✅ Real-time subscriptions

### Developer Experience
- ✅ TypeScript throughout
- ✅ Hot reload development
- ✅ Pre-configured tooling
- ✅ Clear project structure
- ✅ Best practices included

## 📱 Tech Stack

| Layer | Technology | Version |
|-------|-----------|---------|
| **Frontend** | React Native | Latest |
| **Framework** | Expo | Latest |
| **Styling** | NativeWind + TailwindCSS | Latest |
| **Language** | TypeScript | 5.x |
| **Database** | Supabase (PostgreSQL) | Latest |
| **Auth** | Supabase Auth | Built-in |
| **Payments** | Stripe | Latest |
| **Build** | EAS Build | Latest |
| **Package Manager** | npm/yarn | Latest |

## 💰 Pricing

### Monthly Costs (One Platform)

| Service | Cost | Notes |
|---------|------|-------|
| Supabase | $50 | Generous free tier available |
| EAS Build | $5 | Can build 200/month free |
| Stripe | 2.9% + $0.30 | Per transaction only |
| **Total** | **~$55/month** | |

### One-Time Costs

- Apple Developer Account: $99
- Google Play Account: $25

### Combined Web + Mobile

If you also use the [Web Starter Kit](https://github.com/yourusername/web-saas-starter):

```
Supabase:     $50  (shared)
Vercel:       $20  (web)
EAS Build:    $5   (mobile)
─────────────────────────
Total:        ~$75/month
```

**Break-even:** 3 paying users at $29/month

## 🗂️ Project Structure

```
mobile-saas-starter/
├── app/                          # Expo Router screens
│   ├── _layout.tsx              # Root navigation
│   ├── (auth)/                  # Auth screens
│   ├── (app)/                   # Protected app screens
│   └── onboarding/              # Onboarding flow
├── lib/                         # Utilities
│   ├── supabase.ts             # Supabase client
│   ├── stripe.ts               # Stripe setup
│   └── api.ts                  # API helpers
├── components/                 # Reusable components
├── hooks/                      # Custom React hooks
├── types/                      # TypeScript definitions
├── docs/                       # Documentation
├── .env.example               # Environment template
├── app.json                   # Expo config
└── package.json               # Dependencies
```

## 🔧 Configuration

### Environment Variables

Create `.env.local`:

```bash
# Supabase
EXPO_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
EXPO_PUBLIC_SUPABASE_ANON_KEY=your-anon-key

# Stripe
EXPO_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_...
STRIPE_SECRET_KEY=sk_test_...

# App Config
EXPO_PUBLIC_APP_NAME=Your App Name
EXPO_PUBLIC_APP_ENV=development
```

### Supabase Setup

1. Create project at [supabase.com](https://supabase.com)
2. Run SQL schema (see [MOBILE_SETUP_GUIDE.md](./docs/MOBILE_SETUP_GUIDE.md))
3. Configure authentication
4. Add API keys to `.env.local`

### Stripe Setup

1. Create account at [stripe.com](https://stripe.com)
2. Add products for your pricing tiers
3. Add API keys to `.env.local`
4. Configure webhooks (production only)

## 🚀 Deployment

### Development

```bash
# Expo Go (easiest - test on physical device)
expo start

# iOS Simulator
npm run ios

# Android Emulator
npm run android
```

### Production Builds

```bash
# Build for both platforms
eas build --platform all

# Or build individually
eas build --platform ios
eas build --platform android
```

### App Store Submission

```bash
# Submit to both stores
eas submit --platform all
```

See [MOBILE_SETUP_GUIDE.md](./docs/MOBILE_SETUP_GUIDE.md) for detailed deployment instructions.

## 📊 Timeline

- **Setup:** 1-2 hours
- **Customization:** 2-3 days
- **Testing:** 1-2 days
- **App Store Submission:** 1-5 days for approval
- **Total to Launch:** 1-2 weeks

## 🔐 Security

- ✅ Token storage in Secure Store (not AsyncStorage)
- ✅ Supabase Row-Level Security policies included
- ✅ Environment variables for sensitive data
- ✅ HTTPS/SSL by default
- ✅ Input validation included

## 🐛 Troubleshooting

### "Can't connect to Supabase"
- Check internet connection
- Verify `EXPO_PUBLIC_SUPABASE_URL` and keys in `.env.local`
- Try `expo start -c` to clear cache

### "Stripe not working"
- Verify `EXPO_PUBLIC_STRIPE_PUBLISHABLE_KEY`
- Use test card: `4242 4242 4242 4242`
- Check webhook configuration

### "Build fails"
- Run `npm install` again
- Try `expo prebuild --clean`
- Check build logs in EAS dashboard

See [QUICK_REFERENCE.md](./docs/QUICK_REFERENCE.md) for more troubleshooting.

## 📚 Learning Resources

- [Expo Documentation](https://docs.expo.dev)
- [React Native Docs](https://reactnative.dev)
- [Supabase Guides](https://supabase.com/docs)
- [Stripe Mobile Docs](https://stripe.com/docs/mobile)
- [NativeWind Docs](https://www.nativewind.dev)
- [TypeScript Handbook](https://www.typescriptlang.org/docs)

## 🤝 Contributing

Contributions welcome! Please feel free to submit a Pull Request.

## 📄 License

MIT License - see LICENSE file for details

## 🙋 FAQ

**Q: Can I use this with the web starter kit?**  
A: Yes! They share the same Supabase backend. Perfect for maximizing code reuse.

**Q: Do I need Xcode/Android Studio?**  
A: For development yes, but you can use Expo Go app for testing on physical devices without building.

**Q: Can I add my own features?**  
A: Absolutely! This is just the foundation. Add whatever you need.

**Q: Is this production ready?**  
A: Yes, but always test thoroughly and add your own security measures as needed.

**Q: Can I modify the UI?**  
A: Yes, all styling uses TailwindCSS/NativeWind which are easy to customize.

**Q: What about web version?**  
A: Check out our [Web Micro-SaaS Starter Kit](https://github.com/yourusername/web-saas-starter)

## 🎓 Best Practices Included

- ✅ TypeScript for type safety
- ✅ Component-based architecture
- ✅ Custom hooks for logic reuse
- ✅ Environment variable management
- ✅ Error handling patterns
- ✅ Loading states
- ✅ Dark mode support
- ✅ Responsive design
- ✅ Code organization
- ✅ Security best practices

## 🚀 Next Steps

1. Clone the repository
2. Read [README_START_HERE.md](./docs/README_START_HERE.md)
3. Follow [MOBILE_SETUP_GUIDE.md](./docs/MOBILE_SETUP_GUIDE.md)
4. Customize for your product
5. Launch! 🎉

## 💬 Support

- 📖 Check the [documentation](./docs/)
- 🐛 Check [QUICK_REFERENCE.md](./docs/QUICK_REFERENCE.md) troubleshooting
- 💡 Check [GitHub Issues](https://github.com/yourusername/mobile-saas-starter/issues)
- 🌐 Join [Indie Hackers](https://www.indiehackers.com)

## 👨‍💻 Author

Built with ❤️ for indie hackers and bootstrapped founders.

---

**Ready to launch your mobile SaaS?** Star ⭐ this repo and get started!

**[Read the docs →](./docs/README_START_HERE.md)**
