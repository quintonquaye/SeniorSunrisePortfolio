# 06 — Data Design

> **Stage 6 of 15 | Demonstrates: Data Modeling**

---

## Entities

[List every major data object in your feature.
For each entity: what is it? What fields does it have?]

### Medication

[Describe what this entity represents in one sentence.]

This is the core entity representing a medication model in the user's medication list

| Field | Type | Required | Description |
|---|---|---|---|
| `id` | String (UUID) | Yes | Unique identifier |
| `name` | String | Yes | The name of the medication (ex. "Metformin") |
| `dosage` | String | Yes | The amount of a medication needed (ex. "500mg") |
| `frequency` | String | Yes | The amount of times a medication needs to be taken |
| `times` | List\<String>\>?| - | The scheduled times to take a medication in "HH:mm" format (e.g., ["08:00", "20:00"]) |
| `StartDate` | DateTime | Yes | The date the medication is started |
| `endDate` | DateTime? | - | Optional end date (null = ongoing) |
| `isActive` | bool | Yes | This shows if a medication is currently active |
| `stockCount` | int? | - | This shows the current pill count remaining of a medication |
| `refillRemaining` | int? | - | The number of refills remaining on a specific medication |
| `prescribedBy` | String? | - | The prescribing doctor for the medication |
| `instructions` | String? | - | The Special instructions given by the doctor or the pharmacist |


---

### MedicationLog

[Describe what this entity represents.]

An immutable record of a single dose event (taken or skipped).

| Field | Type | Required | Description |
|---|---|---|---|
| `id` | String (UUID) | Yes | Unique identifier |
| `medicationID` | String | Yes | References a `Medication.id` |
| `timestamp` | DateTime | Yes | Shows the time a dose was logged |
| `status` | MedicationStatus | Yes | This is an enumeration of : `taken`, `skipped`, `missed` |

---

### MedicationStats 
> *(Computed — not persisted to database)*

A computed object derived from `MedicationLog` records. Never written to the database.

| Field | Type | Source |
|---|---|---|
| `medicationID` | String | Id from a medication |
| `medicationName` | String | The name of a medication |
| `taken` | int | The count of `taken` logs in range |
| `skipped` | int | The count of `skipped` logs in range |
| `scheduled` | int | Calculated from schedule times x days in range | 
| `adherence` | double | `taken / scheduled` (computed getter) | 
| `missed` | int | `scheduled - (taken + skipped)` (computed getter) |

---

## Database / Storage Structure

[Show how your data is organized in storage. Use a tree or diagram format.]

```
Sembast Database 
│
├── Store: "medications"
│        Key: medicationId (String)
│        Value: EncryptedData { iv: String, encrypted: String }
│                └─ decrypts to -> Medication.toJson()
│   
└── Store: "medication_logs"
         Key: logId (String)
         Value: EncryptedData { iv: String, encrypted: String }
                 └─ decrypts to -> MedicationLog.toJson()

```
---


## Design Decisions

### Why is the data structured this way?

[Explain your most important data design choices. For each decision, explain WHY.
Example questions to answer:]

> Why are logs in a separate store vs. nested inside the parent entity?

- Logs are queried independently of medications - they are used based on date range, and not by parent medication. By keeping them independent in a flat store, optimizes the most common query pattern.

> Why store `times` as `List<String>` instead of `List<DateTime>`?

- Times represent a *daily schedule*, not a specific point in time. Storing a "HH:mm" strings decouples the schedule from any specific date, making it easy to calculate upcoming notification times for any future date.

> Why soft-delete vs. hard-delete?

- Deleting a medication would orphan all its `MedicationLog` records, making medication history views impossible without a link to the parent medication. By marking medications as `isActive: false`, the history screen can still join logs to their parent medication for display purposes.

---

## Validation Rules

[What validation does each field need?
This shows you think about data quality, not just happy-path data.]

| Field | Validation |
|---|---|
| `name` | Required, non-empty string |
| `dosage` | Required, non-empty string |
| `frequency` | Required, non-empty string |
| `times` | At least one time if reminders are enabled |
| `stockCount` | If provided, must be a non-negative integer |
| `refillsRemaining` | If provided, must be a non-negative integer |


[Where is validation enforced — at the UI layer, in the controller, in the repository?
Explain your reasoning.]

The validation of a form is enforced at the **UI Layer** (form validators) before `controller.saveMedication()` is called. The repository assumes valid data.

---

## Data Access Patterns

[How will data be read? List the main query patterns and their performance characteristics.]

| Pattern | How It Works | Performance |
|---|---|---|
| Load all medications | Decrypt all records, sort by name in memory | O(n log n) — acceptable for small n |
| Load active medications | Decrypt all records, then filter `isActive == true` in memory | O(n) |
| Load today's logs | Decrypt all log records, filter by date range in memory | O(n) |
| Load history range | Decrypt all log records, filter by start/end date in memory | O(n) |
| Save medication | Encrypt a medication record, add medication by Id | O(1) |
| Save log | Encrypt a medication log record, add medicationLog by UUID | O(1) |
| Delete log | Find log by ID, delete the log record | O(1) |

