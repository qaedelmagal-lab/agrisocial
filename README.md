# AgriSocial

Refactored, GitHub-ready static frontend for AgriSocial — منتدى المهندس الزراعي.

## Structure

- `index.html` — application markup only.
- `css/style.css` — consolidated application styles.
- `js/firebase-config.js` — Firebase/Cloudinary client configuration.
- `js/app.js` — forum/auth/profile/posts/comments/notifications logic.
- `js/news.js` — realtime news feed.
- `admin/index.html` — news administration UI.
- `admin/admin.js` — news CRUD and admin session gate.
- `admin/admin.css` — admin-only presentation styles.
- `FIREBASE_SETUP.md` — Firebase authorization/setup limitations and production guidance.
- `assets/logo.svg` — local application logo.
- `README.md` — setup and deployment notes.
- `.gitignore` — local/editor artifacts.

## Firebase

The existing Firebase Realtime Database project configuration was preserved from the supplied application. Client Firebase config values are normally public; **Realtime Database Rules must provide the actual authorization boundary**.

The current application uses the project's existing custom `users_v2` session model (`agri_s3`). The admin screen therefore recognizes a signed-in user whose `users_v2/{uid}.verifiedType` is `gold`. This is a UI/session gate, not a cryptographic security boundary.

### Required production hardening

Before treating the admin page as production-secure, migrate authorization to Firebase Authentication + Realtime Database Security Rules (or a trusted backend/Cloud Function). Do not rely on a client-controlled `verifiedType` field for security.

## News data model

Create records under:

`news/{newsId}`

Example:

```json
{
  "title": "عنوان الخبر",
  "body": "نص الخبر",
  "imageUrl": "",
  "url": "",
  "pinned": false,
  "timestamp": 1730000000000,
  "authorId": "ADMIN_UID"
}
```

The public feed reads `news` in realtime. The admin page creates, edits, pins and deletes records.

## Cloudinary

The existing Cloudinary upload preset is preserved for the forum's image/audio workflows. For production, use an appropriately restricted unsigned upload preset and validate file types/sizes.

## GitHub Pages

This is a static site and can be deployed to GitHub Pages. Firebase/Cloudinary remain external services.

For local testing, use a static HTTP server rather than opening `index.html` directly, for example:

```bash
python -m http.server 8080
```

Then open `http://localhost:8080/`.

## Testing performed by the refactor

Automated checks performed locally on the generated source:

- HTML structural checks.
- Balanced/closed HTML tags for the extracted document structure.
- JavaScript syntax checks with Node.js.
- Required project files and relative references checked.
- Firebase configuration presence checked.
- News screen/navigation/admin files checked.
- ZIP contents checked after packaging.

Browser-level Firebase authentication, Realtime Database Rules, Cloudinary uploads, microphone permissions and real network CRUD cannot be fully verified without the deployed environment and valid service-side permissions. Those limitations are intentionally not represented as “100% error-free”.
