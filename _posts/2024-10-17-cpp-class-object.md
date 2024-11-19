---
layout: single
title:  "#6 C++ 클래스와 객체"
categories: C++
tag: [C++]
toc: true
toc_label: "table of content" # toc 이름 설정
toc_icon: "bars" # 아이콘 설정
toc_sticky: true # 마우스 스크롤과 함께 내려갈 것인지 설정
typora-root-url: ../
---

# 1. 객체

세상의 모든것이 객체이다. 

## 1.1 캡슐화

 객체를 캡슐로 싸서 그내부를 보호하고 볼 수 없게 한다.

### 1.1.1 캡슐화의 목적

객체내 데이터에 대한 보안, 보호, 외부접근 제한

## 1.2 객체의 일부는 공개된다.

외부와의 인터페이스( 정보교환, 통신)을 위해 객체의 일부분을 공개한다.

## 1.3 c++객체는 멤버 변수와 멤버함수로 구성된다.

### 1.3.1 TV객체의 상태:

on/off

채널

음량

### 1.3.2 행동:

켜기/끄기

채널 증가/감소

음량 증가/감소

```cpp
멤버 변수:
bool on = true: TV의 전원이 켜져 있음을 나타냅니다.
int channel = 8: 현재 TV의 채널은 8번입니다.
int volume = 16: 현재 TV의 볼륨은 16입니다.
멤버 함수:
void powerOn(): TV를 켜는 함수입니다.
void powerOff(): TV를 끄는 함수입니다.
void increaseChannel(): 채널을 올리는 함수입니다.
void decreaseChannel(): 채널을 내리는 함수입니다.
void increaseVolume(): 볼륨을 높이는 함수입니다.
void decreaseVolume(): 볼륨을 낮추는 함수입니다.
```







# 2. 클래스와 객체

```cpp
클래스가 붕어빵 틀

객체가 붕어
```

## 2.1 클래스

