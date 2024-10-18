---
layout: single
title:  "#7 C++ 생성자와 소멸자"
categories: C++
tag: [C++]
toc: true
toc_label: "table of content" # toc 이름 설정
toc_icon: "bars" # 아이콘 설정
toc_sticky: true # 마우스 스크롤과 함께 내려갈 것인지 설정
typora-root-url: ../
---



# 1. 객체의 생성 및 소멸

## 1.1 객체의 생성

- 객체가 생성될때 생성자가 자동으로 호출된다.

## 1.2 객체의 소멸

- 객체가 소멸될때 소멸자가 자동으로 호출된다.







# 2. 생성자와 소멸자

## 2.1 생성자와 소멸자는 자동으로 호출되는 함수이다.

생성자: `객체가 생성되는 시점`에서 자동으로 호출되는 `멤버함수이다.`

소멸자: `객체를 소멸되는 시점에서` 자동으로 호출되는 `멤버함수이다.`

## 2.2 생성자와 소멸자 함수의 사용

생성자: `객체가 생성될때 객체가 필요한 초기화를 위해`

소멸자: `객체가 소멸될때 객체의 정리를 위해`

## 2.3 생성자와 소명자 이름

생성자: “`클래스이름()`” 형식의 함수

소멸자: “`~클래스이름()`” 형식의 함수

## 2.4 생성자와 소멸자는 리턴값이 없다

`리턴타입이 없다. void도 안된다.`***







# 3. 생성자 특징

객체 생성시 오직 한번만 호출

- 자동으로 호출된다. 임의로 호출되며, `각 객체마다 생성자가 실행된다.***`

 

생성자는 중복 가능하다.

- 생성자는 `한 클래스내에 여러개 가능`하다.
- `중복된 생성자 중 하나만 실행`된다.

매개변수가 있는경우, 없는 경우 → 다른 함수로 인식

따라서 CIrcle2클래스는 2개의 생성자를 가지고 있다.

```cpp
class Circle2
{
public:
    int radius;
    Circle2();        // 기본 생성자 선언
    Circle2(int r);   // 매개변수 있는 생성자 선언
    ~Circle2();       // 소멸자 선언
    double getArea(); // 멤버 함수 선언
};
```

## 3.1 객체 생성 및 생성자 실행 과정

```cpp
Circle donut; 
```

→ 객체 공간할당

→ Circle() 생성자 실행 

```cpp
Circle pizza(30);
```

→ 객체 공간할당

→ Circle(int r) 생성자 실행

## 3.2 디폴트 생성자

### 3.2.1 디폴트 생성자란

매개변수가 없는 생성자. 기본 생성자라고도 부른다.

`객체를 생성할때 별도로 지정하지 않으면, 항상 디폴트 생성자로 초기화된다.`

- 생성자는 꼭 있어야하는가?

→ c++ 컴파일러는 객체가 생성될때, 생성자가 반드시 호출된다.

- 개발자가 클래스에 생성자를 작성해놓지 않으면

→ 컴파일러에 의해 기본 생성자가 자동으로 생성된다.

### 3.2.2 디폴트 생성자가 자동으로 실행되는 경우 예시

- 생성자가 하나도 작성되어있지 않은 클래스의 경우
- 컴파일러가 디폴트 생성자를 자동으로 생성한다.

```cpp
class Circle {
public:
	int radius;
	double getArea();
	//Circle(); 컴파일러에 의해 자동으로 삽입됨
};
//Circle::Circle(){} 컴파일러에 의해 자동으로 삽입됨
int main() {
	Circle donut; //디폴트 생성자를 호출한다.
}
```

### 3.2.3 디폴트 생성자가 자동으로 생성되지 않는 경우

생성자가 `하나라도` 선언된 클래스의 경우

- 컴파일러는 디폴트 생성자를 자동 생성하지 않는다.







# 4. 소멸자 특징

객체가 소멸되는 시점에서 자동으로 호출되는 함수

