# Mobile Micro-SaaS Starter Kit - Project Structure & Key Files

## Complete File Structure

```
mobile-saas-starter/
├── app/                                    # Expo Router screens
│   ├── _layout.tsx                        # Root navigation layout
│   ├── index.tsx                          # Splash/Loading screen
│   ├── onboarding/
│   │   ├── _layout.tsx                    # Onboarding stack
│   │   ├── welcome.tsx                    # Welcome screen
│   │   ├── features.tsx                   # Feature showcase
│   │   └── signup-prompt.tsx              # Call to action
│   ├── (auth)/                            # Auth group
│   │   ├── _layout.tsx                    # Auth stack
│   │   ├── login.tsx                      # Login screen
│   │   ├── signup.tsx                     # Signup screen
│   │   ├── forgot-password.tsx            # Password reset
│   │   └── verify-email.tsx               # Email verification
│   ├── (app)/                             # Protected app screens
│   │   ├── _layout.tsx                    # Bottom tab navigation
│   │   ├── dashboard.tsx                  # Home screen
│   │   ├── billing.tsx                    # Subscription management
│   │   ├── settings.tsx                   # User settings
│   │   └── profile.tsx                    # User profile
│   └── +html.ts                           # Web support (optional)
│
├── components/                            # Reusable UI components
│   ├── Button.tsx                         # Pressable button with variants
│   ├── Input.tsx                          # TextInput wrapper
│   ├── Card.tsx                           # Container component
│   ├── Text.tsx                           # Text wrapper for consistency
│   ├── Spinner.tsx                        # Loading spinner
│   ├── Toast.tsx                          # Toast notifications
│   ├── SafeArea.tsx                       # Safe area wrapper
│   ├── Divider.tsx                        # Separator line
│   ├── Badge.tsx                          # Status badge
│   └── BottomSheet.tsx                    # Modal bottom sheet
│
├── lib/                                   # Utilities and clients
│   ├── supabase.ts                        # Supabase client instance
│   ├── stripe.ts                          # Stripe initialization
│   ├── storage.ts                         # AsyncStorage helpers
│   ├── api.ts                             # API request helpers
│   ├── auth.ts                            # Auth utilities
│   ├── notifications.ts                   # Push notification setup
│   ├── validation.ts                      # Form validation
│   └── constants.ts                       # App constants
│
├── hooks/                                 # Custom React hooks
│   ├── useAuth.ts                         # Authentication hook
│   ├── useUser.ts                         # User profile hook
│   ├── useSubscription.ts                 # Subscription status hook
│   ├── useStorage.ts                      # Local storage hook
│   ├── useBilling.ts                      # Billing utilities
│   └── useNotifications.ts                # Notifications hook
│
├── types/                                 # TypeScript definitions
│   ├── index.ts                           # Main types export
│   ├── auth.ts                            # Auth types
│   ├── user.ts                            # User types
│   ├── subscription.ts                    # Subscription types
│   └── api.ts                             # API response types
│
├── context/                               # React context providers
│   ├── AuthContext.tsx                    # Auth state context
│   ├── UserContext.tsx                    # User state context
│   └── ThemeContext.tsx                   # Theme/dark mode context
│
├── styles/                                # Global styles
│   └── globals.css                        # TailwindCSS + NativeWind
│
├── assets/                                # Images and icons
│   ├── icon.png                           # App icon
│   ├── splash.png                         # Splash screen
│   ├── images/                            # Feature images
│   └── icons/                             # SVG icons
│
├── public/                                # Static assets
│   └── favicon.ico                        # Web favicon (if web enabled)
│
├── .env.example                           # Environment variables template
├── .env.local                             # Local env vars (git ignored)
├── app.json                               # Expo configuration
├── eas.json                               # EAS Build config
├── tsconfig.json                          # TypeScript config
├── tailwind.config.js                     # TailwindCSS config
├── nativewind.config.js                   # NativeWind config
├── package.json                           # Dependencies
├── package-lock.json                      # Dependency lock file
├── README.md                              # Main documentation
├── MOBILE_SETUP_GUIDE.md                  # Setup instructions
└── DEVELOPMENT.md                         # Development guide
```

---

## Key Files with Examples

### 1. `app.json` - Expo Configuration

