# 📱 Chutuki Assistant - Complete Installation Guide

## 🎯 Quick Start (5 Minutes)

### Step 1: Extract Project
```bash
# Extract the ZIP file to your preferred location
unzip ChutukiAssistant.zip
cd ChutukiAssistant
```

### Step 2: Open in Android Studio
1. Launch Android Studio
2. Click **File** → **Open**
3. Select the `ChutukiAssistant` folder
4. Click **OK** and wait for Gradle sync

### Step 3: Configure API Keys (Optional for testing)
Create `local.properties` in project root:

```properties
# Minimum required for basic functionality
PICOVOICE_ACCESS_KEY=demo_key_for_testing

# Optional - Add when you get your keys
WEATHER_API_KEY=your_key_here
NEWS_API_KEY=your_key_here
```

### Step 4: Build & Install
```bash
# Connect Android device with USB debugging enabled
./gradlew installDebug

# Or build APK manually
./gradlew assembleDebug
# APK will be in: app/build/outputs/apk/debug/app-debug.apk
```

---

## 🔑 Complete API Setup (Full Features)

### 1. Picovoice (Wake Word Detection)
**Cost**: FREE (up to 1 device)
**Time**: 5 minutes

1. Go to: https://picovoice.ai/
2. Sign up for free account
3. Navigate to **Console** → **Access Keys**
4. Copy your Access Key
5. Paste in `local.properties`:
   ```
   PICOVOICE_ACCESS_KEY=YOUR_KEY_HERE
   ```

**Custom Wake Words** (Optional):
1. In Picovoice Console, go to **Porcupine**
2. Click **Train Custom Wake Word**
3. Enter "Chutuki" and generate
4. Download `.ppn` file
5. Place in: `app/src/main/assets/wakewords/chutuki.ppn`

---

### 2. OpenWeatherMap (Weather Data)
**Cost**: FREE (60 calls/min)
**Time**: 3 minutes

1. Go to: https://openweathermap.org/api
2. Sign up for free account
3. Navigate to **API Keys** tab
4. Copy the default API key
5. Paste in `local.properties`:
   ```
   WEATHER_API_KEY=YOUR_KEY_HERE
   ```

---

### 3. News API (News & Headlines)
**Cost**: FREE (100 requests/day for developers)
**Time**: 2 minutes

1. Go to: https://newsapi.org/
2. Click **Get API Key**
3. Sign up and verify email
4. Copy API Key from dashboard
5. Paste in `local.properties`:
   ```
   NEWS_API_KEY=YOUR_KEY_HERE
   ```

---

### 4. Google Cloud APIs (Advanced Features)
**Cost**: FREE tier available (generous limits)
**Time**: 15 minutes

