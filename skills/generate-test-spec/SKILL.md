---
name: generate-test-spec
description: Generate test specifications for native mobile apps and mobile web apps as local markdown files under a .test folder.
---

# Test Spec Generation

Generate user-focused test specifications for native mobile apps and mobile web apps, and save them as markdown files under a `.test/` folder in the project root.

## Core Principles

**Test user-facing functionality only:**
- ✅ User interactions, gestures, and navigation (tap, swipe, scroll, back navigation)
- ✅ End-to-end screen and feature functionality
- ✅ Form input, validation, search, filters, and interactive UI elements
- ✅ Mobile-specific behaviors (keyboard input, screen transitions, orientation changes)
- ✅ If sign-in is required for a flow, include login as a precondition and verify the in-app outcome after authentication
- ✅ App relaunch is allowed only when the scenario explicitly requires it; specs must explain why relaunch is needed and what must be re-validated after relaunch
- ❌ APIs, backend endpoints, or server-side logic
- ❌ Validating third-party authentication provider internals (OAuth, Google, Facebook, GitHub)
- ❌ App internals, logs, background services, or implementation details
- ❌ Non-existent screens or functionality not accessible to users

**Quality over quantity:**
- Maximum 5 tests per feature (adjust to actual complexity)
- Understand the user flow thoroughly before generating tests
- Check for existing `.test/*.md` files to avoid duplicate coverage
- Focus on critical workflows users actually need

## User Input & Credentials

Test specs often reference user-specific data such as login credentials, form values, or environment-specific details. Follow these rules:

1. **Never guess or fabricate** credentials, emails, passwords, or account-specific values when writing specs.
2. **Ask the user** if any required test input is unknown — this includes but is not limited to:
   - Login credentials (username, email, password)
   - Form field values (addresses, phone numbers, payment details)
   - Environment-specific URLs or endpoints
   - Account-specific data (user IDs, org names, project names)
3. **Ask during Step 2 (Auth & Preconditions Intake)** — identify all required user inputs while planning and resolve them before writing specs.
4. **Use the exact values provided** by the user in test specs — do not substitute or anonymize them.

> Not asking the user for unknown inputs is a **blocker** — do not skip this step or use placeholder values like `user@test.com` / `password123` unless the user explicitly provides them.

## Workflow Steps

### Step 1 — Analyze the Feature

Read relevant source files to understand the user-facing functionality:
- Identify all pages/screens involved
- Map out user flows and interactions
- Note form fields, buttons, navigation paths, and conditional states
- Refer to the **Test user-facing functionality only** checklist in **Core Principles**
- If any part of the user flow is ambiguous, ask clarifying questions before continuing

### Step 2 — Auth & Preconditions Intake (Required)

Before planning tests, explicitly collect setup requirements from the user:
- Determine whether authentication is required for any target flow.
- If authentication is required, ask for the approved login method and test credentials (or confirm how credentials will be supplied during execution).
- Ask for required account state, seed data, permissions, feature flags, and environment assumptions needed for the flows.
- Ask for MFA/OTP handling instructions if applicable.

Hard rules:
- Never invent credentials, OTP codes, user identities, or test data.
- Never guess login paths or random account details.
- If any required credential or precondition is missing, stop and ask the user before continuing.

### Step 3 — Plan the Tests

For each user flow identified, plan a test with:
- **Name**: A clear, user-action description (e.g., "User filters products by price range")
- **Summary**: A one-line objective describing what the test validates

### Step 4 — Review and Confirm the Test Plan

Present the planned tests to the user for review and confirmation:
- Display all planned tests with their **Name** and **Summary**
- List any preconditions (auth, navigation, data state, permissions)
- Allow the user to confirm, edit, add, or remove tests
- Incorporate the user's feedback and finalize the test plan before writing test files
- Do not write any test files until the user confirms the full plan

### Step 5 — Check for Existing Tests

Before creating tests, check whether any `.test/*.md` files already exist in the project:
- Read the `.test/` directory if it exists
- Compare planned test names against existing file names
- Skip any test that duplicates existing coverage

### Step 6 — Create the `.test/` Folder

If the `.test/` folder does not exist at the project root, create it.

### Step 7 — Write Test Spec Files

For each planned test, create a markdown file in `.test/` using the template format below.

