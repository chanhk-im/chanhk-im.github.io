---
title:  "[STL] vector"
excerpt: "C++의 STL vector에 대해 정리해 보았다.."
toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

categories:
  - study
tags:
  - STL
  - C++
  - 자료 구조
  - vector
---

[C++ STL 시리즈 모음]({{ site.url }}{{ site.baseurl }}/stl_series/)

C++의 STL vector에 대해 정리해 보았다.

## vector의 선언
`vector<int> new_vector;`와 같은 방식으로 선언한다. int에는 다른 타입도 올 수 있다.

## vector 함수

### iterator

- `begin()`: vector의 맨 앞 iterator를 반환
- `end()`: vector의 맨 뒤 iterator를 반환

### 추가 / 삭제

- `push_back(element)`: vector 맨 뒤에 element를 추가
- `pop_back()`: vector 맨 뒤에 원소 삭제
- `erase(iterator)`: iterator가 가리키는 원소 삭제
- `clear()`: 모든 원소 제거
- `insert(idx, cnt, element)`: idx번째에 cnt 개수만큼 element를 추가
- `insert(idx, element)`: idx번째에 element를 추가하고 삽입한 곳의 iterator 반환
- `swap(other_vector)`: other_vector와 원소를 스왑
  
### 조회

- `[idx]`를 통해서 배열처럼 원소에 접근 가능(ex. new_vector[3])
- `at(idx)`: `[idx]`와 동일
- `*iterator`를 이용하여 vector의 원소에 접근 가능
- `front()`: 맨 앞 원소 반환 
- `back()`: 맨 뒤 원소 반환

### 기타

- `empty()`: vector가 비어 있으면 true 그렇지 않으면 false 반환
- `size()`: vector의 길이 반환
- `capacity()`: vector에 할당된 공간 크기 반환