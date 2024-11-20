---
layout: single
title:  "#8 C++ 접근지정자 / const객체 / static 멤버"
categories: C++
tag: [C++]
toc: true
toc_label: "table of content" # toc 이름 설정
toc_icon: "bars" # 아이콘 설정
toc_sticky: true # 마우스 스크롤과 함께 내려갈 것인지 설정
typora-root-url: ../
---

# 1. 접근 지정자

캡슐화의 목적 : 객체 보호, 보안 

객체의 상태를 나타내는 멤버변수에 대한 보호.

중요한 멤버는 다른 클래스나 객체에서 접근할 수 없도록 보호

외부와의 인터페이스를 위해서 일부 멤버는 외부에 접근 허용



## 1.1 멤버에 대한 3가지 종류의 접근 지정자

### Private : 디폴트

- 동일한 `클래스의 멤버함수에만` 제한함
- 클래스 `밖에서 직접 접근 불가`

### protected

`클래스 자신`과 `상속받은 자식 클래스에만` 허용**

### public

- 모든 다른 클래스에 허용. `클래스 밖에서 직접 접근 가능`

`클래스안: 클래스 정의의 내부와 멤버함수 구현`

`클래스 밖: 전역함수 정의나 다른 클래스의 정의 , 클래스로 만들어진 객체`

| 접근 지정자   | 해당 클래스의 멤버 함수에서의 접근 | 파생 클래스의 멤버 함수에서의 접근 | 클래스 밖에서의 접근 |
| ------------- | ---------------------------------- | ---------------------------------- | -------------------- |
| **private**   | O (허용)                           | X (불허)                           | X (불허)             |
| **protected** | O (허용)                           | O (허용)                           | X (불허)             |
| **public**    | O (허용)                           | O (허용)                           | O (허용)             |

## 1.2 중복 접근 지정, 디폴트 접근 지정

### 1.2.1 접근 지정자와 중복 사용 가능

```cpp
class Sample {
private:
    int x, y;    // private 멤버 변수
public:
    Sample();    // public 생성자
private:
    bool checkXY(); // private 멤버 함수
};

```

### 1.2.2 디폴트 접근 지정자는 private

```cpp
class Circle {
    int radius;   // 디폴트 접근 지정자는 private
public:
    Circle();
    Circle(int r);
    double getArea();
};

```

### `1.2.3 멤버변수는 private 지정이 바람직하다.**`

```cpp
class Circle {
private:
    int radius;  // 멤버 변수가 private로 보호받고 있음
public:
    Circle();               // 기본 생성자
    Circle(int r);          // 매개변수가 있는 생성자
    double getArea();       // 원의 면적을 계산하는 함수
};

// 기본 생성자 정의
Circle::Circle() {
    radius = 1;
}

// 매개변수가 있는 생성자 정의
Circle::Circle(int r) {
    radius = r;
}

int main() {
    Circle waffle(5);        // 생성자에서 radius 설정
 //   ~~waffle.radius = 5;~~       // 오류: private 멤버에 직접 접근 불가
}

```

다음중 컴파일 오류가 발생하는 곳은?

```cpp
#include <iostream>
using namespace std;

class PrivateAccessError {
private:
    int a;
    void f();
    PrivateAccessError();  // private 생성자
public:
    int b;
    PrivateAccessError(int x);  // public 생성자
    void g();
};

// private 생성자의 정의
PrivateAccessError::PrivateAccessError() {
    a = 1;  // (1)
    b = 1;  // (2)
}

// public 생성자의 정의
PrivateAccessError::PrivateAccessError(int x) {
    a = x;  // (3)
    b = x;  // (4)
}

// private 멤버 함수 f()의 정의
void PrivateAccessError::f() {
    a = 5;  // (5)
    b = 5;  // (6)
}

// public 멤버 함수 g()의 정의
void PrivateAccessError::g() {
    a = 6;  // (7)
    b = 6;  // (8)
}

int main() {
    PrivateAccessError objA;       // (9) 오류: private 생성자 호출
    PrivateAccessError objB(100);  // (10) 정상: public 생성자 호출
    objB.a = 20;                   // (11) 오류: private 멤버에 접근
    objB.b = 20;                   // (12) 정상: public 멤버에 접근
    objB.f();                      // (13) 오류: private 멤버 함수 호출
    objB.g();                      // (14) 정상: public 멤버 함수 호출
}

```



