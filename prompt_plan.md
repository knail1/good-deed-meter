# Prompt Plan for Good Deed Meter Development

This document outlines the approach for prompting AI assistance during the development of the Good Deed Meter Android application. Each prompt should focus on a specific, self-contained task while building upon previous work.

## Prompt Structure

For optimal results, structure your prompts to the AI assistant as follows:

1. **Context**: Briefly describe where you are in the development process
2. **Objective**: Clearly state what you want to accomplish in this specific step
3. **Constraints**: Mention any technical limitations or requirements
4. **Previous Work**: Reference relevant previous steps if applicable
5. **Expected Output**: Describe what you expect to receive from the assistant

## Example Prompts for Development Phases

### Project Setup Phase

```
Context: I'm starting the Good Deed Meter Android app development.
Objective: Set up the initial project structure with Kotlin and Jetpack libraries.
Constraints: Target Android 8+ (API 26), use Kotlin, and follow MVVM architecture.
Expected Output: Step-by-step instructions for creating the project and configuring essential dependencies.
```

### Database Implementation Phase

```
Context: The project structure is set up, and I'm ready to implement the database.
Objective: Create the SQLite database schema and DAOs as specified in the spec document.
Previous Work: Project structure is already set up with Kotlin and Jetpack libraries.
Expected Output: Complete code for the database implementation including the LocationDao and VisitDao.
```

### UI Development Phase

```
Context: The database layer is implemented, and I'm ready to work on the UI.
Objective: Create the layout for the Dashboard fragment.
Previous Work: Database implementation is complete.
Expected Output: XML layout code for the Dashboard fragment following Material Design guidelines.
```

### Integration Phase

```
Context: Individual components (database, UI, services) are implemented.
Objective: Integrate the geofencing service with the VisitLogger.
Previous Work: Both the geofencing service and VisitLogger components exist but are not connected.
Expected Output: Code that connects these components and handles the geofence entry events.
```

## Tips for Effective Prompting

1. **One Task at a Time**: Focus each prompt on a single, well-defined task
2. **Be Specific**: Provide details about implementation requirements
3. **Show Context**: Include relevant code snippets when asking for modifications
4. **Iterative Approach**: Build upon previous responses rather than asking for everything at once
5. **Request Explanations**: Ask the AI to explain its implementation choices for better understanding
