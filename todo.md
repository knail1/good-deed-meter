# Good Deed Meter Todo List

## 1. Project Setup
- [ ] Create GitHub repo
- [ ] Set up basic file structure
  - [ ] Frontend directory
  - [ ] Backend directory
- [ ] Initialize as node project
- [ ] Set up build pipeline
- [ ] Configure linting, formatting, precommit hooks

## 2. Frontend UI Design
- [ ] Wireframe UI components
  - [ ] Submit Good Deed form
  - [ ] Leaderboard
  - [ ] User profile
- [ ] Create React component hierarchy
- [ ] Style with CSS modules or styled-components

## 3. Frontend Functionality
- [ ] Implement controlled form components
- [ ] Add client-side form validation
- [ ] Make API calls to submit data
- [ ] Retrieve and display leaderboard data
- [ ] Retrieve and display user profile

## 4. Backend Setup with Express
- [ ] Create Express app
- [ ] Define API routes
  - [ ] Submit deed
  - [ ] Get leaderboard
  - [ ] Get user profile
- [ ] Add request validation middleware
- [ ] Implement controller functions

## 5. Firebase Integration
- [ ] Set up Firebase project
- [ ] Add Firebase Admin SDK
- [ ] Implement Firebase Auth
- [ ] Store deed data in Firestore
- [ ] Define Firestore data models

## 6. Backend Deed Submission
- [ ] Get authenticated user from Firebase Auth
- [ ] Validate deed submission data
- [ ] Calculate points for deed
- [ ] Save deed to Firestore
- [ ] Update user points total

## 7. Leaderboard Retrieval
- [ ] Query Firestore for top users by points
- [ ] Populate leaderboard data
  - [ ] Rank
  - [ ] Username
  - [ ] Points total
- [ ] Return leaderboard data in API response

## 8. User Profiles
- [ ] Get authenticated user from token
- [ ] Query Firestore for user document
  - [ ] Username
  - [ ] Points
  - [ ] Deed history
- [ ] Return user profile data in API response

## 9. Real-time Deed Updates
- [ ] Use Firestore onSnapshot listeners
- [ ] Subscribe to latest deeds collection
- [ ] Display new deeds in real-time in UI
- [ ] Implement delete functionality for user's deeds
- [ ] Update total points on delete

## 10. Optimization and Polishing
- [ ] Add pagination to leaderboard and history
- [ ] Implement caching layer for leaderboard
- [ ] Add loading states and error handling
- [ ] Ensure responsive, mobile-friendly layout
- [ ] Perform end-to-end tests
