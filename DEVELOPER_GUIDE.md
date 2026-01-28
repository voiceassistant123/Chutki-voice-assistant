# 👨‍💻 Chutuki Assistant - Developer Guide

## 🏗️ Project Architecture

### Clean Architecture Layers

```
┌─────────────────────────────────────────────────────┐
│                    UI Layer (Compose)                │
│  - Screens, Components, ViewModels, Navigation      │
└─────────────────┬───────────────────────────────────┘
                  │
┌─────────────────▼───────────────────────────────────┐
│              Domain Layer (Use Cases)                │
│  - Business Logic, Models, Repository Interfaces    │
└─────────────────┬───────────────────────────────────┘
                  │
┌─────────────────▼───────────────────────────────────┐
│             Data Layer (Repository)                  │
│  - Data Sources, Database, Network, Cache           │
└──────────────────────────────────────────────────────┘
```

### Module Structure

**1. app/** - Main application module
- UI (Jetpack Compose)
- Services (Background)
- Dependency Injection (Hilt)
- Application class

**2. voicecore/** - Voice & Speech module
- Wake word detection
- Speech recognition
- Text-to-speech
- NLP processing

**3. automation/** - System automation module
- Device controllers
- Task scheduling
- Trigger management

**4. security/** - Security module
- Authentication
- Encryption
- Permission management

**5. apiintegration/** - External API module
- Weather API
- News API
- Web search
- YouTube, Maps, etc.

---

## 🛠️ Development Setup

### Prerequisites

```bash
# Check Java version (required: JDK 17)
java -version

# Check Android Studio version
# Required: Hedgehog (2023.1.1) or later

# Check Kotlin version
# Required: 1.9.20+
```

### Clone & Setup

```bash
# Clone the repository
git clone https://github.com/yourusername/ChutukiAssistant.git
cd ChutukiAssistant

# Create local.properties
touch local.properties

# Add API keys (see INSTALLATION.md)
nano local.properties
```

### Build Variants

```kotlin
// Debug build - Development
./gradlew assembleDebug

// Release build - Production (minified, ProGuard enabled)
./gradlew assembleRelease

// Install on device
./gradlew installDebug
```

---

## 📁 Code Organization

### Package Structure

```
com.chutuki.assistant/
├── ui/
│   ├── screens/
│   │   ├── MainScreen.kt
│   │   ├── SettingsScreen.kt
│   │   └── PermissionsScreen.kt
│   ├── components/
│   │   ├── VoiceVisualizer.kt
│   │   ├── CommandCard.kt
│   │   └── StatusIndicator.kt
│   ├── theme/
│   │   ├── Color.kt
│   │   ├── Theme.kt
│   │   └── Type.kt
│   └── viewmodel/
│       ├── MainViewModel.kt
│       └── SettingsViewModel.kt
│
├── service/
│   ├── VoiceListeningService.kt
│   ├── AccessibilityService.kt
│   └── NotificationListenerService.kt
│
├── data/
│   ├── repository/
│   │   ├── VoiceRepository.kt
│   │   ├── SettingsRepository.kt
│   │   └── CommandHistoryRepository.kt
│   ├── database/
│   │   ├── AppDatabase.kt
│   │   ├── dao/
│   │   └── entity/
│   └── datastore/
│       └── PreferencesManager.kt
│
├── domain/
│   ├── usecase/
│   │   ├── ProcessVoiceCommandUseCase.kt
│   │   ├── ExecuteActionUseCase.kt
│   │   └── ManagePermissionsUseCase.kt
│   └── model/
│       ├── Command.kt
│       ├── Intent.kt
│       └── Response.kt
│
├── di/
│   ├── AppModule.kt
│   ├── NetworkModule.kt
│   ├── DatabaseModule.kt
│   └── VoiceModule.kt
│
└── utils/
    ├── PermissionUtils.kt
    ├── DeviceUtils.kt
    └── Extensions.kt
```

---

## 🎯 Key Components

### 1. Voice Listening Service

```kotlin
@AndroidEntryPoint
class VoiceListeningService : Service() {
    
    @Inject
    lateinit var wakeWordDetector: WakeWordDetector
    
    @Inject
    lateinit var speechRecognizer: SpeechRecognitionEngine
    
    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        // Start foreground service
        startForeground(NOTIFICATION_ID, createNotification())
        
        // Initialize wake word detection
        wakeWordDetector.initialize()
        
        // Start listening
        startWakeWordListening()
        
        return START_STICKY
    }
    
    private fun startWakeWordListening() {
        lifecycleScope.launch {
            wakeWordDetector.wakeWordDetected.collect { event ->
                event?.let {
                    // Wake word detected, start full speech recognition
                    playAcknowledgmentSound()
                    speechRecognizer.startListening()
                }
            }
        }
    }
}
```

### 2. Speech Recognition Engine

```kotlin
@Singleton
class SpeechRecognitionEngine @Inject constructor(
    private val context: Context
) {
    private var mode: RecognitionMode = RecognitionMode.HYBRID
    
    fun startListening(language: String = "hi-IN") {
        when (mode) {
            RecognitionMode.OFFLINE -> startVoskRecognition()
            RecognitionMode.ONLINE -> startGoogleRecognition()
            RecognitionMode.HYBRID -> tryOfflineFirst()
        }
    }
    
    private suspend fun tryOfflineFirst() {
        try {
            startVoskRecognition()
        } catch (e: Exception) {
            // Fallback to online
            startGoogleRecognition()
        }
    }
}

enum class RecognitionMode {
    OFFLINE,  // Vosk
    ONLINE,   // Google Cloud Speech
    HYBRID    // Try offline first, fallback to online
}
```

### 3. Intent Classification

```kotlin
@Singleton
class IntentClassifier @Inject constructor() {
    
    fun classifyIntent(text: String): Intent {
        val normalizedText = text.lowercase().trim()
        
        return when {
            containsPhoneCall(normalizedText) -> Intent.PHONE_CALL
            containsMessage(normalizedText) -> Intent.SEND_MESSAGE
            containsSystemControl(normalizedText) -> classifySystemIntent(normalizedText)
            containsAppControl(normalizedText) -> Intent.APP_CONTROL
            containsInformation(normalizedText) -> classifyInformationIntent(normalizedText)
            else -> Intent.UNKNOWN
        }
    }
    
    private fun containsPhoneCall(text: String): Boolean {
        return text.contains("call") || text.contains("फोन") || 
               text.contains("कॉल")
    }
    
    private fun classifySystemIntent(text: String): Intent {
        return when {
            text.contains("lock") || text.contains("लॉक") -> Intent.LOCK_SCREEN
            text.contains("wifi") -> Intent.WIFI_CONTROL
            text.contains("bluetooth") -> Intent.BLUETOOTH_CONTROL
            text.contains("volume") || text.contains("वॉल्यूम") -> Intent.VOLUME_CONTROL
            text.contains("brightness") -> Intent.BRIGHTNESS_CONTROL
            else -> Intent.SYSTEM_CONTROL_GENERIC
        }
    }
}

sealed class Intent {
    object PHONE_CALL : Intent()
    object SEND_MESSAGE : Intent()
    object LOCK_SCREEN : Intent()
    object WIFI_CONTROL : Intent()
    object APP_CONTROL : Intent()
    object UNKNOWN : Intent()
    // ... more intents
}
```

### 4. Device Controller

```kotlin
@Singleton
class DeviceController @Inject constructor(
    private val context: Context
) {
    
    fun lockScreen(): Result<Unit> {
        return try {
            val deviceAdmin = ComponentName(context, DeviceAdminReceiver::class.java)
            val dpm = context.getSystemService(Context.DEVICE_POLICY_SERVICE) as DevicePolicyManager
            
            if (dpm.isAdminActive(deviceAdmin)) {
                dpm.lockNow()
                Result.success(Unit)
            } else {
                Result.failure(Exception("Device admin not enabled"))
            }
        } catch (e: Exception) {
            Result.failure(e)
        }
    }
    
    fun controlWiFi(enable: Boolean): Result<Unit> {
        return try {
            val wifiManager = context.applicationContext.getSystemService(Context.WIFI_SERVICE) as WifiManager
            wifiManager.isWifiEnabled = enable
            Result.success(Unit)
        } catch (e: Exception) {
            Result.failure(e)
        }
    }
    
    fun setVolume(level: Int, streamType: Int = AudioManager.STREAM_MUSIC): Result<Unit> {
        return try {
            val audioManager = context.getSystemService(Context.AUDIO_SERVICE) as AudioManager
            audioManager.setStreamVolume(streamType, level, 0)
            Result.success(Unit)
        } catch (e: Exception) {
            Result.failure(e)
        }
    }
}
```

---

## 🧪 Testing

### Unit Tests

```kotlin
@RunWith(AndroidJUnit4::class)
class IntentClassifierTest {
    
    private lateinit var classifier: IntentClassifier
    
    @Before
    fun setup() {
        classifier = IntentClassifier()
    }
    
    @Test
    fun `test phone call intent detection`() {
        val text = "Mummy ko call karo"
        val intent = classifier.classifyIntent(text)
        assertEquals(Intent.PHONE_CALL, intent)
    }
    
    @Test
    fun `test WiFi control intent`() {
        val text = "WiFi on karo"
        val intent = classifier.classifyIntent(text)
        assertEquals(Intent.WIFI_CONTROL, intent)
    }
}
```

### UI Tests

```kotlin
@RunWith(AndroidJUnit4::class)
class MainScreenTest {
    
    @get:Rule
    val composeTestRule = createComposeRule()
    
    @Test
    fun testVoiceVisualizerDisplayed() {
        composeTestRule.setContent {
            ChutukiAssistantTheme {
                MainScreen()
            }
        }
        
        composeTestRule
            .onNodeWithTag("voice_visualizer")
            .assertIsDisplayed()
    }
}
```

---

## 🎨 Adding New Features

### 1. Add New Command

**Step 1: Define Intent**
```kotlin
// domain/model/Intent.kt
sealed class Intent {
    // ... existing intents
    object TAKE_SCREENSHOT : Intent()
}
```

**Step 2: Update Classifier**
```kotlin
// domain/usecase/IntentClassifier.kt
fun classifyIntent(text: String): Intent {
    return when {
        // ... existing conditions
        containsScreenshot(text) -> Intent.TAKE_SCREENSHOT
    }
}

private fun containsScreenshot(text: String): Boolean {
    return text.contains("screenshot") || text.contains("स्क्रीनशॉट")
}
```

**Step 3: Create Controller**
```kotlin
// automation/controller/ScreenshotController.kt
@Singleton
class ScreenshotController @Inject constructor(
    private val context: Context
) {
    fun takeScreenshot(): Result<Unit> {
        // Implementation
    }
}
```

**Step 4: Add Use Case**
```kotlin
// domain/usecase/TakeScreenshotUseCase.kt
class TakeScreenshotUseCase @Inject constructor(
    private val controller: ScreenshotController
) {
    suspend operator fun invoke(): Result<Unit> {
        return controller.takeScreenshot()
    }
}
```

**Step 5: Handle in ViewModel**
```kotlin
// ui/viewmodel/MainViewModel.kt
fun handleCommand(command: Command) {
    when (command.intent) {
        is Intent.TAKE_SCREENSHOT -> {
            viewModelScope.launch {
                takeScreenshotUseCase()
            }
        }
    }
}
```

### 2. Add New API Integration

**Step 1: Create API Interface**
```kotlin
// apiintegration/spotify/SpotifyApi.kt
interface SpotifyApi {
    @GET("v1/me/player/currently-playing")
    suspend fun getCurrentlyPlaying(): Response<Track>
    
    @PUT("v1/me/player/play")
    suspend fun play(): Response<Unit>
}
```

**Step 2: Create Repository**
```kotlin
// apiintegration/spotify/SpotifyRepository.kt
@Singleton
class SpotifyRepository @Inject constructor(
    private val api: SpotifyApi
) {
    suspend fun play(): Result<Unit> {
        return try {
            api.play()
            Result.success(Unit)
        } catch (e: Exception) {
            Result.failure(e)
        }
    }
}
```

**Step 3: Add to DI**
```kotlin
// di/NetworkModule.kt
@Provides
@Singleton
fun provideSpotifyApi(retrofit: Retrofit): SpotifyApi {
    return retrofit.create(SpotifyApi::class.java)
}
```

---

## 🚀 Performance Optimization

### Memory Optimization

```kotlin
// Use lazy initialization
private val expensiveObject by lazy {
    ExpensiveObject()
}

// Release resources when not needed
override fun onPause() {
    super.onPause()
    speechRecognizer.release()
}

// Use weak references for callbacks
private var callback: WeakReference<Callback>? = null
```

### Battery Optimization

```kotlin
// Use efficient wake lock
wakeLock = (getSystemService(Context.POWER_SERVICE) as PowerManager).run {
    newWakeLock(PowerManager.PARTIAL_WAKE_LOCK, "Chutuki::WakeLock").apply {
        acquire(10*60*1000L /*10 minutes*/)
    }
}

