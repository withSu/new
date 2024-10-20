---
layout: single
title:  "#9 C++ 객체배열, 객체포인터, 동적할당"
categories: C++
tag: [C++]
toc: true
toc_label: "table of content" # toc 이름 설정
toc_icon: "bars" # 아이콘 설정
toc_sticky: true # 마우스 스크롤과 함께 내려갈 것인지 설정
typora-root-url: ../
---

```cpp
    //p[i]==*(p+i) 이다. 이표현은 외워두자!!!!

```

# 1. 객체배열

## 1.1 객체 배열

### 1.1.1 객체 배열 선언

- 기본타입 배열 선언과 형식 동일
- 객체 배열 정의시 `따로 지정하지 않으면 항상 디폴트 생성자로 초기화`

```cpp
Circle circleArray[3]; //디폴트 생성자로 초기화
```

### 1.1.2 객체 배열의 초기화

배열의 `각 원소 객체당` `생성자 지정`하는 방법

⇒ { } 안에 생성자를 나열한다.

```cpp
Circle circle Array[3] = {Circle(10),Circle(20),Circle()};
```

– circleArray[0] 객체가 생성될 때, 생성자 Circle(10) 호출
– circleArray[1] 객체가 생성될 때, 생성자 Circle(20) 호출
– circleArray[2] 객체가 생성될 때, 생성자 Circle() 호출

Circle 객체의 배열을,  포인터와 배열 방식으로 각각 접근한 코드

```cpp
#include <iostream>
using namespace std;

class Circle {
    int radius;
public:
    // 기본 생성자
    Circle() { radius = 1; }

    // 매개변수를 받는 생성자
    Circle(int r) { radius = r; }

    // 반지름 설정 함수
    void setRadius(int r) { radius = r; }

    // 면적 계산 함수
    double getArea();
};

// 면적 계산 함수의 정의
double Circle::getArea() {
    return 3.14 * radius * radius;
}

int main() {
    // (1) Circle 객체 배열 생성
    Circle circleArray[3];

    // (2) 배열의 각 원소 객체의 멤버에 접근
    circleArray[0].setRadius(10);
    circleArray[1].setRadius(20);
    circleArray[2].setRadius(30);

    // 각 원소 객체의 멤버 함수 호출
    for (int i = 0; i < 3; i++) {
        cout << "Circle " << i << "의 면적은 " << circleArray[i].getArea() << endl;
    }
    ********************************************************************
    // (3) Circle 객체 포인터 선언
    Circle *p;
    // (4) 포인터가 배열을 가리킴
    p = circleArray;

    // (5) 객체 포인터로 배열에 접근
    for (int i = 0; i < 3; i++) {
        cout << "Circle " << i << "의 면적은 " << p->getArea() << endl;
        p++;
    }

    return 0;
}

```

`위 코드를 사용하면 Circle 객체의 배열을 포인터와 배열 방식으로 각각 접근하여 같은 결과를 얻을 수 있다.`

### 1.1.3 객체 배열 생성시 디폴트 생성자가 호출된다

`Circle circleArray[3]; ⇒ 디폴트 생성자Circle()을 호출한다.`

`문제는 사용자가 명시적으로 생성자를 선언할 경우에는 자동으로 디폴트 생성자가 생성되지 않는다는 점이다`. 이 때문에 디폴트 생성자가 필요할 때, 컴파일 오류가 발생할 수 있다.

```cpp
class Circle {
    int radius;
public:
    // 매개변수를 받는 생성자만 정의되어 있음
    Circle(int r) { radius = r; }

    double getArea() {
        return 3.14 * radius * radius;
    }
};

int main() {
    Circle circleArray[3];  // 오류 발생: 디폴트 생성자가 없기 때문
}

```

### 1.1.4 객체 배열의 인자있는 생성자로 초기화

```cpp
#include <iostream>
using namespace std;

class Circle {
    int radius;
public:
    // 기본 생성자
    Circle() { radius = 1; }

    // 매개변수를 받는 생성자
    Circle(int r) { radius = r; }

    // 반지름 설정 함수
    void setRadius(int r) { radius = r; }

    // 면적 계산 함수
    double getArea();
};

// 면적 계산 함수 정의
double Circle::getArea() {
    return 3.14 * radius * radius;
}

int main() {
    // Circle 배열 초기화
    Circle circleArray[3] = { Circle(10), Circle(20), Circle() };

    // 각 객체의 면적 출력
    for (int i = 0; i < 3; i++) {
        cout << "Circle " << i << "의 면적은 " << circleArray[i].getArea() << endl;
    }

    return 0;
}

```

