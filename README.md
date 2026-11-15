# SIK MOBILE 2.0 — Android assistant

This update preserves the v1 home-screen widget and notification listener, and adds:
- Draggable blue holographic floating orb (requires Android “Display over other apps” permission).
- Tap-to-talk voice input and Spanish speech output.
- Optional “Hola SIK” wake phrase listener implemented as a microphone foreground service. Android shows a persistent notification while listening; enable it explicitly and stop it from the app.
- Configurable OpenAI-compatible Chat Completions endpoint, API key and model. Keys are stored in the app's private SharedPreferences; do not use a shared/debug build on a device you do not control.
- Contact lookup and call handoff through the system dialer (user presses call).
- WhatsApp message handoff and email composer; user reviews and sends in the target app.
- Shortcuts to notification and accessibility settings.

## Important platform boundaries
- Android permissions are granted by the user in system settings/runtime dialogs; the app cannot silently enable them.
- The wake phrase is opt-in and uses a visible ongoing microphone notification. Speech recognition availability, battery policy and vendor firmware can interrupt it. It is not guaranteed to be always-on like a system assistant.
- Generic API endpoints must implement the OpenAI-compatible `/chat/completions` request/response shape. For providers with a different schema, use a compatible gateway/proxy.
- API keys are stored locally in app-private preferences. For production distribution, move secrets to Android Keystore-backed encryption and preferably a private proxy; never embed a shared API key in a public APK.
- WhatsApp, email and calling are handed off to their official apps/system UI. SIK does not silently send messages or place calls.
- Accessibility service is registered but intentionally does not automate arbitrary app screens yet. Android accessibility use must be user-enabled and restricted to transparent, user-initiated actions.

## Build from a phone with GitHub Actions
1. Upload the contents of this folder to the root of your `sik-mobile` GitHub repository, replacing the old source files.
2. Open Actions → **Build SIK MOBILE APK** → **Run workflow**.
3. Download artifact `SIK-MOBILE-debug-apk`, extract it and install `app-debug.apk`.
4. In SIK, grant microphone and contacts when requested; separately enable notification access, overlay permission, and (only if needed) accessibility in Android Settings.
5. To use the floating orb, enable **Activar esfera flotante**. To use “Hola SIK”, enable **Activar palabra de activación**; Android displays an ongoing notification while the mic listener runs.

## Build locally
Requires JDK 17, Android SDK 35, and Gradle 8.9: `gradle assembleDebug`.
