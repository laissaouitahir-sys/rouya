name: rouya
on:
  workflow_dispatch:
  push:
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: unzip -q RouyaAndroid-with-CI.zip
      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'
      - run: yes | sdkmanager --licenses > /dev/null || true
      - run: sdkmanager "platforms;android-35" "build-tools;35.0.0"
      - uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.9'
      - run: gradle assembleDebug --no-daemon
        working-directory: RouyaAndroid
      - uses: actions/upload-artifact@v4
        with:
          name: Rouya-APK
          path: RouyaAndroid/app/build/outputs/apk/debug/app-debug.apk
