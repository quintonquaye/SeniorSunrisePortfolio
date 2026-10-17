# 05 — System Design

> **Stage 5 of 15 | Demonstrates: System Architecture**
>
> How the components fit together, how data flows through the system, and where the failure points are.

---

## Architecture Diagram

[Draw your system here using ASCII art or a text diagram.
Show the major components as boxes and draw arrows between them.
Label each arrow with what flows between components.

Minimum required boxes:
- User
- UI (screen/view)
- Controller or ViewModel (business logic)
- Repository or Service (data access)
- Database or Storage
- Any external services (notifications, APIs, etc.)]

```
                ┌─────────────────────────────────────────┐
                │                   USER                  │
                │  (Senior, age 65+, using mobile device) │
                └───────────────────┬─────────────────────┘
                                    │ taps, reads
                                    ▼
┌────────────────────────────────────────────────────────────────────────────┐
│                         PRESENTATION LAYER                                 │
│                                                                            │
│   ┌──────────────────────┐  ┌────────────────────┐  ┌───────────────────┐  │
│   │ MedicationManager    │  │ AddMedication      │  │ MedicationHistory │  │  
│   │ Screen               │  │ Screen             │  │ Screen            │  │
│   │                      │  │                    │  │                   │  │
│   │ Shows: All meds      │  │ Form: name,        │  │ Date range        │  │
│   │ Mark taken/skip/undo │  │ dosage, times,     │  │ log List          │  │
│   │ Adherence %          │  │ stock, prescriber  │  │                   │  │ 
│   │ Stock,               │  │ instructions       │  │                   │  │
│   │ next dose time/      │  │                    │  │                   │  │
│   └──────────┬───────────┘  └─────────┬──────────┘  └──────┬────────────┘  │ 
└──────────────┼────────────────────────┼────────────────────┼───────────────┘
               │                        │                    │
               └─────────────────────┬──┘────────────────────┘
                                     │ Provider.watch / Provider.read
                                     ▼
┌───────────────────────────────────────────────────────────────────────┐
│                            APPLICATION LAYER                          │
│                                                                       │
│   ┌───────────────────────────────────────────────────────────────┐   │
│   │                       MedicationController                    │   │
│   │                        (ChangeNotifier)                       │   │
│   │                                                               │   │
│   │  loadMedications()    markAsTaken()    getAdherence()         │   │
│   │  addMedication()      markAsSkipped()  getMedicationStats()   │   │
│   │  updateMedication()   undoLog()        getTodayAdherence()    │   │
│   │  deleteMedication()   refill()         loadHistoryRange()     │   │
│   └────────────────────────┬──────────────────┬───────────────────┘   │
└────────────────────────────┼──────────────────┼───────────────────────┘
                             │                  │
                   ┌─────────┘                  └──────────────┐
                   ▼                                           ▼
    ┌──────────────────────────────┐        ┌───────────────────────────────────────┐
    │         DATA LAYER           │        │         DEVICE INTEGRATION            │
    │                              │        │                                       │
    │  ┌────────────────────────┐  │        │  ┌─────────────────────────────────┐  │
    │  │  MedicationRepository  │  │        │  │      NotificationScheduler      │  │
    │  │                        │  │        │  │                                 │  │
    │  │  getAllMedications()   │  │        │  │ scheduleMedicationReminder()    │  │
    │  │  getActiveMedications()│  │        │  │                                 │  │
    │  │  saveMedication()      │  │        │  │ cancelAllMedicationReminders()  │  │
    │  │  deleteMedication()    │  │        │  │                                 │  │
    │  │  saveLog()             │  │        │  └───────────────┬─────────────────┘  │
    │  │  getLogsForDate()      │  │        │                  │                    │
    │  │  getLogsBetween()      │  │        │       ┌──────────▼───────────┐        │
    │  │  deleteLog()           │  │        │       │  NotificationService │        │
    │  └───────────┬────────────┘  │        │       │   (action stream)    │        │
    │              │               │        │       └──────────────────────┘        │
    │  ┌───────────▼────────────┐  │        └───────────────────────────────────────┘
    │  │   DatabaseService      │  │
    │  │   (Sembast wrapper)    │  │
    │  └───────────┬────────────┘  │
    │              │               │
    │  ┌───────────▼────────────┐  │
    │  │   Sembast Database     │  │
    │  │   (AES-256 encrypted)  │  │
    │  │                        │  │
    │  │  Store: medications    │  │
    │  │  Store: medication_logs│  │
    │  └────────────────────────┘  │
    └──────────────────────────────┘
```

