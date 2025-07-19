name: Build Unity Android APK

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  build:
    name: Build APK
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v3

      - name: Cache Unity
        uses: actions/cache@v3
        with:
          path: ${{ github.workspace }}/Library
          key: Library-${{ hashFiles('**/*.cs') }}
          restore-keys: |
            Library-

      - name: Setup Unity
        uses: game-ci/unity-actions/setup@v2
        with:
          unityVersion: 2022.3.10f1  # ✅ Replace with your Unity version
          android: true

      - name: Build Android
        uses: game-ci/unity-actions/build@v2
        with:
          targetPlatform: Android

      - name: Upload APK Artifact
        uses: actions/upload-artifact@v3
        with:
          name: PrincipalPanic.apk
          path: build/Android/*.apk  # ✅ Adjust path if needed
