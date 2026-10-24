
name: Build Android APK

on:
  workflow_dispatch:
  push:
    branches: [main, master]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Find and extract project ZIP
        id: project
        run: |
          ZIP=$(find . -maxdepth 1 -type f -name '*.zip' | head -n 1)
          if [ -z "$ZIP" ]; then
            echo "No project ZIP found in repository root"
            exit 1
          fi
          mkdir source
          unzip -q "$ZIP" -d source
          DIR=$(find source -name settings.gradle -type f -printf '%h\n' | head -n 1)
          if [ -z "$DIR" ]; then
            echo "Android Gradle project not found"
            exit 1
          fi
          echo "dir=$DIR" >> "$GITHUB_OUTPUT"

      - name: Build APK
        run: |
          cd "${{ steps.project.outputs.dir }}"
          gradle assembleDebug

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: azhar-study-debug-apk
          path: ${{ steps.project.outputs.dir }}/app/build/outputs/apk/debug/app-debug.apk
