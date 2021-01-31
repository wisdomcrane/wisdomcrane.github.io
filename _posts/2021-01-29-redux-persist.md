---
title: "리액트 네이티브 리덕스 상태 로컬 스토리지에 저장하기"
date: 2021-01-29 00:00:00
categories: journal
---

오늘은 리덕스에서 저장한 상태를 로컬스토리지에 저장하는 걸 했다.

코드를 컴포넌트로 분리해서 정리하고 적어도 내가 보기 불편하지 않게 CSS 수정을 했다.

OKR 샘플이 입력되도록 오브젝트 구조를 정리했다.

# 리덕스 상태 로컬 스토리지에 저장

찾아보니 대부분 redux-persist로 하던데 원리를 이해해 보고 싶어서 그냥 생각해서 구현했다.

먼저 App 컴포넌트에서 store.subscribe로 상태 변화를 관찰하면서 전역 상태의 특정 변수가 업데이트 되면 값을 비교해서 로컬 스토리지에 저장하도록 했다.

```
function savaData() {
  const objectives = store.getState().objectives?.objectives;
  AsyncStorage.setItem('objectives', JSON.stringify(objectives));
  console.log('local saved');
}

let currentValue;
function handleChange() {
  let previousValue = currentValue;
  console.log('이전 밸류', previousValue);
  currentValue = select(store.getState());
  console.log('변경 밸류', currentValue);
  if (previousValue !== currentValue) {
    savaData();
  }
}
store.subscribe(handleChange);
```

그리고 홈 컴포넌트에서 useEffect를 사용하여 로컬스토리지 값을 불러와서 값이 있으면 전역 상태를 업데이트 해줬다.

```
  const getData = async () => {
    const storageData = await AsyncStorage.getItem('objectives');
    const parsed = JSON.parse(storageData);
    if (!parsed || !Object.keys(parsed).length) {
      // storage data가 빈 객체일 때는 업데이트 하지 않는다.
      return;
    }
    onSetState(parsed); // redux initial state 업데이트 dispatch.
    return;
  };
```

# 데일리 로그

뭔가 기본적인 것을 만들어냈다는 생각이 들어 기분이 좋았다.

리액트 네이티브는 리액트에서 스터디 한 것을 바로 사용할 수 있어서 선택을 잘 한 것 같다. 플러터였으면 학습 시간이 좀 더 길었을 것 같다.

css는 생각보다 어려웠던 것 같은데 나중에 시간을 내서 적용해야 겠다. 마음에 드는 컬러만 몇 개 골라봤다. 미디엄 시그린이랑 라이트 스카이 블루가 마음에 들었다!

한달이나 계속할 수 있을까...? 이번에는 한번 도전해봐야지!

![오브젝트 노트](/assets/image/object-note-prototype.png)
