---
title: "리액트 네비게이션 탭바 리덕스 스토어 문제"
date: 2021-02-08 00:00:00
categories: journal
---

오늘은 하루종일 헤멘것 같다.

문제의 발단은 탭바이다.

react navigation의 탭 바를 적용했는데 리렌더링이 되지 않고 중복 렌더링에 의한 자원 낭비가 발생했다.

문제의 원인은 3개의 노트에 하나의 스토어를 공유하여 사용하고 있었기 때문이다.

원래는 하나의 노트를 만들려고 했으나 만들면서 기획이 늘어나 세 개의 노트를 하나의 컴포넌트와 스토어를 바꿔가며 사용하고 있었다.

그런데 탭바는 한번 이동하면 이전의 컴포넌트를 유지하는 속성이 있었다. (이걸 몰라서 엄청 고생했다.)

그래서 스토어의 업데이트가 발생하면 현재 열려 있는 탭바의 컴포넌트에 중복 렌더링을 실시했다.

또 이상하게 스토어가 바뀌었으나 동일한 이름이기에 탭바에 열려있는 컴포넌트에 컴포넌트 초기 렌더링을 하지 않았다.

결국 스토어를 나눠주고 혹시 몰라 컴포넌트도 따로 만들어 주어 해결했다.

처음부터 스토어를 나누었어야 했다. 원인을 몰라 정말 눈물 날뻔했다.

컴포넌트도 로직이 다르면 나누어 주는게 낫다.

기존의 로컬 스토리지 저장 로직도 원래 짠 코드를 redux-persist로 바꾸었다. 탭을 이동할 때 AsyncStorage를 await로 사용하고 있었는데 상당히 느렸기 때문이다.

나는 저장할 때 시간 일지의 경우 날짜키를 달아 변형하여 저장하고 불러올 때 오늘의 일지가 있으면 times 스토어에 저장해주는 방식을 사용하고 있었다.

이 경우 redux-persist의 transform을 사용해 주면 되었다.

```
import { createTransform } from 'redux-persist';

const SetTransform = createTransform(
  // transform state on its way to being serialized and persisted.
  (inboundState, key) => {
    // convert mySet to an Array.
    return { ...inboundState, mySet: [...inboundState.mySet] };
  },
  // transform state being rehydrated
  (outboundState, key) => {
    // convert mySet back to a Set.
    return { ...outboundState, mySet: new Set(outboundState.mySet) };
  },
  // define which reducers this transform gets called for.
  { whitelist: ['someReducer'] }
);

export default SetTransform;
```

inbound의 경우 로컬 스토리지에 저장히기 전 변형하여 저장을 할 수 있는 기능이고

outbound(rehydrated)의 경우 스토리지에서 불러와서 store에 다시 저장하기 전 변환을 하는 기능이다.

매우 유용하게 사용했다.

redux persist도 탭 전환 시 매우 빠르게 반응하지는 않는다. 리액트 네이티브는 아마도 로컬 스토리지에 저장 할 때 좀 느린것 같다.

그리고 redux의 구조에 대해 잘 몰라서 해멘 것도 있다. dispatch를 하면 root reducer에 등록된 모든 리듀서를 돌면서 액션 키 값에 맞는 리듀서를 실행을 한다. 리듀서의 액션 부분을 콘솔을 찍어서 보고 있었는데 2개의 리듀서에서 action이 콘솔에 찍혀서 두 번 날리는 줄 알았다. 중복으로 날리는게 아니라 모든 리듀서를 도는 것이었다. 돌아가는 개념을 몰라서 고생한 경우라고 할 수 있겠다.

프로그래밍이 생각보다 어렵고 함들게 느껴졌다. 조금씩 복잡해 지는 것 같다. 이렇게 복잡한 것을 단순하게 잘 이해하고 풀어내는 사람들이 고수이겠지? 이제 시작이니깐 너무 욕심을 내지는 말자.

프로그래밍은 잘 되지 않았지만 문제의 원인을 파악하고 진단을 하고 해결 방안을 모색하고 실행하는 싸이클을 배울 수 있었다. 아마 다른 곳에도 이런 방법을 적용할 수 있지 않을까?

이제 쉬자. 일이 끝나면 생각하지 말자.

차단!
