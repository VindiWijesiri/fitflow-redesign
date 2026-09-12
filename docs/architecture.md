# FitFlow System Architecture

## Overview

This document describes the high-level architecture of the FitFlow redesign, including system components, data flows, security considerations, and scalability patterns. The architecture is designed to support real-time social features, AI-powered personalization, offline capabilities, and privacy-focused data processing.

---

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                         FitFlow Users                            │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│                   React Native Mobile App                        │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  UI Components  │  Screens  │  Navigation  │  State Mgmt  │  │
│  └──────────────────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │     TensorFlow Lite        │         ML Kit              │  │
│  │   (On-Device AI/ML)        │  (Food Recognition)         │  │
│  └──────────────────────────────────────────────────────────┘  │
└────────────────────┬──────────────────────┬────────────────────┘
                     │                      │
                     ▼                      ▼
┌────────────────────────────────┐  ┌──────────────────────────┐
│   Node.js + Express Backend    │  │   Firebase Services      │
│  ┌──────────────────────────┐  │  │ ┌──────────────────────┐ │
│  │  REST API Endpoints      │  │  │ │  Authentication      │ │
│  │  Business Logic          │  │  │ │  Firestore Database  │ │
│  │  Data Validation         │  │  │ │  Cloud Functions     │ │
│  │  AI Recommendations      │  │  │ │  Cloud Storage       │ │
│  └──────────────────────────┘  │  │ │  Cloud Messaging     │ │
└────────────────┬───────────────┘  │ │  Real-time Sync      │ │
                 │                  │ └──────────────────────┘ │
                 └──────────────────┴──────────────────────────┘
                                    │
                                    ▼
                    ┌───────────────────────────────┐
                    │  External Services (Optional)  │
                    │  - Analytics                   │
                    │  - Monitoring                  │
                    │  - Error Tracking              │
                    └───────────────────────────────┘
```

---

## Component Architecture

### 1. Mobile Application Layer (React Native)

**Purpose**: User-facing mobile application for iOS and Android

**Components**:
- **UI Components**: Reusable components (buttons, cards, forms, charts)
- **Screens**: Full-page views (Home, Workout, Social, Profile, Nutrition)
- **Navigation**: React Navigation for app navigation
- **State Management**: Redux/Context API for global state
- **Services**: API clients, Firebase SDK wrappers, utility functions
- **Hooks**: Custom React hooks for business logic
- **Assets**: Images, fonts, icons, animations

**Key Responsibilities**:
- User interface and experience
- Local state management
- Offline data caching
- On-device AI/ML inference
- Real-time data synchronization

**Technologies**:
- React Native
- TypeScript
- React Navigation
- Redux/Context API
- TensorFlow Lite
- ML Kit

---

### 2. AI/ML Layer (On-Device)

**Purpose**: Privacy-preserving AI processing directly on user devices

#### TensorFlow Lite Integration

**Use Cases**:
1. **Workout Personalization**
   - Input: User history, fitness level, goals
   - Output: Personalized workout recommendations
   
2. **Difficulty Adaptation**
   - Input: Performance metrics, completion rates
   - Output: Adjusted workout difficulty

3. **Exercise Form Analysis**
   - Input: Device sensor data (accelerometer, gyroscope)
   - Output: Form feedback and corrections

**Model Architecture**:
```
User Data → Preprocessing → TF Lite Model → Inference → Recommendations
```

#### ML Kit Integration

**Use Cases**:
1. **Food Recognition**
   - Input: Camera image
   - Output: Identified food items

2. **Barcode Scanning**
   - Input: Camera image of barcode
   - Output: Product information

**Processing Flow**:
```
Camera → Image Capture → ML Kit Processing → Food Labels → Nutrition Data
```

---

### 3. Backend API Layer (Node.js + Express)

**Purpose**: Server-side business logic, data validation, and API endpoints

**API Endpoints**:

```
Authentication:
POST   /api/auth/register
POST   /api/auth/login
POST   /api/auth/logout
POST   /api/auth/refresh-token

Users:
GET    /api/users/:id
PUT    /api/users/:id
DELETE /api/users/:id
GET    /api/users/:id/stats

Workouts:
GET    /api/workouts
POST   /api/workouts
GET    /api/workouts/:id
PUT    /api/workouts/:id
DELETE /api/workouts/:id
POST   /api/workouts/:id/complete

Nutrition:
GET    /api/nutrition/logs
POST   /api/nutrition/logs
GET    /api/nutrition/logs/:id
PUT    /api/nutrition/logs/:id
DELETE /api/nutrition/logs/:id
POST   /api/nutrition/analyze-image

Social:
GET    /api/social/feed
POST   /api/social/posts
GET    /api/social/posts/:id
POST   /api/social/posts/:id/like
POST   /api/social/posts/:id/comment

Challenges:
GET    /api/challenges
GET    /api/challenges/:id
POST   /api/challenges/:id/join
GET    /api/challenges/:id/leaderboard