```json
{
  "expo": {
    "name": "My SaaS App",
    "slug": "my-saas-app",
    "version": "1.0.0",
    "orientation": "portrait",
    "icon": "./assets/icon.png",
    "userInterfaceStyle": "automatic",
    "splash": {
      "image": "./assets/splash.png",
      "resizeMode": "contain",
      "backgroundColor": "#ffffff"
    },
    "assetBundlePatterns": [
      "**/*"
    ],
    "scheme": "com.mysaas.app",
    "owner": "your-expo-username",
    "ios": {
      "supportsTabletMode": true,
      "bundleIdentifier": "com.mysaas.app",
      "infoPlist": {
        "NSCameraUsageDescription": "This app needs camera access",
        "NSPhotoLibraryUsageDescription": "This app needs photo library access"
      }
    },
    "android": {
      "adaptiveIcon": {
        "foregroundImage": "./assets/adaptive-icon.png",
        "backgroundColor": "#ffffff"
      },
      "package": "com.mysaas.app",
      "permissions": [
        "android.permission.CAMERA",
        "android.permission.READ_EXTERNAL_STORAGE"
      ]
    },
    "web": {
      "favicon": "./assets/favicon.ico"
    },
    "plugins": [
      [
        "expo-notifications",
        {
          "icon": "./assets/notification-icon.png",
          "color": "#ffffff"
        }
      ]
    ],
    "runtimeVersion": {
      "policy": "appVersion"
    },
    "updates": {
      "url": "https://u.expo.dev/your-update-group-id",
      "fallbackToCacheTimeout": 0,
      "checkOnLaunch": "always"
    }
  }
}
```

### 2. `.env.example`

```
# Supabase Configuration
EXPO_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
EXPO_PUBLIC_SUPABASE_ANON_KEY=your-anon-public-key

# Stripe Configuration (Test Keys)
EXPO_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_...
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...

# App Configuration
EXPO_PUBLIC_APP_NAME=My SaaS App
EXPO_PUBLIC_APP_ENV=development
EXPO_PUBLIC_API_URL=http://localhost:3000

# Firebase Cloud Messaging (Optional - for push notifications)
EXPO_PUBLIC_FCM_PROJECT_ID=your-project-id
```

### 3. `lib/supabase.ts` - Supabase Client

```typescript
import { createClient } from '@supabase/supabase-js';
import AsyncStorage from '@react-native-async-storage/async-storage';
import * as SecureStore from 'expo-secure-store';

const supabaseUrl = process.env.EXPO_PUBLIC_SUPABASE_URL;
const supabaseAnonKey = process.env.EXPO_PUBLIC_SUPABASE_ANON_KEY;

if (!supabaseUrl || !supabaseAnonKey) {
  throw new Error('Missing Supabase environment variables');
}

export const supabase = createClient(supabaseUrl, supabaseAnonKey, {
  auth: {
    storage: SecureStore,
    autoRefreshToken: true,
    persistSession: true,
    detectSessionInUrl: false,
  },
});
```

### 4. `lib/stripe.ts` - Stripe Setup

```typescript
import { initStripe } from '@stripe/stripe-react-native';

const publishableKey = process.env.EXPO_PUBLIC_STRIPE_PUBLISHABLE_KEY;

if (!publishableKey) {
  throw new Error('Missing Stripe publishable key');
}

export const initializeStripe = async () => {
  await initStripe({
    publishableKey,
    merchantIdentifier: 'merchant.com.mysaas.app',
  });
};
```

### 5. `hooks/useAuth.ts` - Auth Hook

