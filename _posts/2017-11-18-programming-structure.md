---
layout: post
title: "Edge Case: Many Tags"
categories:
  - Programming
tags:
  - Structures(구조체)
  - Unions(공용체)
  - Enumerations(열거형)
---

# Structure, Union, Enumeration
- 구조체는 서로 다른 타입의 value 모음
- 공용체는 멤버들이 같은 저장공간 공유한다는 것을 제외하면 구조체와 유사함
	- 하나의 공용체는 한 번에 하나의 멤버를 저장할 수 있음
- 열거형은 프로그래머가 명명한 상수형 타입


## Structure
1. 구조체 변수
지금까지는 data structure는 배열에 대해서만 알고 있었다. 배열은 두 가지 특징을 가진다.

	- 배열 모든 요소는 같은 타입
	- 배열 요소를 선택하기 위해서 위치를 명시했음(예: a[1])

구조체는 조금 다른 특징이 있다. 구조체의 멤버가 같은 타입일 필요는 없다. 그리고 멤버들이 이름을 가지기 때문에 position을 알려줄 필요는 없다.

관련 데이터를 저장할 때 구조체를 사용한다. 창고 부품을 파악하기 위해서 부품 번호(integer), 부품 이름(string), 보유 부품 수(integer)의 정보를 저장하기 위해서는 다음과 같이 선언한다.

```
struct{
	int number;
    char name[NAME_LEN+1];
    int on_hand;
} part1, part2;

//struct {} 는 타입, part1과 part2는 타입의 변수
```

각 구조체는 새로운 scope를 가져 프로그램 내의 다른 이름과 충돌이 나지 않는다. 각 구조체가 분리된 name space를 가진다.

```
struct{
	int number;
    char name[NAME_LEN+1];
    int on_hand;
} part1, part2;

struct{
	char name[NAME_LEN+1];
    int number;
    char sex;
} employee1, employee2;

// part1, part2의 number와 name 멤버는 employee1, employee2와 충돌되지 않음
```

