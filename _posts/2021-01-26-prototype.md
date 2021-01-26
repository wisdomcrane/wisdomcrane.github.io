---
title: "오브젝트 노트 프로토 타입"
date: 2021-01-26 00:00:00
categories: journal
---

오늘은 2일차이다. 오랜만에 일을 시작해서 일하는 시스템을 먼저 만들었다. 일을 기준으로 정하지 않고 시간을 정하면서 시작하기로 했다. 구글 캘린더에 일한 시간을 타임 블럭으로 기록했다. 2시간 정도 일하고 30분 정도 쉬는 시스템이고 4시간 정도 일하면 원하는 만큼 쉴 수 있다. 오랜만에 시작해서 그런지 조금 어지러웠다. 최대한 쉬는 전략으로 가기로 했다. 시간 기록과 휴식이 전략이다.

먼저 리액트 네이티브로 간단한 오프라인 노트를 만들기로 했다. 에러의 향연이었다. 개념을 정확하게 모르는 상태에서 모르는 부분은 스터디를 해가면서 했다. 설치해야할 dependancy를 설치하지 않아서 생기는 에러가 많았다.

# AsyncStorage

[AsyncStorage 링크](@react-native-async-storage/async-storage)

react native code에 AyncStorage가 더 유지 되지 않는다. @react-native-async-storage/async-storage를 사용해야 한다.

`import AsyncStorage from '@react-native-async-storage/async-storage'`

```
AsyncStorage.getItem('todos')
const parsedTodos = JSON.parse(todos)
```

Async로 가져온 건 JSON.parse()를 해줘야 크러시가 나지 않는다.

```
AsyncStorage.setItem('todos', JSON.stringify(newTodos))
```

setItem을 할 때는 JSON.stringify로 문자열 형태로 만들어줘야 크러시가 나지 않는다.

# react-navigation

[react-navigation 문서](https://reactnavigation.org/docs/getting-started/)

리액트 네이티브의 네비게이션 모듈이다.

# nativebase

[nativebase 문서](https://reactnavigation.org/docs/getting-started/)

UI 모듈이다.

# FlatList

[Flatlist 문서](https://reactnative.dev/docs/flatlist)

리액트 네이티브의 리스팅 컴포넌트이다. ScrollView도 있는데 Flatlist가 좀 더 성능이 좋은 것 같다.

# 데일리로그

학습해야 할게 생각보다 많다. 하나씩 천천히 읽어봐야 겠다. 게으르면 개발자 못하겠구나 ㅋㅋ 열공해보자!
내일은 만든걸 내것으로 소화시키고 에러잡고 기본틀을 잡아야겠다.

오늘은 간단한 오프라인 노트를 완성했다. 힘든데 생각보다 재미있는 면도 있는 것 같다.

![완성한 오브젝트 노트](/assets/image/objectnote-proto.png)