```typescript
import { useState, useEffect } from 'react';
import { useRouter } from 'expo-router';
import { supabase } from '@/lib/supabase';
import type { Session } from '@supabase/supabase-js';

export const useAuth = () => {
  const [session, setSession] = useState<Session | null>(null);
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);
  const router = useRouter();

  useEffect(() => {
    const getSession = async () => {
      try {
        const { data, error } = await supabase.auth.getSession();
        if (error) throw error;
        setSession(data.session);
        setUser(data.session?.user ?? null);
      } catch (err) {
        setError(err instanceof Error ? err.message : 'Auth error');
      } finally {
        setLoading(false);
      }
    };

    getSession();

    const { data } = supabase.auth.onAuthStateChange((_event, session) => {
      setSession(session);
      setUser(session?.user ?? null);
    });

    return () => data.subscription.unsubscribe();
  }, []);

  const signUp = async (email: string, password: string) => {
    try {
      setLoading(true);
      const { error } = await supabase.auth.signUp({ email, password });
      if (error) throw error;
      router.push('/auth/verify-email');
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Signup error');
    } finally {
      setLoading(false);
    }
  };

  const signIn = async (email: string, password: string) => {
    try {
      setLoading(true);
      const { error } = await supabase.auth.signInWithPassword({ email, password });
      if (error) throw error;
      router.replace('/(app)/dashboard');
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Login error');
    } finally {
      setLoading(false);
    }
  };

  const signOut = async () => {
    try {
      await supabase.auth.signOut();
      router.replace('/onboarding/welcome');
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Logout error');
    }
  };

  return { session, user, loading, error, signUp, signIn, signOut };
};
```

### 6. `components/Button.tsx` - Reusable Button

```typescript
import React from 'react';
import { Pressable, Text, StyleSheet, ActivityIndicator } from 'react-native';
import { cn } from '@/lib/utils';

interface ButtonProps {
  onPress: () => void;
  title: string;
  variant?: 'primary' | 'secondary' | 'outline';
  size?: 'sm' | 'md' | 'lg';
  loading?: boolean;
  disabled?: boolean;
  className?: string;
}

export const Button = React.forwardRef<any, ButtonProps>(
  ({ onPress, title, variant = 'primary', size = 'md', loading = false, disabled = false, className }, ref) => {
    const baseClass = 'rounded-lg font-semibold items-center justify-center';
    
    const variants = {
      primary: 'bg-blue-600',
      secondary: 'bg-gray-600',
      outline: 'border border-gray-300',
    };

    const sizes = {
      sm: 'px-3 py-2',
      md: 'px-4 py-3',
      lg: 'px-6 py-4',
    };

    return (
      <Pressable
        ref={ref}
        onPress={onPress}
        disabled={disabled || loading}
        className={cn(baseClass, variants[variant], sizes[size], className, {
          'opacity-50': disabled || loading,
        })}
      >
        {loading ? (
          <ActivityIndicator color="white" />
        ) : (
          <Text className="text-white text-base font-semibold">{title}</Text>
        )}
      </Pressable>
    );
  }
);
```

### 7. `app/(app)/dashboard.tsx` - Main Dashboard Screen

```typescript
import React, { useEffect, useState } from 'react';
import { View, Text, ScrollView } from 'react-native';
import { SafeAreaView } from 'react-native-safe-area-context';
import { useAuth } from '@/hooks/useAuth';
import { useSubscription } from '@/hooks/useSubscription';
import { Button } from '@/components/Button';
import { Card } from '@/components/Card';

export default function DashboardScreen() {
  const { user, signOut } = useAuth();
  const { subscription, tier } = useSubscription();

  return (
    <SafeAreaView className="flex-1 bg-white dark:bg-gray-900">
      <ScrollView className="flex-1 p-4">
        {/* Header */}
        <View className="mb-6">
          <Text className="text-3xl font-bold dark:text-white">Welcome back!</Text>
          <Text className="text-gray-600 dark:text-gray-400">{user?.email}</Text>
        </View>

        {/* Subscription Card */}
        <Card className="mb-4 p-4 bg-blue-50 dark:bg-blue-900">
          <Text className="text-lg font-semibold dark:text-white mb-2">
            Current Plan: {tier}
          </Text>
          <Text className="text-gray-700 dark:text-gray-300 mb-4">
            Status: {subscription?.status}
          </Text>
          <Button
            title="Upgrade Plan"
            variant="primary"
            onPress={() => {}}
          />
        </Card>

        {/* Quick Actions */}
        <View className="gap-3">
          <Button
            title="View Billing"
            variant="secondary"
            onPress={() => {}}
          />
          <Button
            title="Settings"
            variant="outline"
            onPress={() => {}}
          />
          <Button
            title="Sign Out"
            variant="outline"
            onPress={signOut}
          />
        </View>
      </ScrollView>
    </SafeAreaView>
  );
}
```

### 8. `types/index.ts` - Type Definitions