// Schedule background work efficiently
val constraints = Constraints.Builder()
    .setRequiredNetworkType(NetworkType.CONNECTED)
    .setRequiresBatteryNotLow(true)
    .build()

val workRequest = OneTimeWorkRequestBuilder<SyncWorker>()
    .setConstraints(constraints)
    .build()
```

### UI Performance

```kotlin
// Use remember for expensive computations
@Composable
fun ExpensiveComponent() {
    val expensiveValue = remember {
        calculateExpensiveValue()
    }
}

// Use derivedStateOf for computed state
val filteredList by remember {
    derivedStateOf {
        list.filter { it.isActive }
    }
}

// LazyColumn for large lists
LazyColumn {
    items(largeList) { item ->
        ItemCard(item)
    }
}
```

---

## 📝 Coding Standards

### Kotlin Style Guide

```kotlin
// Use meaningful names
val isUserAuthenticated = true // Good
val flag = true // Bad

// Prefer immutability
val name = "Chutuki" // Good
var name = "Chutuki" // Use only when needed

// Use extension functions
fun Context.showToast(message: String) {
    Toast.makeText(this, message, Toast.LENGTH_SHORT).show()
}

// Leverage sealed classes
sealed class UiState {
    object Loading : UiState()
    data class Success(val data: Data) : UiState()
    data class Error(val message: String) : UiState()
}
```

### Documentation

```kotlin
/**
 * Processes voice commands and executes appropriate actions.
 *
 * @param command The recognized voice command
 * @return Result containing the response message or error
 * @throws InvalidCommandException if command cannot be processed
 */
