# Future Nostalgia — Episode Planner
### Shine FM 99.9 · with Lema & Ruthie · Every Sunday 4:20–5:45pm

A fully-featured episode planning and broadcast tool for the Future Nostalgia show. Built with React, Firebase Realtime Database, and Google Sign-In. Deploys as a single `index.html` file on GitHub Pages — no build step needed.

---

## What it does

| Feature | Description |
|---|---|
| **Calendar** | Month view of all Sundays. Click a week card to view or plan an episode. |
| **Episode Builder** | Create episodes with type, guests, air date, production notes, and per-segment cue notes. |
| **Rundown** | Auto-generated segment list based on episode type (A–E). Editable per-segment via modal. |
| **Show Bible** | Read-only reference tab — Fixed Spine, Segment Titles, Episode Types, Monthly Pattern. |
| **Broadcast Mode** | Full-screen live view with segment countdown, show clock (85 min), progress bar, and next-up. Launch from any episode card. |
| **Auto-save** | All changes debounce-save to Firebase Realtime Database with a SAVING / ✓ SAVED indicator. |
| **Google Sign-In** | Login required. Data is stored per-user at `/users/{uid}/fn_episodes`. |

---

## Deploy to GitHub Pages

### Step 1 — Create a GitHub repository

1. Go to [github.com/new](https://github.com/new)
2. Name it, e.g. `fn-planner`
3. Set it to **Public** (required for free GitHub Pages)
4. Click **Create repository**

### Step 2 — Upload the file

Either drag `index.html` into the GitHub web UI, or use Git:

```bash
git init
git add index.html
git commit -m "Initial deploy"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/fn-planner.git
git push -u origin main
```

### Step 3 — Enable GitHub Pages

1. Go to your repo → **Settings → Pages**
2. Under **Source**, select **Deploy from a branch**
3. Branch: `main`, folder: `/ (root)`
4. Click **Save**

Your site will be live at:
```
https://YOUR_USERNAME.github.io/fn-planner/
```

---

## Configure Firebase

### Step 4 — Whitelist your GitHub Pages domain

Google Sign-In will be blocked unless your domain is whitelisted.

1. Go to the [Firebase Console](https://console.firebase.google.com/)
2. Open the **future-nostalgia** project
3. Go to **Authentication → Settings → Authorized domains**
4. Click **Add domain**
5. Enter: `YOUR_USERNAME.github.io`
6. Click **Add**

### Step 5 — Set Realtime Database security rules

1. In the Firebase Console, go to **Realtime Database → Rules**
2. Replace the existing rules with:

```json
{
  "rules": {
    "users": {
      "$uid": {
        ".read": "$uid === auth.uid",
        ".write": "$uid === auth.uid"
      }
    }
  }
}
```

3. Click **Publish**

This ensures each signed-in user can only read and write their own episodes.

---

## Episode Types

| Type | Format | Key segment |
|---|---|---|
| **A** | Interview | Pro-25Flow — main artist/industry guest |
| **B** | FN Court | Artist on trial — 3-part court structure |
| **C** | Conversational | Extended host convo + Aux Battles + drama arc |
| **D** | The Capsule | Emerging student artists + Top-Ten Countdown |
| **E** | Artist Spotlight | Deep-dive + Top-Ten Countdown of artist hits |

**Monthly rotation:** Week 1 → A, Week 2 → C, Week 3 → D, Week 4 → B, Week 5 → E (flex)

---

## Broadcast Mode

Launch from any episode: open the episode card → click **▶ Go Live**

- Counts down each segment individually (time split equally across the show)
- Shows total show time remaining out of **85 minutes**
- Segment name colour-coded by type (purple = interview, green = music, pink = court)
- Progress bar turns rust-red in the final 60 seconds of a segment
- **START / PAUSE** and **NEXT →** controls

---

## Data structure in Firebase

```
/users/{uid}/fn_episodes/{episodeId}
  id, title, date, type, guest, defendant, judge, spotlight,
  notes, createdAt,
  segments: [ { n, t, note } ]
```

---

## Firebase config (already in the file)

```js
const firebaseConfig = {
  apiKey:            "AIzaSyDTJUaTv_LsUBxXsum9rAX119kWs45QuuI",
  authDomain:        "future-nostalgia.firebaseapp.com",
  databaseURL:       "https://future-nostalgia-default-rtdb.firebaseio.com",
  projectId:         "future-nostalgia",
  storageBucket:     "future-nostalgia.firebasestorage.app",
  messagingSenderId: "670094426397",
  appId:             "1:670094426797:web:0c246fb08079adb8c5e1f4",
  measurementId:     "G-TBVRG05V83"
};
```

SDK version: **Firebase 12.11.0** (with Analytics enabled)

---

## That's all

Upload `index.html`, enable Pages, whitelist your domain, set the database rules — and you're live. 🎙️

