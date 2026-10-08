# Firebase setup notes

## Current project

The refactor preserves the Firebase Realtime Database project already embedded in the supplied AgriSocial application.

## News path

The new feature uses:

`news/{newsId}`

The public application reads this path in realtime.

## Admin authorization warning

The supplied application does not use Firebase Authentication. It stores a custom session marker in `localStorage` (`agri_s3`) and user records in `users_v2`.

For that reason, a browser-only rule such as “allow writes when `verifiedType == gold`” is **not a secure authorization mechanism**: a client can modify its own JavaScript/local state.

The included `/admin/` page therefore provides a convenience gate for the existing `gold` session, but it should not be considered a production security boundary.

### Recommended production change

1. Enable Firebase Authentication.
2. Link each AgriSocial user to a Firebase Auth UID.
3. Store the authoritative admin role in a protected location.
4. Write Realtime Database Rules that use `auth.uid`.
5. For highest assurance, perform admin news writes through a trusted backend/Cloud Function.

Until that migration is completed, keep the admin URL private and do not rely on the client-side gate for sensitive administration.

## Cloudinary

The existing unsigned upload preset is preserved because the forum already depends on it. Restrict accepted formats and file sizes in production and rotate the preset if it has been exposed or misconfigured.