1. `circleArray[0]`은 `Circle(10)`으로 생성되어 반지름이 10이고, 면적은 314이다.
2. `circleArray[1]`은 `Circle(20)`으로 생성되어 반지름이 20이고, 면적은 1256이다.
3. `circleArray[2]`은 기본 생성자인 `Circle()`이 호출되어 반지름이 1로 설정되며, 면적은 3.14이다.

이 코드는 `Circle` 객체의 배열을 생성하면서 **각기 다른 생성자**를 사용하여 객체를 초기화하는 방법을 보여준다.

## 1.2 2차원 객체 배열

 

```cpp
#include <iostream>
using namespace std;

class Circle {
    int radius;
public:
    // 기본 생성자
    Circle() { radius = 1; }

    // 매개변수를 받는 생성자
    Circle(int r) { radius = r; }

    // 반지름 설정 함수
    void setRadius(int r) { radius = r; }

    // 면적 계산 함수
    double getArea();
};

// 면적 계산 함수 정의
double Circle::getArea() {
    return 3.14 * radius * radius;
}

int main() {
    // Circle 객체의 2차원 배열 선언
    Circle circles[2][3];

    // 배열의 각 원소에 반지름 설정
    circles[0][0].setRadius(1);
    circles[0][1].setRadius(2);
    circles[0][2].setRadius(3);
    circles[1][0].setRadius(4);
    circles[1][1].setRadius(5);
    circles[1][2].setRadius(6);

		/* 조금더 간편한 방식
		
		// Circle 객체의 2차원 배열을 생성자 초기화로 생성
    Circle circles[2][3] = {
        { Circle(1), Circle(2), Circle(3) },
        { Circle(4), Circle(5), Circle(6) }
    };
    
		*/

    // 2차원 배열의 각 원소의 면적 출력
    for (int i = 0; i < 2; i++) {
        for (int j = 0; j < 3; j++) {
            cout << "Circle [" << i << "," << j << "]의 면적은 " << circles[i][j].getArea() << endl;
        }
    }

    return 0;
}

```







# 2. 객체 포인터

- C언어의 포인터와 동일
- 객체의 주소값을 가지는 변수

## 2.1 포인터로 멤버를 접근할때

객체포인터 → 멤버

```cpp
int main() {
    Circle donut;  // Circle 객체 생성
    double d = donut.getArea();  // 멤버 함수 호출

    // (1) 객체에 대한 포인터 선언
    Circle *p;
    
    // (2) 포인터에 객체 주소 저장
    p = &donut;
    
    // (3) 포인터를 이용하여 멤버 함수 호출
    d = p->getArea();
    //donut.getArea() : 객체 이름으로 멤버 접근

    // 결과 출력
    cout << "Donut 면적: " << d << endl;

    return 0;
}
```

`p->getArea();`는 `포인터를 사용해 donut 객체의 getArea() 멤버 함수를 호출하는 방법`이다. `->` 연산자를 사용하여 포인터로 객체의 멤버 함수에 접근한다

객체 포인터로 멤버 접근하는 예시

```cpp
class PERSON332
{
public:
    int money;
    string name;

    PERSON332() { money = 0; }
    PERSON332(string name_in)
    {
        money = 0;
        name = name_in;
    }
    ~PERSON332()
    {
        cout << name << "의 money는" << money << endl;
    }

    void addMoney(int money_in)
    {
        money += money_in;
    }

    static int sharedMoney;
    static int addShared(int money_in)
    {
        sharedMoney += money_in;
    }
};

/*객체 포인터*/
int ex1010_3()
{
    PERSON332 A("KANG"); //객체 생성
    A.addMoney(100); // 100원추가
    A.addShared(5); // 공금 5원추가

    // p:주소
    // &A : A의 주소
    
    // A: 객체
    // *p : 객체 (p가 가리키도 있는 객체)

		//객체 포인터로 멤버 접근
    PERSON332 *p = &A; //포인터 p에 A의 주소를 저장

    cout << A.money << ' ' << (*p).money << ' ' << p->money << endl; // 100 100 100
}
```

