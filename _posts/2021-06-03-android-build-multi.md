---
title: "리액트 네이티브 안드로이드 멀티환경 세팅 - 같은 기기에 dev, production 빌드"
date: 2021-06-03 00:00:00
categories: journal
---

리액트 네이티브로 개발을 하면서 프로덕션 빌드와 디버그 빌드를 같은 기기에 설치할 수 없는 문제가 있었다.
프로덕션을 설치를 하려면 개발 빌드를 지워줘야 하는데 이렇게 하면 이전 기기에 있는 데이터를 잃어버리는 문제가 있었다.
같은 기기에 설치할 수 없는 이유는 idenfier가 같기 때문인데 환경에 따라 idenfier를 다르게 세팅을 해줌으로써 같은 기기에 동시에 같은 앱을 설치할 수 있다.

1. 환경에 flavors 추가하기

android/app/build.gradle 에서 buildTypes 블록 밑에 다음과 같이 추가해 준다.

```
flavorDimensions "version"
productFlavors {
    local {
        applicationIdSuffix ".local"
    }
    staging {
        applicationIdSuffix ".staging"
    }
    production {}
}
```

이렇게 하면 applicationId 에 Suffix가 붙게 되어 앱의 identifier를 다르게 설정할 수 있다.

2. 환경마다 이름 다르게 하기

환경마다 앱의 이름이 같으면 디바이스에서 분간이 되지 않는다. 그래서 이름을 다르게 해주면 좋다.

android/app/src/{environment}/res/values/strings.xml 을 추가해서 이름을 설정해 주면 된다.

예를 들어 local의 경우 android/app/src/local/res/values/strings.xml 의 형식이 된다.

xml의 다음과 같이 추가해 주지.

```
<resources>
  <string name="app_name">Objective note</string>
</resources>
```

한글 앱 이름이 따로 설정이 되어 있는 경우라면 android/app/src/{environment}/res/values-kr/strings.xml 와 같이 따로 만들어 주면 된다,

3. 앱 실행하기

npx react-native run-android --variant=${flavor}${buildType} 의 형태로 실행해주면 된다.

복잡해 보이지만 local의 debug 빌드 타입의 경우 다음과 같이 하면 된다.

npx react-native run-android --variant=localDebug

조합이 가능한 커맨드는 다음과 같다.

```
localDebug
localRelease
stagingDebug
stagingRelease
productionDebug
productionRelease
```

4. AAB 빌드하기

기존과 같이 ./gradlew bundleRelease 가 가능하다. 하지만 이렇게 하면 환경별로 전부 aab가 다 만들어진다.
이전같이 production AAB만 만들고 싶다면 다음과 같이 하면 된다.

`./gradlew bundleProductionRelease`

귀찮아 보이지만 개발하다 보면 필요하다.

출처 : [https://morioh.com/p/ae25af711b30](https://morioh.com/p/ae25af711b30)