- 오직 한번만 `자동 호출,` 임의로 호출할 수 없다.
- `객체 메모리 소멸 직전에 호출된다.`
- `~클래스 이름()`형식의 함수
- 소멸자는 리턴타입이 없고, 어떤값도 리턴하면 안됨.
- 소멸자는 매개변수가 없다. → 오버로딩불가
- 중복 불가능 : 소멸자는 한클래스내에 오직 한 개만 작성가능하다.
- 소멸자가 선언되어있지 않으면 디폴트소멸자가 자동생성된다.







# 5. 생성자와 소멸자 실행순서 ***

## 5.1 객체가 선언된 위치에 따른 분류

- 지역 객체함수 내에 선언된 객체로서, `함수가 종료하면 소멸`된다.
- 전역 객체함수의 바깥에 선언된 객체로서, 프로그램이 종료할 때 소멸된다.

## 5.2 객체 생성 순서

- 전역 객체는 프로그램에 `선언된 순서`로 생성된다.***
- 지역 객체는 `함수가 호출되는 순간에` 순서대로 생성된다.

## 5.3 객체 소멸 순서

- `함수가 종료하면, 지역 객체가 생성된 순서의 역순으로 소멸된다.`
- `프로그램이 종료하면, 전역 객체가 생성된 순서의 역순으로 소멸된다.`

## 5.4 new를 이용하여 동적으로 생성된 객체의 경우

- **new**를 실행하는 순간 객체가 생성된다.
- **delete** 연산자를 실행할 때 객체가 소멸된다.

```cpp
#include <iostream>
using namespace std;

class Circle {
public:
    int radius;
    Circle();
    Circle(int r);
    ~Circle();
    double getArea();
};

Circle::Circle() {
    radius = 1;
    cout << "반지름 " << radius << " 원 생성" << endl;
}

Circle::Circle(int r) {
    radius = r;
    cout << "반지름 " << radius << " 원 생성" << endl;
}

Circle::~Circle() {
    cout << "반지름 " << radius << " 원 소멸" << endl;
}

double Circle::getArea() {
    return 3.14 * radius * radius;
}

**Circle globalDonut(1000); // 전역 객체 생성 ***
Circle globalPizza(2000); // 전역 객체 생성 ***

void f() {
    Circle fDonut(100);   // 지역 객체 생성
    Circle fPizza(200);    // 지역 객체 생성
}

int main() {
    Circle mainDonut;     // 지역 객체 생성
    Circle mainPizza(30);  // 지역 객체 생성
    f();                  // 함수 f 호출
}**

```

### 위의 코드의 생성자와 소멸자가 실행되는 순서를 자세히 살펴본다.

`메인함수, 외부함수안에서 선언된 지역 객체인지, 전역 객체인지를 잘 살펴야한다.`

```cpp
//먼저 전역객체가 생성된다.
//전역객체는 프로그램의 실행과 함께 실행되어 종료시에 소멸된다.

반지름 1000 원 생성 
반지름 2000 원 생성

//지역객체 생성
반지름 1 원 생성
반지름 30 원 생성

//f함수가 호출 되어 지역객체가 실행된다.
반지름 100 원 생성
반지름 200 원 생성

//함수가 끝나자 지역객체가 역순으로 소멸된다. 
반지름 200 원 소멸
반지름 100 원 소멸

//메인함수의 지역객체가 역순으로 소멸된다.
반지름 30 원 소멸
반지름 1 원 소멸

//전역 객체는 프로그램이 종료시에 소멸되므로 마지막에 소멸된다.
반지름 2000 원 소멸
반지름 1000 원 소멸
```

- 메인함수에 따라서 호출된 함수의 지역객체 생성 → 소멸 → 메인함수의 지역객체 생성 → 소멸

```cpp
void func0930()
{
    Circle donut; //(1) donut이 생성된다.
    double area = donut.getArea();
    cout<< "donut area is " << area << endl;
}  //(2) 이 함수를 나올면서 donut이 소멸된다.

int main()
{
    func0930();

    Circle2 pizza(30); // (3) 피자가 생성된다.      
    double area = pizza.getArea();
    cout << "pizza area is " << area << endl; 

    return 0;

} // (4) 피자가 소멸된다.
```

- 아래와 코드와 같이 소멸은 생성의 역순으로 일어난다는것을 잊으면 안된다.