- 사용자 정의 자료형이라 생각하면 편하다. (일종의 데이터 형의 역할이라고 생각하면 편하다.
- 객체를 만들어내기 위한 설계도라고 생각한다.
- 멤버변수와 멤버 함수를 선언한다.

## 2.2 객체 (object)

- 클래스형에 의해 만들어진 실제 사용되는 변수
- 객체는 생성될 때 클래스의 모양을 그대로 가지고 탄생
- 멤버 변수와 멤버 함수로 구성

## 2.3 클래스와 객체 관계

⇒ 클래스

```cpp
class TV {
    bool on;
    int channel;
    int volume;

public:
    void powerOn() { ... }
    void powerOff() { ... }
    void increaseChannel() { ... }
    void decreaseChannel() { ... }
    void increaseVolume() { ... }
    void decreaseVolume() { ... }
};

```

⇒ 객체

### TV 객체 1

```cpp
- bool on = true
- int channel = 8
- int volume = 16

- 멤버 함수: `powerOn()`, `powerOff()`, `increaseChannel()`, `decreaseChannel()`, `increaseVolume()`, `decreaseVolume()`
```

### TV 객체 2

```cpp
- bool on = false
- int channel = 13
- int volume = 5

- 멤버 함수: `powerOn()`, `powerOff()`, `increaseChannel()`, `decreaseChannel()`, `increaseVolume()`, `decreaseVolume()`
```







# 3. string 클래스와 vector클래스

(기능은 안외워도 된다.)

## 3.1 string클래스

문자열에 대한 처리를 제공.

| **구분**        | **함수**                     | **특징**                                                     |
| --------------- | ---------------------------- | ------------------------------------------------------------ |
| **생성자**      | string str;                  | 디폴트 생성자, 빈 문자열을 가지는 str 이름의 스트링 객체 생성 |
|                 | string str1("string");       | 문자열 상수 "string"으로 초기화하여 str1 객체 생성           |
|                 | string str2(str1);           | 객체 str1 값을 복사한 str2 객체 생성                         |
| **원소값 접근** | str[i]                       | i번째 인덱스에 위치한 문자값을 입력/출력                     |
|                 | str.at(i)                    | i번째 인덱스에 위치한 문자값을 입력/출력                     |
|                 | str.substr(position, length) | position 값에서 시작하여 length 만큼의 길이를 가지는 문자열인 subString 값 리턴 |
| **합당 / 변경** | str1 = str2;                 | 문자열 저장 공간을 설정하고 str2 값으로 초기화               |
|                 | str1 += str2;                | str2 문자열을 str1의 뒤에 연결                               |
|                 | str.empty();                 | str이 빈 스트링일 경우 true 리턴, 그렇지 않으면 false 리턴   |
|                 | str1 + str2;                 | str2 값을 str1의 끝 부분에 연결된 스트링을 리턴              |
|                 | str.insert(pos, str2);       | str2 값을 pos 위치부터 시작하여 str에 삽입                   |
|                 | str.remove(pos, length);     | pos 위치부터 시작하여 length 크기만큼의 substring을 삭제     |
|                 | str1 == str2, str1 != str2   | 두 문자열이 동일한지 비교하여 boolean 값 리턴                |
|                 | str1 < str2, str1 > str2     | 문자열 크기 비교 (사전적 순서)                               |
|                 | str1 <= str2, str1 >= str2   | 문자열 크기 비교 (사전적 순서)                               |
|                 | str1.find(str1)              | str1에서 str1 값이 처음으로 존재하는 인덱스 값 리턴          |

## 3.2 vector클래스 : 동적 배열

실행중에 `크기를 변경할 수 있는 배열 기능`을 제공한다.

### 3.2.1 vector클래스의 주요 멤버와 연산자

| **멤버와 연산자 함수**           | **설명**                                                 |
| -------------------------------- | -------------------------------------------------------- |
| **push_back(element)**           | 벡터의 마지막에 `element` 추가                           |
| **at(int index)**                | `index` 위치의 원소에 대한 참조 리턴                     |
| **begin()**                      | 벡터의 첫 번째 원소에 대한 참조 리턴                     |
| **end()**                        | 벡터의 끝(마지막 원소 다음)을 가리키는 참조 리턴         |
| **empty()**                      | 벡터가 비어 있으면 `true` 리턴                           |
| **erase(iterator it)**           | 벡터에서 `it`가 가리키는 원소 삭제 후 자동으로 벡터 조절 |
| **insert(iterator it, element)** | 벡터 내 `it` 위치에 `element` 삽입                       |
| **size()**                       | 벡터에 들어 있는 원소의 개수 리턴                        |
| **operator**                     | 지정된 원소에 대한 참조 리턴                             |
| **operator=()**                  | 이 벡터를 다른 벡터에 치환(복사)                         |

### 3.2.2 vector다루기 사례

`vector 생성`

```cpp
vector<int> v;
```

정수 벡터 v 생성

`정수 원소 삽입`

```cpp
v.push_back(1);
v.push_back(2);
v.push_back(3);
```

정수 1, 2, 3 삽입

벡터 v의 상태: [1, 2, 3]

원소 개수 s와 벡터의 용량 c

```cpp
int s = v.size();      // s는 3
int c = v.capacity();  // c는 7
```

- **s = 3**: 벡터에 있는 원소의 개수는 3
- **c = 7**: 벡터의 용량은 7 (벡터의 내부적으로 더 많은 원소를 저장할 수 있도록 공간을 할당)

`원소 값 접근 (at() 함수 이용)`

```cpp
v.at(2) = 5;
int n = v.at(1);
```

- v.at(2) = 5: 벡터의 2번 인덱스에 있는 값을 5로 변경벡터 v의 상태: [1, 2, 5]
- n = v.at(1): 벡터의 1번 인덱스에 있는 값을 n에 저장결과: n = 2

`원소 값 접근 (연산자 [] 이용)`

```cpp
v[0] = 10;
int m = v[2];  // m은 5
```

- v[0] = 10: 벡터의 0번 인덱스에 있는 값을 10으로 변경벡터 v의 상태: [10, 2, 5]
- m = v[2]: 벡터의 2번 인덱스에 있는 값을 m에 저장결과: m = 5

이처럼 벡터는 push_back()을 통해 동적으로 크기를 조절할 수 있으며, size()와 capacity()를 통해 현재 상태를 확인할 수 있다. at()와 []를 이용해 각각 안전하게 또는 직접적으로 원소에 접근할 수 있다.

### 3.2.3 vector 클래스 사용하기

```cpp
#include <iostream>#include <vector>using namespace std;

int main() {
    vector<int> v;  // 정수만 삽입 가능한 벡터 생성

    v.push_back(1); // 벡터에 정수 1 삽입
    v.push_back(2); // 벡터에 정수 2 삽입
    v.push_back(3); // 벡터에 정수 3 삽입

    for(int i = 0; i < v.size(); i++)  // 벡터의 모든 원소 출력
        cout << v[i] << " ";           // v[i]는 벡터의 i번째 원소
    cout << endl;

    v[0] = 10;       // 벡터의 첫 번째 원소를 10으로 변경
    int n = v[2];    // n에 3이 저장
    v.at(2) = 5;     // 벡터의 3번째 원소를 5로 변경

    for(int i = 0; i < v.size(); i++)  // 벡터의 모든 원소 출력
        cout << v[i] << " ";           // v[i]는 벡터의 i번째 원소
    cout << endl;
}

```

출력 결과:

```
1 2 3
10 2 5

```







# 4.  클래스 작성

`클래스는 멤버변수와 멤버함수로 구성`된다.

멤버함수는 해당 클래스의 모든 멤버를 직접 사용가능하다. (따로 객체이름을 지정할 필요가 없다.)

`클래스 선언부와 클래스 구현부로 구성된다. ⇒`

## 4.1 클래스 선언부

### 4.1.1 선언부의 주요 구성

- 멤버변수와 멤버 함수를 `선언`한다.
- 멤버 변수는 클래스 선언내에서 `초기화할수 없다.` ***
- 멤버 함수는 `원형 형태`로 선언된다.*** ⇒ 만약에 클래스 선언부에 구현이 되었다면. 자동 인라인 기능이 내재되게 된다.

### 4.1.2 멤버에 대한 접근 권한 지정

- 접근 지정자: private, protected, public 키워드
- 클래스의 멤버를 정의할때 접근 지정자를 사용한다.
- `디폴트는 private`
- public: `다른 모든 클래스나 객체`에서 멤버의 접근이 가능함을 표시한다.

### **4.1.3 <클래스 선언부 예시>**

```cpp
class Circle {
public:
    int radius;               // 멤버 변수
    double getArea();         // 멤버 함수
};
```

- **class 키워드**: 클래스를 선언할 때 사용.
- **public**: 멤버에 대한 접근 지정자.
- **멤버 변수**: radius는 원의 반지름을 저장하는 변수.
- **멤버 함수**: getArea()는 원의 면적을 계산하는 함수.

## 4.2 클래스 구현부

클래스에 정의된 `모든 멤버 함수 구현`. 클래스 선언부에 함께 구현할 수도 있다.

`(여기서 클래스 구현부도 클래스 외부로 간주한다. 따라서, ::를 사용하여야한다.)`

### **4.2.3 <클래스 구현부 예시**>

```cpp
double Circle::getArea() {
    return 3.14 * radius * radius;
}
```

- **범위 지정 연산자(::)**: 클래스 외부에서 멤버 함수를 정의할 때 사용.
- **getArea() 함수**: 원의 면적을 계산하여 리턴.

**클래스 선언과 구현을 분리하는 이유**: ***

- `클래스 선언을 다른 파일에서 활용하기 위해, 선언과 구현을 분리하여 관리함.`

## 4.3 클래스의 선언부, 구현부 정리

```cpp
/* Circle 클래스 선언부 */
class Circle
{
    // 클래스 private이 디폴트이다.

public:
    int radius; //멤버 변수 선언
    double getArea(); //멤버 함수 선언
    
    /* 이렇게 선언부에 멤버함수를 구현할 수도 있다.
    double getArea()
    {
        double area = 3.14 * radius * radius;
        return area;
    }*/
};

/*Circle 클래스의 구현부*/
double Circle::getArea() //getArea함수는 Circle클래스의 멤버함수이다.
 {
     double area = 3.14*radius*radius;
     return area;
 }
 
 
 

int main()
{
    Circle A, B, C; // int a,b,c;와 같은 느낌. 객체 A,B,C를 생성.
    A.radius = 10; //A의 멤버 변수인 radius에 접근

    cout << A.getArea() << endl; //A의 멤버함수 호출
    
    B.radius =20;
    
    double area = B.getArea() << endl;
    cout << area << endl;
    

```

점을 찍으면 사용할수있는 멤버함수와 멤버변수가 보인다.

![image.png](/images/2024-10-17-cpp-class-object/image.png)







# 5. 객체 생성 및 활용

## 5.1 객체 이름 및 객체 생성

```cpp
Circle donut; // 이름이 donut인 Circle 타입의 객체 생성
```

객체인 타입(클래스 이름)  : Circle

객체 이름 : donut

## 5.2 객체의 멤버 변수 접근

```cpp
donut.radius =1; //도넛 객체의 radius멤버변수값을 1로 설정한다.
```

## 5.3 객체의 멤버함수 접근

```cpp
double area = donut.getArea(); //도넛 객체의 면적 알아내기
```







# 6. 객체의 생성 및 응용

## 6.1 멤버 변수의 메모리 할당 시점

`클래스를 정의한다고 멤버변수가 메모리가 할당되는것이 아니다.`

클래스의 `객체를 생성하는 시점에` 멤버 변수가 메모리에 할당된다!

## 6.2 멤버변수나 멤버함수에 접근 방법

일반 객체: .연산자를 사용한다.

동적 객체 : →연산자 사용

```cpp
Circle c1; //일반 객체
c1.radius =2;

Circle *c2 = new Circle; //동적객체, 객체에 대한 포인터
p-> radius =3; 
```

### 6.3.1 객체를 가리키는, 객체에 대한 포인터 (`멤버변수`에 접근)***

`Circle *p = &A;` (Circle은 클래스이고, A는 객체이다)

(*p).radius

p → radius

### 6.3.2 객체에 대한 포인터 (`멤버함수`에 접근)***

    cout << (*p).getArea() << endl; // 314
    cout << p->getArea() << endl;   // 314

```cpp
int main()
{
    Circle A, B, C; 
    A.radius = 10;

    cout << A.getArea() << endl;

    Circle *p = &A;  //객체를 가리키는 포인터 선언
    
    cout << (*p).radius << endl; // 10
    cout << p->radius << endl;   // 10  이방식을 더 많이 사용한다.

    cout << (*p).getArea() << endl; // 314
    cout << p->getArea() << endl;   // 314
}
```

## 6.3 멤버함수는 멤버변수와 생성프로세스가 다르다.

`객체마다 만들어지는것이 아니라`, `클래스 전체에 대해서 한번만 정의`하고 같은 클래스 `객체들이 공유해서 사용`

### 6.3.1 멤버함수에서 사용되는 멤버변수

멤버 함수를 호출한 객체의 것이다.

### 6.3.2 멤버함수와 전역함수의 차이점

- 멤버 함수를 `호출하려면 객체 이름을 지정해야 한다.`

```cpp
Circle c1;
double area = c1.getArea(); //Circle 클래스의 멤버함수를 호출한다.

getArea(); //전역함수를 의미하므로 컴파일 에러. 
//=> 전역범위에 getArea()라는 함수가 정의되어있지 않으므로
```

`위와 같이 .으로 지정해도되고,`

`다음과 같이 포인터를 이용해도 된다.`

```cpp
Circle* p = &c1;
double area = (*p).getArea();
```

### 6.3.3 `멤버함수안에서 같은 클래스에 정의된` 멤버변수, 멤버함수에 접근**

- `객체이름을 지정하지 않아도 된다. (않는다.)`***
- 멤버 함수 안에서는 접근 지정자에 상관없이 클래스의 모든 멤버를 사용가능함.

```cpp
class Circle {
private:
    int radius;

public:
    double getArea() {
        return 3.14 * radius * radius;  // private 멤버 변수에 접근 가능
    }

    void setRadius(int r) {
        radius = r;  // private 멤버 변수에 접근 가능
    }
};

```







# 실습 4-3

```cpp
int main() {
	Rectangle rect;
	rect.width = 3;
	rect.height = 5;
	cout << "사각형의 면적은 " << rect.getArea() << endl;
}
```

메인함수가 잘 작동하도록 너비와 높이를 가지고 면적 계산 기능을 가진 Rectangle 클래스를 작성하고, 전체 프로그램을 완성해라.

```cpp
class Rectangle{
public:
    //멤버변수
    int width; //사각형의 폭
    int height;//사각형의 높이

    int getArea()
    {
        //여기 함수 내에서 선언하는 변수는 멤버가 아닌 지역변수이다.
        int area= width*height;
        return area;

    }
};

int main()
{
    Rectangle rect;
    rect.width =3;
    rect.height =5;
    cout<< "square area is "<<rect.getArea()<<endl;
}
```

또는

다음과 같이 외부에다가 선언

```cpp
class Rectangle{
public:
    //멤버변수
    int width; //사각형의 폭
    int height;//사각형의 높이

    int getArea(); // 멤버함수 선언

};
int Rectangle::getArea()
{
    int area = width*height;
    return area;
}

int main()
{
    Rectangle rect;
    rect.width =3;
    rect.height =5;
    cout<< "square area is "<<rect.getArea()<<endl;
}
```



## 클래스내에 에러 표시

```cpp
class Rectangle1
{
private:
    int width;
    int height;

public:
    int Area()
    {
        int area = width * height;
        return area;
    }
    bool setWidth(int input)  //매개변수가 있는 멤버함수
    {
        if (input < 0)
            return false;
        else
        {
            width = input;
            return true;
        }
    }

    bool setHeight(int input) //매개변수가 있는 멤버함수
    {
        if (input < 0)
            return false;
        else
        {
            height = input;
            return true;
        }
    }

    int getWidth()
    {
        return width;
    }
    int getHeight()
    {
        return height;
    }
};

int main()
{
    Rectangle1 rect;
    if (rect.setWidth(3) == false)
    {
        cout << "width error" << endl;
    }
    if (rect.setHeight(5) == false)
    {
        cout << "height error" << endl;
    }
    rect.setWidth(3);
    rect.setHeight(5);
    cout << "width=" << rect.getWidth() << endl;
    cout << "height=" << rect.getHeight() << endl;
    cout << "area=" << rect.Area() << endl;

    return 0;
}
```





## #private 접근 지정자에 대한 추가 설명

### 접근 가능한 곳:

`클래스 내부:`

`모든 멤버 함수는 private으로 선언된 변수에 직접 접근할 수 있다. 이는 클래스 내부에서 해당 멤버가 필요한 모든 계산과 처리를 할 수 있도록 보장해주기 때문이다.`

```cpp
class Rectangle {
private:
    int width;
    int height;

public:
    void setWidth(int w) {
        width = w;  // private 멤버 변수에 접근 가능
    }

    void setHeight(int h) {
        height = h;  // private 멤버 변수에 접근 가능
    }

    int Area() {
        return width * height;  // private 멤버 변수에 접근 가능
    }
};

```

`여기서 setWidth(), setHeight(), Area()는 모두 private으로 선언된 width와 height에 직접 접근하여 값을 설정하고, 계산할 수 있다.`



### 접근 불가능한 곳:***

`클래스 외부(메인함수 포함, 단 함수구현부의 멤버함수에서는 private변수에 접근 가능하다.!):`



클래스 외부에서는 private 멤버에 직접 접근할 수 없다. `객체를 생성한 후에도 클래스 외부에서는 private으로 선언된 멤버 변수나 멤버 함수에 직접 접근할 수 없다.` 이 때문에 외부에서 접근할 수 있는 public 멤버 함수를 제공하여, `private멤버 변수에 간접적으로 접근하거나 값을 수정하도록 해야 한다`.

```cpp
int main() {
    Rectangle rect;
    rect.setWidth(5);  // public 멤버 함수를 통해서 private 멤버 변수에 접근 가능
    rect.setHeight(10);

    // 아래 코드는 에러가 발생함 (private 변수에 직접 접근하려고 하므로)
    // rect.width = 5;  // Error: width is private
    // rect.height = 10; // Error: height is private

    cout << "Area: " << rect.Area() << endl;  // public 함수로 값을 가져옴
}

```

위의 코드에서 `rect.width 또는 rect.height에 직접 접근하려고 하면 **컴파일 에러**가 발생`한다. 대신 `setWidth(), setHeight() 같은 public 멤버 함수를 사용하여 private 멤버 변수의 값을 설정할 수 있다.`





또 다른 예시이다.
```cpp
#include <iostream>
using namespace std;

class MyClass {
private:
    int privateVar;

public:
    MyClass(int value) : privateVar(value) {} // 생성자

    void setPrivateVar(int value);  // 멤버 함수 선언
    void displayPrivateVar() const; // 멤버 함수 선언
};

// 클래스 외부에서 멤버 함수 구현
void MyClass::setPrivateVar(int value) {
    privateVar = value; // private 멤버 변수에 접근 가능
}

void MyClass::displayPrivateVar() const {
    cout << "Private Variable: " << privateVar << endl; // private 멤버 변수에 접근 가능
}

int main() {
    MyClass obj(10);
    obj.displayPrivateVar(); // Private Variable: 10

    obj.setPrivateVar(20);
    obj.displayPrivateVar(); // Private Variable: 20

    return 0;
}
```