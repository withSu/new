---
layout: single
title:  "#3 C++ 레퍼런스, 함수의 인자전달 개념 및 실습"
categories: C++
tag: [C++]
toc: true
toc_label: "table of content" # toc 이름 설정
toc_icon: "bars" # 아이콘 설정
toc_sticky: true # 마우스 스크롤과 함께 내려갈 것인지 설정
typora-root-url: ../

---



# 1. 레퍼런스의 선언 및 사용

## 1.1 레퍼런스 선언 방식

참조자 &도입

```
이미 존재하는 변수에 대한 다른 이름을 선언한다.
참조 변수는 이름만 생기며, 참조변수에 새로운 공간을 할당하지는 않는다.
```

### `1.1.1 레퍼런스변수는 참조할 변수의 별명이다.`

- ref라는 레퍼런스 변수가 참조할 변수(target)
- 즉, ref는 target의 별명

```cpp
int target =20;
int& ref = target; 
ref =3;
#include <iostream>
using namespace std;

int main(void)
{
    int i =1;
    int n =2;
    int &refn = n; //참조변수 refn 선언. refn은 n의 별명이다.

    n=4;
    refn++; refn=5, n=5;
    cout<<i<<" "<<n<<" "<<refn<<endl; //1 5 5

    refn =i; //refn은 n의 별명이므로 n에 i값을 대입한다.
    refn++; //refn은 n의 별명이므로 n값이 1증가한다.
    cout<<i<<" "<<n<<" "<<refn<<endl; //1 2 2

    int *p=&refn; //p는 refn의 주소값을 저장한다.
    *p=20; //p가 가리키는 곳에 20을 저장한다. 
    //즉, refn에 20을 저장한다.
    //즉, n에 20을 저장한다.

    cout<<i<<" "<<n<<" "<<refn<<endl; //1 20 20
}
void pfunc_2_(int& c, int& d)
{
    cout <<c<<" "<<d <<endl;
    c=30; d=40;
}

int main(void)
{
    int a =10, b=20;
    pfunc_2_(a,b);
    cout <<a<<" "<<b<<endl; 
    return 0;
}
```

- int& c =a; //c는 a의 별명이다.

```cpp
int ex0919_5()
{
    int a =10, b=20;
    int& c =a; //c는 a의 별명이다.
    c=b; //c는 b의 별명이다.
    cout <<c<<endl;  //b의 값인 20이 출력된다.

    int annnnnnnnnn =10, d=20;
    int& e =annnnnnnnnn;
    cout<<e<<endl; //10

    int f[5] = {1,2,3,4,5};

    int& g=f[3];
    cout<<g<<endl; //4

    return 0;
}
```

## 1.2 const레퍼런스

레퍼런스를 선언할때 const키워드를 함께 사용하면, `레퍼런스가 참조하는 변수의 값을 변경할 수 없다`.

```
레퍼런스는 자신이 참조하는 변수에 읽기 전용의 접근만 가능하다.
```

- const의 레퍼런스는 변경할수 없으므로 컴파일 에러

```cpp
int data =100;
const int &ref = data;

cout<<ref; //data의 값이 출력된다.
ref=200; //const의 레퍼런스는 변경할수 없으므로 컴파일 에러 ***
data=200; //data를 직접 변경하는 것은 가능하다.***
```

## 1.3 const레퍼런스의 전달

함수의 인자로 사용할때의 const레퍼런스

구조체나 클래스형을 넘길때 유용하게 사용된다.

```
구조체를 인자로 전달할때 레퍼런스로 전달하면, 구조체를 복사하지 않고 별명으로만 전달한다.
void Print(const STUDENT &s) //s는 s1의 별명이다.
{
	s.grade[0] =0; //함수안에서 s를 변경할 수 없으므로 컴파일 에러
}

int main()
{
	Student s1 = {"kbs", 1, 2, 3, 4, 5};
	Print(s1); //레퍼런스에 의한 전달이므로 s는 s1의 별명이다.		
}
```







# 2. 함수의 인자 전달 비교

함수의 인자 전달 방법을 결정하는 기준



| `값에 의한 전달(call by value)`                              | `주소에 의한 전달(Call by Address)`                          | `레퍼런스(참조)에 의한 전달(call by reference)`              |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 함수를 호출할때 넘겨준 인자를 함수안에서 사용만 하고 변경하지 않을때 | 함수를 호출할때 넘져준 인자를 함수안에서 변경해야할때        | 함수를 호출할때 넘져준 인자를 함수안에서 변경해야할때        |
|                                                              | 함수에 포인터로 변수를 전달하고, 메모리 주소를 참조하여 값을 변경한다. 포인터 연산이 필요하다. | 함수에 참조자를 사용해 변수를 전달하고, 마치 변수 자체를 함수에서 직접 사용하는 것처럼 값을 변경한다. 포인터 없이 간결하게 처리된다. |



## 2.1 레퍼런스(참조)에 의한 호출

```
레퍼런스(참조) 변수를 가장 많이 활용하는 사례 : call by reference
```

