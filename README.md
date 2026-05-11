# Peltola Open live overlay

Static Firebase/GitHub Pages version with team setup in the control panel.

## Files

- `firebase-config.js` - paste your Firebase web app config here.
- `control.html` - scorekeeper/admin page. Add, rename, reorder, and delete teams here.
- `overlay.html` - transparent overlay for PRISM Live Studio Web Browser Widget.

## URLs

After publishing to GitHub Pages:

```text
https://YOUR_GITHUB_USERNAME.github.io/YOUR_REPO/control.html?event=2026
https://YOUR_GITHUB_USERNAME.github.io/YOUR_REPO/overlay.html?event=2026
```

Paste the overlay URL into PRISM Live Studio as the Web Browser Widget URL.

## Firebase Realtime Database rules

Use public reads for the overlay and authenticated writes for the control panel:

```json
{
  "rules": {
    "events": {
      "$eventId": {
        ".read": true,
        ".write": "auth != null && root.child('admins').child($eventId).child(auth.uid).val() === true"
      }
    },
    "admins": {
      ".read": false,
      ".write": false
    }
  }
}
```

## Admin setup

1. Enable Firebase Authentication with email/password.
2. Create a scorekeeper user.
3. Copy that user's UID.
4. Add it to Realtime Database manually:

```json
{
  "admins": {
    "2026": {
      "PASTE_SCOREKEEPER_UID_HERE": true
    }
  }
}
```

The first time an approved scorekeeper opens `control.html?event=2026`, the app creates the event if it does not exist yet.

## Optional debug mode

Open the overlay in a browser with a visible background:

```text
overlay.html?event=2026&debug=1
```
