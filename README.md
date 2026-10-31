# ITI Attendance Book — Standalone Web App

A single-file web app for managing the ITI staff attendance register at
**Govt. International ITI, Dhanuvachapuram, Kerala**.

Built with: HTML · CSS · Vanilla JS · Firebase Auth · Firebase Firestore · GitHub Pages

---

## What changed from the Claude Artifact version

| Feature | Claude Artifact | This version |
|---|---|---|
| Hosting | Claude's servers | GitHub Pages (your own repo) |
| Database | Claude's built-in `db` | Firebase Firestore |
| Login | None (shared link) | Email + password per instructor |
| Data isolation | Shared by everyone | Each instructor has their own private data |
| File downloads | Claude sandbox | Standard browser download |
| Dependencies | Claude account | None — works in any browser |

---

## One-time Setup (do this once)

### Step 1 — Create a Firebase project

1. Go to **https://console.firebase.google.com** and sign in with a Google account.
2. Click **"Add project"** → enter a name (e.g. `iti-attendance`) → click through the steps → **Create project**.
3. When the project is ready, click **Continue**.

### Step 2 — Enable Email/Password Authentication

1. In the Firebase Console sidebar, click **Authentication** → **Get started**.
2. Click the **Sign-in method** tab.
3. Click **Email/Password** → toggle **Enable** → **Save**.

### Step 3 — Create a Firestore database

1. In the sidebar, click **Firestore Database** → **Create database**.
2. Choose **"Start in production mode"** → **Next**.
3. Pick a location close to Kerala (e.g. `asia-south1` — Mumbai) → **Done**.

### Step 4 — Apply the security rules

1. In Firestore, click the **Rules** tab.
2. Replace everything in the editor with the contents of `firestore.rules` (from this repo).
3. Click **Publish**.

The rules ensure each instructor can only read and write their own data — no one can see another instructor's records.

### Step 5 — Get your Firebase config keys

1. In the Firebase Console, click the ⚙️ gear icon (top-left) → **Project settings**.
2. Scroll down to **"Your apps"** section.
3. If no app exists yet, click the **`</>`** (Web) icon → enter a name → **Register app**.
4. Copy the `firebaseConfig` object shown — it looks like:

```js
{
  apiKey: "AIzaSy...",
  authDomain: "your-project-id.firebaseapp.com",
  projectId: "your-project-id",
  storageBucket: "your-project-id.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
}
```

### Step 6 — Paste the config into `index.html`

Open `index.html` in any text editor. Find this block near the top:

```js
const FIREBASE_CONFIG = {
  apiKey:            "YOUR_API_KEY",
  authDomain:        "YOUR_PROJECT_ID.firebaseapp.com",
  projectId:         "YOUR_PROJECT_ID",
  ...
};
```

Replace every `"YOUR_..."` placeholder with your actual values from Step 5. Save the file.

### Step 7 — Create a GitHub repository and enable GitHub Pages

1. Go to **https://github.com** → sign in → click **New repository**.
2. Name it (e.g. `iti-attendance-book`) → set it **Public** → click **Create repository**.
3. Upload `index.html` (and optionally `firestore.rules` and `README.md`) to the repository.
4. Go to the repository **Settings** tab → **Pages** (left sidebar).
5. Under **"Source"**, select **"Deploy from a branch"** → branch: **main** → folder: **/ (root)** → **Save**.
6. After a minute, your app will be live at:
   ```
   https://YOUR-GITHUB-USERNAME.github.io/iti-attendance-book/
   ```

That URL is what you share with your instructors.

---

## Migrating your existing data

If you have data in the old Claude Artifact version:

1. **Open the old Claude Artifact** in your browser.
2. Go to **Settings → Export Data (JSON)** — this downloads a `.json` file.
3. **Open the new app** at your GitHub Pages URL.
4. Create an account (or sign in) with your email and password.
5. Go to **Settings → Import Data** → select the `.json` file you downloaded.
6. Your staff, shifts, holidays, and settings are now in Firestore.

> **Note:** Each instructor imports their own data into their own account separately. Data is not shared between accounts.

---

## Day-to-day use

- Open the GitHub Pages URL in any browser on any device.
- **Create an account** the first time — click "Create Account" on the login screen.
- Any instructor can create their own account at the same URL.
- Each account's data is **completely private** — instructors cannot see each other's records.
- Changes sync automatically across all devices you're signed in to.
- Use **Generate Register** to produce a printable A4 PDF for any month and shift.

---

## Editing the app

All the code is in one file: `index.html`. Open it in any text editor (VS Code, Notepad, etc.).

- To change the institution name, department, or header — use **Settings** inside the app.
- To change the print layout, fonts, or row heights — use **Settings** inside the app.
- To modify the code — edit `index.html` directly and re-upload to GitHub. GitHub Pages auto-publishes within ~1 minute.

---

## Frequently asked questions

**Can I use this without an internet connection?**  
Yes — the app caches your data in the browser's local storage. You can view and edit offline; changes sync to Firestore the next time you're online.

**What happens if two devices edit at the same time?**  
The most recent save wins. Firestore's `onSnapshot` listener keeps all open devices in sync in real time.

**How do I reset a forgotten password?**  
On the login screen, click **"Forgot password?"** and enter your email. You'll receive a reset link.

**Is my data safe?**  
Yes. Firestore security rules (Step 4) ensure that only you — when authenticated — can read or write your own data. Firebase encrypts data in transit and at rest.

**How do I add a new instructor?**  
They just open the app URL and click "Create Account". Each instructor manages their own independent data.

**The app was working yesterday but shows an error today — what do I do?**  
Check the Firebase Console → Authentication to confirm the project is still active. Free-tier Firebase projects are not automatically deleted, but if you upgrade or downgrade a billing plan, check that Firestore is still enabled.

---

## File listing

```
index.html        — The complete app (all HTML, CSS, JS in one file)
firestore.rules   — Firestore security rules (paste into Firebase Console)
README.md         — This setup guide
```

---

*Built for Govt. International ITI, Dhanuvachapuram — Kerala Industrial Training Department*
