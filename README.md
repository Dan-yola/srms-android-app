name: Build APK

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-24.04

    env:
      FORCE_JAVASCRIPT_ACTIONS_TO_NODE24: true

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up Java
        uses: actions/setup-java@v5
        with:
          distribution: temurin
          java-version: '17'

      - name: Set up Android SDK
        uses: android-actions/setup-android@v3

      - name: Make Gradle executable
        run: chmod +x ./gradlew

      - name: Build APK
        run: ./gradlew assembleDebug
