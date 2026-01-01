# AtesKit

**A comprehensive iOS framework for building modern, consistent, and scalable applications with AI-first development in mind.**

[![Swift](https://img.shields.io/badge/Swift-5.10+-orange.svg)](https://swift.org)
[![iOS](https://img.shields.io/badge/iOS-16.0+-blue.svg)](https://developer.apple.com/ios/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

## 🎯 Overview

AtesKit is a Swift Package designed for **rapid iOS app development** using AI-assisted "vibe coding". It provides:

- 🎨 **Design System** - Consistent padding, radius, and spacing tokens
- 💳 **Subscription Management** - RevenueCat integration out-of-the-box
- 🤖 **AI Components** - Ready-to-use chat and image generation views
- ⚙️ **Settings & Profile** - Pre-built settings views with premium prompts
- 🔐 **Security** - Keychain wrapper for sensitive data

---

## 📦 Installation

Add AtesKit to your project using Swift Package Manager:

```swift
// Package.swift
dependencies: [
    .package(url: "https://github.com/devmehmetates/AtesKit.git", branch: "main")
]

// Add to your target
.product(name: "AtesKit", package: "AtesKit"),
.product(name: "AKCoreUtils", package: "AtesKit"),
.product(name: "AKUILibrary", package: "AtesKit")
```

---

## 🏗️ Architecture

AtesKit consists of three modules:

```
┌─────────────────────────────────────────────────────┐
│                   AKUILibrary                        │
│  (UI Components, AI Views, Paywall, Settings)        │
├─────────────────────────────────────────────────────┤
│                   AKCoreUtils                        │
│  (Config, Managers, Keychain, Subscription Logic)    │
├─────────────────────────────────────────────────────┤
│                    AtesKit                           │
│  (Constants, Extensions, Size Utilities)             │
└─────────────────────────────────────────────────────┘
```

---

## 🚀 Quick Start

### 1. Configure Your App Entry Point

```swift
import SwiftUI
import AtesKit
import AKCoreUtils
import AKUILibrary

@main
struct MyApp: App {
    @StateObject private var config: AKConfig = .AKConfigBuilder()
        .setAppName("MyApp")
        .setSubscriptionType(.onlySubscription)
        .setApiKey("appl_YOUR_REVENUECAT_KEY")
        .setTermsAndCondition("https://myapp.com/terms")
        .setPrivacyPolicy("https://myapp.com/privacy")
        .build()

    var body: some Scene {
        WindowGroup {
            ContentView()
                .presentPaywallIfNeeds
                .environmentObject(config)
                .preferredColorScheme(.dark)
        }
    }
}
```

### 2. Use Design System Tokens

```swift
// ❌ DON'T: Use magic numbers
.padding(16)
.cornerRadius(8)

// ✅ DO: Use AtesKit tokens
.padding(AKPadding.xxMedium.rawValue)
.clipShape(.rect(cornerRadius: AKRadius.small.rawValue))
```

### 3. Apply UI Components

```swift
// Primary Button
Button("Continue") { }
    .akPrimaryButton()

// Mystical Background
ZStack {
    content
}.makeAkMysticalBackground

// Settings View
AKSettingsView {
    Section("Custom") {
        Text("My Setting")
    }
}
```

---

## 🎨 Design System

### Padding (`AKPadding`)

| Token | Value | Usage |
|-------|-------|-------|
| `.small` | 8pt | Compact spacing |
| `.xxxMedium` | 12pt | List item spacing |
| `.xxMedium` | 16pt | Standard padding |
| `.medium` | 24pt | Section spacing |
| `.xLarge` | 32pt | Large gaps |

### Radius (`AKRadius`)

| Token | Value | Usage |
|-------|-------|-------|
| `.small` | 8pt | Buttons, cards |
| `.medium` | 16pt | Modals, sheets |
| `.xLarge` | 32pt | Large containers |
| `.circular` | ∞ | Pills, avatars |

---

## 🤖 AI Components

### Chat View

```swift
@State private var result = ""

AKAIChatView(
    resultText: $result,
    prompt: "Your question here",
    systemPrompt: "You are a helpful assistant."
) {
    print("Done!")
}
```

### Image Generation

```swift
@State private var imageData: Data?

AKAIImage(
    data: $imageData,
    prompt: "A beautiful sunset",
    cost: 5,
    ratio: .square
)
```

---

## 💳 Subscription Management

```swift
@EnvironmentObject var config: AKConfig

// Check subscription status
if config.isSubscripted {
    // Show pro features
} else {
    // Show paywall
    Button("Upgrade") {
        config.paywall = true
    }
}

// Credit system
if config.credit >= 5 {
    config.credit -= 5
    // Perform AI operation
}
```

---

## ⚙️ Settings View

```swift
TabView {
    // Other tabs...
    
    AKSettingsView(user: currentUser) {
        Section("App Settings") {
            Toggle("Notifications", isOn: $notifications)
        }
    }
    .tabItem { Label("Settings", systemImage: "gear") }
}
```

> ⚠️ **Important:** Do NOT create a separate "Profile" tab. `AKSettingsView` serves as both settings and profile.

---

## 🔐 Security

### Keychain Storage

```swift
// Store sensitive data
AKKeychainWrapper.standard.set("token123", forKey: "authToken")

// Retrieve data
let token = AKKeychainWrapper.standard.string(forKey: "authToken")
```

---

## 📋 Best Practices

1. **AtesKit First** - Always check if a component exists before creating custom ones
2. **No Magic Numbers** - Use `AKPadding` and `AKRadius` exclusively
3. **Dark Mode** - AtesKit is optimized for dark mode
4. **NavigationStack** - Never use deprecated `NavigationView`
5. **Localization** - Use SwiftUI's native `Localizable.xcstrings`

---

## 📄 License

MIT License - See [LICENSE](LICENSE) for details.

---

## 👨‍💻 Author

**Mehmet Ateş** - [@devmehmetates](https://github.com/devmehmetates)