**File naming**: Use `snake_case` derived from the test name.
- Example: "Add Hindi Language" → `.test/add_hindi_language.md`
- Example: "User filters products by price range" → `.test/user_filters_products_by_price_range.md`

## Test Spec Markdown Template

Every test spec file must follow this exact structure:

```markdown
# Test Case: <Title>

## Objective
<One or two sentences describing what the test validates.>

## Preconditions
<Bulleted list of conditions that must be true before the test starts.>

## Test Flow
<Numbered list of sequential user actions to perform.>

## Expected Result
<Bulleted list of observable outcomes after executing the test flow.>

## Verification
<Bulleted list of specific checks to confirm correctness.>

## Pass Criteria
<Bulleted list — the test passes when these conditions are met.>

## Fail Criteria
<Bulleted list — the test fails when any of these conditions occur.>
```

### Writing Good Test Flows

The **Test Flow** drives the entire test execution. Guidelines:

- **Be specific**: "Tap the 'Add to Cart' button on the product detail screen" not "Add item"
- **Include verification inline where helpful**: "Verify the cart badge shows '1' after adding"
- **Describe the full flow**: Navigate → Interact → Assert
- **Reference actual UI elements**: Use text labels, button names, and screen titles from the source code
- **Keep it sequential**: Write steps in execution order
- **Bold UI element names**: Use **bold** for screen names, buttons, and labels referenced in the flow
- **Do not include "open or launch the app"**: Begin directly with navigation or interaction steps — app launch is assumed

### Example Test Spec

**File:** `.test/add_hindi_language.md`

```markdown
# Test Case: Add Hindi Language in Wikipedia Settings

## Objective
Verify that the Hindi language can be added successfully in Wikipedia after skipping onboarding.

## Preconditions
- Wikipedia app is installed and launched
- User is on the onboarding screen or first-launch flow

## Test Flow
1. Open Wikipedia.
2. Skip onboarding.
3. Go to **Settings**.
4. Add **Hindi** language.
5. Verify that the Hindi language is added successfully.

## Expected Result
- User is able to skip onboarding and reach the main app.
- In **Settings**, Hindi can be selected and added.
- Hindi appears in the list of added/preferred languages.
- The app reflects that Hindi was added successfully.

## Verification
- Confirm **Hindi** is visible in the selected languages list.
- Confirm no error message appears while adding the language.

## Pass Criteria
- Hindi is added and displayed successfully in Wikipedia settings.

## Fail Criteria
- Hindi does not appear after adding.
- An error is shown.
- The app does not save the language selection.
```

## Test Prioritization

1. **Critical user flows** — onboarding, login, checkout, submissions
2. **Interactive features and gestures** — tap, swipe, scroll, search, filters
3. **Form input and validation** — errors, required fields, keyboard interaction
4. **Navigation flows** — screen transitions, back navigation, tab switching
5. **Conditional UI states** — loading, empty, error, success states
6. **Mobile-specific behaviors** — orientation changes, keyboard, dynamic UI

## Quick Checklist

- [ ] Feature analyzed, all user flows mapped
- [ ] Credentials and setup preconditions gathered as needed
- [ ] Existing `.test/*.md` files checked — no duplicates
- [ ] `.test/` folder created if missing
- [ ] Each test name describes a user action (not implementation detail)
- [ ] Test flows are specific, sequential, and include verification steps
- [ ] Every test spec follows the markdown template exactly
- [ ] Files are named in `snake_case` matching the test name
- [ ] No invented credentials, OTPs, or random login attempts

## Anti-Patterns

❌ **Vague test flows**: "Test the login" → No verification, no specifics
✅ **Good test flows**: "Tap Login, enter the provided test credentials, tap Submit, verify home screen loads with welcome message"

❌ **Testing implementation**: "Verify store updates" → Backend/state concern
✅ **Testing user outcome**: "Verify the item count in the cart badge updates to 2"

❌ **One mega-test**: A single test covering 15 different features
✅ **Focused tests**: Each test covers one user workflow (3–8 steps)

❌ **Invented authentication data**: "Try a few emails and passwords until login works"
✅ **Controlled authentication**: Use only user-provided credentials and documented login handling

❌ **Unjustified relaunch**: "Relaunch app and continue" without reason or post-relaunch checks
✅ **Intentional relaunch**: Relaunch only when required and verify the expected restored state after relaunch
