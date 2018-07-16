---
layout: post
title: "Structures, Unions, and Enumerations"
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

구조체는 저장될 변수 리스트로 초기화 한다.

```
struct{
	int number;
	char name[NAME_LEN+1];
    int on hand;
} part1 = {528, "Disk drive", 10},
  part2 = {914, "Printer cable", 5};
```
구조체 초기화 표현은 constant여야 한다. 변수가 될 수 없다(C99에서는 완화되었는데 뒤에서 확인한다.). Initializer의 갯수가 적으면 남은 변수들은 0으로 초기화 된다.

- Designated Initializers
part1 의 초기화
```
{526, "Disk Drive", 10}
```
Designated initializer 이용
```
{.number = 528, .name = "Disk drive", .on_hand = 10}
```
.number, .name, .on_hand 를 designator 라고 부른다.
장점으로는 가독성과 정합성을 체크할 수 있고, 변수값을 순서대로 작성할 필요가 없다.

배열은 []를 이용하여 position으로 요소를 선택하였지만 구조체는 멤버의 이름을 이용한다.
```
구조체명.멤버이름
```
```
printf("Part number: %d\n", part1.number);
printf("Part name: %s\n", part1.name);
printf("Quantity on hand: %d\n", part1.on_hand);
```

배열에서는 = 연산자 사용이 안되지만, 구조체에서는 가능하다.
part1, part2 처럼 동시에 선언된 구조에서만 가능하다.



##Structure Types
구조체의 타입 이름을 정의할 수 있다. 하나는 structure tag, 또 다른 하나는 typedef 로 가능하다.

- structure tag
특정한 구조체를 확인하기 위해 사용한다.
```
struct part {
	int number;
    char name[NAME_LEN+1];
    int on_hand;
};
```
part 태그를 만들면, 변수를 선언하기 위해 사용할 수 있다.
```
struct part part1, part2;
```
part앞에 struct가 오지않으면 다른 이름과 충돌이 난다.
structure tag와 구조체 변수 선언과 결합할 수 있다.
```
struct part{
	int number;
    char name[NAME_LEN+1];
    int on_hand;
} part1, part2
```
struct part 타입으로 선언된 구조체는 다른 것과 호환된다.
```
struct part part1 = {528, "Disk drive", 10};
struct part part2;
part2 = part1;		/* legal; both parts have the same type */
```

- Defining a Structure Type
타입 이름을 정의하기 위해서 typedef 를 사용할 수 있다.
```
typedef struct{
	int number;
    char name[NAME_LEN+1];
    int on_hand;
} Part;
```
타입 이름으로 Part는 끝에 온다. struct 뒤에 오지 않는다. Part는 타입 이름이기 때문에 struct Part로 사용하지 않는다.
```
Part part1, aprt2;
```
###Structures as Arguments and Return Values
함수는 argument나 return 값으로 구조체를 가질 수 있다.
```
void print_part(struct part p)
{
	printf("Part number: %d\n", p.number);
    printf("Part name: %s\n", p.name);
    printf("Quantity on hand: %d\n", p.on_hand);
}
```
print_part를 호출한다.
```
print_part(part1);
```
두 번째 함수는 인자로부터 만들어내는 part 구조체를 리턴한다.
```
struct part build_part(int number, const char *name, int on_hand)
{
	struct part p;
    
    p.number = number;
    strcpy(p.name, name);
    p,on_hand = on_hand;
    return p;
}
```
구조체를 함수로 전달하거나 함수로부터 구조체를 리턴하는 것은 구조체 모든 멤버를 복사하는 것이다. 특히 구조체가 크면 프로그램에 무리를 줄 수 있다. 이러한 과부하를 피하기 위해서 구조체를 직접 전달하기 보다 포인터를 전달하기도 한다. 유사하게 구조체를 리턴하기 보다 구조체의 포인터를 리턴한다. 뒤에서 배울 예정이다.

##Nested Arrays and Structures
??
##Arrays of Structures

##Unions
공용체는 구조체처럼 다른 타입의 여러 멤버들로 구성한다. 하지만 컴파일러는 가장 큰 멤버에 대한 공간만 할당한다. 그결과 하나의 멤버에 새로운 값을 할당하면 다른 멤버의 값도 변한다.