만약에 a에 접근하고 싶다면, `a값을 설정하거나 가져오는 **public 멤버 함수**를 추가한다.`

```cpp
// Setter and Getter for 'a'
void setA(int value) {
    a = value;
}

int getA() const {
    return a;
}

```

그러면 다음과 같이 메인함수에서 접근이 가능하다.

```c++
int main() {
    PrivateAccessError objB(100);  // public 생성자 호출
    objB.setA(20);                // Setter를 통해 private 멤버 'a' 설정
    cout << "a: " << objB.getA() << endl;  // Getter를 통해 private 멤버 'a' 출력

    objB.b = 30;  // public 멤버에 직접 접근
    cout << "b: " << objB.b << endl;

    objB.g();  // public 멤버 함수 호출 (private 멤버에도 접근 가능)
}
```



# 2. C++ 구조체

- 상속, 멤버, 접근지정 등 모든 것이 클래스와 동일하다.
- 클래스와 유일하게 다른점

→ `구조체`의 디폴트 접근 지정: `public`

→ `클래스`의 디폴트 접근 지정: `private`

## 2.1 C++에서 구조체를 수용한 이유?

- C언어와의 호환성 때문
- C의 구조체 100% 호환 수용한다.

## 2.2 구조체 선언 예시

```cpp
struct StructName {
private:
    int privateMember;  // private 멤버

protected:
    int protectedMember;  // protected 멤버

public:
    int publicMember;  // public 멤버

    void setPrivate(int value) {
        privateMember = value;
    }
};

```

## 2.3 구조체 객체 생성

- struc 키워드 생략

c++에서는 구조체 객체를 클래스 객체처럼 생성할 수 있다. 단, C++에서는 struct 키워드 없이도 객체를 생성할 수 있지만, C언어에서는 반드시 struct 키워드를 사용해야한다.

```cpp
StructName obj;  // C++에서는 struct 키워드 없이 객체 생성 가능
obj.publicMember = 10;  // public 멤버에 접근 가능

// C 스타일 구조체 객체 생성 (C 언어에서 사용하는 방식)
struct StructName obj2;  // struct 키워드 필요
```







# 3. 접근자 함수

public접근 권한을 갖는 멤버함수로, 

`클래스의 멤버 변수에 대한 접근을 도와주는 기능 제공.`

접근자 함수는 보통 get함수와 set함수로 구현된다.

## 3.1 Get 함수

get 함수는 `멤버 변수의 값을 반환해주는 역할`을 한다.

```cpp
int getRadius() {
    return radius;
}
```

위 코드에서 getRadius()는 private 멤버 변수 radius의 값을 반환해주므로, 외부에서 객체의 반지름 값을 읽을 수 있게 한다.

## 3.2 Set 함수

set 함수는 멤버 변수의 값을 변경해주는 역할을 한다.

```cpp
bool setRadius(int r) {
    if (r > 30) return false;  // 30보다 큰 값은 설정 불가
    radius = r;
    return true;
```

## 3.3 예시

```cpp
#include <iostream>
using namespace std;

class Circle {
private:
    int radius;  // private 멤버 변수

public:
    // 기본 생성자
    Circle() {
        radius = 0;
    }

    // 생성자 오버로딩
    Circle(int r) {
        setRadius(r);
    }

    // 면적 계산 함수
    double getArea() {
        return 3.14 * radius * radius;
    }

    // 접근자 함수: 반지름 값을 가져오는 함수 (getter함수)***
    int getRadius() {
        return radius;
    }

    // 접근자 함수: 반지름 값을 설정하는 함수 (setter함수)***
    //반지름이 30이하인 수치만 세팅해주는 함수
    bool setRadius(int r) {
        if (r > 30) {
            return false;  // 반지름이 30을 초과하면 false 반환
        }
        radius = r;
        return true;  // 정상적으로 설정되면 true 반환
    }
};

int main() {
    // Circle 객체 생성, 반지름을 5로 설정
    Circle waffle(5);

	// waffle.radius =5; //private 멤버 접근 불가!!! -> 그러므로 setter함수를 만들어놓은것이다.
    // setter 함수로 반지름 설정
    waffle.setRadius(5);

		
	// int radius = waffle.radius; //priavate 멤버 접근 불가!! -> 그러므로 getter함수를 만들어놓은것이다.
    // getter 함수로 반지름 값 출력
    int radius = waffle.getRadius();
    cout << "Circle's radius: " << radius << endl;

    return 0;
}

```