```cpp
#include <iostream>
#include <string>
#include <malloc.h>

using namespace std;

class Circle2
{
public:
    int radius;
    Circle2(); // 기본 생성자 선언
    Circle2(int r); // 매개변수 있는 생성자 선언
    ~Circle2(); // 소멸자 선언
    double getArea(); // 멤버 함수 선언
};

Circle2::~Circle2()
{
    cout << "destructor start. radius is " << radius << endl;
}

// 기본 생성자 정의
Circle2::Circle2()
{
    radius = 1;
    cout <<"constructor1 start. Made circle. "<< "radius is " << radius << endl;
}

// 매개변수 있는 생성자 정의
Circle2::Circle2(int r)
{
    radius = r;
    cout <<"constructor2 start. Made circle. "<< "radius is " << radius << endl;
}

// 면적 계산 멤버 함수 정의
double Circle2::getArea()
{
    return 3.14 * radius * radius;
}

int main()
{
    Circle2 donut; // 기본 생성자 호출 (1)   (4)소멸자 실행
    double area = donut.getArea();
    cout << "donut area is " << area << endl;

    Circle2 pizza(30); // (2)      (3)소멸자 실행
    area = pizza.getArea();
    cout << "pizza area is " << area << endl;

    return 0;
}

```

실행결과

```cpp
constructor1 start. Made circle. radius is 1
donut area is 3.14
constructor2 start. Made circle. radius is 30
pizza area is 2826
destructor start. radius is 30
destructor start. radius is 1
```







# 6. 객체간의 초기화와 대입

## 6.1 초기화

새로운 객체를 만들때,

기존의 객체의 값을 복사해서 만든다.

```cpp
Stack s1(5);  // s1 객체 생성
Stack s2 = s1;  // s1 객체를 복사해서 s2 객체 생성 (복사 생성자 사용)

```

## 6.2 대입

이미 존재하는 객체에 대한 다른 객체의 값을 복사하는것

```cpp
Stack s3;   // s3 객체 생성
s3 = s1;    // s1 객체의 값을 s3 객체에 대입 (대입 연산자 사용)

```







# `실습 5-1`

```cpp
int main()
{
    Rectangle3 rect1;
    Rectangle3 rect2(3, 5);
    Rectangle3 rect3(3);

    if(rect1.isSquare()) cout <<"rect1 is square"<<endl;
    if(rect2.isSquare()) cout <<"rect2 is square"<<endl;
    if(rect3.isSquare()) cout <<"rect3 is square"<<endl;
}
```

위의 메인함수가 잘 작동하도록 Rectangle클래스를 작성하고 프로그램을 완성해라. Rectangle클래스는 width, height의 두 멤버변수와 3개의 생성자, 그리고 isSquare함수를 가진다.

```cpp
/*실습 5-1*/
class Rectangle3{
    public:
        int width;
        int height;

				//첫번째 생성자
        Rectangle3(){
            width = 1;
            height = 1;
        }
        
        
        //두번쨰 생성자
        // 매개변수를 받은다음에 width, height에 대입한다.
        Rectangle3(int width_in, int height_in){
            width = width_in; //this ->width = width
            height = height_in;
        }

				//세번째 생성자
				//받은 길이를 width, height에 다 넣는다.
        Rectangle3(int length){
            width =height = length;
        }
        
        //소멸자가 실행되면 지워졌다고 출력
        ~Rectangle3(){
            
            cout<<"Rectangle is deleted "<<width<<" "<<height<<endl;
        }
        bool isSquare();

};

bool Rectangle3::isSquare(){
    if (width==height) return true; //return true
    else return false; //return false

}

int main()
{
    Rectangle3 rect1;
    Rectangle3 rect2(3, 5);
    Rectangle3 rect3(3);

    if(rect1.isSquare()) cout <<"rect1 is square"<<endl;
    if(rect2.isSquare()) cout <<"rect2 is square"<<endl;
    if(rect3.isSquare()) cout <<"rect3 is square"<<endl;
}
```

출력화면