- `함수의 매개변수를 레퍼런스 타입으로 선언`
- 참조 매개변수라고 부른다. → 참조 매개변수의 이름만 생기고 공간이 생기지 않는다.

```cpp
#include <iostream>
using namespace std;
void foo(int &ref){
    ref++; //ref는 x의 별명이므로 main함수의 x를 증가시킨다.
}

int main(void)
{
    int x=10;

    foo(x);
    cout<<"x="<<x<<endl;

}
```





# 3. 함수의 인자 전달 방법 정리**

## 3.1 값에 의한 전달 (Call by Value)

```cpp
#include <iostream>
using namespace std;

void swap(int a, int b) {
    int tmp;
    tmp = a;
    a = b;
    b = tmp;
}

int main() {
    int m = 2, n = 9;
    swap(m, n);
    cout << m << " " << n;
}
```

## 3.2  주소에 의한 전달( Call by Address)

- 함수의 매개변수는 포인터 타입으로 정의한다.
- 인자를 넘겨줄때는 결과 값을 담고 싶은 `변수의 주소를 넘겨준다.`
- 함수안에서 `결과를 넘겨줄때는 매개변수가 가리키는 곳에 값을 넣어준다.`

```cpp
#include <iostream>
using namespace std;

void swap(int *a, int *b) {
    int tmp;
    tmp = *a;
    *a = *b;
    *b = tmp; //*b는 n을 가리키고 있으므로 n의 값이 바뀐다.
}

int main() {
    int m = 2, n = 9;
    swap(&m, &n);
    cout << m << " " << n;
}
```

## 3.3 참조에 의한 전달 (Call by Reference)

- 함수의 매개변수는 레퍼런스 타입으로 정의한다.
- 인자를 넘겨줄때는 결과값을 담고 싶은 변수를 그대로 넘겨준다.
- 함수안에서 결과를 넘겨줄때는 매개변수에 값을 넣어준다.

```cpp
#include <iostream>
using namespace std;

void swap(int &a, int &b) {
    int tmp;
    tmp = a;
    a = b;
    b = tmp; //b는 n의 별명이므로 n의 값이 바뀐다.
}

int main() {
    int m = 2, n = 9;
    swap(m, n);
    cout << m << " " << n;
}
```





# 4. 함수의 인자 전달 방법 - 1차원 배열, 2차원 배열

## 4.1  1차원 배열을 인자로 전달하는 방법의 정리

- 배열 전체를 한번에 보낼 수 없으므로 `배열의 시작주소를 보낸다.`
- 참조에 의한 호출을 사용하여 전달한다.

```cpp
void UsingArray(int arr[]) { // 또는 int arr[3], int *arr
    ...
    cout << arr[0] << *(arr+1) << "\\n";
}

int main() {
    int arr1[3] = {1, 2, 3};

    UsingArray(arr1);
    UsingArray(arr1 + 1);
    UsingArray(&arr1[2]);
    ...
}
```

- 배열의 주소를 넘겨준다.
- 인자로 넘어온 배열을 사용할때는 그냥 평범한 배열을 사용하듯이 하면 된다.

## 4.2  2차원 배열을 인자로 전달하는 방법의 정리

1차원 배열의 경우와 같이 배열의 주소를 전달한다.

다만, 호출된 함수에서는 몇개의 원소가 있는지 알 수 있도록 한다.

(1차원 원소의 개수는 절대로 생략하지 않는다.)

```cpp
void Using2DArray(int arr[][3]) { // 또는 int arr[5][3]
    ...
}

int main() {
    int array2[5][3] = {
        {1, 2, 3},
        {4, 5, 6},
        {7, 8, 9},
        {10, 11, 12},
        {13, 14, 15}
    };

    Using2DArray(array2);

    ...
}
```





# 5. 참조 리턴

## 5.1 C 언어에서의 함수 리턴 (값 리턴)

```cpp
#include <stdio.h>

int add(int a, int b) {
    return a + b; // 값을 리턴
}

int main() {
    int result = add(3, 4);
    printf("Result: %d\\n", result); // 출력: Result: 7
    return 0;
}
```

이 C 언어 예시에서, 함수 add는 두 개의 정수를 받아 그들의 합을 값으로 리턴한다. 함수가 리턴한 값은 result 변수에 저장된다.

## 5.2 C++에서의 함수 리턴 (참조리턴)

```cpp
#include <iostream>
using namespace std;

int& max(int &a, int &b) {
    return (a > b) ? a : b; // 참조를 리턴
}

int main() {
    int x = 5, y = 10;

    max(x, y) = 20; // 리턴된 참조로 값을 직접 변경
    cout << "x: " << x << ", y: " << y << endl; // 출력: x: 5, y: 20
    return 0;
}
```

이 C++ 예시에서는 max 함수가 두 정수를 받아 더 큰 값을 참조로 리턴한다.

중요한 점은 max(x, y)의 리턴값을 사용하여 변수를 직접 수정할 수 있다는 점이다.

max(x, y) = 20;라는 코드에서, 함수가 리턴한 참조값은 y를 가리키고, 이를 통해 y의 값을 20으로 변경할 수 있다.