여기서 getRadius 대신 zetRadius를 써도 아무런 문제 없다.

함수 이름을 `get`과 `set`으로 정형화해서 사용하는 이유는 **`코드의 가독성**과 **유지보수**` 때문이다.
`getRadius()`와 같은 함수 이름을 사용하면, 누구라도 해당 함수가 멤버 변수 `radius를 "가져오는"` 함수임을 직관적으로 알 수 있다.





## 3.4 유효성 검사기능 (접근자 함수의 장점)

```cpp
bool Circle::setRadius(int r) {
    if (r > 30) return false;  // 30을 초과하면 false 반환 (유효하지 않은 값)
    radius = r;  // 30 이하일 때만 값을 설정
    return true;  // 설정 성공 시 true 반환
}
```







# 4. const객체 /  const 멤버함수

## 4.1 const 객체

### 4.1.1 멤버 변수의 값을 변경할 수 없음.

const의 객체에는 값을 대입할 수 없음

```cpp
const Circle pizza2; // cont를 이용해서 객체를 초기화
pizza2 = pizza1; // const 객체에는 대입할 수 없으므로 컴파일 에러
```

### 4.1.2 const객체의 사용

객체를 함수의 입력인자로 전달할때 주로 사용한다.

const로 선언된 참조는 함수내부에서 객체의 원본데이터를 수정할수 없도록 해준다.

`이함수에 전달된 객체는 읽기 전용으로 취급된다.`

이렇게 하면 원본 데이터를 실수로 수정하지 않도록 보장할 수 있다.

```cpp
//void ShowData(Circle& s); 이게 아닌 const를 사용해보자.

void ShowData(const Circle& s) {
    // s.setRadius(10);  // 컴파일 에러: const 객체인 s의 값을 수정할 수 없음
    cout << "Radius: " << s.getRadius() << endl;

```

## 4.2 const 멤버 함수

객체의 값을 변경하지 않기로 약속하는 멤버 함수

`“이 함수에서는 멤버변수의 값을 변경하지 않으므로, cont 객체가 이 멤버함수를 호출해도 안전하다”는 의미.`

```cpp
class 클래스이름 {
	멤버 함수 선언 const;
};
리턴형 클래스이름::멤버함수이름(인자리스트) const
{
}
```



## 4.3 예시코드

```cpp
#include <iostream>
using namespace std;

class Circle {
private:
    int radius;

public:
    // 생성자
    Circle(int r) : radius(r) {}

    // const 멤버 함수: 반지름 값을 읽기만 하고 변경하지 않음
    int getRadius() const {
        return radius;
    }

    // 비-const 멤버 함수: 반지름 값을 설정할 수 있음
    void setRadius(int r) {
        radius = r;
    }

    // const 멤버 함수: 원의 면적을 계산하는 함수 (상태를 변경하지 않음)
    double getArea() const {
        return 3.14 * radius * radius;
    }
};

int main() {
    const Circle circle1(5);  // const 객체 생성

    // const 객체에서 const 멤버 함수만 호출 가능
    cout << "Radius: " << circle1.getRadius() << endl;
    cout << "Area: " << circle1.getArea() << endl;

    // circle1.setRadius(10);  // 오류 발생: const 객체에서 비-const 함수 호출 불가

    Circle circle2(10);  // 비-const 객체 생성
    circle2.setRadius(15);  // 비-const 객체는 set 함수 호출 가능
    cout << "Updated Radius: " << circle2.getRadius() << endl;

    return 0;
}

```





