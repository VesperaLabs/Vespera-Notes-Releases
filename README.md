# 🌌 Vespera Notes

> A minimalist, privacy-first markdown note-taking environment engineered for speed, data sovereignty, and distraction-free writing.

[![Latest Release](https://img.shields.io/github/v/release/VesperaLabs/Vespera-Notes-Releases?color=7C3AED&label=Latest%20Release&style=flat-square)](https://github.com/VesperaLabs/Vespera-Notes-Releases/releases/latest)
[![Platforms](https://img.shields.io/badge/Platform-Android%20%7C%20Windows-blue?style=flat-square)](https://github.com/VesperaLabs/Vespera-Notes-Releases/releases/latest)
[![Privacy Architecture](https://img.shields.io/badge/Architecture-Zero--Knowledge%20%7C%20Local--First-emerald?style=flat-square)](#-privacy--cryptographic-architecture)
[![License](https://img.shields.io/badge/License-Proprietary-lightgrey?style=flat-square)](#)

---

## 📥 Downloads

Download the latest stable builds directly from GitHub:

| Platform | Recommended Package | Target OS | Direct Download |
| :--- | :--- | :--- | :--- |
| **Android** | `VesperaNotes-Latest.apk` | Android 8.0+ (Oreo or later) | [📥 Download Latest APK](https://github.com/VesperaLabs/Vespera-Notes-Releases/releases/latest/download/VesperaNotes-v1.0.3.apk) |
| **Windows** | `VesperaNotes-Setup-Latest.exe` | Windows 10 / 11 (64-bit) | [📥 Download Windows Setup](https://github.com/VesperaLabs/Vespera-Notes-Releases/releases/latest) |

> 💡 *To browse all historical versions and detailed release notes, check the [All Releases](https://github.com/VesperaLabs/Vespera-Notes-Releases/releases) archive.*

---

## ✨ Features at a Glance

* **⚡ Ultra Lightweight & Native:** Powered by Rust (Tauri) on Windows with sub-3 MB binary size and minimal RAM consumption, paired with an optimized Capacitor runtime for Android.
* **✍️️ Distraction-Free Markdown:** Full syntax highlighting, rich code formatting, tag grouping, and fast search.
* **☁️ Client-Side Google Drive AppData Sync:** Seamless cross-device synchronization using your personal Google account. Zero intermediate servers, zero relay databases, and no monthly cloud subscriptions.
* **🔒 Zero-Knowledge Vault:** Client-side cryptographic isolation for sensitive thoughts and notes powered by authenticated AES-256-GCM.
* **🛡️️ Device-Level Security:** Hardware-backed biometric authentication (Fingerprint, Face Unlock, Windows Hello) and salted PIN lock.
* **🤖 Smart On-Demand AI:** Built-in Gemini-powered note summarization and voice transcription triggered strictly by manual action.

---

## ⚡ The Breakthrough: Zero-Knowledge Google Drive AppData Sync

Unlike conventional note-taking applications that lock your personal thoughts into closed third-party cloud servers or demand complicated self-hosted sync relays, **Vespera Notes** introduces a client-side **Google Drive AppData Sync** architecture:

* 🔒 **Private AppData Sandbox:** Notes are stored exclusively inside your personal Google Drive's hidden `appDataFolder`. No outside servers, no third-party databases, and no monthly subscription fees.
* 🛡️ **Client-Side Zero-Knowledge:** All data is processed directly on your device. We never run backend servers that can inspect, track, or index your content.
* 🔄 **Seamless Cross-Device Continuity:** Seamlessly bridge your notes between Android and Windows whenever an internet connection is available, while retaining complete offline functionality.
* 📦 **Total Data Sovereignty:** You own your data. Because sync files exist strictly inside your personal Google storage, your notes stay permanently in your custody.

---

## 🛡 Privacy & Cryptographic Architecture

Because **Vespera Notes** is distributed as a closed-source client, we uphold a strict **local-first, zero-knowledge architectural model**. You retain complete ownership of your data with no third-party tracking, profiling, or centralized database storage.

```text
┌────────────────────────────────────────────────────────────────────────┐
│                          LOCAL DEVICE RUNTIME                          │
│                                                                        │
│  [ Standard Notes ] ──────► Local Device Storage (Plaintext on Disk)   │
│                                                                        │
│  [ Private Vault ]  ──────► In-Memory Key (PBKDF2 600k + SHA-256)      │
│                               │                                        │
│                               ▼                                        │
│                      [ AES-256-GCM Engine ]                            │
│                               │                                        │
│                               ▼ (Ciphertext Only)                      │
│                      [ Encrypted Vault Store ]                         │
└───────────────────────────────┬────────────────────────────────────────┘
                                │
                 OAuth Scope: drive.appdata ONLY
            (Direct Client-to-Drive / Zero Relay)
                                │
                                ▼
              ┌───────────────────────────────────┐
              │    Google Drive AppData Folder    │
              │  (Isolated Sandbox - No Metadata) │
              └───────────────────────────────────┘
```

<details open>
<summary><strong>1. Local-First Storage & Data Sovereignty</strong></summary>

* **Device-Exclusive Storage:** Your regular notes, tags, and settings are saved exclusively in local device storage.
* **No Central User Database:** There is no proprietary backend server logging your notes, user profiles, or usage habits.
* **Zero Telemetry or Trackers:** No analytics SDKs (e.g., Google Analytics, Mixpanel, Facebook Pixel) or advertising trackers are embedded in the app.
* **Full Offline Independence:** The application operates completely without an internet connection or account registration.

</details>

<details open>
<summary><strong>2. Zero-Knowledge Private Vault (AES-256-GCM)</strong></summary>

For sensitive notes, Vespera Notes features a client-side encrypted Private Vault:

* **Key Derivation (PBKDF2):** When you set a Master Password, a unique 16-byte cryptographic salt is generated, and a 256-bit key is derived using **PBKDF2 with SHA-256 over 600,000 iterations** via the standard Web Cryptography API. This provides high resistance to offline brute-force attacks.
* **Authenticated Encryption:** Note titles, contents, and tags are encrypted using **AES-GCM 256-bit** with a freshly generated 12-byte Initialization Vector (IV) for every note.
* **In-Memory Volatile Key Cache:** Your plaintext password and raw derived keys are never stored on disk. They are held strictly in memory while the vault is unlocked and are purged upon locking, auto-locking, or closing the app.
* **Canary Token Verification:** An encrypted canary token (`VESPERA_VAULT_VERIFIED`) verifies password accuracy without storing any password hashes or hints.

</details>

<details open>
<summary><strong>3. Google Drive Sync Privacy & Restricted Scope</strong></summary>

When you choose to enable cloud sync:

* **Isolated Application Data Folder (`drive.appdata`):** The app requests only the `https://www.googleapis.com/auth/drive.appdata` OAuth scope. This means:
  * The app cannot view, list, read, or modify any existing files, photos, or documents in your personal Google Drive.
  * Sync data is stored in a hidden sandbox folder reserved exclusively for Vespera Notes.
* **Direct Client-to-Cloud Transmission:** The client communicates directly with Google's official API endpoints. There are no intermediate reverse proxies, sync dispatchers, or relay nodes intercepting your payloads.
* **End-to-End Vault Protection in the Cloud:** Hidden vault notes are encrypted client-side before being uploaded to Google Drive. Google and anyone inspecting your cloud storage only see ciphertext; they cannot read the note title, content, or tags.

</details>

<details>
<summary><strong>4. Device Lock & Biometric Protection</strong></summary>

* **SHA-256 PIN Hashing:** Your App Lock PIN is hashed client-side with SHA-256 and a local salt. The plaintext PIN is never stored.
* **Hardware-Bound Biometrics:**
  * **Android:** Direct integration with Android `BiometricPrompt` utilizing the hardware Secure Element.
  * **Windows / Desktop:** Implemented through the W3C WebAuthn standard (Windows Hello).
  * Biometric credentials never leave your hardware chip; the application only receives a verified pass/fail assertion.
* **Configurable Auto-Lock:** Options ranging from *Immediate on minimize* to *1m*, *5m*, and *15m* ensure your notes and unlocked vault keys are cleared automatically if your screen is left unattended.

</details>

<details>
<summary><strong>5. AI Features (Summaries & Voice Transcription)</strong></summary>

* **On-Demand Only:** Text and voice recordings are sent to the Gemini API strictly when you explicitly invoke an AI action (such as summarizing or recording a voice note). There is no continuous background scanning or indexing.
* **Ephemeral Processing:** Note text and audio payloads are processed in-memory by the API and are not logged or retained on any application server.

</details>

---

## 🚀 Installation & Verification

### 🤖 Android Setup
1. Download the latest `VesperaNotes-*.apk` from the [Releases page](https://github.com/VesperaLabs/Vespera-Notes-Releases/releases/latest).
2. Open the file on your Android device.
3. If prompted, allow installation from your browser or file manager (*Install Unknown Apps*).
4. Launch **Vespera Notes** and start writing offline, or connect Google Drive in Settings.

### 💻 Windows Setup
1. Download `VesperaNotes-Setup-*.exe` from the [Releases page](https://github.com/VesperaLabs/Vespera-Notes-Releases/releases/latest).
2. Run the executable to open the setup wizard.
3. *(Optional)* If Microsoft Defender SmartScreen displays a warning, click **More info** ➔ **Run anyway** (common for newly released independent software without costly enterprise EV code-signing certificates).
4. Launch Vespera Notes from your Start Menu or Desktop shortcut.

---

<div align="center">
  <sub>Developed & maintained by <strong>Vespera Labs</strong> • Built for speed, calm, and absolute data sovereignty.</sub>
</div>