suspend fun processCommand(command: String): Result<String> {
    // Implementation
}
```

---

## 🐛 Debugging

### Logging

```kotlin
// Use Timber for logging
Timber.d("Wake word detected: %s", wakeWord)
Timber.e(exception, "Error processing command")

// Log only in debug builds
if (BuildConfig.DEBUG) {
    Timber.d("Debug info: %s", debugData)
}
```

### Profiling

```bash
# CPU profiling
./gradlew :app:profileRelease

# Memory leak detection
# Use LeakCanary in debug builds

debugImplementation 'com.squareup.leakcanary:leakcanary-android:2.12'
```

---

## 📦 Release Checklist

- [ ] Update version code and name in `build.gradle.kts`
- [ ] Test all major features
- [ ] Run ProGuard and test obfuscated build
- [ ] Update CHANGELOG.md
- [ ] Create release notes
- [ ] Generate signed APK/AAB
- [ ] Test on multiple devices
- [ ] Verify all permissions
- [ ] Check crash analytics
- [ ] Update documentation
- [ ] Tag release in Git
- [ ] Create GitHub release

---

## 🤝 Contributing

### Pull Request Process

1. Fork the repository
2. Create feature branch: `git checkout -b feature/amazing-feature`
3. Commit changes: `git commit -m 'Add amazing feature'`
4. Push to branch: `git push origin feature/amazing-feature`
5. Open Pull Request

### Code Review Guidelines

- Follow coding standards
- Write tests for new features
- Update documentation
- Keep PRs focused and small
- Respond to review comments

---

## 📚 Additional Resources

- [Jetpack Compose Documentation](https://developer.android.com/jetpack/compose)
- [Kotlin Coroutines Guide](https://kotlinlang.org/docs/coroutines-guide.html)
- [Hilt Dependency Injection](https://dagger.dev/hilt/)
- [Material Design 3](https://m3.material.io/)
- [Android Architecture Components](https://developer.android.com/topic/architecture)

---

**Happy Coding! 🚀**