---

## Component Descriptions

[Describe what each component is responsible for.
For each component: what is its job? What does it know? What does it NOT know?]

### MedicationManagerScreen
* It is the main page for medications and utilized the Provider pattern to `controller.watch<MedicationController>()` & `controller.read<MedicationController>()` and reactively update the widget tree.
* It is responsible for displaying All medications of the user in an alphabetical list
* Show Medication history button to route the user to review past medication adherences
* Display the add medication button to route the user to add a new medication when needed
* To reactively listen and update each medication adherence and stock level when the user taps on each medication action(skip, take now, undo).

### AddMedicationScreen
* Its responsible for adding a new medication to the user profile and list
* It holds the state of the medication form 
* upon `controller.saveMedication()` the medication screen validates the form data and on success, 
* Sends the `MedicationModel` data to the `MedicationController` to be saved to the `MedicationStore`
* It also manages medication edits, entering prefilled data into the medication form input fields.

### MedicationHistoryScreen
* It is responsible for every medication action taken by the user (Taken/Skipped) in a Date/time list arrangement
* It shows the Global adherence of the users adherence rate. 
* It displays the medication adherence for each medication
* It displays the detailed logs for each medication (name, date, time dosage)


### MedicationController
* Its responsible for maintaining the state of each medication and AdherenceLogs of the user
* It is the central business logic unit for the Medication Management feature
* it extends the `ChangeNotifier` (Observer Pattern)
* It coordinates between the Data Layer(MedicationRepository) and the Device Integration Layer(`LocalNotificationScheduler`)
* It never interacts with the database directly only through the `MedicationRepository`

### MedicationRepository
* Its responsible for all Medication read/write(CRUD) actions for both medications and adherence logs to the medicationStore and medicationLogStore
* its responsible for handling the AES-encryption/decryption methods transparently
* It is responsible for mapping the data to/from the correct data model suitable for the corresponding store with `.toJson()` `.fromJson()`.


### DatabaseService / Storage(Sembast)
* `DatabaseService` is a wrapper around Sembast that provides the database connection and the encryption key (retrieved from secure storage) shared across all repositories in the app. 
* Sembast Database is responsible for persisting all data into their respectable stores and enforcing the storage constraints for each.

### NotificationScheduler
* It is responsible for scheduling notifications for a specific time to the user.
* Its role for medications is to translate medication schedules into `flutter_local_notifications` daily notification objects.
* the scheduler uses composite ID's to prevent notification id collisions for multi-dose medications.

### NotificationService
* Listens for notification action events (e.g. user tapped "Mark As Taken" from a notification "future feature").
* It broadcasts each event as a Dart `Stream` so the `MedicationController` can respond without direct coupling to the notification system

---

## Data Flow — Mark as Taken

[Trace the data flow for your most important operation, step by step.
Example: "What happens when the user marks a dose as taken?"]

```
1. User taps "Mark as Taken" on a medication card
2. UI calls the controller.markAsTaken(medID)
3. Controller:
    a. finds the medication in its memory list
    b. updates the stockCount - If stockCount > 0, decrements stock
    c. calls repository.saveMedication(updatedMed)
    d. saves medicationLog - saveLog(medID, medicationStatus.taken)
    e. medicationLog is saved with medicationID, timeStamp, medicationStatus
    f. calls repository.saveLog(log)
    g. calls loadLogs() to refresh the daily logs
    h. calls notifyListeners()
4. UI rebuilds widgets reactively - medication card shows "Undo"
5. Adherence % recalculates adherence rate via the getter

```

---