# 5. static 멤버

## 5.1 static이란?

- 변수와 함수에 대한 기억 부류의 한종류
- 생명주기 - 프로그램이 시작될때 생성, 프로그램 종류시 소멸
- 사용범위 - 선언된 범위, 접근 지정에 따름

### 5.1.1 static 멤버

- 프로그램이 시작할때 생성
- 클래스 당 하나만 생성, 클래스 멤버라고 불림’
- 클래스의 모든 객체들이 공유하는 멤버

### 5.1.2 non-static 멤버

- 객체가 생성될때 함께 생성
- 객체마다 객체 내에 생성
- 인스턴스 멤버로 불린다.

## 5.2 static 멤버 선언

```cpp
class Person
public:
	double money; // 개인 소유의 돈
	void addMoney(int money) {
		this -> money += money;
		}/*지금까지가 non-static 멤버 선언*/
		

	/*static 멤버 변수 선언 */
	static int sharedMoney; //공금
	
	/*static 멤버 함수 선언 */
	static void addShared(int n) {
		sharedMoney += n;
	}
};

int Person::sharedMoney =10; //sharedMoney를 10으로 초기
```

static 멤버 변수⇒ `static 멤버변수는 클래스 내부에 선언되지만, 모든 객체가 공유하는 특성을 가지고 있다.`

- 중요한 점은, 해당 클래스의 맥락에서만 접근할수있다는 것이다. 즉, `클래스 외부에서는 클래스 이름을 통해서만 접근할 수 있다.`
- 전체 프로그램 내에 한번만 생성된다.

## 5.3 static 멤버와 non-static멤버의 비교

### 5.3.1 static 멤버는 하나만 생성되고 모든 객체들에 의해 공유됨

```cpp
static int sharedMoney; //static 멤버 변수 선언
```

**(아래의 예시코드에서 static 멤버)**:

- 공금(`sharedMoney`)은 `Person` 클래스에 속한 **static 멤버 변수**로, 모든 객체가 **공유**한다.
- `Person::addShared(500)`을 호출하여 공금을 500원 추가하면, `han`, `lee`, `park` 모두 공통된 `sharedMoney` 값이 510으로 증가한다.
- 이 공금은 **모든 객체가 공유**하기 때문에, `Person` 클래스를 통해 접근해야 하며, 각 객체가 아닌 **클래스 단위에서 관리**된다.

```cpp
#include <iostream>
using namespace std;

class Person {
public:
    double money;  // 개인 소유의 돈

    // 개인 돈을 추가하는 함수 (non-static 멤버 함수)
    void addMoney(int money) {
        this->money += money;
    }

    // static 멤버 변수 선언 (모든 사람이 공유하는 공금)
    static int sharedMoney;

    // static 멤버 함수 선언 (공금을 추가하는 함수)
    static void addShared(int n) {
        sharedMoney += n;
    }
};

// static 멤버 변수의 외부 정의 및 초기화
int Person::sharedMoney = 10;  // 초기 공금을 10으로 설정

int main() {
    // 3명의 Person 객체 생성
    Person han, lee, park;

    // 각 사람의 개인 돈 설정
    han.money = 100;
    lee.money = 200;
    park.money = 300;

    // 개인 돈 추가 (non-static 함수 사용)
    han.addMoney(50);  // han은 50원을 추가
    lee.addMoney(100); // lee는 100원을 추가
    park.addMoney(150); // park은 150원을 추가

    // 공금(sharedMoney) 추가 (static 함수 사용)
    Person::addShared(500);  // 모든 사람이 공유하는 공금에 500원을 추가

    // 결과 출력
    cout << "Han's money: " << han.money << endl;
    cout << "Lee's money: " << lee.money << endl;
    cout << "Park's money: " << park.money << endl;
    cout << "Shared money (공금): " << Person::sharedMoney << endl;

    return 0;
}

```

## 5.4 객체의 멤버로 접근

### 5.4.1 static 멤버 변수에 접근

