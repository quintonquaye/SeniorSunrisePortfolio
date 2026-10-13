# 03 — Project Plan

> **Stage 3 of 15 | Demonstrates: Project Planning & Work Breakdown**

---

## Epic Breakdown

```
EPIC 1: Data Layer
 ├── Task 1.1 — Define Medication data model (medication fields, data types, data serialization)
 ├── Task 1.2 — Define MedicationLog data model
 ├── Task 1.3 — Define MedicationStats computed model
 ├── Task 1.4 - setup Sembast database service with AES encryption
 └── Task 1.5 - Implement MedicationRepository (CRUD + log operations)

EPIC 2: Business Logic
 ├── Task 2.1 — Implement MedicationController (ChangeNotifier/Provider)
 ├── Task 2.2 — Implement add, load, update, delete medication operations 
 ├── Task 2.3 — Implement markAsTaken, markAsSkipped, undoLog
 ├── Task 2.4 - Implement adherence calculation (per medication + global)
 ├── Task 2.5 - Implement stock count decrement/restore on taken/undo actions
 └── Task 2.6 - Implement refill operation

EPIC 3: Notifications
 ├── Task 3.1 — Setup flutter_local_notifications package
 ├── Task 3.2 — Implement NotificationScheduler
 ├── Task 3.3 - Schedule reminders when medication is saved
 ├── Task 3.4 - Cancel reminders when medication is deleted or edited
 └── Task 3.5 - Handle notification action stream (mark taken from notification) **future**

EPIC 4: User Interface
 ├── Task 4.1 — Implement MedicationManagerScreen (list View, empty state)
 ├── Task 4.2 — Implement AddMedicationScreen (form with validation)
 ├── Task 4.3 - Implement MedicationHistoryScreen (date range + log list)
 ├── Task 4.4 - Add "Mark as Taken / Skip / Undo" buttons to list items on MedicationManagerScreen
 └── Task 4.5 - Display adherence percentage and stock count on each medication card

EPIC 5: Testing
 ├── Task 5.1 — Unit test: adherence calculation
 ├── Task 5.2 — Unit test: markAsTaken + stock decrement
 ├── Task 5.3 - Unit test: undoLog + stock restore
 ├── Task 5.4 - Unit test: log filtering by date
 ├── Task 5.5 - Integration test: repository save/load cycle
 └── Task 5.6 - Widget test: MedicationManagerScreen renders correctly

EPIC 6: Documentation & Deployment
 ├── Task 6.1 — Write complete README
 ├── Task 6.2 — Write all SDLC stage documents (01-15)
 ├── Task 6.3 - Write Architecture Decision Records (ADRs)
 ├── Task 6.4 - Setup Github Actions CI/CD pipeline
 └── Task 6.5 - Prepare release build (Android APK / iOS Archive)

```

---

## MVP Definition

[What is the MINIMUM set of features needed for the project to be considered working?
List only what is truly required for the core value proposition.]

| Included in MVP | Reason |
|---|---|
| Add/ Edit / Delete | Core Data Management |
| Mark As Taken / skipped | Core Daily Workflow |
| Push Notification Reminders | Core Value Proposition | 
| Today's Medication List | Primary Screen |
| Encrypted Local Storage | Non-Negotiable (PHI) |
| Basic Adherence % Display | Core Feedback Loop

---

## Future Features (Post-MVP)

[What would you build NEXT if you had more time?
This shows forward thinking and the ability to scope a project.]

| Feature | Priority |
|---|---|
| Cloud Sync + Caregiver View | High |
| Drug interaction Warnings | Medium |
| Barcode/Label Scanner to auto-fill medication details | Medium |
| Pharmacy API to look up and connect with medication DB | Medium 
| Weekly Adherence Report Notifications | Low |
| Export Medication List as PDF | Low | 
| Medication Refill Reminder | Low |

---

## Dependencies

[List external packages, APIs, or systems your feature depends on.
Include both external packages and internal dependencies (other app modules).]

| Dependency | Type | Notes |
|---|---|---|
| flutter_local_notifications | External Package | Notification Scheduling |
| Sembast | External Package | Local NoSQL Database |
| encrypt | External Package | AES Encryption |
| flutter_secure_storage | External Package | Secure Key Storage |
| provider | External Package | State Management |
| go-router | External Package | Application Navigation |
| senior_sunrise_core | Internal Package | Shared Data Models

---

## Risks & Mitigations

[What could go wrong? List at least 3 risks and how you'd handle them.
This demonstrates professional maturity — developers who anticipate problems are more valuable.]

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Notification ID's conflicts with multi-dose medications | Medium | High | Use Composite notification ID: `${medicationID}_{timeIndex}` |
| Encrypted data fields cannot be queried by Sembast | High | Medium | Decrypt data first then query/filter |
| User denies notification permission | Medium | High | Gracefully degrade - app still works, just display in-app notification |
| App restart cancels local notifications schedule | Medium | High | Reschedule the notification on app restart |
| Notification message appears with the medication name | Medium | High | Rename notification messages to only use abstract messaging without PHI information |
| App silently fails storing the new medication when device is low in storage space | medium | High | Provide a in app message dialog indicating space is insufficient and needs more space to store new medication |


---

## Assumptions

[What did you assume to be true when planning this project?
Every project has assumptions — document them so they can be challenged.]

1. Users have a smart phone that supports local notifications
2. The core Senior Sunrise app Architecture (DatabaseService, Authentication flow) is already in place
3. Medication data does not need to sync across devices at this stage
4. Application security Architecture is already in place
5. Application viewing timer is already in place

---

## Definition of Done

[When is a task truly "done"? Set your own standard here.
Copy and use this checklist for every task you complete.]

A task is **Done** when:

- [ ] Code is written and follows the layered architecture
- [ ] Code has been self-reviewed (see `11-code-review.md`)
- [ ] Unit or widget tests exist for the feature
- [ ] All tests pass (`flutter test`)
- [ ] Feature works on both iOS and Android simulators
- [ ] Feature works offline (airplane mode test)
- [ ] No PHI appears in log output
- [ ] Committed to Git with a meaningful commit message
- [ ] Documentation updated if needed
