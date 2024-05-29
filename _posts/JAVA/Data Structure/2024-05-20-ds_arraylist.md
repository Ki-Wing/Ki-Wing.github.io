---
title: 배열 (Array)
author: kiwing
date: 2024-05-20 22:36:08 +0800
categories: [JAVA, Data Structure]
tags: [JAVA, Algorithm]
pin: false
---

## ArrayList
- 자바의 표준 라이브러리인 java.util 패키지에 포함된 동적 배열 자료구조
- 동적 크기
    - 필요에 따라 크기를 자동으로 조절
    - 현재 배열 용량이 초과되면 새로운 용량은 기존 용량의 1.5배
    - 새로운 배열을 생성하고 기존 배열의 요소를 새로운 배열로 복사
    - 추가적인 메모리 할당 및 복사 연산이 발생해서 빈번한 용량 조절은 성능에 영향을 미칠 수 있음
- 유사한 데이터 타입
    - 제네릭을 사용하여 특정 타입의 요소만 저장 가능
- 연속된 메모리 위치
    - 내부적으로 배열을 사용하여 요소를 저장해서 빠른 접근 가능
- 다양한 메서드 제공
    - 요소 추가, 삭제, 탐색, 정렬 등을 위한 다양한 메서드 제공
- 배열은 크기가 고정된 데이터 구조에서 매우 효율적
- ArrayList는 크기가 변동되는 데이터 구조에서 유연성과 편리성 제공

<br>

## java 코드

### 선언과 초기화

```java
ArrayList<Integer> arrayList = new ArrayList<>();
```

### 주요 메서드

```java
ArrayList<E> list = new ArrayList<>(); 

// 특정 인덱스에 요소 추가
list.add(int index, E element)

list.remove(0); // 인덱스 0의 요소 삭제
list.remove(Integer.valueOf(1)); // 값이 1인 첫 번째 요소 삭제

int element = list.get(0); // 인덱스 0의 요소 가져오기
int index = list.indexOf(2); // 값이 2인 첫 번째 요소의 인덱스 반환

list.set(int index, E element); // 지정한 위치의 요소를 수정

list.size(); // 리스트의 크기 확인

list.clear(); // 리스트 초기화

list.contains(Object o) // 리스트에 지정한 객체가 포함되어 있는지 확인

Object[] array = list.toArray(); // 리스트를 배열로 변환
Integer[] array2 = list.toArray(new Integer[0]); // Integer 배열로 변환


ArrayList<Integer> list2 = new ArrayList<>(Arrays.asList(4, 5));
list.addAll(list2); // 리스트 병합

```


[nodejs]: https://nodejs.org/
[starter]: https://github.com/cotes2020/chirpy-starter
[pages-workflow-src]: https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-with-a-custom-github-actions-workflow
[latest-tag]: https://github.com/cotes2020/jekyll-theme-chirpy/tags