(*p).name과 p→name은 같은 뜻이다.

![image.png](/images/2024-10-19-multiple-objects/image.png)

![image.png](/images/2024-10-19-multiple-objects/image 1.png)







# 실습 7-1

Circle 클래스와 main()함수를 작성하고 3개의 Circle 객체를 가진 배열을 선언하고, 

반지름 값을 입력받고 면적이 100보다 큰 원의 개수를 출력하는 프로그램을 완성하라.
Circle 클래스도 완성하라

```cpp
// Circle 클래스 정의
class Circle {
    int radius;  // 원의 반지름 값
public:
    // 반지름 설정 함수
    void setRadius(int r) {
        radius = r;
    }

    // 면적 계산 함수
    double getArea() {
        return 3.14 * radius * radius;
    }
};

int main() {
    // Circle 객체 배열 선언
    Circle circles[3];
    int radius;

    // 사용자로부터 반지름 입력받기
    for (int i = 0; i < 3; i++) {
        cout << "원 " << i + 1 << "의 반지름 >> ";
        cin >> radius;
        circles[i].setRadius(radius);
    }

    // 면적이 100보다 큰 원의 개수를 계산
    int count = 0;
    for (int i = 0; i < 3; i++) {
        if (circles[i].getArea() > 100) {
            count++;
        }
    }

    // 결과 출력
    cout << "면적이 100보다 큰 원은 " << count << "개입니다." << endl;

    return 0;
}

int ex1010_4()
{
    // 1. Circle 객체 배열 생성 후 멤버 변수 직접 설정
    Circle circleArray[3];  // Circle 객체 배열 생성

    // 객체의 멤버 변수인 radius에 직접 접근하여 값 설정
    circleArray[1].radius = 50;  // 두 번째 원의 반지름을 50으로 설정
    circleArray[2].radius = 70;  // 세 번째 원의 반지름을 70으로 설정

    // 포인터를 이용하여 객체의 멤버에 접근하는 방법
    Circle *p = &circleArray[1];  // 포인터 p는 circleArray[1]을 가리킴
    cout << (*p).radius << ' ' << p->radius << endl;  // 50 50 출력

    // 2. Circle2 객체 배열을 생성자 초기화로 설정
    Circle2 carray[3] = {Circle2(10), Circle2(20), Circle2(30)};  // 배열 초기화

    // 포인터 q를 통해 carray 배열의 첫 번째 원소에 접근
    Circle2 *q;
    q = carray;
    cout << carray[0].radius << ' ' << q[0].radius << endl;  // 10 10 출력

    // 포인터 q를 carray 배열의 두 번째 원소를 가리키도록 설정
    q = &carray[1];
    cout << (*(q + 1)).radius << endl;  // carray[2]의 radius 출력, 30 출력

    // q[-1]은 carray[0], q[0]은 carray[1], q[1]은 carray[2]
    cout << q[-1].radius << " " << q[0].radius << " " << q[1].radius << endl;
    // 출력: 10 20 30

    // color colors[3];
    // Color* p =colors;
    // colors[1], colors[2], colors[3]
}

```

![image.png](/images/2024-10-19-multiple-objects/image 2.png)







# 실습 7-2 ***

다음은 색의 3요소인 red, green, blue로 색을 추상화한 Color 클래스를 선언하고 활용하는 코드이다. 빈칸을 채워라.

여기서 red, green, blue는 0~255의 값만 가진다.

```cpp
class Color {
    int red, green, blue;
public:
    // 기본 생성자: 빨간색 (red=0, green=0, blue=0)
    Color() { red = green = blue = 0; }

    // 매개변수를 받는 생성자
    Color(int r, int g, int b) { red = r; green = g; blue = b; }

    // 색상 설정 함수
    void setColor(int r, int g, int b) { red = r; green = g; blue = b; }

    // 색상 출력 함수
    void show() { cout << red << ' ' << green << ' ' << blue << endl; }
};

int main() {
    Color screenColor(255, 0, 0);  // 빨간색의 screenColor 객체 생성
    Color *p;  // Color 타입의 포인터 변수 p 선언

    // (1) p가 screenColor의 주소를 가지도록 코드 작성
    _____________________;

    // (2) p와 show()를 이용하여 screenColor 색 출력
    _____________________;

    // (3) Color의 일차원 배열 colors 선언, 원소는 3개
    _____________________;

    // (4) p가 colors 배열을 가리키도록 코드 작성
    _____________________;

    // (5) p와 setColor()를 이용하여 colors[0], colors[1], colors[2]가 각각 빨강, 초록, 파랑색을 가지도록 코드 작성
    _____________________;
    _____________________;
    _____________________;

    // (6) p와 show()를 이용하여 colors 배열의 모든 객체의 색 출력. for문 이용
    _____________________;
    _____________________;
}

```

