# SeniorSunrisePortfolio
My example portfolio on Senior Sunrise app.
The documentation used is an example for interview purposes.

## Overview
The *Medication Manager* is a feature of the *Senior Sunrise* mobile application, a Flutter-based health companion app for senior users (65+).
This feature lets users manage daily medications, receive scheduled reminders about each medication dosage time and log doses as "Taken" or "Skipped" and track adherence over time.

This repository documents the complete development lifecycle: 
Product Brief -> Requirements -> System Design -> Implementation -> Testing -> deployment -> Monitoring/Update.

---

## Problem

Senior users often manage multiple daily medications with complex schedules.
Approximately **50% of seniors do not take medications as prescribed**,
leading to preventable hospitalizations. Existing solutions are often too complex or lack the privacy protections needed for personal health data (PHI).

---

## Solution

A simple, accessible, privacy-first medication management feature that:
- Allows seniors to add and manage their own medications 
- Sends on-device notification reminders at set scheduled times
- Logs doses as "Taken" or "Skipped" with one tap
- Tracks adherence percentage over time
- Works fully **offline** - no internet required
- Keeps all data **encrypted on-device**

---

## Features

| Feature | Description |
|---|---|
Add Medication | Name, Dosage, Frequency, Scheduled times, Stock Count, Prescriber, Instructions |
| Edit Medication | Modify details and reschedule notifications |
| Delete Medication | Remove medication and cancel all current reminders |
| Mark as Taken | Log dose; decrements stock count automatically |
| Mark as Skipped | Log dose as intentionally skipped |
| Undo Last Action | Reverse most recent log; restores stock |
| Adherence Tracking | Calculate adherence % (taken / scheduled) |
| History View | Browse logs by date range |
| Push Notifications | Scheduled daily reminders per time slot |
| Stock Tracking | Track medication count, alert when low |

---

## Architecture

```
┌──────────────────────────────────┐
│        Presentation Layer        │
│  MedicationManagerScreen         │
│  AddMedicationScreen             │
│  MedicationHistoryScreen         │
└────────────────┬─────────────────┘
                 │ Provider.watch<MedicationController>
                 ▼
┌──────────────────────────────────┐
│        Application Layer         │
│  MedicationController            │
│  (ChangeNotifier / Observer)     │
└────────────────┬─────────────────┘
                 │
            ┌────┴───────────────┐
            ▼                    ▼
┌─────────────────────────┐  ┌─────────────────────────┐
│   Data Layer            │  │  Device Integration     │
│  MedicationRepository   │  │  NotificationScheduler  │
│  NotificationService    │  │                         │
└────────────┬────────────┘  └─────────────────────────┘
             ▼
    ┌─────────────────┐
    │  Sembast DB     │
    │ (AES encrypted) │
    └─────────────────┘
```


---


## Technologies

| Layer | Technology | Reason |
|---|---|---|
| UI | Flutter (Dart) | Cross-platform, single codebase |
| State | Provider / ChangeNotifier | Right complexity for feature scope | 
| Database | Sembast | Dart native, supports full data encryption |
| Encryption | AES-256 (encrypt package) | PHI requires encryption at rest |
| Notifications | flutter_local_notifications | On-device scheduling, no server needed |


Full docs -> [`docs/04-Technical-Plan.md`](Medication-Manager/docs/04-Technical-Plan.md)

---

## Documentation Index

| Stage | Document |
|---|---|
| 1 | [Product Brief](Medication-Manager/docs/01-Product-Brief.md) |
| 2 | [Requirements](Medication-Manager/docs/02-Requirements.md) |
| 3 | [Project Plan](Medication-Manager/docs/03-Project-Plan.md) |
| 4 | [Technical Plan](Medication-Manager/docs/04-Technical-Plan.md) |
| 5 | [System Design](Medication-Manager/docs/05-System-Design.md) |
| 6 | [Data Design ](Medication-Manager/docs/06-Data-Design.md) |