## 5.3 값을 리턴하는 함수와 참조를 리턴하는 함수의 차이 비교 **

### 5.3.1 값(char)을 리턴하는 get() 함수

```cpp
char c = 'a';

char get() {
    return c;  // 변수 c의 값을 복사해서 리턴
}

char a = get();  // a는 'a'가 된다.
get() = 'b';  // 컴파일 오류 발생
```

- get() 함수는 c 변수의 값을 리턴한다. 즉, get()은 c의 값을 복사해서 반환한다.
- 하지만 get() 함수는 값을 리턴하기 때문에 `get() = 'b';와 같이 리턴된 값을 직접 변경하려고 하면 오류가 발생한다.` 왜냐하면 리턴된 값은 복사본이기 때문에, 원래 변수인 c의 값에 영향을 주지 못한다.

### 5.3.2 참조를 리턴하는 `find()` 함수

이 부분은 함수가 **참조**를 리턴하는 경우이다.

```cpp
char c = 'a';

char& find() {
    return c;  // 변수 c에 대한 참조를 리턴
}

char &ref = find();  // ref는 c를 참조하게 됨
ref = 'M';  // ref를 통해 c의 값을 변경, c는 'M'이 됨

find() = 'b';  // find()가 리턴한 참조를 통해 c의 값을 변경, c는 'b'가 됨
```

- find() 함수는 c 변수에 대한 참조를 리턴한다. 즉, find() 함수는 변수 c를 가리키는 별명처럼 작동한다.
- ref = find();는 ref가 c를 참조하게 만든다. 따라서 ref = 'M';으로 ref를 수정하면, 실제로는 c의 값이 M으로 바뀐다.
- `또한 find() 함수가 리턴하는 것은 c의 참조이므로, find() = 'b';로도 c의 값을 바꿀 수 있다. 이는 참조가 리턴되었기 때문에 가능한 것이다.`



## 5.4 간단한 참조 리턴 사례

```cpp
#include <iostream>
using namespace std;

char& find(char s[], int index) {
    return s[index];  // 배열 s의 index 위치에 있는 값을 참조로 리턴
}

int main() {
    char name[] = "Mike";
    cout << name << endl;  // 출력: Mike

    find(name, 0) = 'S';  // find 함수가 리턴한 참조를 통해 name[0]을 'S'로 변경
    cout << name << endl;  // 출력: Sike

    char& ref = find(name, 2);  // name[2]의 참조를 ref에 저장
    ref = 't';  // 참조를 통해 name[2]의 값을 't'로 변경
    cout << name << endl;  // 출력: Site
}
```

코드 동작 설명

1. 초기 상태: name[] 배열은 "Mike"로 초기화되어 있다. 첫 번째 출력에서 Mike가 출력된다.
2. 참조 리턴 후 값 변경: find(name, 0) = 'S'; 코드에서 find 함수는 name[0]을 참조로 리턴하고, 그 값을 'S'로 바꾼다. 따라서 name 배열의 첫 번째 문자가 M에서 S로 변경되면서 두 번째 출력은 Sike가 된다.
3. 참조를 이용한 값 변경: 이후, char& ref = find(name, 2); 코드는 name[2]를 참조하는 ref를 만든다. ref = 't';를 통해 name[2]의 값이 k에서 t로 변경된다. 따라서 마지막 출력은 Site가 된다.





# 실습2-4

`레퍼런스(참조) 매개변수를 통해 평균을 리턴`하고, `리턴문을 통해서는 함수의 성공여부를 리턴`하도록 `average()함수를 작성`하려고 한다. bool함수를 완성하여라.

```cpp
//bool average(int a[], int size, float& avg)와 같다.
bool average(int* a, int size, float& avg){
    avg =0.0;
    for(int i=0; i<size; i++){
        avg+=a[i];
    }
    avg=avg/size;
    return true;

}

int main(void)
{
    int x[]={0,1,2,3,4,5};
    float avg;
    average(x,6,avg);
    cout<<"average is "<<avg<<endl;
    return 0;
}
```

![image-20241007030744989](/images/2024-10-07-cpp-reference/image-20241007030744989.png)





# 실습 2-5

bigger()를 작성하고, 사용자로부터 2개의 정수를 입력받아 큰값을 출력하는 프로그램을 작성해라.

bigger()은 인자로 주어진 a,b가 같으면 true, 아니면 false를 리턴.

큰수는 big에 전달한다.

```cpp
bool bigger3(int a, int b, int &big) // big는 레퍼런스 변수이다.
{
    /*
    삼항연사자 이용
    big = (a > b) ? a : b;
    */
    if (a == b)
    {
        return true;
    }
    else
    {
        if (a > b)
        {
            big = a;
        }
        else
        {
            big = b;
        }
        return false;
    }
}

int main()
{
    int x, y, big;
    bool b;
    cout << "input two integers>>";
    cin >> x >> y;
    b = bigger3(x, y, big);

    if (b)
        cout << "same" << endl;
    else
        cout << "bigger integer is " << big << endl;
    return 0;
}
```