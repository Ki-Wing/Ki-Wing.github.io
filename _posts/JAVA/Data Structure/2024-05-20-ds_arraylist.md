---
title: 배열-1 (ArrayList)
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

<br>

## Array vs ArrayList

| 특성                    | 배열 (`Array`)                                                                                     | `ArrayList`                                                                                          |
|-------------------------|----------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| 크기                    | 고정 크기                                                                                          | 동적 크기                                                                                             |
| 초기화 방법             | `int[] array = new int[5];`                                                                         | `ArrayList<Integer> list = new ArrayList<>();`                                                        |
| 요소 추가               | 불가능 (고정 크기)                                                                                 | 가능 (`list.add(value);`)                                                                             |
| 요소 삭제               | 직접 구현 필요 (`for` 루프 사용)                                                                   | 가능 (`list.remove(index);`)                                                                          |
| 요소 접근               | `array[index]`                                                                                     | `list.get(index)`                                                                                     |
| 요소 수정               | `array[index] = value`                                                                             | `list.set(index, value)`                                                                              |
| 기본 타입 저장 여부     | 가능 (`int`, `char` 등)                                                                            | 불가능 (래퍼 클래스 사용: `Integer`, `Character` 등)                                                  |
| 제네릭 지원             | 불가능                                                                                             | 가능 (`ArrayList<T>`)                                                                                 |
| 크기 확인               | `array.length`                                                                                     | `list.size()`                                                                                         |
| 배열 길이 조절          | 불가능                                                                                             | 가능 (초기 용량 초과 시 1.5배로 증가)                                                                  |
| 메모리 효율성           | 고정 크기이므로 메모리 낭비 없음                                                                   | 동적 크기 조절로 인해 메모리 낭비 가능                                                                |
| 초기 용량 설정          | `new int[capacity]`                                                                                | 가능 (`new ArrayList<>(initialCapacity);`)                                                            |
| 데이터 타입 제한        | 모든 타입 (기본 타입 및 참조 타입)                                                                 | 객체 타입만 (기본 타입은 래퍼 클래스 사용)                                                             |
| 반복자 지원             | 불가능                                                                                             | 가능 (`list.iterator()`)                                                                              |
| 포함 여부 확인          | 직접 구현 필요 (`for` 루프 사용)                                                                   | 가능 (`list.contains(value)`)                                                                         |
| 리스트 병합             | 직접 구현 필요                                                                                     | 가능 (`list.addAll(anotherList)`)                                                                     |
| 부분 리스트 추출        | 직접 구현 필요                                                                                     | 가능 (`list.subList(fromIndex, toIndex)`)                                                             |
| 성능                    | 메모리 효율적, 빠른 접근                                                                           | 추가/삭제 시 오버헤드 발생 가능                                                                       |
| 동기화 지원             | 불가능 (동기화 필요 시 직접 구현)                                                                  | `Collections.synchronizedList(new ArrayList<>());` 를 통해 가능                                       |
| 초기 크기 설정          | 가능 (`new int[10];`)                                                                              | 가능 (`new ArrayList<>(10);`)                                                                         |
| 확장/축소 동작          | 불가능                                                                                             | 가능 (내부적으로 크기를 조절하여 확장, 필요 시 축소)                                                   |
| 사용 편의성             | 단순하지만 제한적                                                                                   | 다양한 편의 메서드 제공                                                                               |
| 배열/리스트 변환        | 불필요 (이미 배열)                                                                                 | 가능 (`list.toArray()` 또는 `Arrays.asList(array)`)                                                    |
| 활용 사례               | 고정된 크기의 데이터 처리 (예: 정해진 길이의 입력 데이터)                                           | 동적 크기의 데이터 처리 (예: 리스트에 동적으로 추가/삭제가 빈번한 경우)                                |
| 성능 (읽기/쓰기)        | 읽기 빠름 (`O(1)`), 쓰기 빠름 (`O(1)`), 요소 추가/삭제 느림 (전체 배열을 이동해야 함, `O(n)`)     | 읽기 빠름 (`O(1)`), 쓰기 빠름 (`O(1)`), 요소 추가/삭제 빠름 (`O(n)`), 끝에서 추가 시 빠름 (`O(1)`) |


[nodejs]: https://nodejs.org/
[starter]: https://github.com/cotes2020/chirpy-starter
[pages-workflow-src]: https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-with-a-custom-github-actions-workflow
[latest-tag]: https://github.com/cotes2020/jekyll-theme-chirpy/tags
