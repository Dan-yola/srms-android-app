name: Build APK

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-24.04

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-java@v5
        with:
          distribution: temurin
          java-version: '17'

      - uses: android-actions/setup-android@v3

      - uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.7'

      - name: Build APK
        run: gradle assembleDebug --stacktrace

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: app-debug
          path: '**/build/outputs/apk/**/*.apk'
