---
title: "리액트 네이티브 Pressable 두번 눌러야 저장되는 문제"
date: 2021-04-19 00:00:00
categories: journal
---

스크롤뷰에서 발생하는 문제로

`<ScrollView keyboardShouldPersistTaps={'handled'}>`

이렇게 처리해주면 된다.

키보드가 항상 보여야 한다면

`<ScrollView keyboardShouldPersistTaps="always">`

이렇게 처리해주면 된다.

사소하지만 생각보다 자주 마주치는 문제.

무려 14일만에 블로그당~~!
