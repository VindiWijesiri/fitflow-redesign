# FitFlow Backend API

Node.js + Express REST API backend for FitFlow fitness tracking application.

## Features

- 🔐 JWT authentication and authorization
- 📊 Workout and nutrition data management
- 🏆 Social features and challenge leaderboards
- 🤖 AI workout recommendation endpoints
- 🔥 Firebase integration (Auth, Firestore, Storage)
- 🛡️ Security middleware (helmet, CORS, rate limiting)
- 📝 Request validation with Joi
- 🚀 Serverless deployment ready (Firebase Functions)

## Prerequisites

- Node.js >= 18.0.0
- npm >= 9.0.0
- Firebase account and project

## Installation

```bash
# Install dependencies
npm install

# Copy environment template
cp .env.example .env

# Edit .env with your configuration
```

## Environment Variables

Create `.env` file:

```env
# Server
NODE_ENV=development
PORT=3000

# Firebase Admin SDK
FIREBASE_PROJECT_ID=your-project-id
FIREBASE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n"
FIREBASE_CLIENT_EMAIL=firebase-adminsdk@your-project.iam.gserviceaccount.com

# JWT
JWT_SECRET=your-super-secret-jwt-key-change-this
JWT_EXPIRES_IN=1h
JWT_REFRESH_EXPIRES_IN=7d

# Rate Limiting
RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX_REQUESTS=100

# Database
FIRESTORE_EMULATOR_HOST=localhost:8080  # Only for local development
```

## Running the Server

### Development Mode

```bash
# Run with hot reload
npm run dev

# Server starts at http://localhost:3000
```

### Production Mode

```bash
# Build TypeScript
npm run build

# Start production server
npm start
```

### Using Firebase Emulators (Local Development)

```bash
# Install Firebase CLI
npm install -g firebase-tools

# Login to Firebase
firebase login

# Start emulators
firebase emulators:start

# In another terminal, run backend
npm run dev
```

## Testing

```bash
# Run all tests
npm test

# Watch mode
npm run test:watch

# Coverage report
npm run test:coverage

# Integration tests only
npm run test:integration
```

## Linting and Formatting

```bash
# Run linter
npm run lint

# Auto-fix issues
npm run lint:fix

# Format code
npm run format
```

## API Endpoints

### Authentication

```
POST   /api/auth/register       - Register new user
POST   /api/auth/login          - Login user
POST   /api/auth/logout         - Logout user
POST   /api/auth/refresh-token  - Refresh JWT token
POST   /api/auth/forgot-password - Request password reset
POST   /api/auth/reset-password  - Reset password
```

### Users

```
GET    /api/users/:id           - Get user profile
PUT    /api/users/:id           - Update user profile
DELETE /api/users/:id           - Delete user account
GET    /api/users/:id/stats     - Get user statistics
PUT    /api/users/:id/preferences - Update preferences
```

### Workouts

```
GET    /api/workouts            - Get user workouts (paginated)
POST   /api/workouts            - Create new workout
GET    /api/workouts/:id        - Get workout by ID
PUT    /api/workouts/:id        - Update workout
DELETE /api/workouts/:id        - Delete workout
POST   /api/workouts/:id/complete - Mark workout as completed
GET    /api/workouts/stats      - Get workout statistics
```

### Nutrition

```
GET    /api/nutrition/logs      - Get nutrition logs
POST   /api/nutrition/logs      - Create nutrition log
GET    /api/nutrition/logs/:id  - Get nutrition log by ID
PUT    /api/nutrition/logs/:id  - Update nutrition log
DELETE /api/nutrition/logs/:id  - Delete nutrition log
POST   /api/nutrition/analyze   - Analyze food image
GET    /api/nutrition/stats     - Get nutrition statistics
```

### Social

