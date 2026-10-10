# 01 — Product Brief

> **Stage 1 of 15 | Demonstrates: Product Thinking + Requirements Understanding**
>
> Before a single line of code is written, a developer must understand *what* they are building and *why*. This document captures the product brief for the Medication Manager feature.

---

## Product Name

**Medication Manager** - a medication feature in the Senior Sunrise SeniorApp.

---

## Problem

Senior users (aged 65+) frequently are:
- Forgetting medication doesage or not remembering they took a pill or not
- Accidentally taking an extra dose or the wrong medication
- Managing too many pills and times throughout the day
- Confusion about instructions and how to take, how much, when, with food, water..
- Difficulty reading or understanding medication labels
- Side effects such as dizzyness, sleepiness, confusion, nausea, constipation, or dry mouth.
- Balance problems and increased risk of falls from certain medications
- Interactions with other medications, over-the-counter medicines and others
- High medication costs - leading to skipped or unfilled medications
- Trouble accessing medication or keeping supplies available
- Keeping track of changes, different doctor instructions, outdated medications list
- Continued symptoms while taking prescribed medication

Consequences:
- Worsening health symptoms (higher blood pressure, uncontrolled cholesterol)
- Overdosing on medications/toxicity
- Falling/injuries
- Confusion, difficulty thinking
- Dangerous changes in blood sugar or blood pressure
- Serious Bleeding
- Persistent discomfort
- Reduced independence
- Increased Emergency Room visits/Hospital stays 
- Increased unnecessary medications
- Lower quality of life


Why are current solutions not changing these outcomes much?
- Many current solutions address only one part of a multifactorial problem: helping a senior take pills is not the same as ensuring the medicines are appropriate, affordable, safe, and working together. Reviews find that better medication-taking or prescribing does not consistently translate into fewer falls, hospitalizations, or deaths.

Pillboxes, alarms, and apps cannot by themselves resolve medication costs, troubling side effects, cognitive impairment, or complicated instructions. Different barriers require different support.
- Taking medication correctly does not guarantee the prescription is safe.
- Multiple conditions and many medicines create interacting risks.
- Short-term improvements may not last.
- Tools may not fit the person’s abilities or circumstances.
- Better “process” results are not necessarily better health results.
- Medication problems are only one contributor to poor outcomes. 

The central gap is between a stand-alone tool and sustained, individualized care. The evidence points toward combining appropriate prescribing, simpler regimens, affordability support, caregiver involvement where needed, and follow-up—but it does not establish one universal package that reliably prevents every serious outcome.

> In Apps specifically

For seniors’ medication-management apps, the main issues are: 
- Usability, 
- Keeping information accurate 
- Turning reminders into safer care. 

These limitations vary by app, not every app has all of them.

Common problems:
- Hard-to-use interfaces.
- Too much setup and updating.
- Reminders address only some barriers.
- Limited confirmation that a dose was actually taken.
- Limited medication-safety support.
- Privacy and trust concerns.
- Digital-access barriers.
- Technical problems and inconsistent engagement. 

A 2026 review of reminder technologies, not just apps, found moderate evidence for improvements in blood pressure and physical symptoms, but limited or no evidence for several other outcomes.

*The key distinction is between reminding someone to take a medicine and ensuring that taking it is safe and beneficial. An app may do the first well while leaving prescription appropriateness, side effects, and the need for clinical follow-up unresolved.*

---

## Target User

**Primary:** Senior (without assistance)

**aged:** 65-74 

**Technical Comfort Level:** May have limited technical experience

**User Characteristics:** Medication need is already widespread: 86.9% used a prescription medicine in the previous 12 months. This makes it a strong starting point for an independent-use product, although smartphone comfort should be screened individually.(CDC)

**Context of use:** The app will need to be available offline first any internet connection needs are secondary.

---

**Secondary:** Senior (Assisted onboarding)

**aged:** 75-84 

**Technical Comfort Level:** May have lower technical experience

**User Characteristics:** Prescription use rises to 91.3%. There is substantial potential need, but accessibility and support become important design considerations, not reasons to exclude the group. 
(CDC)

**Context of use:** The app will need to be available offline first any internet connection needs are secondary.

---

**Lastly:** Senior (Caregiver-Assisted)

**aged:** 85+ 

**Technical Comfort Level:** May have lowest technical experience

**User Characteristics:** Prescription use remains high at 91.2%. I would test shared management and assisted use first, while allowing independent users to participate. Age alone does not establish someone’s ability to use an app.
(CDC)

**Context of use:** The app will need to be available offline first any internet connection needs are secondary.

---

## User Need

> *"As A independent senior, I need a simple app that helps me know what medicine to take and when, remember whether I already took it, and keep my medication list current when prescriptions change. It should be easy to read and use, protect my privacy, and let me share information with a family member or healthcare professional when I choose.”*

---

## Proposed Solution

Create a mobile feature (iOS, Android) within the senior Sunrise App, that allows users to:

**Add** their medication with a scheduled times

**Receive reminders** as local notifications at the right times

**Log** each dose as taken or skipped with a single tap

**Track** their adherence over time

**Review** their medication history

In-order to get to the proposed realm of meeting seniors varied needs of (dose confirmation, medication-safety, Privacy, barriers, engagement inconsistencies) with support for the target individual, starting with the basic functionality of managing medications is the best approach. 

---

## Product Goals

1. Reduce missed medications by sending prompt reminders for that specific dosage
2. Provide a medication adherence summary that is easy to understand at a glance
3. Ensure that all medication data (PHI) remains private and encrypted on the device
4. App is accessible offline at all times
5. Logging medication is simplified and no more than 2 taps

---

## Non-Goals

- X Prescription appropriateness - out of scope for V1
- X Side effects - out of scope for V1
- X Clinical follow-up - out of scope for V1
- X Cloud Sync - offline-first priority
- X Multi-user accounts — single user only
- X Integration with Pharmacy systems - future integrated service layer
- X Prescription sharing - future integrated connection layer

---

## Basic User Experience

**First Path**
1. User opens app → sees today's unconfirmed medications in Task List
2. User taps Medication -> taps "Mark as Taken"
3. Adherence updates

**Alternate Path**
1. User opens app -> scrolls down to Quick Actions section -> taps Medications
2. User views listed medications with next dosage time -> User taps "Take Now" on appropriate medication
3. Adherence updates

**Or by notification** 
1. User receives a reminder notification at scheduled time
2. User taps notification or opens the app -> continue with first or alternative path. (Later will take user directly to specified medication)

---


## Success Criteria

| Criterion | Target |
|---|---|
| Dose logging completes in under 2 taps | 100% of flows |
| Feature works offline | 100% of core features |
| All data encrypted | 100% of records |
| App works without internet connection | 100% of core features |
| All PHI is encrypted at rest | 100% of stored records |
| User can add medication in under 2 minutes | > 95% of users |
| Local Medication notifications alert within 5 minutes of the intended consumption window | 100% of alerts | 

---

## Constraints

| Constraint | Detail |
|---|---|
| **Offline-first** | App must function without internet; no cloud dependency for core features |
| **PHI protection** | Medication data is Protected Health Information — must be encrypted at rest |
| **Accessibility** | Must adhere to WCAG standards, especially for seniors (large tap targets, readable fonts, high contrast) |
| **Platform** | iOS 14+ and Android 8+ |
| **No backend changes** | This feature is fully client-side; no new server infrastructure |
| **Architecture** | Must adhere to Zero-Knowledge Architecture requirements | 
| **On-device notifications** | Must use device-native notification scheduling (no push server) |




