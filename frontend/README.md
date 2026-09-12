# FitFlow Mobile Application

React Native cross-platform mobile application for iOS and Android.

## Features

- 🤖 AI-powered personalized workout plans (TensorFlow Lite)
- 📊 Comprehensive workout logging and progress tracking
- 🏆 Social community with challenges and leaderboards
- 🍎 Smart nutrition tracking with camera-based food recognition (ML Kit)
- 🎯 Achievement system and streak tracking
- 🔒 Privacy-focused with on-device AI processing
- 📱 Offline-first architecture
- ♿ Accessibility support

## Prerequisites

- Node.js >= 18.0.0
- npm >= 9.0.0
- React Native CLI
- Xcode (for iOS development, macOS only)
- Android Studio (for Android development)
- CocoaPods (for iOS dependencies)

## Installation

```bash
# Install dependencies
npm install

# iOS only - Install CocoaPods dependencies
cd ios
pod install
cd ..
```

## Configuration

1. Create Firebase project at [Firebase Console](https://console.firebase.google.com/)
2. Enable Authentication, Firestore, and Storage
3. Download configuration files:
   - `google-services.json` → `android/app/`
   - `GoogleService-Info.plist` → `ios/`

4. Create `.env` file in root:
```env
API_URL=http://localhost:3000
FIREBASE_API_KEY=your_api_key_here
FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
FIREBASE_PROJECT_ID=your_project_id
```

## Running the App

### Development Mode

```bash
# Start Metro bundler
npm start

# Run on iOS simulator
npm run ios

# Run on Android emulator
npm run android

# Run on specific iOS device
npm run ios --device "iPhone 14 Pro"

# Run on specific Android device
npm run android --deviceId=<device_id>
```

### Production Build

#### iOS
```bash
cd ios
xcodebuild -workspace FitFlow.xcworkspace \
  -scheme FitFlow \
  -configuration Release \
  -archivePath build/FitFlow.xcarchive \
  archive
```

#### Android
```bash
cd android
./gradlew assembleRelease

# APK located at: android/app/build/outputs/apk/release/app-release.apk
```

## Testing

```bash
# Run all tests
npm test

# Watch mode
npm run test:watch

# Coverage report
npm run test:coverage

# E2E tests (iOS)
npm run e2e:ios

# E2E tests (Android)
npm run e2e:android
```

## Linting and Formatting

```bash
# Run linter
npm run lint

# Auto-fix linting issues
npm run lint:fix

# Format code
npm run format

# Type check
npm run tsc
```

## Project Structure

```
src/
├── components/       # Reusable UI components
│   ├── Button/
│   ├── Card/
│   ├── Input/
│   └── ...
├── screens/         # Application screens
│   ├── Home/
│   ├── Workout/
│   ├── Social/
│   ├── Nutrition/
│   └── Profile/
├── navigation/      # Navigation configuration
│   ├── RootNavigator.tsx
│   ├── AuthNavigator.tsx
│   └── MainNavigator.tsx
├── services/        # API and service integrations
│   ├── api/
│   ├── firebase/
│   ├── ai/
│   └── storage/
├── hooks/           # Custom React hooks
│   ├── useAuth.ts
│   ├── useWorkout.ts
│   └── ...
├── utils/           # Utility functions
│   ├── validation.ts
│   ├── formatters.ts
│   └── ...
├── assets/          # Static assets
│   ├── images/
│   ├── fonts/
│   └── icons/
├── types/           # TypeScript type definitions
└── App.tsx          # Root component
```

## Key Dependencies

- **React Native**: Cross-platform mobile framework
- **React Navigation**: Navigation library
- **Firebase**: Backend services (Auth, Firestore, Storage, Messaging)
- **TensorFlow.js**: AI/ML for on-device personalization
- **ML Kit**: Computer vision for food recognition
- **Redux Toolkit**: State management
- **Axios**: HTTP client
- **Formik & Yup**: Form handling and validation
- **React Native Chart Kit**: Data visualization

## Performance Optimization

- Lazy loading screens with React.lazy()
- Image optimization with FastImage
- Memoization with React.memo and useMemo
- FlatList for efficient scrolling
- Hermes JavaScript engine enabled

## Accessibility

- Screen reader support (VoiceOver, TalkBack)
- Proper accessibility labels and hints
- Keyboard navigation support
- High contrast mode support
- Adjustable text sizes

## Troubleshooting

### iOS Build Issues
```bash
# Clean build
cd ios
rm -rf Pods Podfile.lock
pod install
cd ..
npm start -- --reset-cache
```

### Android Build Issues
```bash
# Clean build
cd android
./gradlew clean
cd ..
npm start -- --reset-cache
```

### Metro Bundler Issues
```bash
# Reset cache
npm start -- --reset-cache

# Clear watchman
watchman watch-del-all
```

## Contributing

1. Create feature branch: `git checkout -b feature/your-feature`
2. Make changes and test thoroughly
3. Run linter and tests: `npm run lint && npm test`
4. Commit: `git commit -m 'Add feature'`
5. Push: `git push origin feature/your-feature`
6. Create Pull Request

## License

MIT License - see [LICENSE](../LICENSE) for details.

---

**Platform**: iOS 13+, Android 8.0+  
**Framework**: React Native 0.72+  
**Language**: TypeScript