풀이

```cpp
class Color {
    int red, green, blue;
    public:
        Color() { red = green = blue = 0; }
        Color(int r, int g, int b) { red = r; green = g; blue = b; }
        void setColor(int r, int g, int b) { red = r; green = g; blue = b; }
        void show() { cout << red << ' '<< green << ' ' << blue << endl; }
};

int main()
{
    Color screenColor(255, 0, 0); // 빨간색 객체 생성
    Color *p; // Color 포인터 p 선언
    p = &screenColor; // (1) p가 screenColor주소를 가지도록 코드 작성.

    (*p).show(); // (2) p와 show()를 이용하려 screenClor 색을 출력한다.
    // p->show();  이렇게 표현할 수 있다.

    Color colors[3]; // (3) Color의 일차원 배열 colors선언. 원소는 3개
    p = colors; // (4) p가 colors배열을 가리키도록 코드 작성

    colors[0].setColor(255, 0, 0); // (5) colors[0]의 색을 빨간색으로 설정.
    //빨강: p[2] => *(p+2)

    colors[1].setColor(0, 255, 0); // (6) colors[1]의 색을 초록색으로 설정
    colors[2].setColor(0, 0, 255); // (7) colors[2]의 색을 파란색으로 설정

    //(6) p와 show()를 이용하여 colors배열의 모든 객체의 색을 출력한다.
    for (int i = 0; i < 3; i++)
    {
        p[i].show();
    }
    /* 
        colors[0].show(); 
        colors[1].show();
        colors[2].show();
    */

    return 0;
}
```

![image.png](/images/2024-10-19-multiple-objects/image 3.png)







# 3. 동적메모리 할당 및 반환

## 3.1 동적메모리의 필요성

- 동적 메모리

  프로그램 실행중에 메모리의 할당과 해제가 결정되는 메모리

- 동적메모리를 사용하면

  실행중에 꼭 필요한 만큼 메모리를 할당받아서 사용

  미리 정해진 크기가 아니라 원하는 크기만큼 할당받는것도 가능

  ⇒ 메모리 낭비를 해결한다.

## 3.2 정적할당과 동적할당

### 3.2.1 정적할당

변수 선언을 통해 필요한 메모리 할당

많은양의 메모리는 배열 선언을 통해 할당

### 3.2.2 동적할당

필요한 양이 예측되지 않는 경우. 프로그램 작성시 할당 받을 수 없다.

힙으로부터 할당.

### 3.2.3 정적 메모리 vs 동적 메모리

![image.png](/images/2024-10-19-multiple-objects/image 4.png)

## 3.3 new와 delete 연산자

### 3.3.1 **new/delete 연산자의 기본 형식**

```cpp
데이터타입 *포인터변수 = new 데이터타입;
delete 포인터변수;
```

- **`new 연산자**는 **동적 메모리 할당**을 수행하며`, 포인터변수는 할당된 메모리의 주소를 저장한다.
- **`delete 연산자**는 동적으로 할당된 메모리를 **해제**`한다. 이 작업을 통해 메모리 누수를 방지할 수 있다.

### 3.3.2. **new/delete 연산자의 사용 예시**

```cpp
int *pInt = new int;         // int 타입의 메모리 동적 할당
char *pChar = new char;      // char 타입의 메모리 동적 할당
Circle *pCircle = new Circle(); // Circle 클래스 타입의 메모리 동적 할당

delete pInt;   // 할당 받은 정수 공간 반환
delete pChar;  // 할당 받은 문자 공간 반환
delete pCircle; // 할당 받은 객체 공간 반환

```

- **new 연산자**를 통해 int, char, Circle 타입의 동적 메모리를 할당받은 후, 각각의 메모리를 **delete** 연산자를 통해 해제한다.