- union
```
union{
	int i;
    double d;
} u;
```
- structure
```
struct{
	int i;
    double d;
} s;

s와 u는 한가지 다르다. s는 멤버들이 메모리안에 다른 주소를 가지지만 u는 같은 주소에 저장된다.

![ex_screenshot](JMan8989.github.io/_screenshots/20171119_140904.png)

구조체 s는 i와 d가 메모리의 다른 위치에 저장되고 12바이트이다. 공용체 u는 i와 d가 오버랩되고 i는 d의 처음 4바이트에 저장된다. u는 8바이트이고 i와 d는 같은 주소를 가진다.

공용체는 구조체와 거의 같은 특성을 지닌다. 선언할 때 union tags와 union types로 할 수 있다. 또한 = 연산자를 사용하여 복사하고 함수로 전달도 되고 리턴값으로 받을 수도 있다.

공용체는 구조체와 초기화 방법이 비슷하다. 하지만 공용체의 첫번째 멤버만 초기화 할 수 있다. u의 멤버 i를 0으로 초기화 한다.
```
union{
	int i;
    double d;
} u = {0};
```

Designated initializer를 이용할 수 있다. 이것을 이용하면 첫번째 멤버가 아니더라고 하나의 멤버를 초기화 할 수 있다.
```
union{
	int i;
    double d;
} u = {.d = 10.0};
```
공용체는 몇 가지 사용 예가 있다. 그 중 2가지에 대해 알아볼 것이다.
###Using Unions to Save Space
공용체는 구조체의 공간을 절약하기 위해 사용한다. 선물 카탈로그를 통해 팔리는 아이템들의 정보를 담는 구조체를 만든다고 가정해보자. 카탈로그는 3가지 상품(books, mugs, shirts)가 있다. 각각 재고번호, 가격 그리고 아이템별로 다른 정보를 가진다.
- Book: Title, author, number of pages
- Mugs: Design
- Shirts: Design, colors available, sizes available

처음으로 다음과 같이 구조체를 설계한다.
```
struct catalog_item{
	int stock_number;
    double price;
    int item_type;
    char title[TITLE_LEN+1];
    char author[AUTHOR_LEN+1];
    int num_pages;
    char design[DESIGN_LEN+1];
    int colors;
    int sizes;
};
```
`item_type` 멤버는 `BOOK`, `MUG`, `SHIRT` 값 중 하나를 가진다.
`colors`와 `sizes`멤버는 색과 사이즈의 조합으로 저장된다. 비록 구조체는 사용가능하지만, 공간 낭비다. 왜냐하면 구조체의 정보 일부분만 카탈로그 모든 아이템의 공통으로 사용되기 때문이다. 아이템이 책이면 `design`, `colors`, `sizes`는 필요없다. `catalog_item` 구조체 내에 공용체를 사용하면 공간을 줄일 수 있다. 공용체의 멤버는 아이템이 필요하는 데이터를 담은 구조체가 될 것이다.
```
struct catalog_item{
	int stock_number;
    double price;
    int item_type;
    union{
    	struct{
        	char title[TITLE_LEN+1];
            char author[AUTHOR_LEN+1];
            int num_pages;
        } book;
        struct{
        	char design[DESIGN_LEN+1];
        } mug;
        struct{
        	char design[DESIGN_LEN+1];
            int colors;
            int sizes;
        } shirt;
    } item;
};
```
만약 c가 책을 나타내는 `catalog_item`구조체라면, 우리는 책의 이름을 다음과 같이 보여줄 수 있다.
```
printf("%s", c.item.book.title);
```
###Using Unions to Build Mixed Data Structures
다른 타입의 데이터를 담은 데이터 구조를 만들 수 있다. `int`와 `double` 값을 가지는 배열이 필요하다고 하자. 배열은 같은 타입이여야 하기 때문에 이것은 불가능하다. 하지만 공용체를 사용하게 되면 가능하다. 배열 내에 다른 종류의 데이터를 나타내는 공용체를 정의한다.
```
typedef union{
	int i;
    double d;
} Number;
```
다음으로, 요소가 `Number`값인 배열을 만든다. 각 `number_array`의 요소는 `Number` 공용체이다. `number_array`는 `int`와 `double`값을 혼합하여 저장할 수 있다.
```
number_array[0].1 = 5;
number_array[1].d = 8.395;
```
###Adding a "Tag Field" to a Union

##Enumerations
enumberated type는 각 값들에게 이름이 부여된 리스트의 값들이다. 