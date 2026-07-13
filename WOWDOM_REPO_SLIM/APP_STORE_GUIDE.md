# WOWDOM™ APPS — ANDROID & iOS LAUNCH GUIDE
### What's in this kit, what Peter does, in order. Prepared 2026-07-13.

## WHAT'S ALREADY BUILT (in this folder)
- `wordeazy/` and `matheazy/` — complete Capacitor app projects, each containing:
  - `www/` — the game (with IAP module), PWA manifest, icons
  - `android/` — full Android Studio project (appId com.wowdom.wordeazy / .matheazy)
  - `ios/` — full Xcode project
  - `cordova-plugin-purchase` wired into both platforms (Google Play Billing + Apple StoreKit)
- `.github/workflows/` — automated cloud builds: **Android compiles on GitHub's Linux machines, iOS on GitHub's Mac machines** (this is how we build iOS with no Mac)
- IAP products coded in both games (dormant on web, active in apps):
  | Product ID | Type | Suggested price (YOU set it in the stores) |
  |---|---|---|
  | wowdom_hints_10 | consumable | $0.99 |
  | wowdom_hints_50 | consumable | $2.99 |
  | wowdom_remove_ads | non-consumable | $2.99 |
  In-game: WORDEAZY gets a 🛒 button + store opens when out of hints; MATHEAZY's Power Shop grows a "💎 REAL DEALS" section. Purchases grant hints instantly / set the no-ads flag.

## PETER'S CHECKLIST (in order)

### 1. GitHub (free, 10 min) — unlocks ALL builds
1. Create account at github.com → New repository → name `wowdom-apps` (private is fine)
2. Upload this folder's contents (GitHub Desktop app is easiest, or I drive the browser)
3. The Actions tab will start building automatically: Android APK/AAB + iOS compile check, for both games, every time the code changes

### 2. Google Play (when WOWdom LLC + D-U-N-S ready → ORGANIZATION account, $25 once)
1. play.google.com/console → create org account (needs the D-U-N-S)
2. Create both apps → upload the `app-release.aab` files from GitHub Actions artifacts
3. **Signing:** generate an upload keystore once (`keytool -genkey -v -keystore upload.keystore -alias wowdom -keyalg RSA -keysize 2048 -validity 10000`), add it to GitHub secrets (KEYSTORE_BASE64 = base64 of the file, plus KEYSTORE_PASSWORD, KEY_ALIAS, KEY_PASSWORD) — the workflow then signs automatically. Enroll in Play App Signing.
4. **IAP products:** Play Console → each app → Monetize → In-app products → create the three product IDs EXACTLY as in the table above, set prices
5. Store listing: descriptions/covers already exist (SUBMISSION_KIT.md + covers folder); privacy policy required — host PRIVACY_POLICY.md on wowdom.games
6. Pre-launch report tests on real devices automatically (solves the no-Android-device problem)

### 3. Apple App Store ($99/yr, needs LLC too for a clean org account)
1. developer.apple.com → enroll (organization enrollment also uses D-U-N-S — same number)
2. App Store Connect → create both apps (bundle IDs com.wowdom.wordeazy / com.wowdom.matheazy)
3. Certificates: create a Distribution certificate + provisioning profiles, add to GitHub secrets, and the iOS workflow gets extended to produce a signed IPA (standard fastlane setup — I'll do the workflow change when certs exist)
4. **IAP products:** App Store Connect → each app → In-App Purchases → same three product IDs, set prices
5. **TestFlight:** install on YOUR iPhone/iPad to test before release — this is how you personally test with no Mac
6. Review notes: Apple requires apps to feel native; our games are fullscreen, offline, touch-first — good position. If rejected for "web wrapper," we add haptics + Game Center (planned enhancement).

### IMPORTANT HONEST NOTES
- The IAP purchase flow can only be end-to-end tested against the stores' sandboxes (TestFlight / Play internal testing) — code is defensive and can't break the games, but expect one round of sandbox testing before release.
- Ads (AdMob) are NOT in the apps yet — "Remove Ads" is sold but currently there's nothing to remove; we add AdMob in the same session we first publish, so buyers get real value. Don't enable the remove_ads product in the stores until AdMob is in.
- Store prices are set by YOU in each console; the game displays whatever price the store returns.
