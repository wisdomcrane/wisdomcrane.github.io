---
title: "리액트 네이티브 로컬 알람 안드로이드"
date: 2021-03-16 00:00:00
categories: journal
---

# 세팅

세팅은 [dev yakuza](https://dev-yakuza.posstree.com/ko/react-native/react-native-push-notification/)님의 블로그를 참고 했다.

[react-native-push-notification](https://github.com/zo0r/react-native-push-notification)를 사용했다.

/android/app/src/main/AndroidManifest.xml에서 권한 설정을 할 때 중요한 점은 channel 이름을 설정해 줘야 한다.

```
<meta-data  android:name="com.dieam.reactnativepushnotification.notification_channel_name"
android:value="CHANGE YOUR INFO!!"/>
<meta-data  android:name="com.dieam.reactnativepushnotification.notification_channel_description"
android:value="CHANGE YOUR INFO!!"/>
```

CHANGE YOUR INFO 부분에 특정한 채널명을 적어준다. 나는 app.objectnote로 했다.

# 멀티덱스

빌드를 할 때 multidex 관련 에러가 났는데 이는 yaml 파일에 많은 패키지를 import 할 때 하나의 .dex 파일로 합쳐줄 때 발생하는 에러이다. 그래서 멀티덱스를 enabled 해주면 된다.

android/app/build.gradle에서 멀티덱스를 사용 설정해 주면 된다.

```
    defaultConfig {
        ...
        multiDexEnabled true
    }
```

# 로컬 알람 설정 시 파이어베이스 not initialized 문제

앱을 실행하면 react native default firebaseapp is not initialized in this process 에러가 난다. 나는 로컬 알람만 사용할거라 파이어베이스가 필요없는데 에러가 나서 찾아봤더니 react-native-push-notification 문서에 다음과 같이 나와있었다.

```
  /**
   * (optional) default: true
   * - Specified if permissions (ios) and token (android and ios) will requested or not,
   * - if not, you must call PushNotificationsHandler.requestPermissions() later
   * - if you are not using remote notification or do not have Firebase installed, use this:
   *     requestPermissions: Platform.OS === 'ios'
   */
  requestPermissions: true,
```

리모트 알림이나 파이어베이스를 사용하지 않는다면 requestPermissions: Platform.OS === 'ios' 로 주면 된다는 이야기이다. 이 경우 ios일 경우만 requestPermissions 가 true가 된다. 안드로이드에서는 true가 되면 파이어베이스 관련 코드를 init하는거 같다. 나처럼 로컬 알림만 사용하고 싶은 사람들은 참고하자.

# configure 설정 및 컴포넌트에서 사용

나의 경우 index.js 파일에 설정을 기술했다.

```
PushNotification.configure({
    onNotification: function (notification) {
      console.log('LOCAL NOTIFICATION ==>', notification);
    },

    requestPermissions: Platform.OS === 'ios',
  });
```

그리고 컴포넌트에서 사용할 때는 함수를 만들어서 export해서 사용하면 된다. 예를 들면 lib/LocalNotification.js를 만들어서 다음과 같은 함수를 만들어서 사용한다.

```
export const LocalNotification = () => {
  PushNotification.localNotificationSchedule({
    channelId: 'app.objectnote',
    message: 'My local push notification', // (required)
    date: new Date(Date.now() + 3 * 1000), in 3 secs
    playSound: true, // (optional) default: true
    soundName: 'default',
  });
};
```

주의 할 점은 안드로이드에서는 channelId 부분을 적어야 작동한다. 앞서 세팅할 때 만들었던 채널 이름을 사용한다.

채널이 등록이 되지 않을 경우 푸시가 알림이 오지 않는데 다음과 같이 채널을 설정해 준다.

```
PushNotification.createChannel(
  {
    channelId: 'app.objectnote', // (required)
    channelName: 'app.objectnote', // (required)
    channelDescription: 'app.objectnote', // (optional) default: undefined.
    playSound: true, // (optional) default: true
    soundName: 'default', // (optional) See `soundName` parameter of `localNotification` function
    importance: 4, // (optional) default: 4. Int value of the Android notification importance
    vibrate: true, // (optional) default: true. Creates the default vibration patten if true.
  },
  (created) => console.log(`createChannel returned '${created}'`), // (optional) callback returns whether the channel was created, false means it already existed.
);
```

채널이 있을 경우 콜백 부분의 created가 true로 나온다. false는 이미 있다는 것을 의미한다.

참고로 사운드는 앱이 foreground에 있을 때는 진동만 오고 background에 있을 때는 소리와 진동이 같이 온다.

react-native-push-notification은 앱이 사용중에 있든 백그라운드에 있든 닫혀있든 잘 온다. 로컬 기기에서 사용하기에 좋은 알람이다.
