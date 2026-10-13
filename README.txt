STEPS (all from your phone browser, GitHub website)
1. GitHub > New repository (e.g. linkon-ot) > Add file > Upload files:
   index.html, manifest.json, sw.js, icon.jpg, builder.jpg
2. Create the workflow: Add file > Create new file > name it
   .github/workflows/build-apk.yml  (typing the slashes makes folders) > paste build-apk.yml content > Commit.
3. Firebase console (project linkon-ot):
   - Build > Authentication > Sign-in method > enable Email/Password
   - Build > Firestore Database > Create (production mode) > Rules tab > paste rules below > Publish
   - Project settings > Your apps > Web (</>) > copy config into the FB={...} line at top of index.html
4. GitHub repo > Actions tab > "Build APK" > Run workflow. After ~5-8 min open the run > Artifacts > LINKON-OT-apk (zip) > extract > install app-debug.apk.

FIRESTORE RULES
rules_version = '2';
service cloud.firestore {
  match /databases/{db}/documents {
    match /users/{u} {
      allow read: if request.auth != null;
      allow create, update: if request.auth != null;
    }
    match /posts/{p} {
      allow read: if request.auth != null;
      allow create: if request.auth != null && request.resource.data.uid == request.auth.uid;
      allow update: if request.auth != null;
      allow delete: if request.auth != null && resource.data.uid == request.auth.uid;
    }
  }
}
