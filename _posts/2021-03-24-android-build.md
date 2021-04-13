---
title: "리액트 네이티브 안드로이드 빌드"
date: 2021-03-24 00:00:00
categories: journal
---

## 안드로이드 빌드

- 버전 업그레이드 : android/app/build.gradle 에서 versionCode 와 versionName 업그레이드 하기

- AAB 파일 만들기 :

```
cd android
./gradlew bundleRelease
```

## 기기에서 테스팅 하기

- 에뮬레이터가 켜져 있다면 꺼주기

- debug 버전이 깔려있다면 시그니쳐 충돌이 나므로 삭제하고 release 버전깔기

```
cd ..
npx react-native run-android --variant=release
```

- aab 파일 플레이스토어 업로드

폴더 위치 : android/app/build/outputs/bundle/release/app.aab

나의 경우 내부 테스트를 거쳐 플레이스토어에 업로드
