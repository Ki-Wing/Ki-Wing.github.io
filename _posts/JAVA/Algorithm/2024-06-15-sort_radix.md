---
title: Sort- (Radix)
author: kiwing
date: 2024-06-16 21:49:24 +0800
categories: [JAVA, Algorithm]
tags: [JAVA, Algorithm]
pin: false
---

# Radix Sort

## 개요

- 비교 기반 정렬 알고리즘 X
- LSD와 MSD로 나눌 수 있음

#### LSD 기수 정렬 (Least Significant Digit First)
- 가장 낮은 자리수부터 정렬 진행
- 각 자리수의 값에 따라 데이터를 버킷(큐)으로 분배 후 분배된 데이터를 순서대로 다시 합침

#### MSD 기수 정렬 (Most Significant Digit First)
- 가장 높은 자리수부터 시작해 정렬을 진행
- 각 자리수의 값에 따라 데이터를 버킷으로 분배 후 재귀적으로 정렬

<br>

## Radix & Bucket 

```java
[170, 45, 75, 90, 802, 24, 2, 66]
```

<img width="100%" alt="1" src="https://github.com/Ki-Wing/Ki-Wing.github.io/assets/92567807/581e058c-df05-4139-bcca-9bfd02b7c5f6">

<img width="100%" alt="2" src="https://github.com/Ki-Wing/Ki-Wing.github.io/assets/92567807/3abc0682-16cd-4f05-b2ce-844272c32db5">


<br>

## LSD

```java
[170, 45, 75, 90, 802, 24, 2, 66]
```

- 첫 번째
    - 1의 자리수를 기준으로 정렬
    - 170 -> 0
    - 45 -> 5
    - 75 -> 5
    - 90 -> 0
    - 802 -> 2
    - 24 -> 4
    - 2 -> 2
    - 66 -> 6

```java
[170, 90, 802, 2, 24, 45, 75, 66]
```

- 두 번째
    - 10의 자리수를 기준으로 정렬

```java
[802, 2, 24, 45, 66, 170, 75, 90]
```

- 세 번째
    - 100의 자리수를 기준으로 정렬

```java
[2, 24, 45, 66, 75, 90, 170, 802]
```

- 안정성
    - 일한 값의 상대적인 순서가 정렬 후에도 유지
- 공간 복잡도: 각 자릿수에 대해 추가적인 버킷 공간이 필요
- 시간 복잡도 
    - O(d×(n+k)) 
    - d는 최대 자릿수
    - n은 데이터 개수 
    - k는 자릿값의 범위

<br>

## MSD

```java
[170, 45, 75, 90, 802, 24, 2, 66]
```
- 첫 번째
    - 100의 자리수를 기준으로 정렬
    - 170 -> 1
    - 45 -> 0
    - 75 -> 0
    - 90 -> 0
    - 802 -> 8
    - 24 -> 0
    - 2 -> 0
    - 66 -> 0

```java
[45, 75, 90, 24, 2, 66, 170, 802]
```

- 두 번째
    - 10의 자리수를 기준으로 정렬

```java
[2, 24, 45, 66, 75, 90, 170, 802]
```

- 세 번째
    - 1의 자리수를 기준으로 정렬

```java
[2, 24, 45, 66, 75, 90, 170, 802]
```

<br>

## MSD vs LSD

| 특성                  | MSD (Most Significant Digit)       | LSD (Least Significant Digit)          |
|-----------------------|------------------------------------|---------------------------------------|
| **정렬 시작 위치**    | 가장 높은 자리수부터 정렬           | 가장 낮은 자리수부터 정렬              |
| **재귀적 접근 방식**  | O                                 | X                                 |
| **주로 사용되는 경우**| 긴 문자열이나 큰 숫자               | 고정 길이 숫자나 짧은 문자열            |
| **안정성**            | 안정적                              | 안정적                                 |
| **공간 복잡도**       | O(n + k)                            | O(n + k)                               |
| **시간 복잡도**       | O(d \* (n + k))                     | O(d \* (n + k))                        |
| **분할 방식**         | 각 자릿수별로 재귀적 분할            | 각 자릿수별로 반복적 분할              |
| **적용 예시**         | 전화번호, 긴 문자열 정렬             | 정수 정렬, 고정 길이 문자열 정렬        |