```cpp

//1번과 3번만 isSquare()함수가 true이다.
rect1 is square
rect3 is square

Rectangle is deleted 3 3
Rectangle is deleted 3 5
Rectangle is deleted 1 1
```







# `실습 5-2 ***`

`매우 중요!!`

```cpp
정수값의 사각형 너비와 높이를 가지는 width, height 변수 멤버
너비와 높이값을 매개변수로 받는 생성자
너비와 높이를 1로 초기화하는 매개변수 없는 생성자
width와 height를 출력하는 소멸자
타원의 너비를 리턴하는 getWidth() 함수 멤버
타원의 높이를 리턴하는 getHeight() 함수 멤버
타원의 너비와 높이를 변경하는 set(int w, int h) 함수 멤버
타원의 너비와 높이를 화면에 출력하는 show() 함수 멤버
Oval 클래스를 활용하는 코드의 사례와 결과는 다음과 같다.

int main() {
    Oval a, b(3, 4);
    a.set(10, 20);
    a.show();
    b.show();
    cout << b.getWidth() << "," << b.getHeight() << endl;
}

실행 결과:

width = 10, height = 20
3, 4
Oval 소멸 : width = 3, height = 4
Oval 소멸 : width = 10, height = 20
```

타원클래스는 주어진 사각형에 내접하는 타원을 추상화한 클래스이다.

타원클래스의 멤버는 모두 다음과 같다. Oval클래스를 선언부와 구현부로 나누어 작성하라.

풀이

```cpp
#include <iostream>
using namespace std;

class Oval {
    // 변수 width, height
    int width, height;

public:
    // 디폴트 생성자: Oval()
    Oval() {
        width = 1;
        height = 1;
    }

    // 매개변수 2개짜리 생성자: Oval(int w, int h)
    Oval(int w, int h) {
        width = w;
        height = h;
    }

    // 함수 set()
    // 입력받은 두 숫자를 각각 width, height에 대입해
    // 넓이와 높이를 변경
    void set(int w, int h) {
        width = w;
        height = h;
    }

    // 너비를 출력하는 함수 show()
    void show() {
        cout << "width = " << width << ", height = " << height << endl;
    }

    // getWidth(), getHeight() 함수
    int getWidth() {
        return width;
    }
    int getHeight() {
        return height;
    }

    // 소멸자 : ~Oval()
    ~Oval() {
        cout << "Oval 소멸 : width = " << width << ", height = " << height << endl;
    }

    //double getArea(); // getArea 함수 선언
};
/*
double Oval::getArea()
{
    return 3.14 * width * height;
}
*/

int main() {
    Oval a, b(3, 4);
    a.set(10, 20);
    a.show();
    b.show();  // b 객체의 show()도 호출
    cout << b.getWidth() << ", " << b.getHeight() << endl;
}

```







# 짝수 홀수문제 실습 (인라인 함수 이용)

```cpp

inline bool isEven(int B)
{
    if (B%2==0)return true;
    else return false;
}

int main()
{
    int A =10;
    if (isEven(A)) // if(isEven(A)==true)
        cout<<"짝수입니다."<<endl;
    else
        cout<<"홀수입니다."<<endl;
}
```







# 7. 분할 컴파일

## 7.1 바람직한 c++ 프로그램 작성법

클래스를 헤더파일과 cpp파일로 분리하여 작성한다.

### 1) 클래스 선언 부 : 헤더 파일(.h)에 저장

클래스 이름.h

### 2) 클래스 구현 부 : cpp 파일에 저장 (멤버함수의 구현)

클래스가 선언된 헤더 파일 include
클래스 이름.cpp

## 7.2 실습

먼저 Circle.h에는 클래스 선언부를 작성한다.

`Circle.h` 함수 선언

```cpp
class Circle {
private:
	int radius;
public:
	void SetRadis();
	int GetRadius();
	double getArea();
};
```

`Circle.cpp` 함수 구현부

#include "Circle.h" 해준다

```cpp
#include <iostream>
using namespace std;
#include "Circle.h"

void Circle::setRadis(int r) {
	radius = r;
}
int getRadius() {
	return radius;
}
double Circle::getArea() {
	return 3.14*radius*radius;
}
```

`main.cpp` 메인함수 선언