### 3.3.3. **delete 사용 시 주의 사항**

- 동적으로 할당된 `메모리 주소를 저장하는 포인터 변수가 **사라지는 것이 아니다**.`
- 따라서 **delete 연산자로 `동적 메모리를 해제한 후**에는 해당 포인터 변수를 **널 포인터로 초기화**하는 것이 안전하다.`

예를 들어:

```cpp
delete pInt;   // 동적 메모리 해제
pInt = nullptr; // 포인터를 널 포인터로 초기화

```

이렇게 해야지 해제된 메모리에 접근하려는 실수를 방지할 수 있다.

### 3.3.4. **정수형 공간의 동적 할당 및 반환 예시**

```cpp
#include <iostream>
using namespace std;

int main() {
    int *p;

    // 동적 메모리 할당
    p = new int;
    if (!p) {
        cout << "메모리를 할당할 수 없습니다." << endl;
        return 0;
    }

    *p = 5;  // 할당 받은 메모리 공간에 값 5 삽입
    int n = *p;

    cout << "*p = " << *p << '\\n';
    cout << "n = " << n << '\\n';

    // 메모리 해제
    delete p;

    return 0;
}

```

이 코드는 동적으로 int형 메모리를 할당하고, 할당된 메모리에 값을 저장한 뒤 해제하는 과정을 보여준다. 이때 메모리 할당에 실패하면 프로그램을 종료하도록 한다.

### 3.3.5. **delete 사용 시 주의 사항**

(1) **적절치 못한 포인터로 delete 사용 시 오류**

```cpp
int n;
int *p = &n;
delete p;  // 오류! 정적 메모리의 주소를 해제하려고 함

```

- delete 연산자는 동적으로 할당된 메모리만 해제할 수 있다. 위 코드는 n이라는 정적 메모리의 주소를 delete하려고 시도하기 때문에 오류가 발생한다.

(2) **동일한 메모리 두 번 반환 시 오류**

```cpp
int *p = new int;
delete p;  // 정상적으로 메모리 반환
delete p;  // 오류! 이미 반환한 메모리를 다시 반환하려고 함

```

- 한 번 해제한 메모리를 다시 해제하려고 하면 오류가 발생한다. 따라서 delete를 한 후에는 포인터를 nullptr로 초기화하는 것이 안전하다.

코드 예

```cpp
#include <iostream>
using namespace std;

class A {
public:
    int value; // A 클래스의 value 변수
};

class B {
public:
    A sub; // A 클래스 객체를 포함하는 sub 변수
    int counter; // B 클래스의 counter 변수
};

int main() {
    B abc;

    abc.sub.value = 100;  // A 클래스의 value 변수에 값 할당
    abc.counter = 200;    // B 클래스의 counter 변수에 값 할당

    cout << "abc.sub.value = " << abc.sub.value << endl;  // 출력: 100
    cout << "abc.counter = " << abc.counter << endl;      // 출력: 200

    // 동적 메모리 할당
    int* p = new int; // int 타입 메모리 공간 동적 할당
    *p = 50; // 할당된 메모리에 값 할당

    cout << "*p = " << *p << endl;  // 출력: 50

    // 동적 할당된 메모리 해제
    delete p;  // 할당된 메모리 반환

    return 0;  // 프로그램 정상 종료
}

```

## `3.4 기본적인 동적 메모리 할당, 해제 정리`

위 내용은 **동적 메모리 할당과 해제**에 대한 기본적인 사용 방법을 설명하고 있다. 동적 메모리 할당과 관련된 주요 사항들을 정리해보면 다음과 같다.

### 3.4.1 **동적 메모리 할당**

- 메모리를 할당할 때는 `new` 연산자를 사용하여 타입과 크기를 지정한다. 이때 컴퓨터는 해당 메모리 공간을 할당하고 그 메모리의 **주소**를 반환한다.
- 반환된 **주소**를 통해 메모리 공간에 접근하고 데이터를 저장하거나 읽을 수 있다.

```cpp
int *data = new int; // int형 크기의 메모리 공간 동적 할당
*data = 6; // 할당된 메모리 공간에 값 6 저장

```

### 3.4.2 **메모리 해제**

