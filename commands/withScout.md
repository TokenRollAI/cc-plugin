---
description: Use scout agent to gather information and then proceed with the task
---

This command is designed to tackle complex tasks through a structured exploration framework. It emphasizes meticulous **planning** before action, efficient information gathering via **parallel** `scout` agents, and a gradual approach to the final solution through **iteration**.

### Core Philosophy

1.  **Plan Before You Act**: Before launching any scout, the primary, often ambiguous, goal must be broken down into a set of clear, independently investigable questions.
2.  **Divide and Conquer**: For complex systems, deploying multiple `scout` agents (2-3 maximum) simultaneously, each with a distinct and independent area of responsibility, dramatically increases the efficiency and breadth of information gathering.
3.  **Iterative Convergence**: Accept that a single round of scouting may not solve the entire problem. Each subsequent round should build upon the last, asking more precise and deeper questions to progressively narrow the scope of the unknown and converge on an executable solution.

---

### Workflow

#### Step 1: Deconstruction & Planning

This is the most critical step in the entire process. First, deeply understand the user's end goal, then break it down.

1.  **Define the Ultimate Goal**: Clarify the final task the user wants to accomplish (e.g., "Implement a JWT token refresh feature").
2.  **Deconstruct into Research Areas**: Break the goal down into several key areas that require separate investigation.
    - Area A: Authentication state management and API call logic in the frontend codebase.
    - Area B: Authentication middleware, routes, and token generation/validation logic in the backend codebase.
3.  **Assign Scouts to Each Area**: Assign a `scout` agent to each research area and define its **independent responsibilities**.
    - `Scout A (Frontend)`: Responsible for analyzing the client-side code.
    - `Scout B (Backend)`: Responsible for analyzing the server-side code.
4.  **Formulate Initial Research Questions**: Design a set of highly specific, targeted questions for each `scout`.

#### Step 2: Parallel Scout Deployment

Use the `Task` tool to launch multiple `scout` agents **concurrently**. Use the following template to ensure tasks are independent and comprehensive.

**Parallel Scout Task Template**:

```
=== Global Context ===
Primary Goal: [Describe the overall task to be completed]
Working Directory: [Provide the project root directory]
Known Information: [List any known context to avoid redundant work]

=== Scout A: [Role name for Scout A, e.g., Frontend Auth Investigator] ===
Responsibility: [Define Scout A's scope, e.g., Investigate auth patterns and token handling in the frontend codebase]
Research Questions:
1. [Specific question 1, e.g., After a user logs in, where is the JWT stored (localStorage, Redux store)?]
2. [Specific question 2, e.g., How are authentication headers injected into API requests?]
3. [Specific question 3, e.g., Is there a global interceptor for handling 401/403 errors?]

=== Scout B: [Role name for Scout B, e.g., Backend API Investigator] ===
Responsibility: [Define Scout B's scope, e.g., Investigate the authentication mechanism of the backend API]
Research Questions:
1. [Specific question 1, e.g., Which middleware is responsible for validating incoming JWTs?]
2. [Specific question 2, e.g., What are the endpoints for issuing and refreshing tokens?]
3. [Specific question 3, e.g., What is the token payload structure and how is its expiration set?]

...
```

**Note**: It is recommended to keep the number of parallel scouts at 2-3 to avoid excessive management overhead and information overload.

#### Step 3: Synthesize & Evaluate

Once all `scout` agents have completed their tasks, consolidate their findings.

1.  **Integrate Reports**: Combine the reports from each `scout` to form a holistic view of the current system.
2.  **Identify Key Connections**: Find the links between different areas. For example, how does the frontend API call method found by `Scout A` correspond to the backend route handler found by `Scout B`?
3.  **Discover Knowledge Gaps or Conflicts**: Is the current information complete? Are there any contradictions? Have new, unexpected questions emerged?

#### Step 4: Iterate or Execute

This is a critical decision point. Ask yourself: **"Based on the information gathered, do we have a clear and complete blueprint to begin execution?"**

- **If the answer is "No" (Iterate)**:

  1.  Based on the knowledge gaps identified in the previous step, **formulate a new round of more specific, in-depth research questions**.
  2.  Return to **Step 2** and launch a new round of scouting. The goal of this cycle is to fill the gaps, bringing your understanding closer to an executable state. This loop can be repeated multiple times, with each iteration getting closer to the final goal.

- **If the answer is "Yes" (Execute)**:

  1.  You now have sufficient information. Proceed to the next step to take concrete action.

#### Step 5: Action Phase

Based on the comprehensive information obtained from the scouting rounds, begin executing the user's final request.

- **Implement a Feature**: Develop using the identified code patterns and architecture.
- **Fix a Bug**: Pinpoint the root cause and apply the fix.
- **Write Documentation**: Accurately describe the various parts of the system.

#### Step 6: Summarize & Report

When delivering the final result to the user, clearly explain the entire process.

1.  **Summary of Investigation**: Briefly explain how you used iterative scouting to understand the problem.
2.  **Key Findings**: Present the core discoveries from the `scout` agents.
3.  **Actions Taken**: Detail the specific steps you took based on those findings.
4.  **Final Outcome**: Show the completed code, fix, or documentation.

---

### Example: Research and Implement JWT Token Refresh

**User**: "Use scout to figure out our project's auth logic and then add a JWT token refresh feature."

**Process**:

1.  **Deconstruction & Planning**:

    - **Goal**: Implement JWT refresh.
    - **Areas**: Frontend state management, Backend API.
    - **Scouts**: `Scout A (Frontend)`, `Scout B (Backend)`.
    - **Questions**: Formulate specific questions about token storage, API calls, middleware, and refresh logic.

2.  **First Round of Parallel Scouting**:

    - Launch `Scout A` and `Scout B` simultaneously.
    - `Scout A` finds that the frontend uses an `axios` interceptor and `Redux` to store the token.
    - `Scout B` finds that the backend has `/login` and `/verify-token` endpoints but no `/refresh-token` endpoint.

3.  **Synthesize & Evaluate**:

    - **Findings**: A basic auth flow exists, but the refresh mechanism is missing.
    - **Knowledge Gap**: The backend needs a new `/refresh-token` endpoint. How should it be implemented securely? What should it accept as a credential (e.g., a dedicated refresh token)?

4.  **Iterate or Execute -\> Iterate**:

    - The information is insufficient to proceed directly. A second round of scouting is needed.

5.  **Second Round of Parallel Scouting (More Focused)**:

    - **New Goal**: Design a secure refresh mechanism.
    - `Scout A (Web Search)`: Responsibility changes to "Research industry-standard security strategies for JWT refresh tokens."
    - `Scout B (Backend)`: Responsibility changes to "Find the most suitable location and implementation method to add a `/refresh-token` endpoint in the existing auth code."

6.  **Synthesize & Evaluate**:

    - `Scout A` returns with best practices on using `httpOnly` cookies for storing refresh tokens.
    - `Scout B` identifies that a new method can be added in the `auth.controller.ts` file.
    - **Conclusion**: We now have a clear and secure implementation blueprint.

7.  **Iterate or Execute -\> Execute**:

    - Information is now sufficient. Begin coding.

8.  **Action Phase**:

    - Add the `/refresh-token` endpoint to the backend.
    - Modify the frontend `axios` interceptor to call the refresh endpoint on a 401 error and then retry the original request.

9.  **Summarize & Report**:

    - Explain to the user: "Through two rounds of scouting, we first understood the existing auth structure, then researched a secure refresh strategy, and finally completed the implementation..."
