---
name: level-up-achievements
description: Guidance for Unity developers to design, structure, and implement achievements to satisfy Google Play's Level Up program guidelines.
---

# Level Up Achievements Integration Guide for Unity

Achievements are a core gaming experience that recognizes and rewards players
for their accomplishments. Under Google Play's **Level Up** program, games must
implement a high-quality, structured achievements system using Google Play Games
Services (PGS) to qualify for platform promotion and benefits.

This guide provides the core Level Up requirements for achievements and design
principles.

---

## 1. Level Up Achievements Requirements

To qualify for the Google Play Games Level Up program, your achievements must
satisfy the following criteria:

| Requirement | Description |
| :--- | :--- |
| **Minimum Count** | A **minimum number of achievements** (as specified in https://developer.android.com/games/pgs/quality#achievements), spread across the lifetime of the game. |
| **Recommended Count** | Google recommends providing **40+ achievements** for deep player engagement. |
| **Early Gameplay** | A **minimum number of early gameplay achievements** (as specified in https://developer.android.com/games/pgs/quality#achievements), must be reasonably and reliably achievable within the **first hour of gameplay** by *every* player. This is also a requirement to unlock eligibility for Play Games **Quests**. |
| **Metadata Quality** | Every achievement must have a **unique name** and a **clear description** explaining exactly what the player needs to do to unlock it. |
| **Exemptions** | • Games primarily targeting players under 13.• Games completely lacking clear event progression (lifetime high scores, levels, item collections, story stages, or XP are considered progression mechanisms and *disqualify* games from this exemption). |

> [!IMPORTANT]
> Achievements **should not require real-money purchases** to be unlocked. All
achievements should be obtainable through gameplay.
>
> When suggesting the implementation of achievements, the AI agent must confirm
with the user before proceeding.

---

## 2. Setting Up in Google Play Console

Before writing code in Unity, you must configure the achievements in the Play
Console:

1. Open the [Google Play Console](https://play.google.com/console).
2. Select your game, then navigate to **Grow users** > **Play Games Services** > **Setup and management** > **Achievements**.
3. Click **Create achievement** and define:
   - **Name & Description**: Unique text detailing the task.
   - **Icon**: Upload an image that meets the specifications in https://developer.android.com/games/pgs/quality#achievements.
   - **Incremental achievements**: *Non-incremental* (one-time event) or *Incremental* (requires multiple steps).
   - **Steps**: (For Incremental only) Number of steps required to unlock (e.g., 50 coins collected).
   - **Initial State**: *Revealed* (default) or *Hidden* (for spoiler protection).
   - **Points (XP Value)**: Specify the experience points (each achievement must grant XP; total game limit is 200,000 XP).
   - **List order**: The number determining the sequence in which this
   achievement appears relative to others (e.g., 1 for the first achievement,
   2 for the second, etc.) when players view them in the Play Games overlay or
   app.
4. Save and click **Publish changes** to push achievements to the draft/production configuration.
5. From **Grow users** > **Play Games Services** > **Configuration** > **Get resources**, copy the XML structure containing all achievement IDs for the Unity Setup.

---

## 3. Unity Plugin Setup

1. Import the **Google Play Games plugin for Unity** package (GPGS SDK / PGS V2)
from the [current build repository][plugin-repo] as a custom package.
If the PGS V2 package is not installed, download and install it from this URL or
prompt the user to do so.
2. In the Unity menu, go to **Window** > **Google Play Games** > **Setup** > **Android Setup**.
3. Paste the **Resources Definition XML** copied from **Grow users** > **Play Games Services** > **Configuration** > **Get resources** on the Play Console.
4. Click **Setup**. This generates a constant-filled static class (typically `GPGSIds.cs`) containing all achievement and leaderboard ID strings mapped to C# variables.

---

## 4. Play Games Services V2 API Integration

### Deprecated Social Class
> [!WARNING]
> The traditional Unity `UnityEngine.Social` APIs are **deprecated** for Google
Play Games Services V2 (PGS V2) and must be avoided. Using them can result in
silent failures, authentication blocks, or incompatibility with modern Play
Games ecosystem features.

### Recommended PlayGamesPlatform API
Instead of `Social`, always use the **PlayGamesPlatform API** provided by the
PGS V2 package for initializing, authenticating, unlocking, and incrementing
achievements.

If the PGS V2 package is not installed, download and install it from the
official current-build URL:
* [Google Play Games plugin for Unity (Current Build)](https://github.com/playgameservices/play-games-plugin-for-unity/tree/master/current-build)
* Alternatively, prompt the user/developer to install from this repository before proceeding.

---

## Official Guidelines & Verification

> [!IMPORTANT]
> **Always refer to the official documentation for the latest guidelines:**
> * **Primary Guidelines:** [Google Play Games Level Up Guidelines](https://developer.android.com/games/guidelines)

[plugin-repo]: https://github.com/playgameservices/play-games-plugin-for-unity/tree/master/current-build