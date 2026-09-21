name: Build APK
on: workflow_dispatch

jobs:
  build:
    runs-on: ubuntu-24.04
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-java@v5
        with: { distribution: 'temurin', java-version: '17' }

      # Installs and licenses the Android SDK platform/build-tools your
      # app needs to compile.
      - uses: android-actions/setup-android@v3

      # Officially maintained by Gradle. Installs exactly the Gradle
      # version requested and gives you a working `gradle` command —
      # no gradlew/gradle-wrapper.jar needed in the repo at all, and no
      # fragile "bootstrap a wrapper with whatever's pre-installed" step.
      - uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: 8.6

      # Diagnostic: confirms the project files landed where Gradle
      # expects them (repo root), so a structural mistake shows up here
      # instead of causing a confusing failure later.
      - name: List repo root (diagnostic)
        run: |
          echo "Repo root contents:"
          ls -la
          echo "---"
          echo "Looking for settings.gradle.kts:"
          find . -maxdepth 2 -name "settings.gradle.kts"

      # --stacktrace prints the full error if this step fails, instead
      # of a bare "exit code 1"
      - run: gradle assembleDebug --stacktrace

      - uses: actions/upload-artifact@v4
        with: { name: app-debug, path: app/build/outputs/apk/debug/*.apk }
# srms-android-app
