name: Build APK

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-24.04

    steps:
      - name: Checkout
        uses: actions/checkout@v7

      - name: Set up Java
        uses: actions/setup-java@v6
        with:
          distribution: temurin
          java-version: '17'

      - name: Set up Android SDK
        uses: android-actions/setup-android@v4

      - name: Make Gradle executable
        run: chmod +x ./gradlew

      - name: Build APK
        run: ./gradlew assembleDebug

      - name: Upload APK
        uses: actions/upload-artifact@v6
        with:
          name: app-debug
          path: app/build/outputs/apk/debug/*.apk
