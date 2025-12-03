# 🎲 SessionKeeper - D&D Campaign Companion

<div align="center">

**A European-focused mobile companion app for Dungeons & Dragons campaigns, with automatic summaries, campaign wiki, and AI-powered insights.**

[![Platform](https://img.shields.io/badge/Platform-iOS_&_Android-blue.svg)]()

[![Category](https://img.shields.io/badge/Category-Tabletop_RPG_Companion-purple.svg)]()

[![License](https://img.shields.io/badge/License-Private-red.svg)]()

[![Region](https://img.shields.io/badge/Market-Europe-green.svg)]()

[Features](#-features) • [Architecture](#-architecture) • [Tech Stack](#-tech-stack) • [Folder Structure](#-folder-structure) • [Download](#-download--installation) • [Screenshots](#-screenshots)

</div>

---

## 📱 Project Overview

**SessionKeeper** is a mobile application designed for tabletop RPG groups, especially **Dungeons & Dragons 5e** players in the **European market**.  

The app records your sessions, processes audio in the cloud, and automatically turns your game into structured summaries, session logs, and a living campaign wiki.

While the party is derailing the DM’s carefully crafted plot, SessionKeeper quietly keeps perfect notes in the background.

### Key Highlights

- 🎧 **Hands‑free note‑taking**: Record your D&D sessions and let SessionKeeper handle transcription and structuring  
- 📚 **Self‑updating wiki**: NPCs, locations, quests, items, factions and more – organized automatically as a campaign knowledge base  
- 🤖 **AI‑powered assistant**: Ask campaign‑specific questions and get answers based on your actual sessions and wiki content  
- 🏆 **Player achievements**: Unlock fun badges and milestones for memorable in‑game moments (players & DMs)  
- 🖼️ **AI character & scene portraits**: Generate art based on your characters, places and campaign highlights  
- 🔔 **Smart notifications**: Reminders about upcoming sessions, new summaries, achievements and wiki updates  
- 🌍 **Built for Europe**: Optimized UX and content for European players, with English UI and room for future localization  
- 🔐 **Privacy‑first**: Clear recording indicators, secure storage, no training on your private campaign data  
- 💳 **Free & premium tiers**: Join campaigns for free; unlock advanced DM and AI tools via **Adventurer**, **Hero**, and **Legendary** subscriptions  

---

## 📥 Download & Installation

Get SessionKeeper on your mobile device:

<div align="center">

### 📱 Available on

[![App Store](https://img.shields.io/badge/Download_on_App_Store-0D96F6?style=for-the-badge&logo=app-store&logoColor=white)](https://apps.apple.com/app/sessionkeeper/id6737173822)

[![Google Play Store](https://img.shields.io/badge/Download_on_Google_Play-3DDC84?style=for-the-badge&logo=google-play&logoColor=white)](https://play.google.com/store/apps/details?id=com.ga.sessionkeeper)

### 📊 App Information

| Detail | iOS | Android |
|--------|-----|---------|
| **Version** | 1.1.1 | 1.1.1 |
| **Size** | ~109 MB | ~80 MB |
| **Language** | English | English |

</div>

---

## 🌍 Environments & Flavors

SessionKeeper ships with three build flavors to cover the full lifecycle:

- **Development (`development`)** – Local builds with debug tooling, dev Firebase project, and stubbed payments.
- **Staging (`staging`)** – QA/UAT builds wired to staging services for pre-release validation.
- **Production (`production`)** – Store-ready builds with hardened configs. **Firebase Analytics** and **Firebase Crashlytics** are enabled **only** on the production flavor to keep telemetry clean and compliant with GDPR expectations.

Every flavor has its own bundle identifiers, Firebase configs, API keys, RevenueCat environments, and notification topics.

---

## 🔄 CI/CD Pipeline

- **Android**
  - **GitHub Actions** workflows trigger on pushes to dedicated branches (e.g., `development`, `staging`, `production`).
  - The workflow invokes **Fastlane**, runs automated checks, builds the matching flavor, signs artifacts, and uploads them to **Firebase App Distribution** for testers.

- **iOS**
  - **Codemagic** handles scheme-based builds for every flavor, manages code signing, runs tests, and distributes builds to TestFlight or internal testers.

This setup ensures consistent, reproducible releases with minimal manual intervention for both platforms.

---

## 🛠️ Tech Stack

### Core Framework

- **Flutter** – Cross‑platform UI framework for iOS & Android  
- **Dart** – Single language for UI, domain logic, and integrations  

### State Management & Navigation

- **provider / flutter_bloc** – Predictable BLoC/Cubit & app‑wide state (auth, campaigns, subscriptions, settings)  
- **Declarative navigation** `go_router` – Typed routes and deep‑link handling for campaigns and sessions  

### Networking & API

- **dio** – REST communication with backend & AI services  
- **pretty_dio_logger** – Request/response logging in debug builds  

### Backend, Auth & Cloud

- **Firebase Core** – Project bootstrap and configuration  
- **Firebase Authentication** – Secure user identity and sign‑in flows (email/social as needed)  
- **Firebase Firestore / Realtime Database** – Campaign metadata, sessions, wiki entries and user data  
- **Firebase Storage** – Chunked audio uploads, portraits, and media assets  
- **Firebase Cloud Functions** – Serverless orchestration layer that connects audio chunk storage with Assimply AI, processes results, and pushes updates back to the app  

### AI, Audio & Processing

- **Assimply AI** – Audio processing, transcription, speaker separation (diarization), summarization, highlight and achievements extraction  
- **On‑device audio recorder** – Long‑running session recording with the stream split into **audio chunks** that are uploaded to Firebase Storage every few minutes for stability and lower data‑loss risk  

### Notifications & Messaging

- **Firebase Cloud Messaging (FCM)** – Push transport layer  
- **OneSignal** – Cross‑platform notification orchestration, segmentation and delivery analytics  

### Payments & Subscriptions

- **RevenueCat** – Cross‑platform in‑app purchases and subscription management (Free / Adventurer / Hero / Legendary)  

### Analytics & Crash Reporting

- **Firebase Analytics** – Screen views, feature usage, retention and funnel tracking (production flavor only)  
- **Firebase Crashlytics** – Crash reports and non‑fatal error monitoring (production flavor only)  

### Local & Secure Storage

- **flutter_secure_storage / shared_preferences** – Tokens, small cached settings, feature flags and lightweight offline data  

### UI / UX & Design

- **flutter_screenutil / responsive layout helpers** – Consistent sizing across European device profiles  
- **SVG / vector support** – Icons and illustrations  
- **cached_network_image / shimmer** – Optimized remote artwork loading and skeleton states for wiki & portraits  

### Integrations

- **Discord bot integration** – Remote session recording directly from Discord for online games  
- **Deep links & app links** – Join‑campaign codes, invitations, and campaign‑specific navigation  
- **share_plus / url_launcher** – Share invites, open external links (Terms, Privacy, Discord, etc.)  

---

## 🏗️ Architecture

SessionKeeper follows **Clean Architecture** combined with **feature‑first modularization**, long‑running recording, and AI features. The goal is to keep **Presentation**, **Domain**, and **Data** layers clearly separated so that features (Campaigns, Sessions, Wiki, Assistant, Subscriptions) are easy to evolve and test.

```
┌─────────────────────────────────────┐
│          Presentation Layer        │
│  (UI, Screens, State Management)   │
└─────────────────────────────────────┘
                 ↕
┌─────────────────────────────────────┐
│           Domain Layer              │
│ (Entities, Use Cases, Repositories) │
└─────────────────────────────────────┘
                 ↕
┌─────────────────────────────────────┐
│            Data Layer               │
│ (API, Local Cache, DTOs, Mappers)   │
└─────────────────────────────────────┘
```

### Presentation Layer

- **Pages / Screens**: Onboarding, campaigns list, create/join campaign, campaign dashboard (Activity / Sessions / Wiki), session recording, session highlights, wiki entry details, assistant chat, profile, settings, subscriptions paywall  
- **BLoC / Providers**: `CampaignsBloc`, `SessionsBloc`, `WikiBloc`, `AssistantCubit`, `SubscriptionBloc`, etc. to manage state and coordinate with use cases  
- **Widgets / Design system**: Buttons, cards, bottom sheets, tabs, badges, chips, avatars, waveform recorder, progress indicators – all built on a unified theme and ready for future RTL/localisation support  

### Domain Layer

- **Entities**: `Campaign`, `Session`, `Recording`, `Transcript`, `WikiEntry`, `Member`, `Achievement`, `SubscriptionPlan`, `NotificationPreference`, etc.  
- **Use Cases** (interactors):  
  - `StartSessionRecording`, `StopSessionRecording`, `UploadRecording`  
  - `GenerateSessionSummary`, `FetchSessionHighlights`  
  - `GetCampaigns`, `CreateCampaign`, `JoinCampaignWithCode`, `ArchiveCampaign`  
  - `GetWikiEntries`, `CreateWikiEntry`, `UpdateWikiEntry`, `ArchiveWikiEntry`  
  - `GetAchievementsForCampaign`, `GetUserSubscriptionStatus`, `PurchaseSubscription`  
- **Repository Interfaces**: `CampaignRepository`, `SessionRepository`, `WikiRepository`, `RecordingRepository`, `SubscriptionRepository`, `UserRepository`, with clear contracts that decouple domain logic from data implementations  

### Data Layer

- **Models (DTOs)**: JSON‑serializable models for campaigns, sessions, transcripts, wiki entries, achievements, subscriptions, notifications, etc.  
- **Remote Data Sources**:  
  - Firebase (Firestore / RTDB / Storage / Auth / Cloud Functions) for campaign content, audio chunks, and user accounts  
  - Assimply AI APIs for ingesting chunk references, generating transcripts, performing speaker diarization, and producing summaries, highlights, and achievements  
  - RevenueCat APIs for tracking subscription entitlements across app stores  
  - OneSignal for notification topics and device tokens  
- **Local Data Sources**: `shared_preferences` and `flutter_secure_storage` for lightweight caching, tokens, user settings, and basic offline data  

---

## ✨ Features

### 🎙️ Session Recording & Summaries

- One‑tap session recording with clear **recording indicators**  
- Speaker detection to distinguish players and DM where possible  
- Automatic **session summaries** answering “what happened last time?”  
- Session duration tracking and basic statistics per campaign  
- Ability to **upload existing recordings** and process them through the same pipeline  

### 📚 Campaign Wiki & Knowledge Base

- Self‑updating wiki with structured categories: **NPCs, Locations, Quests, Items, Factions, Lore**  
- Detail pages with images, tags (e.g. *Active*, *Retired*, *Location*), and rich text descriptions  
- Automatic linking between sessions and wiki entries (who appeared, where, when)  
- Manual editing, adding, and archiving of wiki entries, with restore options  
- Campaign‑level search and filters for fast lookup during play  

### 🤖 AI Assistant

- In‑campaign assistant chat (“SessionKeeper Assistant”) scoped to a specific campaign  
- Contextual answers powered by your **actual transcripts and wiki**, not generic D&D content  
- Brainstorming support for plot hooks, NPC personalities, and world‑building ideas  
- Safe, transparent behaviour – no access to other users’ campaigns or external training on your data  

### 🏆 Achievements & Highlights

- Automatic detection of memorable moments from each session  
- Visual **session highlights** screen with audio snippets and short narrative analysis  
- Per‑character and DM achievements with badge art and short descriptions  
- History of unlocked achievements per campaign and per member  

### 🎴 Campaign & Player Management

- Create campaigns with title, description, **game system**, schedule, art style, thumbnail, and total play time estimates  
- Invite players via **join code** or direct sharing  
- Member management screen for assigning roles (DM / player) and handling unclaimed characters  
- Session attendance & recording consent confirmation sheet at session start  

### 🧭 In‑Session Tools

- Session “Activity / Sessions / Wiki” tabs per campaign  
- Timeline of wiki edits, session starts, and highlights in the **Activity** tab  
- Quick access to session details and highlights from the **Sessions** tab  
- Fast search and navigation through the wiki mid‑game  

### 👤 Profile & Settings

- Profile screen with account details and subscription status  
- Notification and recording preference configuration  
- Transcript & recording retention settings (e.g., auto‑delete rules)  
- Data export options for audio and transcripts  
- Account deactivation and log‑out flows  

### 💳 Subscriptions & Monetization

- **Free**: Join unlimited campaigns as a player  
- **Adventurer**: Create one campaign with summaries, portraits, and achievements  
- **Hero**: Multiple campaigns, full campaign wiki, Discord bot integration, advanced DM tools  
- **Legendary**: Share premium benefits across the whole party and unlock the full toolset  

---

## 📁 Folder Structure

```
lib/ or src/
├── core/                          # Shared core functionality (theme, routing, utils)
│   ├── api/                       # API config and endpoints
│   ├── assets/                    # Icons, illustrations, lottie, etc.
│   ├── errors/                    # Failure / error classes
│   ├── helpers/                   # Utilities and formatters
│   ├── localization/              # i18n files (en, future EU languages)
│   ├── routing/                   # Route names & router setup
│   ├── style/                     # Colors, typography, spacing
│   └── widgets/                   # Shared reusable UI building blocks
│
├── features/                      # Feature modules (clean + feature-first)
│   ├── onboarding/                # First-time experience, permissions, intro
│   ├── auth/                      # Authentication (sign-in, sign-up, account linking)
│   ├── campaigns/                 # Campaign list, creation, archive, details
│   ├── join_campaign/             # Join-by-code flows
│   ├── sessions/                  # Session list, recording, details, highlights
│   ├── wiki/                      # Wiki list, filters, entry details, editing
│   ├── assistant/                 # In-campaign AI assistant chat
│   ├── achievements/              # Achievements UI and logic
│   ├── members/                   # Player management and attendance
│   ├── subscriptions/             # Plans, paywall, entitlement checks
│   ├── settings/                  # Recording prefs, notifications, privacy
│   └── profile/                   # User profile & account management
│
└── main.dart                      # App entry point & root widget
```

---

## 📸 Screenshots

<div align="center">

### Onboarding & Splash

<img src="https://github.com/user-attachments/assets/a15e8ceb-ce46-4d30-bf59-5be1d1ef8f98" alt="SessionKeeper Splash Screen" width="250"/>
</br>
<img src="https://github.com/user-attachments/assets/10946943-6cf2-4ff4-916a-833531c40acd" alt="SessionKeeper Splash Screen" width="250"/>
<img src="https://github.com/user-attachments/assets/b226dfdd-2bce-4714-ae01-735227f5da56" alt="SessionKeeper Splash Screen" width="250"/>
<img src="https://github.com/user-attachments/assets/08e31463-5ee4-43bb-aa07-67cce3c9c0ed" alt="SessionKeeper Splash Screen" width="250"/>

### Campaigns Home

<img src="https://github.com/user-attachments/assets/acfb4fea-f005-4964-af42-e4f1d276b369" alt="Campaigns Home - Active campaigns list" width="250"/>
<img src="https://github.com/user-attachments/assets/113ff2a8-8234-4c58-9109-3da6c893ba04" alt="Campaigns Home - Active campaigns list" width="250"/>

### Create & Join Campaign

<img src="https://github.com/user-attachments/assets/e3ec9574-377f-45b5-91d1-c4a00e15f36a" alt="Create Campaign - Game system" width="250"/>
<img src="https://github.com/user-attachments/assets/464c6f16-cd65-48fc-be3e-4dc4ab0157f8" alt="Create Campaign - Details and schedule" width="250"/>
<img src="https://github.com/user-attachments/assets/0e176514-cffd-4597-ba9d-473c5b1c28ef" alt="Join Campaign via code" width="250"/>

### Campaign Dashboard

<img src="https://github.com/user-attachments/assets/6ae03762-c7b1-437e-bde7-a2760b9cccaf" alt="Campaign Activity tab with recent updates" width="250"/>
<img src="https://github.com/user-attachments/assets/e58fcaa9-02b3-49b0-99cf-1740a5aeefa4" alt="Campaign Sessions tab with session list" width="250"/>
<img src="https://github.com/user-attachments/assets/2c779c41-722b-4771-ae7f-7798772c3e57" alt="Campaign Wiki tab with categories and entries" width="250"/>

### Wiki & Details

<img src="https://github.com/user-attachments/assets/b00a5288-cf1f-4cce-952b-0a7df76850f6" alt="Wiki entry details with artwork and description" width="250"/>

### Recording & Highlights

<img src="https://github.com/user-attachments/assets/c80e80d5-280c-4324-a1ab-19cdb56e6279" alt="Recording a live D&D session" width="250"/>
<img src="https://github.com/user-attachments/assets/d6f52d83-31f9-4690-aa50-9a47ad567eb4" alt="Session highlights and achievements" width="250"/>

### Assistant & Settings

<img src="https://github.com/user-attachments/assets/c23154d9-fb04-4bbb-a3c7-f1f02fd26ef6" alt="SessionKeeper AI Assistant chat" width="250"/>
<img src="https://github.com/user-attachments/assets/23448645-4cd8-4a63-8ea8-895a8aea405b" alt="Transcript and recording settings" width="250"/>
<img src="https://github.com/user-attachments/assets/4a3a01fd-311e-49c3-a342-512c8daf20f1" alt="Profile and subscription management" width="250"/>

</div>

---

## 👨‍💻 Developer & Contact

<div align="center">

### **Ahmed Nasr**

**Flutter Developer & Mobile App Specialist**

📧 **Email**: ahmed.nasr.fahmey@gmail.com  
🌐 **LinkedIn**: https://www.linkedin.com/in/ahmed-nasr-fahmey/

---

</div>

[⬆ Back to Top](#-sessionkeeper---dd-campaign-companion)