1. 객체를 통해 접근: `han.sharedMoney`로도 **static 변수**에 접근할 수 있다. 이 경우 `han` 객체가 `sharedMoney`에 접근하는 것처럼 보이지만, 사실 **모든 객체가 공유**하는 변수를 수정하는 것이다.

2. 클래스 이름을 통해 접근: `Person::sharedMoney`로 **클래스 이름**을 통해 직접 static 멤버 변수에 접근할 수 있다. `객체를 생성하지 않고도 값을 변경할 수 있다.`

   `(static멤버는 클래스마다 오직 한개만 생성되기 때문이다.)`

   ```cpp
   han.sharedMoney = 200; <=> Person::sharedMoney = 200;  //클래스명으로도 접근가능
   lee.addShared(200); <=> Person::addShared(200); //클래스명으로도 접근가능
   ```

### 5.4.2 static 멤버 함수에 접근

```cpp
int main() {
    // 공금 추가: static 멤버 함수 호출
    Person::addShared(100);  // 클래스 이름으로 호출 (방법 1)
    cout << "Shared Money after addShared: " << Person::sharedMoney << endl;

    
    // 객체를 통해서도 static 멤버 함수 호출 가능
    Person han;
    han.addShared(50);  // 객체를 통해 호출 (방법 2)
    cout << "Shared Money after han adds: " << Person::sharedMoney << endl;

    return 0;
}


```

1. 객체를 통해 호출: `han.addShared(50)`처럼 **객체를 통해서도** static 함수를 호출할 수 있다. 하지만 여전히 함수는 **클래스에 속한 공통 자원**인 `sharedMoney`에만 작동한다.
2. 클래스 이름으로 호출: `Person::addShared(100)`은 **클래스 이름**으로 static 함수를 호출하는 방식이다. 객체를 생성하지 않고도 공금을 추가할 수 있다.

### 5.4.3 객체 포인터로 static멤버에 접근*

객체.static멤버

객체포인터 → static멤버

예시)

```cpp
Person lee;
lee.sharedMoney = 500; // 객체.static멤버 방식

Person *p;
p = &lee;
p→addShared(200); // 객체포인터→ static멤버 방식
```

## 5.5 static 활용정리

- 전역변수나 전역 함수를 클래스에 캡슐화

- 전역변수나 전역함수를 static으로 선언하여 클래스 멤버로 선언한다.

- 객체 사이에 공유변수를 만들고자 할때 → static 멤버를 선언하여 모든 객체들이 공유



## 5.6 static 멤버함수는 static 멤버만 접근 가능

### 5.6.1 static 멤버함수가 접근할 수 있는 것

- `static 멤버 함수`
- `static 멤버 변수`
- `함수내의 지역 변수`