- 동적으로 할당된 메모리는 더 이상 필요하지 않을 때 반드시 **delete** 연산자를 사용해 해제해야 한다.
- 메모리를 해제하지 않으면 **메모리 누수**가 발생할 수 있다.

```cpp
delete data; // 동적 메모리 해제

```

동적 메모리 할당의 핵심은 메모리 할당 후 그 주소를 기억하고 있다가, 작업이 끝나면 할당된 메모리를 명시적으로 해제해줘야 한다는 것이다.







## 실습 코드**

```cpp
//int ex1014_2(int number)
int ex1014_2()
{

    int A[5]; // <---컴파일러가 자동으로 메모리 할당
    //int A[number]; //배열안에 변수를 넣을 수 없다.
    A[0] = 100;
    A[1] = 200;
    A[2] = 300;
    A[3] = 400;
    A[4] = 500;

    //A=100; //변경 불가! 배열은 주소를 가지고 있기 때문에 주소에 값을 넣을 수 없다.

    for(int n =0; n<5; n++)
    {
        printf("%d \n", A[n], *(A+n));
    }
    delete[] A; // <---- 메모리 해제
}

int ex1014_3(int number)
{

    int* A = (int*)malloc(number*sizeof(int)); // <----- 개발자가 동적할당
    A[0] = 100;
    A[1] = 200;
    A[2] = 300;
    A[3] = 400;
    A[4] = 500;

    for(int n =0; n<5; n++)
    {
        printf("%d \n", A[n], *(A+n));
    }

    free(A); // <---- 메모리 해제

}

int ex1014_4(int number)
{

    int* A = new int[number]; // <----- 개발자가 동적할당
    A[0] = 100;
    A[1] = 200;
    A[2] = 300;
    A[3] = 400;
    A[4] = 500;

    for(int n =0; n<5; n++)
    {
        printf("%d \n", A[n], *(A+n));
    }

    delete[] A; // <---- 메모리 해제

}

int main(void)
{

    ex1014_2();
    ex1014_3(5);
    ex1014_4(5);
    return 0;
}
```







# 4. 객체(배열)의 동적 생성 및 반환

## 4.1 `동적 객체` 생성 및 반환 과정

```cpp
Circle *p = new Circle;  // 동적으로 Circle 객체를 생성
Circle *q = new Circle(30);  // 매개변수를 받아 생성자를 호출해 radius를 30으로 설정
delete p;  // 동적으로 할당된 객체 메모리 반환
delete q;  // 동적으로 할당된 객체 메모리 반환

```

여기서 `new` 연산자를 사용해 객체를 동적으로 생성합니다. 생성된 객체는 힙 메모리 공간에 할당되며, 사용이 끝나면 반드시 `delete`를 통해 메모리를 반환해야 합니다. 생성된 순서와 상관없이 메모리 반환은 자유롭게 가능합니다.

## 4.2 `동적 객체 배열` 생성 및 반환 과정

```cpp
Circle *pArray = new Circle[3];  // Circle 객체 배열 3개를 동적으로 생성
pArray[0].setRadius(10);  // 첫 번째 객체의 반지름 설정
delete [] pArray;  // 객체 배열 메모리 반환

```

여기서는 동적 객체 배열을 생성합니다. 동적으로 할당된 배열은 기본 생성자를 통해 각 객체가 초기화됩니다. 객체 배열의 메모리를 반환할 때는 `delete[]`를 사용해야 합니다. 배열의 각 원소는 기본 생성자로 초기화되기 때문에, 명시적으로 `setRadius()` 등의 메서드로 값을 설정할 수 있습니다.

