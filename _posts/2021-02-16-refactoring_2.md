---
title: "리팩터링 2"
date: 2021-02-16 00:00:00
categories: journal
---

오늘은 앱을 사용하기 좋게 개선했다.

다음은 react-persist에서 특정 키를 지우는 소스이다.

```
    const persist = await AsyncStorage.getItem('persist:root');
    let parse = JSON.parse(persist);
    delete parse['specifickey'];
    await AsyncStorage.setItem('persist:root', JSON.stringify(parse));
```

css absolute position에서 가운데 정렬을 하려면 앱솔루트에 다음과 같이 css를 주면 된다. 간단하다.

```
AbsoluteButton: {
  position: 'absolute',
  alignSelf: 'center',
  bottom: 10
  }
```

리액트 네이티브의 글로벌 언어 지원을 위해 i18next를 사용했다.

https://react.i18next.com/getting-started#installation

코드 정리를 했다. 뭔가 rn에 한계가 많은 것 같다. 휴 어떻게 해야 하나... 일단 공식문서라도 좀 더 읽어봐야겠다!
