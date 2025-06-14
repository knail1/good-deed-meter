# Project Blueprint

## 1. Set up project skeleton 
- Create GitHub repo
- Set up basic file structure
  - Frontend directory 
  - Backend directory
- Initialize as node project
- Set up build pipeline
- Configure linting, formatting, precommit hooks
<prompt>
```
Set up a new node project with a basic file structure including frontend and backend directories. Initialize it as a git repo. Configure ESLint, Prettier, and Husky for linting, formatting and precommit hooks. Set up a basic webpack build pipeline. Include package.json, tsconfig, README, and gitignore.
```
</prompt>

## 2. Design frontend UI 
- Wireframe UI components
  - Submit Good Deed form
  - Leaderboard
  - User profile 
- Create React component hierarchy
- Style with CSS modules or styled-components
<prompt> 
```
Create a wireframe for the main UI components including a form to submit good deeds, a leaderboard showing top users, and a user profile page. Design a React component hierarchy to implement this UI. Add styling using CSS modules or styled-components. 
```
</prompt>

## 3. Implement frontend functionality
- Implement controlled form components 
- Add client-side form validation
- Make API calls to submit data
- Retrieve and display leaderboard data
- Retrieve and display user profile
<prompt>
```
Implement the Good Deed submission form using controlled React components. Add client-side validation with error handling. Make axios API calls to submit the form data to the backend. Retrieve leaderboard and user profile data from API endpoints and display it in the relevant components.  
```
</prompt>

## 4. Set up backend with Express
- Create Express app
- Define API routes 
  - Submit deed
  - Get leaderboard
  - Get user profile
- Add request validation middleware 
- Implement controller functions
<prompt>
```
Set up an Express server with routes for submitting a good deed, retrieving the leaderboard, and getting a user's profile. Add middleware for request validation and error handling. Implement the controller functions for each route, with stub return values.
```  
</prompt>

## 5. Integrate with Firebase
- Set up Firebase project
- Add Firebase Admin SDK 
- Implement Firebase Auth
- Store deed data in Firestore
- Define Firestore data models
<prompt>
```
Create a new Firebase project and integrate the Firebase Admin SDK into the backend. Implement Firebase Authentication for user sign-up and login. Store submitted good deeds in Firestore, with fields for the deed description, timestamp, user, and points. Define the Firestore data models.
```
</prompt>

## 6. Implement backend deed submission 
- Get authenticated user from Firebase Auth
- Validate deed submission data
- Calculate points for deed
- Save deed to Firestore
- Update user points total
<prompt>
```
In the backend deed submission route, get the authenticated user from the Firebase Auth token. Validate that the submitted deed data is properly formatted. Calculate the points for the deed based on predefined rules. Save the deed to Firestore, including the user ID. Increment the user's total points in Firestore.
```
</prompt>

## 7. Implement leaderboard retrieval
- Query Firestore for top users by points 
- Populate leaderboard data
  - Rank
  - Username
  - Points total
- Return leaderboard data in API response
<prompt>
```
Implement the leaderboard API endpoint to retrieve the top users ranked by their total points. Query Firestore to get users ordered by descending point totals. Map the results to an array of leaderboard entries with the rank, username, and total points. Return this leaderboard data in the API response.  
```
</prompt>

## 8. Implement user profiles
- Get authenticated user from token
- Query Firestore for user document 
  - Username
  - Points 
  - Deed history
- Return user profile data in API response
<prompt>
```
For the user profile endpoint, get the authenticated user from the Firebase Auth token. Query Firestore for that user's document. Retrieve their username, total points, and history of submitted deeds. Return this user profile data in the formatted API response.
```  
</prompt>

## 9. Add real-time deed updates
- Use Firestore onSnapshot listeners 
- Subscribe to latest deeds collection
- Display new deeds in real-time in UI
- Implement delete functionality for user's deeds
- Update total points on delete
<prompt>
```
Use Firestore's onSnapshot listeners to subscribe to real-time updates from the deeds collection. When a new deed is added, display it immediately in the UI, without requiring a refresh. Allow users to delete their own submitted deeds. Decrement their points total upon deletion. Ensure the leaderboard stays updated.
```
</prompt>

## 10. Optimize and polish
- Add pagination to leaderboard and history
- Implement caching layer for leaderboard
- Add loading states and error handling
- Ensure responsive, mobile-friendly layout 
- Perform end-to-end tests
<prompt>
```
Optimize the app by adding pagination to the leaderboard and deed history views. Implement a caching layer on the backend for expensive leaderboard queries. Thoroughly handle loading states and error cases on the frontend. Ensure the UI is responsive and mobile-friendly. Write full end-to-end tests for all key user flows.
```
</prompt>