AI Recommendations:
POST   /api/ai/workout-recommendations
POST   /api/ai/nutrition-suggestions
```

**Architecture Pattern**: Model-View-Controller (MVC)

```
Routes → Controllers → Services → Models → Database
```

**Key Responsibilities**:
- Request validation and sanitization
- Business logic execution
- Data transformation
- External API integration
- Caching and optimization
- Rate limiting and security

---

### 4. Firebase Services Layer

#### Firebase Authentication

**Purpose**: Secure user authentication and authorization

**Supported Methods**:
- Email/Password
- Google Sign-In
- Apple Sign-In
- Facebook Login
- Phone Number (SMS)
- Anonymous Authentication

**Security Features**:
- JWT token-based authentication
- Automatic token refresh
- Session management
- Multi-factor authentication (MFA)

#### Firestore Database

**Purpose**: Real-time NoSQL database

**Data Collections**:

```
users/
  {userId}/
    profile: { name, email, age, gender, ... }
    preferences: { units, notifications, privacy, ... }
    stats: { totalWorkouts, streak, ... }

workouts/
  {workoutId}/
    userId: reference
    type: string
    exercises: array
    duration: number
    date: timestamp
    metrics: { calories, distance, ... }

nutrition/
  {nutritionId}/
    userId: reference
    meals: array
    date: timestamp
    totals: { calories, protein, carbs, fats }

social/
  posts/
    {postId}/
      userId: reference
      content: string
      type: string
      likes: array
      comments: array
      timestamp: timestamp

challenges/
  {challengeId}/
    name: string
    description: string
    startDate: timestamp
    endDate: timestamp
    participants: array
    leaderboard: array
```

**Security Rules** Example:
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users can only read/write their own data
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
    
    // Anyone authenticated can read posts
    match /social/posts/{postId} {
      allow read: if request.auth != null;
      allow create: if request.auth != null;
      allow update, delete: if request.auth != null && 
                             resource.data.userId == request.auth.uid;
    }
  }
}
```

#### Cloud Functions

**Purpose**: Serverless backend logic triggered by events

**Function Types**:

1. **Scheduled Functions**:
```javascript
// Daily workout reminders
exports.sendDailyReminders = functions.pubsub
  .schedule('every day 08:00')
  .timeZone('UTC')
  .onRun(async (context) => {
    // Send push notifications to users
  });
```

2. **Database Triggers**:
```javascript
// Update challenge leaderboard when workout completed
exports.updateLeaderboard = functions.firestore
  .document('workouts/{workoutId}')
  .onCreate(async (snap, context) => {
    // Update challenge rankings
  });
```

3. **HTTP Functions**:
```javascript
// Generate AI workout plan
exports.generateWorkoutPlan = functions.https
  .onCall(async (data, context) => {
    // Call AI service and return plan
  });
```

4. **Auth Triggers**:
```javascript
// Send welcome email on new user registration
exports.onUserCreate = functions.auth
  .user()
  .onCreate(async (user) => {
    // Send welcome email
  });
```

#### Cloud Storage

**Purpose**: Store user-generated content

**Bucket Structure**:
```
fitflow-storage/
  users/
    {userId}/
      profile-pictures/
      workout-photos/
      food-images/
```

**Security Rules** Example:
```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /users/{userId}/{allPaths=**} {
      allow read: if request.auth != null;
      allow write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

#### Cloud Messaging (FCM)

**Purpose**: Push notifications

**Notification Types**:
- Workout reminders
- Social interactions (likes, comments)
- Challenge updates
- Milestone achievements
- Friend activity

---

## Data Flow Diagrams

### 1. User Authentication Flow

```
User
  │
  ├─> Enter credentials
  │
  ▼
React Native App
  │
  ├─> Firebase Auth SDK
  │
  ▼
Firebase Authentication
  │
  ├─> Verify credentials
  ├─> Generate JWT token
  │
  ▼
App (Authenticated)
  │
  ├─> Store token locally
  ├─> Access protected resources
```

### 2. Workout Logging Flow

```
User
  │
  ├─> Log workout
  │
  ▼
React Native App
  │
  ├─> Validate data locally
  ├─> Store in local cache (offline support)
  │
  ▼
Node.js Backend API
  │
  ├─> Validate workout data
  ├─> Save to Firestore
  │
  ▼
Firestore Database
  │
  ├─> Store workout document
  ├─> Trigger Cloud Function
  │
  ▼
Cloud Function
  │
  ├─> Update user statistics
  ├─> Update challenge leaderboard
  ├─> Send notifications to friends
```

### 3. AI Workout Recommendation Flow

```
User
  │
  ├─> Request personalized workout
  │
  ▼
React Native App
  │
  ├─> Gather user data (history, preferences, goals)
  ├─> Load TensorFlow Lite model
  │
  ▼
TensorFlow Lite (On-Device)
  │
  ├─> Process user data
  ├─> Generate recommendations
  │
  ▼
React Native App
  │
  ├─> Display personalized workout plan
  ├─> Store plan locally for offline access
```

### 4. Food Recognition Flow

```
User
  │
  ├─> Take photo of food
  │
  ▼