#include "Circle.h" 해준다

```cpp
#include <iostream>
using namespace std;
#include "Circle.h"

int main() {
	Circle donut;
	donut.setRadius(1);
	double area = donut.getArea();
	cout << "donut 면적은 ";
	cout << area << endl;
	
	Circle pizza;
	pizza.setRadius(30);
	area = pizza.getArea();
	cout << "pizza 면적은 ";
	cout << area << endl;
}
```

## 7.3 헤더파일의 중복 include문제

**동일한 헤더 파일을 여러 번 포함**하면, 헤더 파일의 내용이 중복되어 정의되기 때문에 컴파일러는 오류를 발생시킨다.

### 7.3.1 해결방법 (조건 컴파일)

`main.cpp에서 #include “Circle.h”를 하면 Circle.h로 이동한다`

- `#ifndef CIRCLE_H`와 `#define CIRCLE_H`는 `CIRCLE_H`라는 심볼이 정의되지 않은 경우에만 해당 헤더 파일을 처리하도록 한다.
- 헤더 파일이 처음 포함될 때만 파일 내용을 읽어들이고, 두 번째로 `#include`될 때는 이미 `CIRCLE_H`가 정의되어 있기 때문에 포함되지 않는다.
- `#endif`는 조건부 컴파일 블록의 끝을 나타낸다.

main.cpp

```cpp
#include <iostream>using namespace std;
#include "Circle.h"#include "Circle.h"#include "Circle.h"  // 여러 번 include해도 오류 발생하지 않음int main() {
    // main 함수 내용
}

```

Circle.h (헤더 파일)

```cpp
#ifndef CIRCLE_H
#define CIRCLE_H

class Circle {
private:
    int radius;

public:
    void setRadius(int r);
    int getRadius();
    double getArea();
};

#endif

```

여기서 다음 세줄을 세트라고 생각하면 된다.

- #ifndef CIRCLE_H
- #define CIRCLE_H
- #endif







# 실습 5-4

main.cpp

```cpp
#include "cal.h"  // Calculator 클래스 선언 포함
int main() {
    Calculator calc; // Calculator 객체 생성
    calc.run(); // run() 함수 호출
    return 0;
}
```

Calculator.cpp

```cpp
#include "cal.h"
#include <iostream>

using namespace std;

// Adder 클래스의 생성자 정의
Calculator::Adder::Adder(int a, int b) {
    op1 = a;
    op2 = b;
}

// Adder 클래스의 process 함수 정의
int Calculator::Adder::process() {
    return op1 + op2;
}

// Calculator 클래스의 run 함수 정의
void Calculator::run() {
    cout << "<< write two number >> ";
    int a, b;
    cin >> a >> b;               // 두 개의 정수를 입력받음
    Adder adder(a, b);           // Adder 객체 생성
    cout << adder.process() << endl;  // 덧셈 결과 출력
}

```

cal.h

```cpp

#ifndef CAL_H
#define CAL_H

class Calculator {
    class Adder {
        int op1, op2;
    public: // 생성자와 process 함수를 public으로 수정
        Adder(int a, int b); // 생성자 선언
        int process(); // 덧셈 처리 함수 선언
    };

public:
    void run(); // 계산기 실행 함수 선언
};

#endif

```











## (참고) vscode에서 분할 컴파일이 안될때

### 1. 이전 실행 파일 삭제

Windows에서는 명령 프롬프트 또는 PowerShell에서 `del` 명령어를 사용하여 이전 실행 파일을 삭제할 수 있습니다.

```bash
del main.exe

```

이 명령어는 현재 디렉터리의 `main.exe` 파일을 삭제합니다.

### 2. 다시 컴파일

이전 실행 파일을 삭제한 후, 새로 코드를 컴파일합니다:

```bash
g++ main.cpp Calculator.cpp -o main

```

이 명령어는 새롭게 `main.exe` 실행 파일을 생성합니다.

### 3. 실행

새로 생성된 실행 파일을 실행하려면 다음 명령어를 사용합니다:

```bash
.\main
```

이제 수정된 코드가 실행되어야 합니다.

![image.png](image.png)