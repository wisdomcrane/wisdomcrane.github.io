---
title: "리액트 네이티브 안드로이드 빌드"
date: 2021-03-24 00:00:00
categories: journal
---

- 버전 업그레이드 : android/app/build.gradle 에서 versionCode 와 versionName 업그레이드 하기

- AAB 파일 만들기 :

```
cd android
./gradlew bundleRelease
```

- 기기에서 테스팅 하기

```
cd ..
npx react-native run-android --variant=release
```

- aab 파일 플레이스토어 업로드

폴더 위치 : android/app/build/outputs/bundle/release/app.aab

나의 경우 내부 테스트를 거쳐 플레이스토어에 업로드