`static멤버함수는 non-static멤버에 접근 불가하다`. ( 객체가 생성되지 않은 시점에서 static멤버함수가 호출될수 있기 때문이다.

```cpp
class PersonError {
	int money;
public:
	static int getMoney() { return money; } //오류 코드. static멤버함수는 nonstactic 멤버 에 접근불가
	
	void setMoney(int money) { // 정상 코드
		this->money = money; //this포인터는 호출된 객체를 가리킨다.
        // this를 사용하여 멤버 변수에 매개변수 값을 대입
}
};

int main(){
	int n = PersonError::getMoney();  // errorKim 객체의 money 값을 가져옴
	PersonError errorKim;
	errorKim.setMoney(100);  // errorKim 객체의 money를 100으로 설정
}
```

main()이 시작하기전 money는 아직 생성되지 않았다.

int n = PersonError::getMoney(); ⇒ `생성되지 않은 money라는 변수를 접근하게 되는 오류`

PersonError errorKim;
⇒ `money는  errorKim이라는 객체가 생길때 비로소 생성된다.`



### 5.6.2 non-static 멤버함수는 static에 접근 가능하다.

```cpp
class Person {
	public: double money; // 개인 소유의 돈
	static int sharedMoney; // 공금. (static 멤버 변수이다.)
....
	int total() { // non-static 함수는 non-static이나 static 멤버에 모두 접근 가능
		return money + sharedMoney;
	}
}; 
```

## 5.7 this

### 5.7.1 this는 왜 필요한가
- **멤버 변수와 매개변수 이름 충돌 방지**: `멤버 변수와 매개변수 이름이 동일할 때`, 멤버 변수를 명확히 구분하기 위해 필요하다.
- **객체의 정확한 참조**: 멤버 함수가 호출된 객체를 가리켜 해당 객체의 상태를 조작하거나 확인할 수 있게 한다.

---

### 5.7.2 this의 역할 요약
- **현재 객체의 주소를 가리키는 포인터**로, 클래스의 멤버 함수 내에서 사용된다.

- 멤버 변수와 매개변수 이름이 동일할 때, **멤버 변수를 참조**하도록 돕는다.

  ```c++
  void setMoney(int money) {
      money = money;  // 논리적 오류: 매개변수에 자기 자신을 대입함
  }
  
  
  //해결
  void setMoney(int money) {
      this->money = money;  // this를 사용하여 멤버 변수에 값을 대입
  }
  
  ```

  

- `static` 함수에서는 객체와 독립적으로 동작하므로 **`this`가 존재하지 않는다**.

---

### 5.7.3 this의 생명 주기
- **멤버 함수 호출 시 생성**: 함수가 실행되면 컴파일러가 호출된 객체의 주소를 `this` 포인터에 자동으로 전달한다.
- **함수 실행 동안 존재**: 함수가 실행되는 동안만 유효하며, 실행이 끝나면 자동으로 소멸한다.
- **개발자가 명시적으로 생성하거나 소멸시키지 않아도 된다**.



# 실습 6-1

컴퓨터의 주기억장치를 모델링하는 클래스 Ram을 구현하려고 한다. 

Ram클래스는 데이터가 기록될 메모리 공간과 크기 정보를 가지고, 주어진 주
소에 데이터를 기록하고(write), 주어진 주소로부터 데이터를 읽어온다
(read). Ram 클래스는 다음과 같이 선언된다.

```cpp
class Ram {
	char mem[100*1024]; // 100KB 메모리
	int size;
public:
	Ram(); // mem을 0으로 초기화하고 size를 100*1024로 초기화
	~Ram(); // "메모리 제거됨" 문자열 출력
	char read(int address); // address 주소의 메모리를 읽어 리턴
	void write(int address, char value); // address 주소에 value 저장
// 주소가 범위를 벗어나는 오류 발생하면 에러메시지 출력함.
};
```

다음 main() 함수는 100번지에 20을 저장하고, 101번지에 30을 저장한
후, 100번지와 101번지의 값을 읽고 더하여 102번지에 저장하는 코드이
다.

```cpp
int main() {
	Ram ram;
	ram.write(100, 20); // 100 번지에 20 저장
	ram.write(101, 30); // 101 번지에 30 저장
	char res = ram.read(100) + ram.read(101); // 20 + 30 = 50
	ram.write(102, res); // 102 번지에 50 저장
	cout << "102 번지의 값 = " << (int)ram.read(102) << endl; // 102 번지 메모리 값 출력
}
```

실행결과를 참고하여 Ram.h, Ram.cpp, mail.cpp로 헤더파일과 cpp파일
을 분리하여 프로그램을 완성하라.

풀이

```cpp
/*메모리 읽기 쓰기*/
class Ram{
    char mem[100 * 1024]; // 100KB 메모리
    int size;

public:
    Ram();  // mem을 0으로 초기화하고 size를 100 * 1024로 초기화
    ~Ram(); // 메모리 해제 시 문자열 출력
    char read(int address); // address에 해당하는 메모리의 값을 반환
    void write(int address, char value); // address에 해당하는 메모리에 value를 씀
};

/*클래스 구현부*/
Ram::Ram() {
    for(int i = 0; i < 100 * 1024; i++) {
        mem[i] = 0;
    }
    size = 100 * 1024;  // 클래스의 멤버 변수 size를 초기화
}

Ram::~Ram() {
    cout << "Memory released" << endl;
}

char Ram::read(int address) {
    if (address < 0 || address >= size) {
        cout << "Error: Address out of bounds" << endl;
        return 0;  // 잘못된 주소에 접근하면 0을 반환
    }
    return mem[address];
}

void Ram::write(int address, char value) {
    if (address < 0 || address >= size) {
        cout << "Error: Address out of bounds" << endl;
        return;  // 잘못된 주소에 접근하려고 할 경우 처리
    }
    mem[address] = value;
}

int main() {
    Ram ram;

    // 메모리 쓰기 예시
    ram.write(1024, 'A'); // 1024번지에 'A' 저장
    ram.write(100 * 1024, 'B'); // 잘못된 주소, 오류 메시지 출력

    // 메모리 읽기 예시
    cout << "Value at 1024: " << ram.read(1024) << endl;
    cout << "Value at 100*1024: " << ram.read(100 * 1024) << endl; // 잘못된 주소, 오류 메시지 출력

    return 0;
}
```

# 실습 6-2

전역함수들을 가진 좋지 않은 코딩 사례

```cpp
#include <iostream>
using namespace std;

int abs(int a) { return a>0?a:-a; }
int max(int a, int b) { return (a>b)?a:b; }
int min(int a, int b) { return (a>b)?b:a; }

int main() {
	cout << abs(-5) << endl;
	cout << max(10, 8) << endl;
	cout << min(-3, -8) << endl;
}
```

위쪽 코드를 static 멤버를 가진 Math클래스로 작성하고 멤버함수를 호출하라

```cpp
class MyMath {
    public:
    static int abs(int a) { return a > 0 ? a : -a; }
    static int max(int a, int b) { return a > b ? a : b; }
    static int min(int a, int b) { return a < b ? a : b; }
};

int main() {
    // 객체를 안 만들고 static 함수를 호출
    cout << MyMath::abs(-5) << endl;
    cout << MyMath::max(10, 8) << endl;
    cout << MyMath::min(-3, -8) << endl;

    return 0;
}
```

# 실습 6-3 **

```cpp
클래스 : Person
• 멤버변수 : int money; string name;
• 정적멤버변수 : int sharedmoney (0으로 초기화)
• Default 생성자 : money = 0으로 초기화
• 생성자 : Person(string name_in) : money = 0; name = name_in으로 초기화
• 파괴자 : name과 money 를 출력
• addMoney(int money_in) : money_in을 money에 누적
• 정적변수 : sharedMoney
• 정적멤버함수 : addShared(int sharedmoney_in) :
sharedmoney_in를 sharedMoney에 누적
```

```cpp
int main() {
	Person A("KANG"), B("KIM");
	// 3월
	A.addMoney(100);
	A.addShared(5);
	B.addMoney(200);
	B.addShared(5);
	// 4월
	A.addMoney(100);
	A.addShared(5);
	B.addMoney(200);
	B.addShared(5);
	cout << "공금 = " << Person::sharedMoney << endl;
	Person::addShared(100);
	cout << "공금 = " << Person::sharedMoney << endl;
}
```

위에 설명된 클래스 Person을 참고하여 필요한 코드를 작성하시오.

풀이

```cpp
class Person2 {
    public:
    int money;
    string name;

    Person2() {money =0;}
    Person2(string name_in) {
        money =0;
        name = name_in;
    }
    ~Person2(){
        cout<< name <<"의 money는"<<money<<endl;

    }

    void addMoney(int money_in){
        money+= money_in;
    }

    static int sharedMoney;
    static int addShared(int money_in){
        sharedMoney +=money_in;
    }

};

int Person2::sharedMoney = 0;

int main() {
    Person2 A("KANG"), B("KIM");
    //3월
    A.addMoney(100);
    A.addShared(5);
    B.addMoney(200);
    B.addShared(5);

    //4월
    A.addMoney(100);
    A.addShared(5);
    B.addMoney(200);
    B.addShared(5);
    cout<<"공금" <<Person2::sharedMoney<<endl;
    cout<<"공금"<<Person2::sharedMoney<<endl;

}

```