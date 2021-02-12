---
title: "UI 개선하기"
date: 2021-02-11 00:00:00
categories: journal
---

내가 사용하기 편할 정도로 UI를 개선했다.

# SVG 적용

기존 SVG를 수정하고 프로젝트에서 사용했다.

[https://github.com/react-native-svg/react-native-svg](react-native-svg)를 사용했다.

컴파일 할 때 준비하고 사용하기 위해서 react-native-svg-transformation도 설치했다.

metro.config.js의 기존 코드를 다음과 같은 코드로 교체했다. 나는 react native cli를 사용중이다.

```
const { getDefaultConfig } = require('metro-config');

module.exports = (async () => {
  const {
    resolver: { sourceExts, assetExts },
  } = await getDefaultConfig();
  return {
    transformer: {
      babelTransformerPath: require.resolve('react-native-svg-transformer'),
    },
    resolver: {
      assetExts: assetExts.filter(ext => ext !== 'svg'),
      sourceExts: [...sourceExts, 'svg'],
    },
  };
})();
```

이렇게 하면 빌드할 때 svg 파일들을 사용가능하게 준비해준다. 그러면 기존 컴포넌트처럼 사용해 주면 된다.

```
import Logo from './logo.svg';

<Logo width={120} height={40} />
```

아이콘은 material ui를 사용하는데 정말 아름답다.

아이콘을 편집할 때는 inkscape를 사용했다. 사각형이나 원 등의 도형을 그리고 노드를 연결하는 선으로 쉽게 편집할 수 있었다. 좋다!

# 모달 창 바깥을 클릭하면 창 닫기

모달창 바깥을 클릭하면 창이 닫히게 하고 싶었다. 이럴 경우 TouchableWithoutFeedback으로 전체 뷰를 감싼 뒤 클릭이 되지 않을 컨텐츠 영역을 빈 함수가 들어있는 TouchableWithoutFeedback으로 감싸면 된다.

```
<TouchableWithoutFeedback onPress={() => setIsVisible(false))}>
        <Modal isVisible={isVisible}>
            <TouchableWithoutFeedback onPress={() => {}}>
                    ... 커스텀 뷰
            </TouchableWithoutFeedback
       </Modal>
</TouchableWithoutFeedback>
```

# 날짜 객체

date 객체를 만들고 메쏘드를 사용하는 방식이다.

```
new Date().toLocaleString()

let today = new Date()
let birthday = new Date('December 17, 1995 03:24:00')
let birthday = new Date('1995-12-17T03:24:00')
let birthday = new Date(1995, 11, 17) // the month is 0-indexed
let birthday = new Date(1995, 11, 17, 3, 24, 0)
let birthday = new Date(628021800000) // passing epoch timestamp
```

그 외에도 UI 잔잔바리들을 잔잔하게 쳤다. 좀 피곤한듯...

# vs code git history 보기 위한 위젯 설치

- Git history (폴더에서 마우스 우클릭을 하면 깃히스토리를 편하게 볼 수 있다.)
- git graph (선택사항. 깃 히스토리를 그래프로 볼 수 있다.)