```cpp
class Circle3
{
    int radius;

public:
    Circle3();
    Circle3(int r);
    ~Circle3();
    void setRadius(int r) { radius = r; }
    double getArea() { return 3.14 * radius * radius; }
};
Circle3::Circle3()
{
    radius = 1;
    cout << "생성자 실행 radius = " << radius << endl;
}
Circle3::Circle3(int r)
{
    radius = r;
    cout << "생성자 실행 radius = " << radius << endl;
}
Circle3::~Circle3()
{
    cout << "소멸자 실행 radius = " << radius << endl;
}

int ex1014_6()
{
    Circle3 *p;
    Circle3 *q;
    p = new Circle3;
    q = new Circle3(10);

    cout<<(*p).getArea()<<endl;
    // cout<<p->getArea()<<endl; (*p).getArea()와 동일
    cout<<(*q).getArea()<<endl;
    // cout<<q->getArea()<<endl; (*q).getArea()와 동일

    delete p;
    delete q;

    return 0;
}

int main()
{
    Circle3 *p = new Circle3[3];

    cout<<p[0].getArea()<<endl;
    cout<<p[1].getArea()<<endl;
    cout<<p[2].getArea()<<endl;

    Circle3* q =p;
    cout<<q[0].getArea()<<endl;
    cout<<q[1].getArea()<<endl;
    cout<<q[2].getArea()<<endl;

    cout<< "----------------------\n";
    cout<<(*(q+0)).getArea()<<endl;  //(q+0)->
    cout<<(*(q+1)).getArea()<<endl;  //(q+1)->
    cout<<(*(q+2)).getArea()<<endl; //(q+2)->

    

    delete[] p;
    
    return 0;
}
```

## 4.3 객체 배열 동적 생성과 소멸자 호출 순서

### 객체 배열에서 소멸자 호출

```cpp
Circle *pArray = new Circle[3];  // 기본 생성자를 통해 객체 3개 생성
pArray[0].setRadius(10);  // 첫 번째 객체 반지름 설정
delete [] pArray;  // 객체 배열 해제, 소멸자 역순으로 호출

```

### 1. **객체 배열의 동적 생성**

`new Circle[3];`는 Circle 객체의 배열을 힙 메모리 상에 동적으로 할당합니다. 이때, **각각의 객체**는 기본 생성자에 의해 초기화됩니다.

- `Circle[0]`, `Circle[1]`, `Circle[2]`이 각각 기본 생성자를 호출하여 생성됩니다.
- 만약 기본 생성자 외의 다른 생성자가 있다면, `new Circle[3]` 형태로는 반드시 기본 생성자가 있어야 합니다. 그렇지 않으면 오류가 발생합니다.

### 2. **객체 배열에 대한 메모리 해제**

`delete[]`를 사용해 배열을 해제할 때, 각 객체에 대해 **소멸자**가 역순으로 호출됩니다.

예를 들어, 다음과 같은 순서로 소멸자가 호출됩니다:

- `pArray[2]`의 소멸자가 먼저 호출됩니다.
- 그 다음 `pArray[1]`의 소멸자가 호출됩니다.
- 마지막으로 `pArray[0]`의 소멸자가 호출됩니다.

※객체에 대한 포인터 배열의 생성 및 사용 이 내용은 스킵







# 5. 멤버함수의 this 포인터

## 5.1 this 포인터의 개념

- **this**는 멤버 함수 내에서 객체 자신을 가리키는 포인터이다.
- **this** 포인터는 클래스의 `멤버 함수에서만 사용 가능`하며, `객체의 멤버 함수가 호출될 때 묵시적으로 전달`된다.
- 개발자가 명시적으로 선언하는 변수가 아니며, 컴파일러가 자동으로 삽입해주는 매개 변수이다.

```cpp
class Circle {
    int radius;
public:
    Circle() { this->radius = 1; }  // 생성자에서 this 포인터 사용
    Circle(int radius) { this->radius = radius; }
    void setRadius(int radius) { this->radius = radius; }  // this 포인터 사용
};

```

## 5.2 this와 객체

- 각 객체 속의 **this**는 서로 다른 객체를 가리키며, 자기 자신에 대한 포인터이다.
- 예를 들어, 여러 객체(c1, c2, c3)가 있을 때, 각각의 **this** 포인터는 해당 객체 자신을 가리킨다.

```cpp
int main() {
    Circle c1, c2, c3;
    c1.setRadius(4);  // c1의 this는 c1을 가리킴
    c2.setRadius(5);  // c2의 this는 c2를 가리킴
    c3.setRadius(6);  // c3의 this는 c3를 가리킴
}

```

## 5.3 this가 유용한 경우

- **멤버 변수**와 **매개 변수**의 이름이 동일할 경우, **this** 포인터는 멤버 변수를 명확하게 구분하는 데 사용된다.
- 멤버 함수가 **객체 자신**의 주소를 리턴할 때도 유용하다.

```cpp
void setRadius(int radius) {
    this->radius = radius;  // this를 통해 멤버 변수와 매개 변수를 구분
}

```