#### Setup Google Cloud Project:
1. Go to: https://console.cloud.google.com/
2. Create new project: "Chutuki Assistant"
3. Enable billing (free tier won't be charged)

#### Enable Required APIs:
Click **APIs & Services** → **Library** and enable:
- ✅ Cloud Speech-to-Text API
- ✅ Cloud Text-to-Speech API  
- ✅ Google Custom Search JSON API
- ✅ YouTube Data API v3
- ✅ Google Maps API
- ✅ Google Calendar API

#### Create API Keys:
1. Go to **APIs & Services** → **Credentials**
2. Click **+ CREATE CREDENTIALS** → **API Key**
3. Copy the generated key
4. Repeat for each API or use same key

#### Add to `local.properties`:
```properties
GOOGLE_CLOUD_API_KEY=YOUR_KEY_HERE
YOUTUBE_API_KEY=YOUR_KEY_HERE
GOOGLE_MAPS_API_KEY=YOUR_KEY_HERE
GOOGLE_SEARCH_API_KEY=YOUR_KEY_HERE
GOOGLE_SEARCH_ENGINE_ID=YOUR_SEARCH_ENGINE_ID
```

#### For Google Custom Search Engine ID:
1. Go to: https://programmablesearchengine.google.com/
2. Click **Add** to create new search engine
3. Enter "Entire Web" and create
4. Copy **Search Engine ID**

---

## 📥 Download Speech Models

### Vosk Offline Speech Recognition

#### Hindi Model (Recommended):
```bash
# Download from: https://alphacephei.com/vosk/models
wget https://alphacephei.com/vosk/models/vosk-model-small-hi-0.22.zip

# Extract
unzip vosk-model-small-hi-0.22.zip

# Move to project
mv vosk-model-small-hi-0.22 ChutukiAssistant/app/src/main/assets/model-vosk-model-small-hi-0.22/
```

#### English Model (Optional):
```bash
wget https://alphacephei.com/vosk/models/vosk-model-small-en-us-0.15.zip
unzip vosk-model-small-en-us-0.15.zip
mv vosk-model-small-en-us-0.15 ChutukiAssistant/app/src/main/assets/model-vosk-model-small-en-us-0.15/
```

---

## 🏗️ Building the Project

### Option 1: Debug Build (Testing)
```bash
# From project root
./gradlew assembleDebug

# Install directly to connected device
./gradlew installDebug

# APK location
app/build/outputs/apk/debug/app-debug.apk
```

### Option 2: Release Build (Production)

#### Generate Keystore:
```bash
keytool -genkey -v -keystore chutuki-release.keystore \
  -alias chutuki -keyalg RSA -keysize 2048 -validity 10000
```

#### Add to `local.properties`:
```properties
RELEASE_STORE_FILE=../chutuki-release.keystore
RELEASE_STORE_PASSWORD=your_password
RELEASE_KEY_ALIAS=chutuki
RELEASE_KEY_PASSWORD=your_password
```

#### Build Release APK:
```bash
./gradlew assembleRelease

# APK location
app/build/outputs/apk/release/app-release.apk
```

---

## 📲 Installation on Device

### Method 1: Direct Install (USB)
```bash
# Enable USB debugging on phone
# Settings → About Phone → Tap Build Number 7 times
# Settings → Developer Options → Enable USB Debugging

# Connect phone to computer
adb devices

# Install APK
adb install -r app/build/outputs/apk/debug/app-debug.apk
```

### Method 2: Transfer & Install
1. Transfer APK to phone via:
   - USB cable (copy to Download folder)
   - Email attachment
   - Cloud drive (Google Drive, Dropbox)
   - Bluetooth
   
2. On phone:
   - Open **Files** app
   - Navigate to APK location
   - Tap APK file
   - Allow "Install from Unknown Sources" if prompted
   - Click **Install**

---

## ⚙️ First-Time Setup on Phone

### 1. Grant Required Permissions

When you first open Chutuki, it will request:

✅ **Microphone** - For voice input
✅ **Phone** - For making calls
✅ **SMS** - For sending messages  
✅ **Contacts** - For contact access
✅ **Location** - For location-based features
✅ **Calendar** - For event management
✅ **Camera** - For visual features
✅ **Notifications** - For alerts

**Grant ALL permissions** for full functionality.

### 2. Enable Accessibility Service

1. App will prompt: **"Enable Accessibility Service"**
2. Click **Settings**
3. Find **Chutuki Assistant** in accessibility list
4. Toggle **ON**
5. Click **Allow** in popup

This enables:
- Lock/unlock screen
- Control system settings
- Open/close apps
- Adjust volume
- Take screenshots

### 3. Enable Notification Access

1. Go to **Settings** → **Apps** → **Chutuki Assistant**
2. Click **Notifications** → **Notification Listener**
3. Enable **Chutuki Assistant**

This allows reading and responding to notifications.

### 4. Set as Device Administrator

1. Go to **Settings** → **Security** → **Device Administrators**
2. Enable **Chutuki Assistant**

This enables screen lock/unlock functionality.

### 5. Disable Battery Optimization

1. Go to **Settings** → **Battery** → **Battery Optimization**
2. Find **Chutuki Assistant**
3. Select **Don't optimize**

This ensures the app runs in background without being killed.

### 6. Voice Authentication Setup

First launch will guide you through:

1. **Record your voice**:
   - Say "Chutuki" 5 times
   - Say "Hey Chutuki" 5 times
   - Say "Jarvis" 5 times

2. **Set preferences**:
   - Your name
   - Preferred language (Hindi/English/Both)
   - Wake word sensitivity
   - Voice personality

---

## 🎤 Testing Voice Commands

### Basic Test Commands:

```
✅ "Chutuki" (wake word - should respond with beep)
✅ "Battery kitni hai?" (battery level)
✅ "Time kya hua hai?" (current time)
✅ "Mausam kaisa hai?" (weather)
✅ "Volume badha do" (increase volume)
✅ "WiFi on karo" (turn on WiFi)
```

### Advanced Commands:

```
✅ "Mummy ko call karo"
✅ "WhatsApp kholo"
✅ "Screenshot le lo"
✅ "Music chala do"
✅ "YouTube pe trending kya hai?"
✅ "Kal subah 7 baje alarm set karo"
```

---

## 🐛 Troubleshooting

### Issue: Wake Word Not Detecting

**Solutions**:
1. Check microphone permission is granted
2. Speak clearly and slightly louder
3. Reduce background noise
4. In app settings, increase wake word sensitivity
5. Verify Picovoice API key is correct

### Issue: "Vosk model not found"

**Solutions**:
1. Verify model is in correct location:
   `app/src/main/assets/model-vosk-model-small-hi-0.22/`
2. Check folder name matches exactly
3. Ensure model files are not corrupted (re-download if needed)
4. Rebuild project: `./gradlew clean build`

### Issue: Speech recognition not working

**Solutions**:
1. Check microphone permission
2. Test microphone in another app
3. Switch to online mode in settings
4. Check internet connection (for online mode)
5. Verify Google Cloud API key (for online mode)

### Issue: App crashes on launch

**Solutions**:
1. Clear app data: Settings → Apps → Chutuki → Storage → Clear Data
2. Reinstall app
3. Check all permissions are granted
4. Check Android version (needs 8.0+)
5. Check logcat: `adb logcat | grep Chutuki`

### Issue: Cannot lock/unlock screen

**Solutions**:
1. Enable Accessibility Service
2. Grant Device Administrator permission
3. Go to Settings → Accessibility → Chutuki → Enable
4. Restart app

### Issue: Battery drains quickly

**Solutions**:
1. Disable "Always Listening" mode
2. Use wake word activation instead
3. Reduce wake word sensitivity
4. Close app when not in use
5. Enable battery optimization for background

---

## 🔄 Updating the App

### Method 1: Rebuild & Reinstall
```bash
# Pull latest changes (if from Git)
git pull

# Clean build
./gradlew clean

# Build new APK
./gradlew assembleDebug

# Install (will update existing installation)
adb install -r app/build/outputs/apk/debug/app-debug.apk
```

### Method 2: Manual Update
1. Uninstall old version: Settings → Apps → Chutuki → Uninstall
2. Install new APK (will lose app data)

---

## 📊 Performance Optimization

### Reduce Memory Usage:
- Edit `app/build.gradle.kts`:
```kotlin
android {
    defaultConfig {
        // Reduce memory
        maxSdkVersion = 33
    }
}
```

### Reduce APK Size:
```kotlin
android {
    buildTypes {
        release {
            minifyEnabled = true
            shrinkResources = true
        }
    }
}
```

### Enable ProGuard:
Already enabled in release builds for code obfuscation and optimization.

---

## 🆘 Getting Help

### Check Logs:
```bash
# View real-time logs
adb logcat | grep -i chutuki

# Save logs to file
adb logcat > chutuki_logs.txt
```

### Report Issues:
1. Open GitHub Issues
2. Provide:
   - Android version
   - Device model
   - Steps to reproduce
   - Log output
   - Screenshots

### Community Support:
- Discord: [Join server]
- Email: support@chutukiassistant.com
- Reddit: r/ChutukiAssistant

---

## ✅ Post-Installation Checklist

- [ ] App installed successfully
- [ ] All permissions granted
- [ ] Accessibility service enabled
- [ ] Notification listener enabled
- [ ] Device administrator enabled
- [ ] Battery optimization disabled
- [ ] Voice authentication completed
- [ ] Wake word detection working
- [ ] Speech recognition working
- [ ] Basic commands responding
- [ ] Phone control working
- [ ] API integrations tested

---

## 🎉 You're All Set!

Chutuki is now ready to be your personal AI assistant!

Try saying:
**"Hey Chutuki, introduce yourself"**

Enjoy! 🚀
