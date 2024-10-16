---
layout: single
title:  "#5 C++ 함수 템플릿"
categories: C++
tag: [C++]
toc: true
toc_label: "table of content" # toc 이름 설정
toc_icon: "bars" # 아이콘 설정
toc_sticky: true # 마우스 스크롤과 함께 내려갈 것인지 설정
typora-root-url: ../
---



# 1. 함수 중복의 약점 - 중복 함수의 `코드 중복`

상황: 두 함수는 매개변수만 다르고, 나머지 코드는 동일하다.

```cpp
/*함수템플릿*/
//두함수는 매개변수만 다르고, 나머지 코드는 모두 동일하다.
void myswap(int &a, int &b)
{
    int tmp;
    tmp =a;
    a=b;
    b=tmp;

}

void myswap(double &a, double &b)
{
    double tmp;
    tmp =a;
    a=b;
    b=tmp;

}

int main(void)
{
    int a=4, b=5;
    myswap(a,b);
    cout<<a<<'\t'<<b<<endl;

    double c=4.3, d=5.6;
    myswap(c,d);
    cout<<c<<'\t'<<d<<endl;

    return 0;
}
```







# 2. 해결 방법 : 템플릿

## 2.1 제너릭 또는 일반화

`함수나 클래스를 일반화`시키고, `매개변수 타입을 지정하여` 틀에서 찍어내듯이 함수나 클래스 코드를 생산.

## 2.2 템플릿

`함수나 클래스를 일반화하는 c++도구`

template 키워드로 함수나 클래스 선언 → 변수나 매개변수의 타입만 다르고, 코드 부분이 동일한 함수를 일반화시킨다.

⇒ 상황: `아래의 두함수는 매개변수만 다르고, 나머지 코드는 모두 동일하다.`

```cpp
void myswap(int &a, int &b)
{
    int tmp;
    tmp = a;
    a = b;
    b = tmp;
}

void myswap(double &a, double &b)
{
    double tmp;
    tmp = a;
    a = b;
    b = tmp;
}
```

## 2.3 템플릿 선언

- template : 템플릿을 선언하는 키워드

- class: 제너릭 타입을 선언하는 키워드

- T :  제너릭 타입 T를 선언

  ```cpp
  template <class T> 
  void myswap(T &a, T &b)
  {
      T tmp;
      tmp =a; 
      a=b;
      b=tmp;
  }
  ```

## 2.4 템플릿 사용

`→ 함수의 템플릿을 이용하여 중복된 함수를 해결한다.`

템플릿 함수가 자동으로 매개변수의 타입을 추론하여 적절한 타입에 대해 동작하게 된다.

```cpp
template <class T> 
void myswap(T &a, T &b)
{
    T tmp;
    tmp =a; 
    a=b;
    b=tmp;

}
```

### `a,b 둘다 int이므로 T대신 int가 대입된다.`

```cpp
int main(void)
{
    int a=4, b=5;
    myswap(a,b); //myswap(int &a, int &b)함수 호출. 
    cout<<a<<'\t'<<b<<endl;

```

### `a,b 둘다 double이므로 T대신 double이 대입된다.`

```cpp
    double c=4.3, d=5.6;
    myswap(c,d); //myswap(double &a, double &b)함수 호출
    cout<<c<<'\t'<<d<<endl;

    return 0;
}
```







# 실습3-3

배열과 크기를 매개변수로 받아 합을 구하여 리턴하는 제너릭 함수 add()를 작성하려고 한다.

빈칸을 채워 프로그램을 완성하시오.

### `중복된 함수로 코드를 짜보기`

함수 이름은 같지만 매개변수의 타입이나 개수가 다를 때, 동일한 이름의 여러 함수를 정의한 함수오버로딩이 발생.

```cpp
int add(int* x, int num)
{
    int sum = 0;
    for(int n=0; n<num; n++)
    {
        sum += x[n];
    }
    return sum;
}

double add(double*x, int num)
{
    double sum=0;
    for (int n =0; n<num; n++)
    {
        sum += x[n];
    }
    return sum;
}

int main() {

    int x[]= {1,2,3,4,5};
    double d[] = {1.2, 2.3, 3.4, 4.5, 5.6, 6.7};
    cout<< "sum of x[] is "<< add(x,5)<<endl;
    cout<< "sum of d[] is "<< add(d,6)<<endl;
}
```

