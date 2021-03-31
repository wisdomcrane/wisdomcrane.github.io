---
title: "리액트 네이티브 안드로이드 낮은 대비 대응하기"
date: 2021-03-31 00:00:00
categories: journal
---

리액트 네이티브로 안드로이드 빌드를 하고 테스트를 올리면 구글에서 기계가 테스트를 해준다.

이 때 나의 경우 가장 많이 발생하는 이슈는 낮은 색깔 대비 문제였다.

안드로이드에서는 큰 텍스트의 경우 색상 대비가 3.0 작은 텍스트의 경우 4.5 이상을 권장한다.

color contrast 는 다음 사이트에서 확인할 수 있다.

https://webaim.org/resources/contrastchecker/

[contrast checker](https://webaim.org/resources/contrastchecker/)

나는 light seagreen을 주요 색상으로 사용하고 있었는데 색상 대비가 모자랐다. 약 2:1 정도가 나왔던 것 같다.

그러면 좋은 색상 대비는 어떻게 찾을 수 있을까? 현재 사용하고 있는 색상을 넣으면 가장 가까운 색상 대비 색을 찾아주는 사이트가 있다.

https://learnui.design/tools/accessible-color-generator.html

[acceessible color generator](https://learnui.design/tools/accessible-color-generator.html)

나의 경우 lightseagreen의 hex code는 #20b2aa 였는데 해당 값을 넣으면 컬러 컨트라스트가 4.5가 넘는 #00847d 색상을 추천해준다.

이 색을 적용해보니 좀 마음에 안들기는 했지만 그래도 색깔을 인식하는 것을 어려워하는 사람들을 고려해서 반영하기로 했다.