```
GET    /api/social/feed         - Get social feed (paginated)
POST   /api/social/posts        - Create post
GET    /api/social/posts/:id    - Get post by ID
PUT    /api/social/posts/:id    - Update post
DELETE /api/social/posts/:id    - Delete post
POST   /api/social/posts/:id/like - Like/unlike post
POST   /api/social/posts/:id/comment - Add comment
GET    /api/social/friends      - Get friends list
POST   /api/social/friends/:id  - Send friend request
```

### Challenges

```
GET    /api/challenges          - Get active challenges
GET    /api/challenges/:id      - Get challenge details
POST   /api/challenges/:id/join - Join challenge
DELETE /api/challenges/:id/leave - Leave challenge
GET    /api/challenges/:id/leaderboard - Get leaderboard
```

### AI Recommendations

```
POST   /api/ai/workout-plan     - Generate personalized workout plan
POST   /api/ai/exercise-suggestions - Get exercise suggestions
POST   /api/ai/nutrition-plan   - Generate nutrition recommendations
```

## Project Structure

```
src/
├── controllers/      # Request handlers
│   ├── auth.controller.ts
│   ├── workout.controller.ts
│   ├── nutrition.controller.ts
│   └── social.controller.ts
├── routes/          # API routes
│   ├── auth.routes.ts
│   ├── workout.routes.ts
│   └── ...
├── middleware/      # Express middleware
│   ├── auth.middleware.ts
│   ├── validation.middleware.ts
│   ├── error.middleware.ts
│   └── rateLimit.middleware.ts
├── services/        # Business logic
│   ├── auth.service.ts
│   ├── workout.service.ts
│   ├── ai.service.ts
│   └── ...
├── models/          # Data models and schemas
│   ├── user.model.ts
│   ├── workout.model.ts
│   └── ...
├── config/          # Configuration
│   ├── firebase.config.ts
│   ├── database.config.ts
│   └── app.config.ts
├── utils/           # Utility functions
│   ├── logger.ts
│   ├── validators.ts
│   └── helpers.ts
└── index.ts         # Entry point
```

## Security Features

- **Helmet**: HTTP headers security
- **CORS**: Cross-origin resource sharing
- **Rate Limiting**: Prevent abuse
- **JWT Authentication**: Secure token-based auth
- **Input Validation**: Joi schema validation
- **SQL Injection Prevention**: Parameterized Firestore queries
- **XSS Protection**: Content sanitization
- **HTTPS Enforcement**: In production

## Deployment

### Firebase Functions

```bash
# Install Firebase CLI
npm install -g firebase-tools

# Login
firebase login

# Initialize Functions
firebase init functions

# Deploy
npm run deploy
```

### Traditional Hosting (Heroku, AWS, DigitalOcean)

```bash
# Build
npm run build

# Set environment variables on your platform

# Start
npm start
```

## Logging

Winston logger configured with different levels:

```typescript
logger.error('Error message');
logger.warn('Warning message');
logger.info('Info message');
logger.debug('Debug message');
```

Logs stored in `logs/` directory:
- `error.log`: Error-level logs
- `combined.log`: All logs

## Error Handling

Centralized error handling middleware:

```typescript
// Custom error example
throw new ApiError(404, 'Workout not found');

// Automatic validation errors
// Joi validation errors automatically formatted
```

## Database Schema

See [Firestore Collections](../docs/architecture.md#firestore-database) in architecture documentation.

## Performance Optimization

- Response compression with gzip
- Database query caching
- Pagination for large datasets
- Connection pooling
- Lazy loading of services

## Monitoring

- Request logging with Morgan
- Error tracking (integrate Sentry)
- Performance monitoring (integrate Firebase Performance)
- Health check endpoint: `GET /health`

## Contributing

1. Create feature branch
2. Write tests for new features
3. Ensure all tests pass: `npm test`
4. Run linter: `npm run lint`
5. Submit Pull Request

## License

MIT License - see [LICENSE](../LICENSE) for details.

---

**Runtime**: Node.js 18+  
**Framework**: Express 4.x  
**Language**: TypeScript  
**Database**: Firebase/Firestore
