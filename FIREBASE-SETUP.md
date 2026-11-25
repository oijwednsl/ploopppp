# Ploop Firebase setup

## Included integration
- Firebase Authentication (email/password)
- Firestore-backed accounts, rooms, capsules, canvases and reactions
- Authenticated cross-device sign-in and live Firestore listeners
- Username-based friend records with reciprocal friend-list updates

## Before deploying
1. In Firebase Console, open **Authentication → Settings → Authorized domains** and add the exact GitHub Pages hostname, e.g. `oijwednsl.github.io`.
2. In **Firestore Database → Rules**, paste the contents of `firestore.rules` and publish.
3. Confirm Email/Password is enabled in Authentication.
4. Upload the contents of this folder to the root of the GitHub repository (preserve `img/` and `art.js`).
5. Open the GitHub Pages URL in a new private/incognito browser window and create a new test account using email, password and username.

## Important migration note
Existing username-only accounts in browser localStorage are not Firebase Auth accounts and cannot be securely reused for cross-device login. Create new Firebase-backed accounts (or implement a separate verified migration flow). Existing local demo data remains on the old browser and is not automatically uploaded.

## Security notes
- The Firebase web config in `index.html` is public client configuration, not a server secret.
- Do not put service-account keys or Admin SDK credentials in this repository.
- Firestore rules restrict account writes to the signed-in owner, with a narrow exception allowing a user to add/remove their own username in another account's friend list. Room data is limited to room members.
- Review the rules against your final feature set before inviting real users. In particular, current prototype room members can update room documents; add role-based restrictions before production.

## Troubleshooting
- `auth/unauthorized-domain`: add the exact hostname shown in the browser address bar to Authentication → Settings → Authorized domains.
- `permission-denied`: confirm the rules are published and that the signed-in account has a matching `users/{uid}` mapping document.
- `auth/operation-not-allowed`: enable Email/Password in Authentication → Sign-in method.
- Firebase console → Firestore → Data should show `accounts`, `users`, `rooms`, `capsules`, `canvases` and `reactions` after activity.
