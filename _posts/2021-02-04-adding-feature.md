---
title: "기능 추가하기"
date: 2021-02-04 00:00:00
categories: journal
---

리스트가 많아지기 시작하면서 조금씩 성능 문제가 드러나기 시작했다.

```
VirtualizedList: You have a large list that is slow to update - make sure your renderItem function renders components that follow React performance best practices like PureComponent, shouldComponentUpdate, etc. {dt: 25379, prevDt: 1765, contentLength: 3795.636474609375}
```

자주 나오지는 않지만 리렌더링해야 하는 수가 많아지면 이런 에러가 나는것 같다.

리액트는 리렌더링을 하는 기준이

- 부모의 props (속성)이 업데이트 되었을 때
- state (상태)가 업데이트 되었을 때
- forceUpdate()를 실행했을 때이다.

처음에는 좀 놀란게 부모의 상태가 업데이트 되면 자식들도 다시 업데이트 한다. 좀 놀랐지만 이렇게 하는데 상태 업데이트를 최신으로 하는 확실한 방법이라는 걸 이해하게 됐다.

하지만 전혀 관계없는 속성이 업데이트 되었는데 자식의 리스트가 업데이트 된다면 낭비이다. 그래서 React.memo로 최적화를 해줬다.

함수형 컴포넌트를 쓴다면 `export default React.memo(Item);` 를 써주면 된다. 이렇게 하면 props로 전달되는 변수와 함수가 업데이트 될 때만 리렌더링을 한다.

memo는 같은 인자가 들어왔을 때 같은 결과값이 나올 때 이미 저장된 값을 사용하기 때문에 리렌더링을 하지 않는 원리이다.

함수도 최적화를 하고 싶다면 props로 전달되는 함수에 useCallbak을 사용하여 최적화를 하면 된다. 아직 이것까지는 할 필요가 없어서 일단 안했다.

리액트는 리렌더링이 많아서 한번씩 최적화를 해줘야 하는 것 같다. memo를 써보고 리액트가 참 좋다는 생각이 들었다. 하지만 리렌더링이 많이 일어나는건 그렇게 좋지는 않은것 같다.

# 데일리 로그

오늘은 노트의 템플릿을 교체하는 기능을 추가했다. 생각보다 복잡했다. 좀 뭔가 직접적으로 교체를 하는 방법이 좋을 것 같은데 아직 떠오르지는 않는다.

작은 부분에 집중할 지 좀 더 확장하는 방향으로 나가야할지는 고민 중이다. 확장하는 방향으로 가면 좋기는 한데 코드가 복잡해 진다.

# 오늘 배운 작은 것들

- asyncStorage의 모든 값 가져오기

```
    try {
      const keys = await AsyncStorage.getAllKeys();
      let result = await AsyncStorage.multiGet(keys);
      console.log(result);
    } catch (error) {
      console.error(error);
    }
```

- object key 값 있는 지 확인

`obj.hasOwnProperty("key") // true`

- react navigation의 stack navigator는 navigation.navigate를 사용하면 스택을 쌓아두기 때문에 리액트가 남아있는 스택들에도 업데이트를 해준다. 이런 경우는 navigation.replace를 사용해 준다. 탭 네비게이터이는 리플레이스 함수가 없었다.
