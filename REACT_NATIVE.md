Deploying a React Native app built with Expo to app stores (Google Play Store for Android and Apple App Store for iOS) involves a few key steps. Expo simplifies the process significantly compared to bare React Native, as it handles much of the build configuration for you. Here’s a step-by-step guide to get your app live as of March 19, 2025:

---

### Prerequisites
1. **Expo Account**: Sign up for a free Expo account at [expo.dev](https://expo.dev).
2. **Project Setup**: Ensure your app is built with Expo (e.g., created via `npx create-expo-app my-app`).
3. **Testing**: Test your app locally using `expo start` on simulators/emulators or real devices via the Expo Go app.

---

### Step 1: Configure Your App
1. **Update `app.json`**:
   - Open `app.json` (or `app.config.js`) in your project root and configure key details:
     ```json
     {
       "expo": {
         "name": "My App",
         "slug": "my-app",
         "version": "1.0.0",
         "orientation": "portrait",
         "icon": "./assets/icon.png",
         "splash": {
           "image": "./assets/splash.png",
           "resizeMode": "contain",
           "backgroundColor": "#ffffff"
         },
         "android": {
           "package": "com.yourcompany.myapp",
           "versionCode": 1
         },
         "ios": {
           "bundleIdentifier": "com.yourcompany.myapp",
           "buildNumber": "1.0.0"
         }
       }
     }
     ```
   - **Slug**: Unique name for your app on Expo’s servers.
   - **Package/Bundle Identifier**: Unique reverse-domain name (e.g., `com.yourcompany.myapp`).
   - **Icon/Splash**: Prepare a 1024x1024 PNG icon and a splash image.

2. **Install Expo CLI** (if not already installed):
   ```bash
   npm install -g expo-cli
   ```
   Login with `expo login`.

---

### Step 2: Build Your App
Expo offers two build options: **EAS Build** (Expo Application Services, recommended) or the older **Classic Build**. EAS Build is the modern, supported method in 2025, so I’ll focus on that.

1. **Set Up EAS**:
   - Install EAS CLI:
     ```bash
     npm install -g eas-cli
     ```
   - Log in:
     ```bash
     eas login
     ```

2. **Configure EAS**:
   - Run:
     ```bash
     eas build:configure
     ```
   - This creates an `eas.json` file. Edit it to define build profiles:
     ```json
     {
       "build": {
         "release": {
           "android": {
             "buildType": "apk"
           },
           "ios": {
             "simulator": false
           }
         },
         "preview": {
           "android": {
             "buildType": "apk"
           },
           "ios": {
             "simulator": true
           }
         }
       }
     }
     ```

3. **Build for Android**:
   - Run:
     ```bash
     eas build --platform android
     ```
   - Choose `apk` (smaller, for testing) or `aab` (App Bundle, required for Google Play).
   - Expo will build it in the cloud and provide a download link (e.g., `.aab` file).

4. **Build for iOS**:
   - Run:
     ```bash
     eas build --platform ios
     ```
   - You’ll need an Apple Developer account ($99/year). Provide credentials when prompted (or configure them in `eas.json`).
   - Expo generates an `.ipa` file.

5. **Monitor Builds**:
   - Check build status with `eas build:list` or on your Expo dashboard.

---

### Step 3: Deploy to Google Play Store (Android)
1. **Google Play Console Account**:
   - Sign up at [play.google.com/console](https://play.google.com/console) ($25 one-time fee).
   - Create a new app under “All apps” > “Create app.”

2. **Upload Your Build**:
   - Go to “Release” > “Production” > “Create new release.”
   - Upload the `.aab` file from your EAS build.
   - Fill out release notes (e.g., “Initial release v1.0.0”).

3. **Store Listing**:
   - Complete the “Store listing” section:
     - App name, description, screenshots (use a device frame tool like Figma).
     - Upload icons (512x512 PNG) and feature graphics.
   - Set content rating and pricing (free or paid).

4. **Submit**:
   - Submit for review under “Publishing overview.” Approval typically takes 1-7 days.

---

### Step 4: Deploy to Apple App Store (iOS)
1. **Apple Developer Account**:
   - Enroll at [developer.apple.com](https://developer.apple.com) ($99/year).

2. **App Store Connect**:
   - Log in to [appstoreconnect.apple.com](https://appstoreconnect.apple.com).
   - Create a new app under “My Apps” > “+” > “New App.”
   - Use the same `bundleIdentifier` from `app.json`.

3. **Submit Build with EAS**:
   - After building the `.ipa`, submit it directly to App Store Connect:
     ```bash
     eas submit --platform ios
     ```
   - Alternatively, download the `.ipa` and upload it manually via Apple’s Transporter app.

4. **App Information**:
   - In App Store Connect, fill out:
     - App name, description, keywords, screenshots (use iPhone sizes like 6.5”).
     - Privacy policy URL (required, even a basic one works).
     - App category and version details.

5. **Submit for Review**:
   - Go to “Versions,” select your build, and submit. Apple’s review process takes 1-3 days typically.

---

### Step 5: Post-Deployment
- **Updates**: Increment `version` (iOS) or `versionCode` (Android) in `app.json`, rebuild, and resubmit via EAS.
- **Analytics**: Use Expo’s EAS Insights or integrate a tool like Firebase to track usage.
- **OTA Updates**: Push JavaScript updates without resubmitting by using `expo publish` (for minor changes).

---

### Tips
- **Test Builds**: Before submitting, test the `.apk` or `.ipa` locally:
  - Android: `eas build --platform android --profile preview` and sideload.
  - iOS: Use TestFlight after building and submitting to App Store Connect.
- **Credentials**: Store sensitive keys (e.g., Apple certificates) securely with EAS or locally.
- **Expo Free Tier**: Basic builds are free, but complex apps may require an EAS subscription ($29/month).

Let me know if you hit a snag or need help with a specific part of this process!

Let’s create a simple React Native app with Expo that runs on iOS and Android, featuring a bottom menu (tab navigation) with "Eat," "Fun," and "Shop" options, and a middle page with a splash screen displaying a "Welcome" message. I’ll guide you step-by-step using Visual Studio Code (VS Code) as your editor. This assumes you’re starting fresh and have Node.js installed.

---

### Step 1: Set Up Your Environment
1. **Install Node.js** (if not already installed):
   - Download from [nodejs.org](https://nodejs.org) (LTS version recommended).
   - Verify: `node -v` and `npm -v` in your terminal.

2. **Install Expo CLI**:
   ```bash
   npm install -g expo-cli
   ```

3. **Install VS Code** (if not already installed):
   - Download from [code.visualstudio.com](https://code.visualstudio.com).
   - Open VS Code after installation.

4. **Recommended VS Code Extensions**:
   - Install “Prettier” (for code formatting) and “ESLint” (for linting) from the Extensions marketplace (Ctrl+Shift+X or Cmd+Shift+X).

---

### Step 2: Create the Expo Project
1. **Create the App**:
   - Open a terminal (in VS Code: `Terminal > New Terminal`).
   - Run:
     ```bash
     npx create-expo-app SimpleApp
     cd SimpleApp
     ```

2. **Open in VS Code**:
   - In the terminal, type:
     ```bash
     code .
     ```
   - This opens the `SimpleApp` folder in VS Code.

3. **Install Navigation Dependencies**:
   - For bottom tabs, we’ll use `@react-navigation/native` and `@react-navigation/bottom-tabs`.
   - Run:
     ```bash
     npx expo install @react-navigation/native @react-navigation/bottom-tabs react-native-screens react-native-safe-area-context
     ```

---

### Step 3: Structure the App
1. **Clean Up Default Files**:
   - Open `App.js` and replace its content with a blank slate:
     ```javascript
     import React from 'react';
     import { View, Text } from 'react-native';

     export default function App() {
       return (
         <View>
           <Text>Hello, World!</Text>
         </View>
       );
     }
     ```

2. **Create Folders**:
   - In the Explorer (Ctrl+Shift+E or Cmd+Shift+E), right-click the root folder (`SimpleApp`):
     - New Folder: `screens`
     - New Folder: `components`

3. **Create Screen Files**:
   - Inside `screens/`, create:
     - `EatScreen.js`
     - `FunScreen.js`
     - `ShopScreen.js`
     - `SplashScreen.js`

---

### Step 4: Build the Screens
1. **SplashScreen.js** (Middle Welcome Page):
   ```javascript
   import React from 'react';
   import { View, Text, StyleSheet } from 'react-native';

   const SplashScreen = () => {
     return (
       <View style={styles.container}>
         <Text style={styles.text}>Welcome to SimpleApp!</Text>
       </View>
     );
   };

   const styles = StyleSheet.create({
     container: {
       flex: 1,
       justifyContent: 'center',
       alignItems: 'center',
       backgroundColor: '#f0f0f0',
     },
     text: {
       fontSize: 24,
       fontWeight: 'bold',
     },
   });

   export default SplashScreen;
   ```

2. **EatScreen.js**:
   ```javascript
   import React from 'react';
   import { View, Text, StyleSheet } from 'react-native';

   const EatScreen = () => {
     return (
       <View style={styles.container}>
         <Text style={styles.text}>Eat Screen</Text>
       </View>
     );
   };

   const styles = StyleSheet.create({
     container: {
       flex: 1,
       justifyContent: 'center',
       alignItems: 'center',
     },
     text: {
       fontSize: 20,
     },
   });

   export default EatScreen;
   ```

3. **FunScreen.js**:
   ```javascript
   import React from 'react';
   import { View, Text, StyleSheet } from 'react-native';

   const FunScreen = () => {
     return (
       <View style={styles.container}>
         <Text style={styles.text}>Fun Screen</Text>
       </View>
     );
   };

   const styles = StyleSheet.create({
     container: {
       flex: 1,
       justifyContent: 'center',
       alignItems: 'center',
     },
     text: {
       fontSize: 20,
     },
   });

   export default FunScreen;
   ```

4. **ShopScreen.js**:
   ```javascript
   import React from 'react';
   import { View, Text, StyleSheet } from 'react-native';

   const ShopScreen = () => {
     return (
       <View style={styles.container}>
         <Text style={styles.text}>Shop Screen</Text>
       </View>
     );
   };

   const styles = StyleSheet.create({
     container: {
       flex: 1,
       justifyContent: 'center',
       alignItems: 'center',
     },
     text: {
       fontSize: 20,
     },
   });

   export default ShopScreen;
   ```

---

### Step 5: Set Up Bottom Tab Navigation
1. **Update `App.js`**:
   - Replace its content with:
     ```javascript
     import React from 'react';
     import { NavigationContainer } from '@react-navigation/native';
     import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
     import EatScreen from './screens/EatScreen';
     import FunScreen from './screens/FunascendancyTabBarOptions = {
       tabBarActiveTintColor: '#00f',
       tabBarInactiveTintColor: '#999',
     };
     import SplashScreen from './screens/SplashScreen';
     import ShopScreen from './screens/ShopScreen';

     const Tab = createBottomTabNavigator();

     export default function App() {
       return (
         <NavigationContainer>
           <Tab.Navigator screenOptions={tabBarOptions}>
             <Tab.Screen name="Eat" component={EatScreen} />
             <Tab.Screen name="Fun" component={FunScreen} />
             <Tab.Screen name="Splash" component={SplashScreen} />
             <Tab.Screen name="Shop" component={ShopScreen} />
           </Tab.Navigator>
         </NavigationContainer>
       );
     }
     ```

   - This sets up a bottom tab navigator with four tabs: Eat, Fun, Splash (middle welcome screen), and Shop. The `tabBarOptions` customizes the tab bar colors.

---

### Step 6: Test the App
1. **Start the App**:
   - In the terminal (Ctrl+`` or Cmd+`` to open in VS Code), run:
     ```bash
     npx expo start
     ```

2. **Run on iOS/Android**:
   - **iOS**: Open the Expo Go app on an iPhone, scan the QR code from the terminal.
   - **Android**: Open Expo Go on an Android phone, scan the QR code.
   - Alternatively, use simulators:
     - iOS: Install Xcode (Mac only), then press `i` in the terminal.
     - Android: Install Android Studio, set up an emulator, then press `a`.

3. **Verify**:
   - You should see a bottom menu with "Eat," "Fun," "Splash," and "Shop."
   - Tapping "Splash" shows the welcome screen.

---

### Step 7: Customize (Optional)
- **Icons**: Add icons to tabs using `tabBarIcon` in `screenOptions` (requires an icon library like `react-native-vector-icons`).
- **Styles**: Adjust colors, fonts, or layout in the `StyleSheet` objects.

---

### Step 8: Prepare for App Store Deployment
Follow my previous response on deploying with Expo to the Google Play Store and Apple App Store. Update `app.json` with:
```json
{
  "expo": {
    "name": "SimpleApp",
    "slug": "simple-app",
    "version": "1.0.0",
    "icon": "./assets/icon.png",
    "splash": {
      "image": "./assets/splash.png",
      "resizeMode": "contain",
      "backgroundColor": "#ffffff"
    },
    "android": {
      "package": "com.yourname.simpleapp"
    },
    "ios": {
      "bundleIdentifier": "com.yourname.simpleapp"
    }
  }
}
```
- Add `icon.png` (1024x1024) and `splash.png` to an `assets/` folder.

---

That’s it! You’ve built a simple React Native app with a bottom menu and splash screen using VS Code. Let me know if you need help with any step or want to add features!