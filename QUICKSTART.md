# ⚡ Quick Start - Build & Run in 5 Minutes

## 🎯 Goal
Get Chutuki Assistant running on your Android phone in under 5 minutes!

---

## Step 1: Open Project (1 minute)

1. **Extract files** to any location
2. **Open Android Studio**
3. Click **File** → **Open**
4. Select the **ChutukiAssistant** folder
5. Click **OK** and wait for Gradle sync

---

## Step 2: Basic Setup (1 minute)

Create `local.properties` file in project root:

```properties
# Add this ONE line for basic functionality
PICOVOICE_ACCESS_KEY=demo_key_for_testing
```

**Note**: For production, get free key from https://picovoice.ai/

---

## Step 3: Build APK (2 minutes)

### Option A: Android Studio (Easiest)
1. Click **Build** → **Build Bundle(s) / APK(s)** → **Build APK(s)**
2. Wait for build to complete (~2 minutes)
3. Click **locate** when build finishes
4. APK is ready!

### Option B: Command Line (Faster)
```bash
# From project root
./gradlew assembleDebug

# APK location
app/build/outputs/apk/debug/app-debug.apk
```

---

## Step 4: Install on Phone (1 minute)

### Method 1: USB Cable
```bash
# Enable USB debugging on phone first
# Settings → About → Tap Build Number 7 times
# Settings → Developer Options → USB Debugging ON

# Connect phone and run
adb install -r app/build/outputs/apk/debug/app-debug.apk
```

### Method 2: Transfer APK
1. Copy APK to phone (USB, email, cloud)
2. Open APK on phone
3. Allow "Install from Unknown Sources"
4. Click **Install**

---

## Step 5: Launch & Test (30 seconds)

1. **Open** Chutuki app
2. **Grant permissions** when prompted (Allow All)
3. **Test wake word**: Say **"Chutuki"**
4. **Test command**: Say **"Battery kitni hai?"**

---

## ✅ Success!

You now have a working AI voice assistant!

### Next Steps:

1. **Enable more features**: Add API keys (see INSTALLATION.md)
2. **Customize**: Change wake word, voice, theme
3. **Explore**: Try 300+ voice commands (see FEATURES.md)

---

## 🆘 Quick Troubleshooting

### Wake word not working?
- Check microphone permission
- Say "Chutuki" clearly and slightly louder
- Try in a quiet room

### Build fails?
```bash
# Clean and rebuild
./gradlew clean
./gradlew assembleDebug
```

### App crashes?
- Ensure Android 8.0+ (API 26+)
- Grant all permissions
- Clear app data and restart

---

## 📚 Full Documentation

- **Installation Guide**: INSTALLATION.md (detailed setup)
- **Feature List**: FEATURES.md (300+ features)
- **Developer Guide**: DEVELOPER_GUIDE.md (code structure)
- **README**: README.md (comprehensive overview)

---

## 🎉 Enjoy Your Personal AI Assistant!

Try saying:
- "Hey Chutuki, introduce yourself"
- "Mausam kaisa hai?"
- "Volume badha do"
- "WiFi on karo"
- "Phone lock karo"

**Have fun! 🚀**
