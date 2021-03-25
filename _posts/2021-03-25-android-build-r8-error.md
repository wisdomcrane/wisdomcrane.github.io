---
title: "리액트 네이티브 안드로이드 빌드 r8 관련 에러"
date: 2021-03-25 00:00:00
categories: journal
---

- 에러명

`Task :app:transformClassesAndResourcesWithR8ForRelease FAILED`

r8이란? 안드로이드 빌드 프로세스에서 프로가드를 대체하기 위한 리소스 축소기 (Shrinker) 로써 빌드 시간을 단축 시축시켜 주는 기술.

- 에러 원인

안드로이드 스튜디오에 할당된 memory heap size의 부족. 즉 안드로이드의 IDE나 gradle 데몬에 할당된 램이 부족해서 생기는 에러

- 에러 수정

안드로이드 스튜디오에 할당된 메모리 사이즈를 늘려준다.

Settings->Apperance & Behavior tab-> Memory Settings에서 다음과 같이 조치함

![r8 빌드 에러](/aseets/image/r8-error.png)

추가사항 : 그림의 노란색 글씨에서 볼 수 있듯이 나의 경우 JDK 버전이 여러개가 공존하고 있었는데 특정 버전을 지정해줬다. 메모리를 4기가로 변경 후 바로 된게 아니라 gradle sync (안드로이드 스튜디오에서 android 폴더에 위치하면 IDE의 우측 상단에 쥐모양이 뜬다.)와 에디터 재부팅, 컴퓨터 재부팅 등 해준 후 약간의 시간이 경과한 후 빌드가 성공했다.