<br>

### Java

```java
import java.util.Arrays;

public class RadixSortLSD {

    // 배열에서 최대 값을 찾는 함수
    static int getMax(int arr[]) {
        int max = arr[0];
        for (int i = 1; i < arr.length; i++)
            if (arr[i] > max)
                max = arr[i];
        return max;
    }

    // 주어진 자릿수(exp)에 대해 계수 정렬을 수행하는 함수
    static void countingSort(int arr[], int exp) {
        int n = arr.length;
        int output[] = new int[n];
        int count[] = new int[10];
        Arrays.fill(count, 0);

        // 각 숫자의 자릿수에 해당하는 count 증가
        for (int i = 0; i < n; i++)
            count[(arr[i] / exp) % 10]++;

        // count 배열 수정하여 실제 위치 계산
        for (int i = 1; i < 10; i++)
            count[i] += count[i - 1];

        // output 배열에 정렬된 값 저장
        for (int i = n - 1; i >= 0; i--) {
            output[count[(arr[i] / exp) % 10] - 1] = arr[i];
            count[(arr[i] / exp) % 10]--;
        }

        // 원래 배열에 정렬된 값 복사
        for (int i = 0; i < n; i++)
            arr[i] = output[i];
    }

    // LSD 기수 정렬 함수
    static void radixSort(int arr[]) {
        int m = getMax(arr);
        // 각 자릿수마다 계수 정렬 수행
        for (int exp = 1; m / exp > 0; exp *= 10)
            countingSort(arr, exp);
    }

    // 배열을 출력하는 함수
    static void printArray(int arr[]) {
        for (int num : arr)
            System.out.print(num + " ");
        System.out.println();
    }

    // 메인 함수
    public static void main(String args[]) {
        int arr[] = {2, 20, 22, 21, 31, 130};
        radixSort(arr);
        printArray(arr);
    }
}

```

```java
import java.util.Arrays;

public class RadixSortMSD {

    static int getMax(int[] arr) {
        int max = arr[0];
        for (int i = 1; i < arr.length; i++) {
            if (arr[i] > max) {
                max = arr[i];
            }
        }
        return max;
    }

    static void msdRadixSort(int[] arr, int left, int right, int exp) {
        if (left >= right || exp == 0) {
            return;
        }

        int[] count = new int[10];
        int[] output = new int[right - left + 1];
        int[] indices = new int[10];

        for (int i = left; i <= right; i++) {
            int digit = (arr[i] / exp) % 10;
            count[digit]++;
        }

        indices[0] = left;
        for (int i = 1; i < 10; i++) {
            indices[i] = indices[i - 1] + count[i - 1];
        }

        for (int i = left; i <= right; i++) {
            int digit = (arr[i] / exp) % 10;
            output[indices[digit] - left] = arr[i];
            indices[digit]++;
        }

        for (int i = 0; i < output.length; i++) {
            arr[left + i] = output[i];
        }

        int start = left;
        for (int i = 0; i < 10; i++) {
            int end = (i == 9) ? right : indices[i] - 1;
            msdRadixSort(arr, start, end, exp / 10);
            start = end + 1;
        }
    }

    public static void main(String[] args) {
        int[] arr = {2, 20, 22, 21, 31, 130};
        int max = getMax(arr);

        int exp = 1;
        while (max / exp >= 10) {
            exp *= 10;
        }

        msdRadixSort(arr, 0, arr.length - 1, exp);

        System.out.println(Arrays.toString(arr));
    }
}
```


<br>

[nodejs]: https://nodejs.org/
[starter]: https://github.com/cotes2020/chirpy-starter
[pages-workflow-src]: https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-with-a-custom-github-actions-workflow
[latest-tag]: https://github.com/cotes2020/jekyll-theme-chirpy/tags

