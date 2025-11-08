#  System Architecture - MyWoman

## Overview
MyWoman is designed as a mobile-first web application that focuses on emotional support, community interaction, and access to mental health resources for new mothers.

---

## 1. Frontend Architecture
- **Framework:** React Native
- **Purpose:** To create a simple and intuitive mobile interface.
- **Key Components:**
  - Registration and Login Screens
  - Homepage Dashboard
  - Community Circles Page
  - Mood Check Page
  - Diary page (DearMom)
  - MomSupport Booking Interface
  - MomAid Quick Access Panel
  - Anonymous Mode Toggle (available across relevant pages)
- **UI Libraries:** Tailwind CSS or Material UI for clean and consistent design.

---

## 2. Backend Architecture
- **Framework:** Node.js with Express
- **Purpose:** Handles authentication, data processing, and API routing via REST endpoints.
- **Key Functions:**
  - Validating user data during sign-up and login.
  - Ensures secure authentication, including password hashing and safe session handling
  - Managing chat and community interactions (including Anonymous Mode)
  - Scheduling and managing counselling sessions (MomSupport)
  - Storing and retrieving diary entries securely (DearMom)
  - Recording, retrieving, and analyzing Mood Check data to provide personalized recommendations
  - Ensuring users can access MomAid resources and exercises for quick emotional support

---

## 3. Database
- **Type:** Firebase Firestore (NoSQL database)
- **Data Stored:**
  - User profiles and authentication tokens (with support for Anonymous Mode)
  - User-uploaded files such as profile pictures or diary attachments (stored securely in Firebase Storage)
  - Community posts and private chats.
  - Diary entries (DearMom)
  - Mood logs and reflection data (Mood Check)
  - Counselling session schedules and bookings (MomSupport)
  - MomAid resources and exercises
- **Security:** Firebase Authentication ensures only verified users can access or update their data.

---

## 4. Component Communication
- **Frontend and Backend:** Communicate through RESTful APIs.
- **Data Flow:**
  - The frontend sends requests via HTTPS to the backend.
  - Backend interacts with Firebase to store or fetch data for:
  - Community posts and chats (Community Circles)
  - Counselling session bookings and professional/volunteer info (MomSupport)
  - Mood logs and reflections (Mood Check)
  - Diary entries (DearMom)
  - MomAid resources and exercises
  - Responses are sent back to the frontend in JSON format.
- **Anonymous Mode:** Backend handles anonymization when users choose to stay unidentified.
- **Real-time Updates:** Firebase enables instant updates in chats and community posts.

---

## 5. Technical Feasibility
This structure is:
- **Scalable:** Firebase allows the app to handle many users, communities, and counselling sessions without server overload (supports Community Circles and MomSupport).
- **Secure:** Authentication and cloud-based storage protect private diary entries, mood logs, and user identities (supports DearMom, Mood Check, and Anonymous Mode).
- **Cost-efficient:** Using cloud services reduces hosting and maintenance costs while delivering features like MomAid resources and real-time updates.
- **User-friendly:** The React Native interface ensures accessibility even for non-technical users.

---

## 6. Future Improvements
- AI-powered mood insights: Provide personalized tips and track emotional trends (Mood Check).
- Integration with telehealth services: Enable faster access to professionals and volunteers (MomSupport).
- Voice-based journaling: Allow mothers to record thoughts and reflections instead of typing (DearMom).
- Smart Community features: Suggest relevant circles or connections and improve moderation (Community Circles).
- Enhanced MomAid: Deliver personalized exercises and support based on user needs.
- Improved privacy options: Strengthen Anonymous Mode and other data protection measures.

---

## 7. Architecture Diagram
```
┌─────────────────────────────────────────────────────────────┐
│                      MYWOMAN MOBILE APP                     │
│                   (React Native Frontend)                   │
│                                                             │
│  ┌─────────────┐                                            │
│  │ Onboarding  │  <-- Sign Up / Login Page                  │
│  └─────────────┘                                            │
│          │                                                  │
│          ▼                                                  │
│  ┌───────────────────────────────┐                          │
│  │ Navigation & Dashboard         │                         │
│  │ - Displays multiple cards     │                          │
│  │ - Stats, counts, figures      │                          │
│  └───────────────────────────────┘                          │
│      │            │            │                            │
│      ▼            ▼            ▼                            │
│  ┌─────────┐   ┌─────────┐   ┌─────────────┐                │
│  │ MoodCheck│  │Community│  │ MomSupport   │                │
│  │  Page    │  │  Page   │  │  Page        │                │
│  └─────────┘   └─────────┘   └─────────────┘                │
│      ▲             ▲            ▲                           │
│      │             │            │                           │
│  Navigation Tab can also access each page directly          │
│                                                             │
│  ┌─────────────┐   ┌──────────────┐                         │
│  │ MomAid Page │   │ DearMom Page │                         │
│  └─────────────┘   └──────────────┘                         │
│       ▲                  ▲                                  │
│ Navigation Tab direct access                                │
│                                                             │
└───────────────┬─────────────────────────────────────────────┘
                │
                ▼
┌─────────────────────────────────────────────────────────────┐
│ BACKEND (Node.js + Express)                                 │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐            │
│  │ Auth Service│ │ Diary       │ │ Mood Check  │            │
│  │ (Login/Anon)│ │ Service     │ │ Service     │            │
│  └─────────────┘ └─────────────┘ └─────────────┘            │
│         │                 │                │                │
│         └─────────────────┴────────────────┘                │
└───────────────┬─────────────────────────────────────────────┘
                │
                ▼
┌─────────────────────────────────────────────────────────────┐
│ FIREBASE BACKEND SERVICES                                   │
│ ┌────────────┐ ┌───────────────┐ ┌──────────────┐ ┌────────┐│
│ │ Firestore  │ │ Firebase Auth │ │ Firebase     │ │ Cloud  ││
│ │ (NoSQL DB) │ │ (Login/Anon)  │ │ Storage      │ │ Func   ││
│ │ User Data, │ │ Secure Access │ │ Files/Images │ │ API    ││
│ │ Posts,     │ │               │ │ Attachments  │ │ Trigger││
│ │ Diary      │ │               │ │              │ │        ││
│ └────────────┘ └───────────────┘ └──────────────┘ └────────┘│
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│ SUPPORTING & FUTURE SERVICES                                │
│ ┌───────────────┐ ┌───────────────┐ ┌───────────────┐ ┌────┐│
│ │ AI Mood Engine│ │ Telehealth API│ │Notifications  │ │Analy│
│ │ (Future)      │ │ (Future)      │ │ (Firebase)    │ │Repor│
│ └───────────────┘ └───────────────┘ └───────────────┘ └────┘│
└─────────────────────────────────────────────────────────────┘

```