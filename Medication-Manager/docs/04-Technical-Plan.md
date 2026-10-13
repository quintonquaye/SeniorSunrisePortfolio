# 04 — Technical Plan

> **Stage 4 of 15 | Demonstrates: Technology Decision Making**
> PURPOSE: Answer "HOW will we build it?" — and more importantly, Explain WHY you chose it and what trade-offs you accepted.**

---

## Technology Stack

| Layer | Technology | Version | Why This Choice |
|---|---|---|---|
| **Language** | Dart | 3.x | Required by Flutter, Dual Engine Compilation(JIT + AOT + Web Target), Null Safety, Clean Familiar Syntax, Robust Concurrency with Isolates, Multi-paradigm Capabilities (OOP, Functional Programming, etc.), Tooling out of the box (Package manager, linter, tester) |
| **UI Framework** | Flutter | 3.x | Single Codebase for all platforms(iOS, Android, Web, Desktop), Fast Development(Hot-Reload), Native-Like performance, Custom UI Controls  |
| **State Management** | Provider/Change Notifier | 6.x | Sufficient for a single feature controller; simpler than BLoC, more explicit than Riverpod for this scope |
| **Database** | Sembast | 3.x | Pure-Dart NoSQL Database store; Simpler scheme than SQLite; Supports full-payload encryption natively |
| **Authentication** | Biometric / PIN| 3.x | Allows local on-device authentication of the User without remote authentication methods, staying within our scope of Zero-Knowledge |
| **Notifications** | flutter_local_notifications | 17.x | Performs on-device notification scheduling; no push notifications sent from a server required; Supported for both iOS and Android |
| **Navigation** | go_router | 13.x | Provides declaritive routing; consistency with the rest of the application |
| **Testing** | flutter_test (Built-in) | - | Standard Flutter testing framework |
| **Version Control** | Git / GitHub | - | Industry standard; Enables CI/CD integration for application versioning |
| **CI/CD** | GitHub Actions | - | Free for open-source; integrates directly with repository |

---

## Key Technology Decisions (Expanded)

[Structure: Why I chose X, what alternatives I considered, and what trade-off I accepted.]

### Decision 1 — Why Provider/ChangeNotifier over BLoC or Riverpod for State Management?

**Why I chose it:**
I chose Provider because of its low complexity in rebuilding descending widgets in the UI when data changes and has a shared app state. Its easy to understand, encourages separation of concerns, allows for Dependency Injection and is testable and flexible.

**Alternatives I considered:**
I also considered BLoC State Management because of previous use in another project which dealt with data streaming, but BLoC adds an extra layer of complexity that was out of scope for this type of application. 

Riverpod was also a consideration because it too utilizes a modern state management solutions inspired my Provider, with explicit providers, dependency management and testing support. But due to it introducing its own concepts and APIs to learn I decided to keep this decision simple.


**Trade-off I accepted:**
1. By choosing Provider, I gave up being able easily maintain a class if many features are included. | **Mitigation:** I would need to split state management by features and responsibility.
2. I would need to manually state `NotifyListeners()`, but forgetting this call would introduce stale data, calling it unnecessarily can introduce extra rebuilds | **Mitigation:** Centralize state-changing operations and use Selector or context.select when only a part of the state matters.

---

### Decision 2 — Why Sembast over SQLite?

**Why I chose it:**
I chose Sembast for its storage structure and its Native Dart friendly integration.
Since I had many experiences with Firebase and No-SQL Databases, it felt familiar and easy to adopt for this project.

**Alternatives I considered:**
SQLite was highly considered due to its ability to work well with structured data that has clear relationships, the reason i did not go with it at this time is due to its strong schema design and requirement to know SQL.

**Trade-off I accepted:**
Even though Sembast is great for flexible record structures and simple repository integration, large datasets and complex queries need careful consideration. In that, the current application doesn't manage large complex storage requirements and datasets.

---

### Decision 3 — Why Zero-Knowledge Architecture?

**Why I chose it:** 
- All data stored **only on the user's device**
- No cloud storage, no external servers
- Data encrypted with device secure storage
- Developer/vendor cannot access user data
- Optional P2P sync with explicit user consent

**Benefits for Compliance**:
- Minimizes breach risk (no central database to attack)
- User has full control over their PHI
- No automatic data transmission = reduced HIPAA scope
- Simplified compliance compared to cloud solutions

---

**Alternatives I considered:**
I considered Using Amazon AWS and Google Firebase, signing BAAs, but due to the scope of our project and mission, we wanted full trust of the user and affirm that their data is only controlled and stored by them and NO ONE ELSE. WE also didn't want to take on such a large burden in the beginning with HIPPA regulations if we didn't need to. 

**Trade-off I accepted:**
By keeping everything on the users device and FULLY under the users control, data breaches and unauthorized account access are kept to a minimum, although, if their passcode is forgotten, or they haven't setup a biometric login, or saved their One-time access code, the users data would be cleared as a last result, resulting in all data deleted.

---

## Architecture Approach

"This feature follows a layered architecture: Presentation → Application → Data.
Upper layers depend on lower layers; lower layers never depend on upper layers."

```
Presentation Layer  -> Flutter widgets and screens (MedicationManagerScreen, AddMedicationScreen, MedicationHistoryScreen)
Application Layer   -> MedicationController (Business Logic)
Data Layer          -> MedicationRepository (Data Access)
Infrastructure      -> DatabaseService (Sembast), NotificationScheduler
```

This is documented in full in [`07-architecture.md`](07-Architecture.md)