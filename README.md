# 🤖 Chutuki - JARVIS-Style Personal Voice Assistant

<div align="center">

![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)
![Android](https://img.shields.io/badge/Android-26%2B-green.svg)
![Kotlin](https://img.shields.io/badge/Kotlin-1.9.20-purple.svg)
![License](https://img.shields.io/badge/license-MIT-orange.svg)

**Your Personal AI Assistant with Full Phone Control & Caring Female Voice**

[Features](#-features) • [Architecture](#-architecture) • [Setup](#-setup-guide) • [Usage](#-usage) • [API Keys](#-api-keys-required)

</div>

---

## 🌟 Features

### 🎤 **Advanced Voice System**
- ✅ Wake word detection ("Chutuki", "Hey Chutuki", "Jarvis")
- ✅ Always-on listening mode with low battery consumption
- ✅ Hybrid speech recognition (Offline + Online)
- ✅ Natural language processing and understanding
- ✅ Caring, lovely female voice (Hindi + English)
- ✅ Context-aware conversations with memory
- ✅ Multi-language support (Hindi, English, Hinglish)

### 📱 **Complete Phone Control**
- ✅ Lock/Unlock device
- ✅ WiFi, Bluetooth, Mobile Data control
- ✅ Flashlight ON/OFF
- ✅ Volume control (Media, Ring, Alarm)
- ✅ Take screenshots
- ✅ Open/Close apps
- ✅ Make calls & send messages
- ✅ Read notifications
- ✅ Media playback control
- ✅ Silent/Do Not Disturb mode
- ✅ Brightness adjustment
- ✅ Location-based actions

### 🚀 **Smart Automation**
- ✅ Battery-based automation (Power save <20%)
- ✅ Night mode automation (auto dark theme)
- ✅ Driving mode detection
- ✅ Location-based triggers (Home, Office, etc.)
- ✅ Morning/Night routines
- ✅ Smart reminders & alarms
- ✅ Auto-reply system
- ✅ Scheduled tasks
- ✅ Emergency mode with priority contacts

### 🎨 **JARVIS-Style UI**
- ✅ Dark futuristic theme with neon accents
- ✅ Holographic style interface
- ✅ Circular voice wave animations
- ✅ Pulse microphone animation
- ✅ Fully responsive (all screen sizes)
- ✅ Jetpack Compose modern UI
- ✅ Lottie animations support
- ✅ Smooth transitions & effects

### 🔒 **Security & Privacy**
- ✅ Voice authentication (only authorized user)
- ✅ Encrypted local data storage
- ✅ Secure API communication (HTTPS + TLS)
- ✅ PIN/Password fallback
- ✅ Permission manager
- ✅ Biometric authentication support
- ✅ No data sent to third parties
- ✅ On-device processing priority

### 🌐 **API Integrations**
- ✅ Weather API (OpenWeatherMap)
- ✅ Web Search (Google Custom Search)
- ✅ YouTube API
- ✅ Google Maps/Navigation
- ✅ Google Calendar
- ✅ News API
- ✅ Cloud backup (Google Drive)
- ✅ Translation API

### 🧠 **AI Features**
- ✅ Context awareness
- ✅ Mood detection
- ✅ Personalized responses
- ✅ Learning user preferences
- ✅ Smart suggestions
- ✅ Conversational memory
- ✅ Proactive assistance
- ✅ Natural dialogue flow

---

## 🏗️ Architecture

### **Modular Structure**

```
ChutukiAssistant/
├── app/                          # Main application module
│   ├── ui/                       # Jetpack Compose UI
│   │   ├── screens/              # Main screens
│   │   ├── components/           # Reusable components
│   │   ├── theme/                # App theming
│   │   └── navigation/           # Navigation graph
│   ├── service/                  # Background services
│   │   ├── VoiceListeningService # Foreground voice service
│   │   ├── AccessibilityService  # System control
│   │   └── NotificationListener  # Notification access
│   ├── data/                     # Data layer
│   │   ├── repository/           # Data repositories
│   │   ├── database/             # Room database
│   │   └── datastore/            # Preferences
│   ├── domain/                   # Business logic
│   │   ├── usecase/              # Use cases
│   │   └── model/                # Domain models
│   └── di/                       # Dependency injection
│
├── voicecore/                    # Voice & Speech Module
│   ├── recognition/              # Speech recognition
│   │   ├── SpeechRecognitionEngine
│   │   ├── VoskRecognizer        # Offline recognition
│   │   └── GoogleRecognizer      # Online recognition
│   ├── tts/                      # Text-to-speech
│   │   ├── TTSEngine             # TTS management
│   │   └── VoicePersonality      # Voice customization
│   ├── wakeword/                 # Wake word detection
│   │   └── WakeWordDetector      # Porcupine integration
│   └── nlp/                      # Natural language processing
│       ├── IntentClassifier      # Intent detection
│       ├── EntityExtractor       # Entity extraction
│       └── ContextManager        # Conversation context
│
├── automation/                   # Automation Module
│   ├── controller/               # System controllers
│   │   ├── DeviceController      # Lock, unlock, etc.
│   │   ├── ConnectivityController# WiFi, Bluetooth, etc.
│   │   ├── MediaController       # Volume, playback
│   │   └── AppController         # App management
│   ├── scheduler/                # Task scheduling
│   │   ├── AutomationScheduler   # WorkManager tasks
│   │   └── RoutineManager        # Daily routines
│   └── trigger/                  # Event triggers
│       ├── BatteryTrigger        # Battery events
│       ├── LocationTrigger       # Location events
│       └── TimeTrigger           # Time-based events
│
├── security/                     # Security Module
│   ├── auth/                     # Authentication
│   │   ├── VoiceAuthenticator    # Voice biometrics
│   │   ├── BiometricAuth         # Fingerprint/Face
│   │   └── PinAuth               # PIN/Password
│   ├── encryption/               # Data encryption
│   │   ├── DataEncryptor         # AES encryption
│   │   └── SecureStorage         # Encrypted storage
│   └── permission/               # Permission management
│       └── PermissionManager     # Runtime permissions
│
└── apiintegration/               # API Integration Module
    ├── weather/                  # Weather service
    ├── search/                   # Web search
    ├── youtube/                  # YouTube integration
    ├── maps/                     # Google Maps
    ├── calendar/                 # Calendar sync
    ├── news/                     # News feeds
    └── backup/                   # Cloud backup
```

### **Tech Stack**

| Category | Technology |
|----------|-----------|
| **Language** | Kotlin 100% |
| **UI Framework** | Jetpack Compose |
| **Architecture** | MVVM + Clean Architecture |
| **DI** | Dagger Hilt |
| **Database** | Room + DataStore |
| **Async** | Kotlin Coroutines + Flow |
| **Networking** | Retrofit + OkHttp |
| **Wake Word** | Picovoice Porcupine |
| **Speech Recognition** | Vosk (offline) + Google Cloud Speech (online) |
| **TTS** | Google ML Kit TTS |
| **NLP** | Google ML Kit Language |
| **Background Tasks** | WorkManager |
| **Animations** | Lottie |

---

## 🚀 Setup Guide

### **Prerequisites**

- Android Studio Hedgehog (2023.1.1) or later
- JDK 17
- Android SDK 26+
- Minimum 4GB RAM
- Internet connection (for API setup)

### **Step 1: Clone the Project**

```bash
# Navigate to your projects directory
cd ~/AndroidStudioProjects

# Extract the provided ZIP file
unzip ChutukiAssistant.zip
cd ChutukiAssistant
```

### **Step 2: Open in Android Studio**

1. Open Android Studio
2. Click `File` → `Open`
3. Navigate to `ChutukiAssistant` folder
4. Click `OK`
5. Wait for Gradle sync to complete

### **Step 3: Configure API Keys**

Create a file `local.properties` in the project root:

```properties
# Picovoice Wake Word Detection
PICOVOICE_ACCESS_KEY=your_picovoice_access_key

# Google Cloud Speech API
GOOGLE_CLOUD_API_KEY=your_google_cloud_api_key

# Weather API (OpenWeatherMap)
WEATHER_API_KEY=your_openweathermap_api_key

# News API
NEWS_API_KEY=your_newsapi_key

# Google Custom Search
GOOGLE_SEARCH_API_KEY=your_google_search_api_key
GOOGLE_SEARCH_ENGINE_ID=your_search_engine_id

# YouTube API
YOUTUBE_API_KEY=your_youtube_api_key
```

### **Step 4: Download Required Models**

#### **Vosk Speech Recognition Model**

1. Download Hindi model: [vosk-model-small-hi-0.22](https://alphacephei.com/vosk/models)
2. Extract and place in: `app/src/main/assets/model-vosk-model-small-hi-0.22/`

Alternative models:
- English: `vosk-model-small-en-us-0.15`
- Hindi-English: `vosk-model-hi-0.22`

#### **Porcupine Wake Word Models**

Custom wake word models are included in the library. If you want custom keywords:

1. Go to [Picovoice Console](https://console.picovoice.ai/)
2. Create custom wake words: "Chutuki", "Hey Chutuki", "Jarvis"
3. Download `.ppn` files
4. Place in: `app/src/main/assets/wakewords/`

### **Step 5: Build the Project**

#### **Option A: Build APK**

```bash
# Debug APK
./gradlew assembleDebug

# Release APK (signed)
./gradlew assembleRelease
```

APK location: `app/build/outputs/apk/`

#### **Option B: Build from Android Studio**

1. Click `Build` → `Build Bundle(s) / APK(s)` → `Build APK(s)`
2. Wait for build to complete
3. Click `locate` to find the APK

### **Step 6: Install APK**

```bash
# Connect your Android device via USB (with USB debugging enabled)
adb install app/build/outputs/apk/debug/app-debug.apk

# Or transfer APK to phone and install manually
```

---

## 🎯 API Keys Required

### **1. Picovoice (Wake Word Detection)** - FREE
- Website: https://picovoice.ai/
- Steps:
  1. Sign up for free account
  2. Go to Console
  3. Copy Access Key
  4. Paste in `local.properties`

### **2. OpenWeatherMap (Weather Data)** - FREE
- Website: https://openweathermap.org/api
- Steps:
  1. Create free account
  2. Go to API Keys
  3. Copy API Key
  4. Paste in `local.properties`

### **3. News API** - FREE
- Website: https://newsapi.org/
- Steps:
  1. Sign up
  2. Get API Key
  3. Paste in `local.properties`

### **4. Google Cloud APIs** - FREE (with limits)
- Website: https://console.cloud.google.com/
- Required APIs:
  - Cloud Speech-to-Text API
  - Cloud Text-to-Speech API
  - Google Custom Search API
  - YouTube Data API v3
  - Google Maps API
  - Google Calendar API

Steps:
1. Create Google Cloud Project
2. Enable above APIs
3. Create credentials (API Keys)
4. Copy keys to `local.properties`

---

## 📖 Usage

### **First Launch**

1. **Grant Permissions**: The app will request:
   - Microphone (for voice input)
   - Phone (for calls/SMS)
   - Location (for location-based features)
   - Accessibility Service (for system control)
   - Notification Access (for reading notifications)
   - Device Admin (for lock/unlock)

2. **Voice Authentication**: Record your voice for authentication
   - Say "Chutuki" 5 times
   - Say "Hey Chutuki" 5 times
   - System will learn your voice profile

3. **Setup Preferences**:
   - Set your name
   - Choose language (Hindi/English/Both)
   - Configure wake words
   - Set up automation rules

### **Basic Commands**

#### **System Control**
```
"Chutuki, phone lock karo"
"Hey Chutuki, screen off karo"
"Jarvis, WiFi on karo"
"Bluetooth band karo"
"Flashlight on karo"
"Volume badha do"
"Screenshot le lo"
"Silent mode on karo"
```

#### **App Control**
```
"WhatsApp kholo"
"YouTube band karo"
"Chrome mein search karo"
"Instagram khol do"
```

#### **Communication**
```
"Mummy ko call karo"
"Papa ko message bhejo"
"Last notification padho"
"SMS bhejo Rahul ko"
```

#### **Information**
```
"Mausam kaisa hai?"
"Aaj ki news sunao"
"YouTube pe trending kya hai?"
"Kal ka reminder set karo"
```

#### **Smart Features**
```
"Battery kitni hai?"
"Location batao"
"Music chala do"
"Volume low karo"
"Morning routine activate karo"
```

### **Advanced Features**

#### **Conversational AI**
```
User: "Chutuki, main thak gaya hun"
Chutuki: "Aapko rest karna chahiye. Kya main relax karne ke liye music chala dun?"

User: "Haan please"
Chutuki: "Music start kar rahi hun. Kya aapko lights bhi dim karni hai?"
```

#### **Proactive Assistance**
- Morning: "Good morning! Aaj ka weather 28°C hai. Traffic normal hai. Aapki pehli meeting 10 AM ko hai."
- Low Battery: "Battery 15% reh gayi hai. Kya main power saving mode on kar dun?"
- Driving: "Drive safe! Kya main Do Not Disturb mode activate kar dun?"

---

## 🎨 UI Customization

### **Theme Colors**

Edit `app/src/main/java/com/chutuki/assistant/ui/theme/Color.kt`:

```kotlin
val NeonBlue = Color(0xFF00F0FF)
val NeonPink = Color(0xFFFF006E)
val DarkBackground = Color(0xFF0A0E27)
```

### **Voice Personality**

Edit `voicecore/src/main/java/com/chutuki/voicecore/tts/VoicePersonality.kt`:

```kotlin
val caringResponses = listOf(
    "Main yahaan hoon aapki madad ke liye",
    "Bilkul, main kar deti hun",
    "Aapki marzi, just say the word"
)
```

---

## 🔧 Troubleshooting

### **Wake Word Not Detecting**

1. Check microphone permission
2. Ensure background audio permission is granted
3. Verify Picovoice API key is correct
4. Check if wake word sensitivity is too low (increase in settings)

### **Speech Recognition Not Working**

1. **Offline mode**: Ensure Vosk model is downloaded and placed correctly
2. **Online mode**: Check internet connection and Google API key
3. Check microphone permission
4. Try switching between offline/online mode

### **App Crashes on Launch**

1. Clear app data and cache
2. Reinstall the app
3. Check if all required permissions are granted
4. Verify API keys are correctly configured

### **Accessibility Features Not Working**

1. Go to Settings → Accessibility
2. Enable "Chutuki Assistant" service
3. Grant all requested permissions
4. Restart the app

---

## 📱 Minimum Requirements

- **Android**: 8.0 Oreo (API 26) or higher
- **RAM**: 2GB minimum, 4GB recommended
- **Storage**: 500MB free space
- **Microphone**: Required
- **Internet**: Optional (required for online features)

---

## 🔐 Privacy & Security

- ✅ **Voice data processed on-device** (offline mode)
- ✅ **No voice recordings stored** (unless explicitly enabled)
- ✅ **Encrypted local storage** (AES-256)
- ✅ **No third-party analytics**
- ✅ **Open source** (code is transparent)
- ✅ **GDPR compliant**

---

## 🤝 Contributing

Contributions are welcome! Please follow these guidelines:

1. Fork the repository
2. Create feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **Picovoice** for wake word detection
- **Vosk** for offline speech recognition
- **Google ML Kit** for TTS and NLP
- **Jetpack Compose** community
- All open-source contributors

---

## 📞 Support

For issues, questions, or feature requests:

- **GitHub Issues**: [Create an issue](https://github.com/yourusername/ChutukiAssistant/issues)
- **Email**: support@chutukiassistant.com
- **Discord**: [Join our community](https://discord.gg/chutuki)

---

<div align="center">

**Made with ❤️ for the Indian developer community**

⭐ **Star this repo if you find it helpful!** ⭐

</div>
