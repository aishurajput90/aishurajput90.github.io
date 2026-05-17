# 🐦 Sky Dasher — React Native Game

A polished, store-ready Flappy Bird-style mobile game built with **Expo + React Native**.

---

## ✨ Features

| Feature | Details |
|---|---|
| 🎮 Core gameplay | Gravity physics, tap-to-flap, increasing difficulty |
| 🎨 Visual polish | Animated stars, moon glow, gradient sky that shifts as you progress |
| 🐦 Animated bird | Flapping wings, motion trail, glow ring, angle rotation |
| 🪙 Coin system | Coins float in pipe gaps, bobbing 3D spin animation |
| 🏆 6 unlockable skins | Birdie, Rocket, Dragon, Alien, Angel, Ninja |
| 💫 Combo system | COMBO → ON FIRE → BLAZING → GODLIKE bonuses |
| 💾 Persistence | High score, coins, skins saved via AsyncStorage |
| 📳 Haptics | Light tap on jump, error on collision, success on combos |
| ⏸ Pause/Resume | Full pause screen with quit-to-menu |
| 🏅 Medals | Bronze → Silver → Gold → Trophy → Legendary |
| 📺 Screens | Start, Game, Pause, Game Over — all animated |
| 🌌 Dynamic sky | Transitions from night → purple → deep space |
| 💥 Screen shake | On collision with particle explosion |

---

## 🚀 Quick Start

### Prerequisites
- Node.js 18+
- Expo CLI: `npm install -g expo-cli`
- EAS CLI: `npm install -g eas-cli`
- Expo Go app on your phone (for testing)

### 1. Install dependencies
```bash
cd SkyDasher
npm install
```

### 2. Add placeholder assets
```bash
# Create a simple icon (200x200 PNG) at:
assets/icon.png
assets/adaptive-icon.png  # Android
assets/splash.png          # 1284x2778 recommended
assets/favicon.png         # 32x32 for web

# You can generate these with:
npx expo install expo-asset
# Or use any image editor / AI image tool
```

### 3. Run on your device
```bash
npx expo start
# Scan the QR code with Expo Go (iOS/Android)
```

### 4. Run on simulator/emulator
```bash
npm run android   # needs Android Studio + emulator
npm run ios       # needs Xcode (Mac only)
```

---

## 📦 Building for App Stores

### Setup EAS (one-time)
```bash
eas login             # log in with your Expo account
eas build:configure   # creates eas.json (already done)
```

### Update app.json
Edit `app.json` and change:
- `ios.bundleIdentifier` → e.g. `com.yourname.skydashergame`
- `android.package` → same format
- `extra.eas.projectId` → get this from `eas build:configure`

### Build for Android (Google Play)
```bash
# Production AAB (upload to Play Console)
eas build --platform android --profile production

# Internal APK for testing
eas build --platform android --profile preview
```

### Build for iOS (App Store)
```bash
# Requires Apple Developer account ($99/yr)
eas build --platform ios --profile production
```

### Submit to stores
```bash
# Android (needs google-play-key.json — see below)
eas submit --platform android

# iOS
eas submit --platform ios
```

---

## 🏪 Store Submission Checklist

### Both Stores
- [ ] App icon (1024x1024 PNG, no alpha for iOS)
- [ ] Screenshots (at least 3 per device size)
- [ ] Short description (80 chars)
- [ ] Full description
- [ ] Privacy policy URL (required for both stores)
- [ ] Content rating questionnaire

### Google Play
1. Create account at [play.google.com/console](https://play.google.com/console)
2. Pay $25 one-time registration fee
3. Create app → upload AAB → fill store listing
4. For `eas submit`: create a Service Account key in Google Cloud Console
   - Go to Google Play Console → Setup → API access
   - Link to Google Cloud project → Create Service Account
   - Download JSON key → save as `google-play-key.json`

### Apple App Store
1. Enroll at [developer.apple.com](https://developer.apple.com) ($99/year)
2. Create App ID in Certificates, IDs & Profiles
3. Create app in App Store Connect
4. EAS handles certificates and provisioning profiles automatically

---

## 🗂️ Project Structure

```
SkyDasher/
├── App.js                    # Root: screen routing & state
├── app.json                  # Expo + store config
├── eas.json                  # Build profiles
├── package.json
├── babel.config.js
├── assets/
│   ├── icon.png
│   ├── adaptive-icon.png
│   ├── splash.png
│   └── favicon.png
└── src/
    ├── constants/
    │   └── index.js          # Physics constants, skin definitions
    ├── hooks/
    │   ├── useStorage.js     # AsyncStorage persistence
    │   ├── useGameLoop.js    # Pure physics functions
    │   └── useHaptics.js     # Haptic feedback
    ├── components/
    │   ├── Background.js     # Stars, moon, sky gradient, ground
    │   ├── Bird.js           # Animated bird with trail & glow
    │   ├── Pipe.js           # Gradient pipes with caps
    │   ├── Coin.js           # Bobbing, spinning coin
    │   ├── HUD.js            # Score, coins, pause button
    │   ├── ComboFlash.js     # Combo notification
    │   └── ParticleSystem.js # Particle burst effects
    └── screens/
        ├── StartScreen.js    # Main menu + skin picker
        ├── GameScreen.js     # Core gameplay loop
        ├── GameOverScreen.js # Results + medals
        └── PauseScreen.js    # Pause overlay
```

---

## 🎮 Gameplay Tuning

Edit `src/constants/index.js` to adjust feel:

```js
export const GRAVITY = 0.5;          // Higher = faster fall
export const JUMP_VELOCITY = -10.5;  // More negative = higher jump
export const PIPE_SPEED_BASE = 3.0;  // Starting speed
export const GAP_MAX = 200;          // Starting gap size (pixels)
export const GAP_MIN = 135;          // Minimum gap at high scores
```

Difficulty scaling in `useGameLoop.js`:
```js
export function getSpeed(score) {
  return PIPE_SPEED_BASE + score * 0.07; // Speed increase per point
}
```

---

## 🪙 Adding New Skins

In `src/constants/index.js`, add to the `SKINS` array:
```js
{ id: 'cat', emoji: '🐱', name: 'Kitty', cost: 200, color: '#ff9ef7' },
```

The system handles unlocking, display, and storage automatically.

---

## 🔊 Adding Sound Effects (Optional)

```bash
npm install expo-av
```

Place MP3 files in `assets/sounds/` then in `GameScreen.js`:
```js
import { Audio } from 'expo-av';

const sound = await Audio.Sound.createAsync(require('../assets/sounds/flap.mp3'));
await sound.playAsync();
```

Recommended sounds:
- `flap.mp3` — on jump
- `point.mp3` — on score
- `hit.mp3` — on collision
- `coin.mp3` — on coin collect

---

## 📱 Tested On

- iOS 16+ (iPhone 12, 14, 15)
- Android 10+ (Pixel 6, Samsung Galaxy S22)
- Expo SDK 51

---

## 📄 License

MIT — free to publish, modify, and monetize.
# github.io
# github.io