### `템플릿 이용해서 다시 작성` (템플릿을 두개 사용해봄) **

```cpp
template <class T1, class T2> 
T1 add(T1* x, T2 num)
{
    
    T1 sum=0;

    for (int n=0; n<num;n++) {
        sum+= x[n];
    }
    return sum;
}

int main() {

    int x[]= {1,2,3,4,5};
    double d[] = {1.2, 2.3, 3.4, 4.5, 5.6, 6.7};
    float f[] = {1.1, 2.2, 3.3, 4.4, 5.5};
    cout<< "sum of x[] is "<< add(x,5)<<endl; 
    cout<< "sum of d[] is "<< add(d,6)<<endl;
    cout<< "sum of f[] is "<< add(f,5)<<endl;
}

```







## 3.1 템플릿 구체화 예시

암시적 인스턴스화

```cpp
cout << GetMax(10, 20) << endl; // T = int으로 올바르게 추론
char ch = GetMax('A', 'B'); // T = char으로 올바르게 추론
cout << GetMax(3.14, 10.5) << endl; // T = double으로 올바르게 추론
```

명시적 인스턴스화

```cpp
cout << GetMax<int>(5, 10.5) << endl; // T= int
```

GetMax<int>(5, 10.5)에서 템플릿 매개변수를 명시적으로 int로 지정했으므로, `두 번째 인수 10.5는 int로 변환되어 처리된다`. T는 int로 설정되며 함수가 생성된다.

## 3.2 구체화 오류

```cpp
cout << GetMax(5, 10.5) << endl; // 컴파일 에러
```

⇒ 첫 번째 인수는 `int`이고 두 번째 인수는 `double`이다. 이 경우 컴파일러는 `T를 추론할 수 없기 때문`에 **컴파일 오류**가 발생한다.

`해결책 : 다중으로 타입을 지정한다.`

```cpp
template <class T1, class T2> void GetMax(T1 &a, T2 &b)
```







# 4. 템플릿의 장점과 제너릭 프로그래밍

## 4.1 템플릿의 장단점

장점: 함수코드의 재사용

단점: 컴파일러에 따라 지원하지 않을 수 있다.

## 4.2 제네릭 프로그래밍

일반화 프로그래밍이라고도 부름.

제너릭 함수나 제네릭 클래스를 활용하는 프로그래밍 기법







# 5. (추가) 템플릿 함수보다 중복함수가 우선이다.

```cpp
template <class T>
void print(T array[], int n) {
    for (int i = 0; i < n; i++)
        cout << array[i] << '\t';
    cout << endl;
}

/*char 배열 전용*/
void print(char array[], int n) { 
    for (int i = 0; i < n; i++)
        cout << (int)array[i] << '\t'; // char를 int로 변환하여 출력
    cout << endl;
}
int main() {
    int x[] = {1, 2, 3, 4, 5};
    double d[] = {1.1, 2.2, 3.3, 4.4, 5.5};
    char c[] = {1, 2, 3, 4, 5};

    print(x, 5); // 템플릿 함수 호출
    print(d, 5); // 템플릿 함수 호출
    print(c, 5); // 중복된 char 배열 전용 함수 호출

    return 0;
}

```

print(c, 5); 에서 `템플릿함수보다 중복함수가 우선이므로,` 

`템플릿 함수가 아닌,` 

`void print(char array[], int n)함수가 실행된다.`

⇒ 출력결과

```cpp
1    2    3    4    5
1.1  2.2  3.3  4.4  5.5
1    2    3    4    5
```







# `실습 3-4 **`

배열의 원소를 반대 순서로 뒤집는 reverseArray()함수를 템플릿으로 작성하라.

reverseArray()의 첫번째 매개변수는 배열에 대한 포인터이며, 두번째 매개변수는 배열의 개수이다.

```cpp
int main() {
	int x[] = {1, 10, 100, 5, 4};
	reverseArray(x, 5);
	for(int i=0; i<5; i++)
	cout << x[i] << ' '; // 4 5 100 10 1 이 출력된다.
	cout << endl;
}
```

sol) 템플릿을 이용하여 작성해보겠다.

```cpp
template <class T1, class T2>
T1 reverseArray(T1* x, T2 N)
	for (T2 i = 0; i < N / 2; i++) {
        T1 temp = x[i];
        x[i] = x[N - 1 - i];
        x[N - 1 - i] = temp;
    }
}

```

