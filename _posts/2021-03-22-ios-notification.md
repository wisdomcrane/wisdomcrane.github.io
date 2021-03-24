---
title: "리액트 네이티브 ios 알람"
date: 2021-03-22 00:00:00
categories: journal
---

ios 로컬 알람은 하루만에 끝났다. react-native-push-notification 이 @react-native-community/push-notification-ios 에 기반하여 만들어진 거라 ios 쪽이 훨씬 잘되어 있다.

https://github.com/react-native-push-notification-ios/push-notification-ios 의 문서 설정을 따라하면 된다.

디버그 빌드에서 로컬 통신 허용 알림이 떠서 놀라기는 했는데 디버그에서 리액트가 로컬 서버와 통신할 때 필요한 권한인것 같다. 릴리즈 버전에서는 발생하지 않을거라 생각해서 더이상 신경쓰지는 않았다.

그리고 ios 알림에서 weekly 설정이 가능하지 않고 daily 설정만 가능하다고 react-native-push-notification 문서에 나와있었는데 실제로 해보니 weekly 반복도 잘되었다. 문서에 잘못나와 있는것 같다.

ios 쪽에 역시 UI 이슈가 있어서 해결하고 마무리했다. 걱정 많이 했는데 생각보다 잘되어서 좋다!
