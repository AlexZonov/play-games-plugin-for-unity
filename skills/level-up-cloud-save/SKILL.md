---
name: level-up-cloud-save
description: Guidance for Unity developers to design, structure, and implement cloud save solutions based on Google Play's Level Up guidelines.
---

# Level Up Cloud Save Integration Guide for Unity

Google Play's **Level Up** program requires all games saving player data across
sessions to implement a reliable **Cloud Save** solution. This ensures players
can seamlessly transition between devices or restore their progress after
reinstalling.

This guide details Level Up cloud save guidelines and conflict resolution
policies.

---

## 1. Level Up Cloud Save Guidelines

To qualify for the Level Up program, your cloud save implementation must meet
these standards:

| Requirement | Description |
| :--- | :--- |
| **Off-Device Storage** | Game state must be saved off-device in the cloud and retrieve it when they start the game. |
| **Conflict Resolution** | A clear policy must handle local/cloud sync discrepancies. For example, you can present a manual selection UI so the player decides how to resolve conflicts. |
| **Multiple Account Support** | Correctly handle scenarios where different users play on the same physical device using different accounts. |
| **Provider Independence** | You can use the **Saved Games API** (Recommended) or **any third-party cloud provider** (Firebase, PlayFab, Unity Cloud Save, etc.) to satisfy these guidelines. |
| **Exemptions** | • Games that do not save *any* state across sessions (no guest or cloud accounts).• Games whose save file size exceeds the PGS limit (as specified in https://developer.android.com/games/pgs/savedgames#limit) where automated alternative cloud setups are cost-prohibitive (in which case, providing manual export/import options is recommended). |

---

## 2. Implementation Workflow & Best Practices

> [!IMPORTANT]
> **Determine Solution Preference**: If the cloud save solution (e.g., Google
Play Games Services Saved Games API, Unity Cloud Save) is not specified by the
user, you must ask the user about their preference before proceeding.
>
> **Check Existing Implementation**: When implementing or modifying cloud save,
always inspect the codebase first to check the current actual implementation of
cloud save. Ensure alignment or integration with the existing system rather than
creating a redundant system.

> [!IMPORTANT]
> **Autosave Frequency**: Do not attempt to save to the cloud on every frame or
local update. Limit cloud save attempts to key milestones (e.g., level completed
, transaction completed, or when the player manually exits/pauses the
application).

> [!WARNING]
> **Limit Save File Size**: The Google Play Games Services Saved Games API has a
strict file size limit specified in
https://developer.android.com/games/pgs/savedgames#limit. Keep your
serialization clean.

> [!NOTE]
> **Thread-Safety & Locking**: Prevent double-saving errors. When a save is in
progress, if there is a save UI, lock the save trigger UI element so the player
cannot spam click the save button and cause overlapping network transactions.

## Official Guidelines & Verification

> [!IMPORTANT]
> **Always refer to the official documentation for the latest guidelines:**
> * **Primary Guidelines:** [Google Play Games Level Up Guidelines](https://developer.android.com/games/guidelines)