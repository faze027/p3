# P3 Podcast

F1 prediction game for Fayez and Leona. Sealed picks, auto scoring and a season leaderboard for the last five races of 2026. Picks sync live between your devices through Firebase (free tier).

## Files

- `index.html` — the game
- `firebase-config.js` — your Firebase keys go here
- `logo.svg` — the P3 logo (also the browser-tab icon)

## 1. Set up Firebase (about 10 minutes, free)

1. Go to https://console.firebase.google.com and click **Create a project**. Name it `p3-podcast`. You can turn Google Analytics off.
2. In the left menu, open **Build → Firestore Database → Create database**.
   - Location: `northamerica-northeast1 (Montréal)` is closest to Ottawa.
   - Start in **production mode**.
3. Open the **Rules** tab, replace everything with the rules below and click **Publish**.
4. Go to **Project settings** (gear icon) → **General** → **Your apps** → click the **Web** icon (`</>`). Name it `p3-web`. Skip Firebase Hosting.
5. Copy the `firebaseConfig` values it shows into `firebase-config.js`.

### Firestore rules

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /picks/{id} {
      allow read: if true;
      allow write: if id.matches('[a-z]+-2026_(fayez|leona)')
        && request.resource.data.keys().hasOnly(['player','race','top5','pole','firstOut','fastest','dotd','sc','surprise','flop','crazy','locked','updatedAt']);
    }
    match /results/{id} {
      allow read: if true;
      allow write: if id.matches('[a-z]+-2026')
        && request.resource.data.keys().hasOnly(['race','top5','pole','firstOut','fastest','dotd','sc','judge','final','updatedBy','updatedAt']);
    }
    match /{document=**} {
      allow read, write: if false;
    }
  }
}
```

These rules only allow the game's own data. There are no logins, so anyone who has the link can play. Keep the link between the two of you.

## 2. Put it on GitHub Pages

1. Create a new repository on GitHub, for example `p3-podcast`. It can be public.
2. Upload `index.html`, `firebase-config.js`, `logo.svg` and `README.md` to the root of the repo (Add file → Upload files → Commit).
3. Go to **Settings → Pages**. Under **Build and deployment**, pick **Deploy from a branch**, branch `main`, folder `/ (root)`, and save.
4. After a minute your site is at `https://<your-username>.github.io/p3-podcast/`.

## 3. Play

Send Leona the link. Each of you taps your own name at the top right once on your device. Pick, lock in, and after the race either of you enters the real results and taps **Finalize & score**.

The status line under the race name says **Live board** when Firebase is connected.

## Changing the drivers or races

Both lists are near the top of the `<script>` in `index.html` (`DRIVERS` and `RACES`). Race times are in UTC.
