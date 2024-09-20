---
layout: single
title:  "#1 C++ 객체지향 프로그래밍 기본 개념 - 실습코드"
categories: C++
tag: [C++]
toc: true
toc_label: "table of content" # toc 이름 설정
toc_icon: "bars" # 아이콘 설정
toc_sticky: true # 마우스 스크롤과 함께 내려갈 것인지 설정
typora-root-url: ../

---



# `실습1-1`

끝수를 입력해서 끝수까지의 합을 구하여라.

```cpp
int ex0909_5()
{
    int sum=0;
    int n;
    cout<<"please enter last number>>";
    cin>>n;
    for(int i=1; i<=n; i++)
    {
        sum+=i;
    }
    cout<<"result of sum 1 to "<<n<<" is "<<sum<<endl;

    return 0;
}
int main(void)
{
		ex0909_5();
    return 0;

}
```



# 실습 1-2

구구단 프로그램 작성

```cpp
#include <iostream>
using namespace std;

int main(void)
{
    for(int i=1;i<10;i++)
    {
        for(int j=1;j<10;j++)
        {
            int result=i*j;
            cout<<i<<"*"<<j<<"="<<result;
        }
        cout<<endl;
    }
}
```

# `실습 1-3**`

소수점을 가지는 5개의 실수를 입력 받아 제일 큰 수를 화면에 출력하라. (반드시 `함수를 사용`하라. )

```cpp
double biggest(double b[], int n) //배열의 이름과 배열의 크기를 받아야함
//위의 코드는 이함수내에서 선언하는것과 같은 효과를 가진다.

//double biggest(double *b, int n) //변수의 주소와 변수의 크기를 받아야함
{
    double a_max = b[0];

    for (int i = 1; i < n; i++)
    {
        if (a_max< b[i])
        {
            a_max = b[i];
        }
        
    }
    return a_max;
}

int ex0909_6(void)
{

    /*배열에 입력받기*/
    double a[5]; // a[0] ~ a[4]

    cout<<a; //배열의 이름은 배열의 시작주소를 의미함. 0x61fed0

    cout << "type 5 floating point5 numbers>>";
    for (int n = 0; n < 5; n++)
    {
        cin >> a[n];
    }
    for (int n = 0; n < 5; n++)
    {
        cout << a[n] << " ";
    }

    /*큰수 찾기*/
    int N = 5;
    double a_max=biggest(a,N); //호출할때는 타입이 필요없고, 배열의 이름만 적어주면 됨***

    cout << "max number is " << a_max << endl;

}
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/28371a76-45b8-4a6b-b260-a8589b339055/f4830537-ad84-430e-92b1-97d2cafe046c/image.png)

```
int b =100;
int* p =&b
//p는 b라는 변수의 주소를 저장하는 변수이다.
```

# 실습 1-4

문장을 입력받은 후, x의 개수를 구하여라

```cpp
#include <iostream>
#include <cstring>
using namespace std;

int main(void)
{
    char c[100]; 
    int count =0;
    cout << "Enter a string: "<<endl;
    cin.getline(c, 100);

    for(int i =0;c[i] != '\\0';i++)
    {
        if(c[i] == 'x')
        {
            count++;
        }

    }
    cout<<"The number of 'x' characters in the string is: "<<count<<endl;
    return 0;

}
```

# 실습 1-5

문자열을 2개 입력 받고 2개의 문자열이 같은지 검사해라.

```cpp
#include <iostream>
#include <cstring>
using namespace std;

int main(void)
{
    char str[100], str2[100];
    cout<<"write sentence>>";
    cin.getline(str, 100);

    cout<<"write sentence one more time>>";
    cin.getline(str2, 100);

    if(strcmp(str, str2) == 0) // strcmp는 두 문자열이 같은지 다른지 비교하는 함수
    // 같으면 0을 반환하고 다르면 0이 아닌 값을 반환한다.
    {
        cout<<"sentences are same"<<endl;
    }
    else
    {
        cout<<"sentences are different"<<endl;
    }

    return 0;

}
```

## 만약 string 클래스를 이용한다면,

```cpp
#include <string>  // string 클래스를 사용하기 위해 포함
using namespace std;

int main(void)
{
    string str, str2; // string 클래스를 사용하여 문자열 선언
    cout << "Write a sentence >> ";
    getline(cin, str); // <string>의 getline 함수는 cin과 함께 사용

    cout << "Write the sentence one more time >> ";
    getline(cin, str2); // 두 번째 문자열 입력

    if (str == str2) // string 클래스에서는 비교 연산자를 바로 사용할 수 있다.
    {
        cout << "Sentences are the same" << endl;
    }
    else
    {
        cout << "Sentences are different" << endl;
    }

    return 0;
}
```

# 실습 1-6

이름 주소, 나이를 입력받아 다시 출력하는 프로그램을 작성하라.

```cpp
#include <iostream>
#include <cstring>
using namespace std;

int main(void)
{
    char name[100], address[100];
    int age;

    cout << "what is your name? ";
    cin.getline(name, 100);

    cout << "what is your address? ";
    cin.getline(address, 100);

    cout << "how old are you? ";
    cin >> age;

    cout<<"name: "<<name<<endl;
    cout<<"address: "<<address<<endl;
    cout<<"age: "<<age<<endl;
}
```

## 만약 string 클래스를 이용한다면,

```cpp
#include <iostream>
#include <string>
using namespace std;

int main(void)
{
    string name, address;
    int age;

    cout << "enter name>>";
    getline(cin, name);
    cout << "enter address>>";
    getline(cin, address);
    cout << "enter age>>";
    cin >> age;

    cout<< "name: "<<name<<", address: "<<address<<", age: "<<age<<endl;
}
```

# 실습 1-7

Namespace 활용

```cpp
#include <iostream>
using namespace std;

namespace KIM
{
    float area(float r)
    {
        return 3.14 * r * r;
    }

}

namespace Lee
{
    float area(float x)
    {
        return x * 10;
    }

}

int main(void)
{
    float A = KIM::area(5.0);
    float B = Lee::area(5.0);
    cout << A << " " << B << endl;

}
```