React Native App
  │
  ├─> Capture image
  ├─> Pass to ML Kit
  │
  ▼
ML Kit (On-Device)
  │
  ├─> Analyze image
  ├─> Identify food items
  │
  ▼
React Native App
  │
  ├─> Display recognized foods
  ├─> Fetch nutrition data
  │
  ▼
Node.js Backend API
  │
  ├─> Look up nutrition information
  ├─> Return calorie and macro data
  │
  ▼
React Native App
  │
  ├─> Display nutrition information
  ├─> Save nutrition log
```

### 5. Social Feed Real-time Updates Flow

```
User A
  │
  ├─> Create post / Complete workout
  │
  ▼
React Native App
  │
  ├─> Create social post
  │
  ▼
Firestore Database
  │
  ├─> Write new post document
  ├─> Real-time sync enabled
  │
  ▼
User B's App
  │
  ├─> Firestore listener detects change
  ├─> Automatically update UI
  │
  ▼
Display new post in feed (Real-time)
```

---

## Security Architecture

### 1. Authentication & Authorization

- **JWT Tokens**: Firebase Authentication tokens for API access
- **Token Expiration**: 1-hour token lifetime with automatic refresh
- **Secure Storage**: Tokens stored in secure device storage (Keychain/KeyStore)
- **HTTPS Only**: All API communication over HTTPS

### 2. Data Protection

- **Encryption in Transit**: TLS 1.3 for all network communication
- **Encryption at Rest**: Firebase default encryption for stored data
- **On-Device Processing**: AI/ML runs locally to protect sensitive data
- **Minimal Data Collection**: Only collect data necessary for features

### 3. API Security

- **Rate Limiting**: Prevent abuse with request rate limits
- **Input Validation**: Sanitize all user inputs
- **SQL Injection Prevention**: Use parameterized queries (Firestore)
- **XSS Prevention**: Content Security Policy headers
- **CORS Configuration**: Restrict API access to mobile app

### 4. Privacy Controls

- **Granular Permissions**: User controls for data sharing
- **Privacy Settings**: Control visibility of workouts and profile
- **Data Export**: GDPR-compliant data export feature
- **Account Deletion**: Complete data removal on account deletion

---

## Scalability Patterns

### 1. Database Scaling

- **Firestore**: Automatically scales horizontally
- **Indexing**: Create indexes for frequently queried fields
- **Denormalization**: Duplicate data to reduce query complexity
- **Pagination**: Limit query results (25-50 items per page)

### 2. Backend Scaling

- **Horizontal Scaling**: Add more Node.js instances as load increases
- **Load Balancing**: Distribute requests across instances
- **Caching**: Redis for frequently accessed data
- **CDN**: CloudFlare/Firebase CDN for static assets

### 3. Real-time Scaling

- **Firebase**: Handles up to 1 million simultaneous connections
- **Connection Pooling**: Reuse database connections
- **Selective Listeners**: Only subscribe to relevant data

### 4. AI/ML Scaling

- **On-Device**: Scales naturally with user base
- **Model Optimization**: Use quantized models for smaller size
- **Progressive Loading**: Download models only when needed

---

## Monitoring & Observability

### Key Metrics

1. **Performance Metrics**:
   - API response time
   - App launch time
   - Screen render time
   - Database query latency

2. **Business Metrics**:
   - Daily Active Users (DAU)
   - Workout completion rate
   - Social engagement rate
   - Retention rate

3. **Error Tracking**:
   - Crash rate
   - API error rate
   - Failed authentication attempts

### Tools

- **Firebase Analytics**: User behavior and app usage
- **Firebase Crashlytics**: Crash reporting
- **Firebase Performance Monitoring**: App performance metrics
- **Backend Logging**: Structured logging with Winston

---

## Deployment Architecture

### Environments

1. **Development**: Local development with Firebase emulators
2. **Staging**: Firebase project for testing
3. **Production**: Firebase project for live users

### CI/CD Pipeline

```
Git Push
  ↓
GitHub Actions
  ↓
Run Tests
  ↓
Build App (iOS & Android)
  ↓
Deploy Backend (Firebase Functions)
  ↓
Deploy to App Stores (TestFlight / Google Play Beta)
```

---

## Disaster Recovery

### Backup Strategy

- **Firestore**: Automatic daily backups
- **User Data**: Export functionality for users
- **Code**: Git version control with GitHub

### Recovery Plan

1. **Data Loss**: Restore from daily Firestore backups
2. **Service Outage**: Failover to backup Firebase project
3. **Security Breach**: Rotate all credentials, audit logs

---

## Future Architecture Considerations

### Phase 1 (Current)
- Monolithic Node.js backend
- Firebase for all services
- On-device AI only

### Phase 2 (Growth)
- Microservices architecture
- Dedicated AI inference servers
- Advanced analytics pipeline

### Phase 3 (Scale)
- Kubernetes orchestration
- Multi-region deployment
- Custom ML infrastructure
- GraphQL API layer

---

**Last Updated**: September 12, 2026  
**Document Version**: 1.0  
**Author**: FitFlow Development Team
