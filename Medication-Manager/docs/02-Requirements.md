# 02 — Requirements

> **Stage 2 of 15 | Demonstrates: Requirements Engineering**

---

## Functional Requirements

[List everything the system MUST be able to do.
Each requirement should be one clear, testable statement.
Start simple — you can always add more.]

| ID | Requirement |
|---|---|
*USER*
| FR-001 | User can add a new medication to the medication list with: (name, dosage, frequency, reminder times, pill count, doctor instructions, doctor name) |
| FR-002 | User can view all medications in a list |
| FR-003 | User can edit existing medication details |
| FR-004 | User can delete a medication |
| FR-005 | User can mark a scheduled medication as **Taken** |
| FR-006 | User can mark a scheduled medication as **Skipped** |
| FR-007 | When a does is marked as taken, the medication **Stock Count** decrements by 1 |
| FR-008 | User can **Undo** the most recent marked medication action |
| FR-009 | When an **Undo** is performed on a "Taken" log, the stock count is restored by 1 |
| FR-010 | User can manually **Refill** a medication's stock count |
| FR-011 | Medication notifications are **Cancelled** when a medication is deleted or edited | 
| FR-012 | User can view **Medication History** filtered by range |
*SYSTEM*
| FR-013 | System displays an **Empty State** when no medications have been added
| FR-014 | System schedules and sends a **Local Notification Reminder** at each medication scheduled time(s) |
| FR-015 | System calculates and displays **Adherence Percentages** (taken/scheduled dosages) |

---

## Non-Functional Requirements

[List the qualities the system must have — not behaviors, but HOW WELL it behaves.
Think about: performance, security, accessibility, reliability, maintainability.]

| ID | Requirement |
|---|---|
| NFR-001 | **Offline First:** All Core features (CRUD, Log, View) must work without internet access |
| NFR-002 | **Security:** all medication data (PHI) must be AES-encrypted at rest on the device |
| NFR-003 | **Privacy:** No PHI (names, dosages, times) must appear in system logs - only opaque IDs |
| NFR-004 | **Accessibility:** UI must adhere to WCAG specifications (large font, tap targets > 44pt, high-contrast) |
| NFR-005 | **Performance:** Medication list and history must load within 500ms on a mid-range smart phone device |
| NFR-006 | **Maintainability:** Code must follow layered architecture with clear separation of concerns |
| NFR-007 | **Reliability:** Notification scheduling and stored records must survive app restarts |
| NFR-008 | **Architecture:** system architecture must follow "Zero-knowledge Architecture" rules |

---

## User Stories

[Write at least 3-4 user stories in the standard format.
Each story should cover a different core feature or flow.
Then add acceptance criteria — this is what makes requirements testable.]

---

### Story 1 — Adding a medication to the medication list

```
As a 66 year old senior,
I want to be able to add my list of medications to the medication list,
So that I can track each one individually with confidence, without error of which one I need to take, the dosage and when.
```

**Acceptance Criteria:**

```
Given I am on the Medications screen

When I tap the "Add Medication" button
The Add Medication page should appear with medication inputs ready for me to input.
I enter a valid medication name, dosage, at least one frequency of each dose, the times i want for each dose reminder, (optional) pill count, (optional) doctor name, instructions and tap "save"

The Add Medication page disappears and returns to the medication list page

Then the medication appears in my list of medications
And a push notification is scheduled for the reminding times
And the medication's data is encrypted and saved on the device

```

---

### Story 2 — View all medications in a list

```
As a 65 yr old Senior,
I want to open the app, goto the medications page and see a list of my added medications.
when needed I can track each one individually with confidence, without error of which one I need to take, the dosage and when.

```

**Acceptance Criteria:**

```
Given I have added a medication to the app already, and I have logged out our left the app.

When i open the app and log in, it should take me to the main page.
from there it should be < 2 taps to get to the medications page

I scroll to the medications button in Quick Actions section
I tap the medications button, and the medications page loads
I can view all added medications, correctly displaying the right information(name, dosage, time, doctor).

I should be able to close the app or leave the app briefly and come back or goto another page and back, the medication should still display

When I tap on the medication, it should open and show the correct entered medication data and information

```

---

### Story 3 — Local Notification Reminder

```
As a senior I want to be able to add a medication to the app, when a new medication is added I can also add a reminder time for when a medication dose is due for taking.

when the time arrives for that medication dose, whether i'm in the app, browsing another app or my device is off, the app will display the medication notification.

```

**Acceptance Criteria:**

```
Given i'm outside of the app and I have set a medication reminder to 8:00 a.m.

When its 8:00 a.m. on any day, the medication is active

Then I receive a push notification titled with "Medication Reminder" and not the "medication name" for privacy

On notification tap, it opens the app, I log in, and it takes me to the medication page or the medication needed to take, or a list of multiple medications needing to take at the same time.

```

---

### Story 4 — Marking a medication as Taken or Skipped

```
As a 70 yr old Senior,
I want to be able to log if I have taken a medication or not
So that I can confidently know a medication dose has been adhered to.
I can review and assert completion till the next dose.

```

**Acceptance Criteria:**

```
Given I have already added a medication to the medication list and have a scheduled medication dose notification set for 8:00 a.m.

When I receive the medication notification at 8:00 a.m.
I tap on the notification, i'm taken into the app, log in and it brings me to the main page or medication list page.

On the medication page my medications are listed alphabetically and easy to find.
I then can find which does is due and tap take now button

the medication card then displays "taken". with the option to undo the action with a "undo" text button next to it. 

if tapped skipped the card displays "skipped" with undo text button next to it.

```

---

### Story 5 — Medication Adherence Percentages

```
As a 77 yr old Senior,
I want to be able to log my medication throughout the day and know how much of my medication has been taken, to have confidence i'm making progress on my meds and when i reach 100% i can sigh relief.

```

**Acceptance Criteria:**

```
Given I have already added a medication to the medication list
I am on the medication page and have tapped the medication "Take now" button

then the medication system should calculate based on the amount of medications in my medication list that are due this day / which items have been taken and display the
daily medication complete percentage.

```