- **멤버 함수가 객체 자신의 주소를 반환**할 때도 **this**를 사용하여 객체의 주소를 리턴할 수 있다.

```cpp
class Sample {
public:
    Sample* f() {
        return this;  // 객체 자신의 주소를 리턴
    }
};

```

## 5.4 this 포인터의 제약 사항

- **this**는 **멤버 함수** 내에서만 사용할 수 있으며, 일반 함수에서는 사용할 수 없다.
- **static 멤버 함수** 내에서는 **this**를 사용할 수 없다. **static** 함수는 객체와 관계없이 호출되기 때문에, 객체에 대한 참조가 없으므로 **this**를 사용할 수 없다.

```cpp
class Sample {
    int a;
public:
    static void func() {
        // this 사용 불가
    }
};

```







# 실습 7-3

다음은 Person은 사람을, , Family는 가족을 추상화한 클래스로서 완성되지 않은 클래스이다.

```cpp
class Person
{
    string name;

public:
    Person() { name = ""; }
    Person(string name) { this->name = name; }
    string getName() { return name; }
    void setName(string name) { this->name = name; }
};

class Family
{
    string name;
    Person p[3];//Person 배열 포인터
    //Person *p; // Person 배열 포인터
    int size;  // Person 배열의 크기. 가족 구성원 수
public:
    Family(string name);
    //Family(string name, int size); // size 개수만큼 Person 배열 동적 생성
    void setName(int index, string name);
    void show(); // 모든 가족 구성원 출력
    ~Family();
};
```

다음은 main()이 작동되도록 Person과 Family클래스에 필요한 멤버들을 추가하고, 코드를 완성하라.

```cpp
int main()
{
    Family simpson("Simpson", 3); // 3명으로 구성된 Simpson 가족
    simpson.setName(0, "Mr. Simpson");
    simpson.setName(1, "Mrs. Simpson");
    simpson.setName(2, "Bart Simpson");
    simpson.show();
}
```

풀이

```cpp
/* 실습 7-3 */
class PERSON3
{
    std::string name; // string을 std::string으로 변경

public:
    PERSON3() { name = ""; }
    PERSON3(std::string name) { this->name = name; }
    std::string getName() { return name; } // return 타입도 std::string으로 변경
    void setName(std::string name) { this->name = name; } // 인자 타입도 std::string으로 변경
};

class Family
{
    std::string name; // string을 std::string으로 변경
    PERSON3 *p; // PERSON3 배열 포인터
    int size; // PERSON3 배열의 크기. 가족 구성원 수

public:
    Family(std::string name_in, int size_in); // 생성자 인자의 string도 std::string으로 변경
    void setName(int index, std::string name); // 인자 타입도 std::string으로 변경
    void show(); // 모든 가족 구성원 출력
    ~Family();
};

Family::Family(std::string name_in, int size_in)
{
    name = name_in;
    size = size_in;
    p = new PERSON3[size]; // size 개수만큼 PERSON3 배열 동적 생성
}

void Family::setName(int index, std::string name)
{
    p[index].setName(name); // PERSON3 객체의 setName 호출
}

void Family::show()
{
    cout << name << " family is consist of " << size << " members" << endl;
    for (int n = 0; n < size; n++)
        cout << p[n].getName() << endl; // getName 호출 후 출력
}

Family::~Family()
{
    cout << name << " family is deleted" << endl;
    delete[] p; // 동적 할당한 PERSON3 배열 메모리 해제
}

int main()
{
    Family simpson("Simpson", 3); // 3명으로 구성된 Simpson 가족
    simpson.setName(0, "Mr. Simpson");
    simpson.setName(1, "Mrs. Simpson");
    simpson.setName(2, "Bart Simpson");
    simpson.show();
}
```

![image.png](/images/2024-10-19-multiple-objects/image 5.png)







# 실습 7-4

실습 7-3의 코드를 포인터 변수를 사용하도록 수정해보자.

```cpp
int main() {
    Family* simpson = new Family("Simpson", 3); // 3명으로 구성된 Simpson 가족
    simpson->setName(0, "Mr. Simpson");
    simpson->setName(1, "Mrs. Simpson");
    simpson->setName(2, "Bart Simpson");
    simpson->show();
    delete simpson; // 동적 할당 해제
}

```

 (클래스의 포인터는 스킵)