```typescript
export interface User {
  id: string;
  email: string;
  full_name?: string;
  avatar_url?: string;
}

export interface UserProfile extends User {
  subscription_status: 'free' | 'active' | 'cancelled' | 'past_due';
  subscription_tier: 'free' | 'pro' | 'enterprise';
  stripe_customer_id?: string;
  stripe_subscription_id?: string;
  billing_cycle_start?: string;
  billing_cycle_end?: string;
  created_at: string;
  updated_at: string;
}

export interface Product {
  id: string;
  name: string;
  description?: string;
  price_monthly: number;
  price_annually: number;
  stripe_product_id: string;
  stripe_price_id_monthly: string;
  stripe_price_id_annual: string;
  features: string[];
  is_active: boolean;
}

export interface Subscription {
  id: string;
  user_id: string;
  product_id: string;
  stripe_subscription_id: string;
  status: 'active' | 'past_due' | 'cancelled' | 'trialing';
  current_period_start: string;
  current_period_end: string;
  cancel_at_period_end: boolean;
}
```

### 9. `tailwind.config.js`

```javascript
module.exports = {
  content: ['./app/**/*.{js,jsx,ts,tsx}', './components/**/*.{js,jsx,ts,tsx}'],
  theme: {
    extend: {
      colors: {
        primary: '#3B82F6',
        secondary: '#6B7280',
      },
      spacing: {
        'safe': 'max(16px, env(safe-area-inset-bottom))',
      },
    },
  },
  plugins: [],
};
```

### 10. `package.json` - Dependencies

```json
{
  "name": "mobile-saas-starter",
  "version": "1.0.0",
  "main": "expo-router/entry",
  "scripts": {
    "start": "expo start",
    "dev": "expo start",
    "ios": "expo start --ios",
    "android": "expo start --android",
    "web": "expo start --web",
    "prebuild": "expo prebuild",
    "build:ios": "eas build --platform ios",
    "build:android": "eas build --platform android",
    "build": "eas build",
    "submit": "eas submit",
    "type-check": "tsc --noEmit"
  },
  "dependencies": {
    "expo": "latest",
    "expo-router": "latest",
    "expo-notifications": "latest",
    "expo-secure-store": "latest",
    "expo-local-authentication": "latest",
    "expo-dev-client": "latest",
    "react": "latest",
    "react-native": "latest",
    "react-native-safe-area-context": "latest",
    "react-native-screens": "latest",
    "@react-native-async-storage/async-storage": "latest",
    "@supabase/supabase-js": "latest",
    "@stripe/stripe-react-native": "latest",
    "nativewind": "latest",
    "tailwindcss": "latest",
    "clsx": "latest",
    "class-variance-authority": "latest"
  },
  "devDependencies": {
    "@types/react": "latest",
    "@types/react-native": "latest",
    "typescript": "latest"
  },
  "private": true
}
```

---

## Implementation Order

1. **Setup Phase**
   - Clone kit
   - Install dependencies
   - Configure `.env.local`

2. **Backend Phase**
   - Set up Supabase project
   - Run database schema
   - Configure auth

3. **Auth Phase**
   - Build auth screens
   - Test sign up/login
   - Implement secure token storage

4. **Core Features**
   - Build dashboard
   - Implement core features
   - Add offline support

5. **Payments Phase**
   - Configure Stripe
   - Build billing screens
   - Test payment flows

6. **Polish Phase**
   - Add animations
   - Implement analytics
   - Dark mode refinement

7. **Build Phase**
   - Create app icons & splash
   - Build for iOS and Android
   - Submit to app stores

---

## Best Practices

### Code Organization
- Keep components small and focused
- Use TypeScript strictly
- Organize by feature, not by type
- Reuse hooks for logic

### Performance
- Use `React.memo()` for expensive components
- Implement FlatList for long lists
- Optimize images with `expo-image`
- Lazy load screens

### Security
- Store tokens in Secure Store (not AsyncStorage)
- Validate user input
- Use RLS policies in Supabase
- Never expose secret keys

### Testing
- Unit test hooks and utilities
- Integration test user flows
- Test on real devices before launch
- Use TestFlight and Play Store beta

---

This structure provides everything needed to ship a professional, scalable mobile SaaS product!
