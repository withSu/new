---
layout: single
title:  "OpenCV 스트레칭과 히스토그램"
categories: OpenCV
tag: [C++]
toc: true
toc_label: "table of content" # toc 이름 설정
toc_icon: "bars" # 아이콘 설정
toc_sticky: true # 마우스 스크롤과 함께 내려갈 것인지 설정
typora-root-url: ../
---

# 1. 스트레칭

![image.png](/images/2024-12-06-stretching-histogram/image.png)

스트레칭은 이미지 처리에서 **픽셀 밝기 값을 다른 값으로 변환**하는 과정을 뜻한다. 각 픽셀의 밝기 값을 특정 구간에서 변환하거나 확장(혹은 축소)하여 이미지의 대비를 조절할 수 있다.

## 이진화와 스트레칭의 차이점

### 이진화 (Thresholding)

- **목적**: 이미지를 **두 가지 값**(보통 0과 255)으로 변환해 흑백으로 표현하는 것.
- **방식**: 이진화는 특정 임계값(Threshold)을 기준으로 픽셀 값을 0 또는 255로 변환한다. 픽셀 값이 임계값보다 크면 255(흰색), 작으면 0(검정색)으로 바뀐다.
- **결과**: 결과 이미지는 두 가지 값(흑/백)만 남아 단순화된 형태가 된다.

### 스트레칭 (Stretching)

- **목적**: 이미지의 **밝기 값의 범위를 조정**하여 명암 대비를 높이거나 특정 구간의 밝기를 조절하는 것.
- **방식**: 픽셀 값이 0에서 255 사이에서 선형적 또는 비선형적으로 변환된다. 특정 구간을 확대하거나 축소할 수 있으며, 이미지 전체 밝기를 조정할 수 있다.
- **결과**: 결과 이미지는 여전히 0부터 255 사이의 다양한 값들이 남아 있어 이진화보다 더 많은 정보와 디테일을 보존한다.

## 반올림 처리 개념

### (1) 소수점 버림:

```cpp
int y;
float x = 1.5;
y = (int)x; // y = 1

```

여기서는 실수 `x` 값을 강제로 **정수형**으로 변환(cast)하고 있습니다. **(int)**를 사용하여 소수점 이하를 **버리는** 방식입니다.

- `x = 1.5`였지만, **정수 변환** 후에는 `y = 1`이 됩니다.
- 소수점 이하가 모두 제거되므로 버림 처리됩니다.

### (2) 반올림:

```cpp
int y;
float x = 1.5;
y = (int)(x + 0.5); // y = 2

```

반면, 반올림을 원한다면 `0.5`를 더한 후 **정수형으로 변환**하는 방식으로 해결할 수 있습니다.

- `x = 1.5`일 때, `x + 0.5 = 2.0`이 되고, `2.0`을 정수로 변환하면 `y = 2`가 됩니다.
- 이 방법은 실수를 반올림한 결과를 얻는 간단한 방법입니다.

영상처리에서 반올림 예시 코드

```cpp
void ImageStretch(int a, int b, int c, int d, int** img, int height, int width, int** img_out)
{
    for (int y = 0; y < height; y++) {
        for (int x = 0; x < width; x++) {
            if (img[y][x] < a)
                img_out[y][x] = (int)((float)c / a * img[y][x] + 0.5);
            else if (img[y][x] < b)
                img_out[y][x] = (int)(((float)d - c) / (b - a) * (img[y][x] - a) + c + 0.5);
            else
                img_out[y][x] = (int)((255.0 - d) / (255 - b) * (img[y][x] - b) + d + 0.5);
        }
    }
}

```

### 주요 내용:

1. **반올림 적용**:

   - 각 픽셀의 밝기 값을 변환하는 과정에서 **0.5를 더하고 정수형으로 변환**하여 반올림을 구현합니다.
   - 이는 이미지를 변환할 때, 보다 정확한 값을 얻기 위한 방법입니다. 예를 들어, 소수점 계산 중 값이 1.8이라면 이를 정수형으로 변환할 때 2로 반올림됩니다.

2. **변환 수식**:

   - **첫 번째 구간**: `img[y]< a`일 때, 픽셀 값을 `c/a` 비율로 변환합니다.

     ```cpp
     img_out[y][x] = (int)((float)c / a * img[y][x] + 0.5);
     
     ```

   - **두 번째 구간**: `a ≤ img[y]< b`일 때, `a`와 `b` 사이의 값을 `c`에서 `d` 사이로 선형 변환합니다.

     ```cpp
     img_out[y][x] = (int)(((float)d - c) / (b - a) * (img[y][x] - a) + c + 0.5);
     
     ```

   - **세 번째 구간**: `b ≤ img[y][x]`일 때, 값은 `d`에서 255까지 선형 변환됩니다.

     ```cpp
     img_out[y][x] = (int)((255.0 - d) / (255 - b) * (img[y][x] - b)
     
     ```

**(문제)** 반올림하는 연산을 매크로로 작성하고 예제 프로그램을 작성하시오.

**(답)**

```c
#define RoundUp(x) ((int)(x + 0.5))

void main()
{
    int y = RoundUp(2.6);
    printf("y = %d", y);  // 3이 출력
}

```

### `픽셀 밝기값을 다른 값으로 변환 ⇒ {특정 구간의 확대또는 축소} + {밝기값의 shift}복합적으로 작용.`

![image.png](/images/2024-12-06-stretching-histogram/image 1.png)

- **특정 구간의 확대 또는 축소**:
  - 이는 이미지의 **특정 범위의 밝기 값**을 조정하는 방법이다.
  - 예를 들어, 원래 50에서 100 사이의 밝기 값을 가지는 구간을 0에서 255까지의 전체 범위로 **확대**할 수 있다. 이렇게 하면 그 구간에 속하는 픽셀들이 더 다양한 밝기 값을 갖게 되어 대비가 높아진다.
  - 반대로, 어떤 구간을 **축소**할 수도 있다. 예를 들어, 원래 0에서 255까지의 밝기 범위를 50에서 100 사이의 작은 범위로 압축하면, 전체적으로 대비가 줄어든다.
  - 요약하자면, 이 방법은 **특정 구간의 픽셀 값들을 더 넓게 펼치거나, 좁게 압축하는** 변환이다.
- **밝기 값의 shift**:
  - **shift**는 이미지의 **전체적인 밝기**를 이동시키는 방법이다.
  - 예를 들어, 모든 픽셀 값에 +50을 더하면 전체 이미지가 밝아진다. 반대로, -50을 하면 이미지가 어두워진다.
  - 이 방법은 **픽셀 값 전체를 일정한 값만큼 상하로 이동**시키는 방식이다. 밝기 값의 분포는 유지하되, 그 분포가 상하로 이동하게 된다.

### `픽셀 값(img[y][x])이 임계점(a)보다 작을 때, 해당 픽셀 값을 **255에 맞게 스케일링(확대)** 하는 작업을 수행합니다.`

- **픽셀 값이 `a`보다 작을 때**:
  - 픽셀 값 `img[y][x]`을 **255로 맞춰서 스케일링** 합니다. 이를 통해, `a` 이하의 밝기 값을 가지는 픽셀들이 0에서 255 사이의 값으로 변환됩니다.
  - 이때 `255 / a`는 **스케일링 비율**입니다. 임계점 `a`에서 **가장 밝은 값**이 255가 되도록 픽셀 값들을 조정합니다.
  - 예를 들어, `a = 100`이면, `255 / 100 = 2.55`로 모든 픽셀 값이 2.55배 확장됩니다. 즉, 원래 밝기 값이 50이면 `50 * 2.55 = 127.5`가 되고, 이를 반올림하여 128로 설정됩니다.

```cpp
void Stretch1(int** img, int a, int height, int width, int** img_out)
{

	for (int y = 0; y < height; y++)
	{
		for (int x = 0; x < width; x++)
		{
			if (img[y][x] < a)
			{
				img_out[y][x] = ((255 / (float)a) * img[y][x] + 0.5); //반올림하기 위해 0.5를 더한다.
				//정수 나누기 float 은 float이다.
				//img_out[y][x] = (255.0 / a) * img[y][x]; 이렇게해도 된다.
				
				/*기본적으로 C에서 두 개의 정수를 나누면 그 결과도 정수가 된다.
				예를 들어, 255 / 100은 정수 나눗셈이므로 결과가 2로 나오는 반면,
				(float)a로 변환해서 255 / (float)100으로 하면 실수 나눗셈이 되어 결과는 2.55가된다. */
			}
			else {
				img_out[y][x] = 255;
			}
		}
	}
}

int ex1002_1()
{
	int height, width;
	int** img = ReadImage((char*)"./images/lena.png", &height, &width);

	int** img_out = (int**)IntAlloc2(height, width);

	int a = 100;

	Stretch1(img, a, height, width, img_out);

	ImageShow((char*)"input", img, height, width);
	ImageShow((char*)"output", img_out, height, width);

	return 0;
}

int main() //반올림
{
	float a = 100.4;
	int b = a + 0.5;
	std::cout << a;
	ex1002_1();
}
```

`실제로 식을 세운다음에 → 구현할 수 있어야한다.`

![image.png](/images/2024-12-06-stretching-histogram/image 2.png)

a=100, b=150, c=50, d=200

- **밝기 값이 100 미만**인 픽셀은 0에서 50 사이로 조정됩니다.
- **100에서 150 사이**의 밝기 값을 가지는 픽셀은 50에서 200 사이로 변환됩니다.
- **150 이상의 밝기 값**을 가지는 픽셀은 200에서 255까지의 값으로 변환됩니다.

```cpp
void Strech2(int** img, int a,int b, int c,int d,  int height, int width, int** img_out)
{

	
	for (int y = 0; y < height; y++)
	{
		for (int x = 0; x < width; x++)
		{
			if (img[y][x] < a)
			{
				img_out[y][x] = ((float)c / a) * img[y][x] + 0.5;
				/*
				그래프에서 0부터 a 사이의 값들을 0에서 c까지 선형적으로 늘리는 작업을 합니다. 
				여기서 + 0.5는 반올림을 위한 값입니다.
				*/
			}
			else if (img[y][x] < b)
			{
				img_out[y][x] = ((float)d - c) / (b - a) * (img[y][x] - a) + c + 0.5;
				/*
				이 구간은 a와 b 사이의 밝기 값을 선형적으로 c에서 d까지 변환하는 과정입니다. 
				이는 두 번째 그래프의 구간을 나타냅니다. 
				a와 b 사이의 값들은 c에서 d까지 비율에 맞게 조정됩니다.
				*/
			}
			else {
				img_out[y][x] = (255.0 - d) / (255 - b) * (img[y][x] - b) + d + 0.5;
				/*
				이 구간은 밝기 값이 b보다 큰 값들을 처리합니다. 
				이 값들은 d에서 255까지 선형적으로 변환됩니다. 
				그래프에서 b 이후 구간의 값들을 255까지 확장하는 작업입니다.
				*/
			}
		}
	}

}

int main(void)
{
	int height, width;
	int** img = ReadImage((char*)"./images/lena.png", &height, &width);
	int** img_out = (int**)IntAlloc2(height, width);

	int a = 150, b = 150, c = 50, d = 200;

	Strech2(img,  a, b, c,d, height, width, img_out);

	ImageShow((char*)"input", img, height, width);
	ImageShow((char*)"output", img_out, height, width);

}
```

### 결과

### 1. **입력 행렬 (왼쪽)**

원본 이미지의 픽셀 값들이 아래와 같습니다:

```

50   200  150
120  240  110
170  210  130

```

### 2. **변환 공식 적용 구간**

주어진 공식과 구간을 통해 밝기 값을 변환하는데, 변환은 다음의 세 구간으로 나뉩니다:

1. **첫 번째 구간: I<aI < aI<a**

   - 즉, 픽셀 값이 `100`보다 작을 때는 다음 공식을 사용하여 변환:
     f(I)=ac​×I
     이 공식은 픽셀 값을 `0`에서 `a`까지의 구간에서 `0`에서 `c` 사이로 변환합니다.

       f(I)=ca×If(I) = \frac{c}{a} \times I

       - 예: 50 → 25

2. **두 번째 구간: a≤I<ba \leq I < ba≤I<b**

   - 즉, 픽셀 값이 `100` 이상, `150` 미만일 때는 다음 공식을 사용:
     f(I)=b−ad−c​×(I−a)+c
     이 공식은 `100`에서 `150` 사이의 값을 `c`에서 `d`까지 선형 변환합니다.

       f(I)=d−cb−a×(I−a)+cf(I) = \frac{d - c}{b - a} \times (I - a) + c

       - 예: 120 → 110

3. **세 번째 구간: I≥bI \geq bI≥b**

   - 즉, 픽셀 값이 `150` 이상일 때는 다음 공식을 사용:
     f(I)=255−b255−d​×(I−b)+d
     이 공식은 `150` 이상의 값을 `d`에서 `255`까지 선형 변환합니다.

       f(I)=255−d255−b×(I−b)+df(I) = \frac{255 - d}{255 - b} \times (I - b) + d

       - 예: 200 → 226

### 3. **변환된 출력 행렬 (오른쪽)**

주어진 변환 공식을 각각 적용한 결과는 다음과 같습니다:

```

25   226  200
110  247   80
210  231  140

```

### 요약

이 그림은 주어진 임계값(a, b, c, d)에 따라 원본 픽셀 값들을 선형적으로 변환하여, 특정 구간을 확대하거나 축소하는 과정을 보여줍니다. 이를 통해 이미지의 대비를 조정하거나 밝기 값을 재조정할 수 있습니다.

![image.png](/images/2024-12-06-stretching-histogram/image 3.png)

### 왼쪽 이미지 (a=100, b=200, c=25, d=250)

- 이 변환은 픽셀 값이 100 미만인 경우, 해당 픽셀을 0에서 25 사이로 변환하고, 100에서 200 사이의 값을 25에서 250으로 변환하며, 200 이상의 값은 250에서 255로 선형적으로 변환하는 방식입니다.
- **특징**:
  - `이미지의 대비가 크게 늘어났습니다. 밝은 영역은 더 밝게, 어두운 영역은 더 어둡게 변환되었습니다.`
  - `명암이 더 극적으로 변환`되어 이미지의 디테일이 강하게 나타납니다.

### 오른쪽 이미지 (a=100, b=200, c=100, d=150)

- 이 변환은 픽셀 값이 100 미만인 경우 0에서 100 사이로 변환하고, 100에서 200 사이의 값을 100에서 150으로 변환하며, 200 이상의 값은 150에서 255로 선형적으로 변환합니다.
- **특징**:
  - `이 변환은 밝기 차이를 크게 줄였습니다. 밝은 영역과 어두운 영역 간의 차이가 크지 않으며, 전체적으로 낮은 대비를 유지합니다.`
  - `이로 인해 이미지가 상대적으로 부드럽게 나타나고, 극단적인 밝기 변화가 없습니`다.

### 결론

- **왼쪽 이미지**는 명암 대비가 극대화되어 보다 선명하고 디테일이 두드러지게 나타납니다.
- **오른쪽 이미지**는 밝기 값이 더 균일하게 유지되며, 전체적으로 부드러운 이미지를 제공합니다

## 파라미터 이용

### 파라미터방식을 사용하는 이유

예를 들어, 이전 방식은 이렇게 개별로 전달했었지:

```cpp
Stretch2(img, a, b, c, d, height, width, img_out)
```

이 경우 매번 **네 개의 변수 `a`, `b`, `c`, `d`**를 순서에 맞게 함수에 전달해야 해. 변수 수가 많아지면 코드를 이해하고 관리하기 어려워지기 때문에, **구조체**를 사용하여 이 값들을 하나로 묶어서 전달하는 방식이 훨씬 편리해.

### 구조체 사용 방식

구조체를 사용하면, 네 개의 변수를 개별로 전달하지 않고, **`Parameter`라는 하나의 구조체 변수**만 전달하면 돼:

```cpp
Stretch3(param, img, height, width, img_out);
```

이렇게 `param` 하나로 묶어서 전달하고, 함수 내부에서는 이 구조체에 있는 변수들을 꺼내서 사용하면 돼:

```cpp
if (img[y][x] < param.a)
{
    img_out[y][x] = ((float)param.c / param.a) * img[y][x] + 0.5;
}

```

```cpp
struct Parameter
{
	int a, b, c, d;

}; //

void Stretch3(
	Parameter param, int** img, int height, int width, int** img_out)
{

		for (int y = 0; y < height; y++)
	{
		for (int x = 0; x < width; x++)
		{
			if (img[y][x] < param.a)
			{
				img_out[y][x] = ((float)param.c / param.a) * img[y][x] + 0.5;
			}
			else if (img[y][x] < param.b)
			{
				img_out[y][x] = ((float)param.d - param.c) / (param.b - param.a) * (img[y][x] - param.a) + param.c + 0.5;
			}
			else {
				img_out[y][x] = (255.0 - param.d) / (255 - param.b) * (img[y][x] - param.b) + param.d + 0.5;
			}
		}
	}

}

int main(void)
{
	int height, width;
	int** img = ReadImage((char*)"./images/lena.png", &height, &width);
	int** img_out = (int**)IntAlloc2(height, width);

	//int a = 150, b = 150, c = 50, d = 200;

	//Strech2(img,  a, b, c,d, height, width, img_out);

	//Parameter param라고 해도 된다.
	struct Parameter param;
	param.a = 100;
	param.b = 150;
	param.c = 50;
	param.d = 200;

	Stretch3(param, img,  height, width, img_out);

	ImageShow((char*)"input", img, height, width);
	ImageShow((char*)"output", img_out, height, width);

}

```

이번코드가  이전 코드와 달라진점

- **첫 번째 코드**는 **밝기 변환을 위한 변수**들만 구조체로 묶어서 전달하고, 나머지 값들은 개별적으로 전달한다.
- **두 번째 코드**는 **모든 필요한 값**을 구조체로 묶어서 전달한다.
- **매개변수 전달 방식**:
  - **첫 번째 코드**는 필요한 변수 일부만 구조체로 묶어 관리하고, 이미지 관련 변수는 개별적으로 전달한다.
  - **두 번째 코드**는 모든 값을 구조체로 묶어 한 번에 전달한다.

```cpp
struct ParameterAll {
	int a, b, c, d;
	int** img;
	int height;
	int width;
	int** img_out;
};

void Stretch4(ParameterAll p)
	{

		for (int y = 0; y < p.height; y++)
		{
			for (int x = 0; x < p.width; x++)
			{
				if (p.img[y][x] < p.a)
				{
					p.img_out[y][x] = ((float)p.c / p.a) * p.img[y][x] + 0.5;
				}
				else if (p.img[y][x] < p.b)
				{
					p.img_out[y][x] = ((float)p.d - p.c) / (p.b - p.a) * (p.img[y][x] - p.a) + p.c + 0.5;
				}
				else {
					p.img_out[y][x] = (255.0 - p.d) / (255 - p.b) * (p.img[y][x] - p.b) + p.d + 0.5;
				}
			}
		}

}

int main(void)
{
	int height, width;
	int** img = ReadImage((char*)"./images/lena.png", &height, &width);
	int** img_out = (int**)IntAlloc2(height, width);

	//int a = 150, b = 150, c = 50, d = 200;

	//Strech2(img,  a, b, c,d, height, width, img_out);

	//Parameter param라고 해도 된다.
	//struct Parameter param;
	//param.a = 100;
	//param.b = 150;
	//param.c = 50;
	//param.d = 200;

	//Stretch3(param, img,  height, width, img_out);
	struct ParameterAll p;
	p.a = 100;
	p.b = 150;
	p.c = 50;
	p.d = 200;
	p.img = img;
	p.height = height;
	p.width = width;
	p.img_out = img_out;

	Stretch4(p);

	ImageShow((char*)"input", img, height, width);
	ImageShow((char*)"output", img_out, height, width);

}

```

# 2. 히스토그램

- 히스토그램이란? 픽셀 밝기값의 빈도수이다.

![image.png](/images/2024-12-06-stretching-histogram/image 4.png)

- 히스토그램에 따른 영상 밝기 특성

![image.png](/images/2024-12-06-stretching-histogram/image 5.png)

## 히스토그램을 저장할 배열을 다음과 같이 선언한다.

```cpp
int Hist[256] = {0}; // 빈도수를 저장하는 배열이므로 0으로 모두 초기화
```


🦒


**`Hist[img[0][0]]++`**:

- `img[0][0]` 값이 100이라면, 이 코드는 `Hist[100]`을 **1 증가**시킵니다.
- 즉, 이미지의 첫 번째 픽셀이 밝기 값이 100이라면, 히스토그램 배열에서 밝기 값 100의 카운트를 하나 증가시켜서, **해당 밝기 값이 한 번 더 등장했음**을 기록하는 것입니다.


```cpp
//첫번째 행
Hist[img[0][0]]++;
Hist[img[0][1]]++;
Hist[img[0][2]]++;

//두번째 행
Hist[img[1][0]]++;
Hist[img[1][1]]++;
Hist[img[1][2]]++;

//세번째 행
Hist[img[2][0]]++;
Hist[img[2][1]]++;
Hist[img[2][2]]++;

```

다음과 같이 표현

```cpp
for(int y = 0; y<3; y++){
for(int x=0;x<3;x++){
Hist[img[y][x]]++; }
}
```

```cpp
// 주어진 값이 이미지에서 몇 번 등장하는지 카운트하는 함수
int GetCount(int value, int** img, int height, int width)
{
	int count = 0;  // 값이 등장한 횟수를 저장할 변수

	// 이미지의 모든 픽셀을 순회
	for (int y = 0; y < height; y++)
	{
		for (int x = 0; x < width; x++)
		{
			// 현재 픽셀의 값이 주어진 값과 같으면 카운트 증가
			if (img[y][x] == value)
			{
				count++;
			}
		}
	}
	// 값이 등장한 횟수를 반환
	return count;
}

// 이미지의 히스토그램을 계산하는 함수
// 각 그레이스케일 값(0~255)이 이미지에서 몇 번 등장하는지 배열에 저장
void GetHistogram(int** img, int height, int width, int* histogram)
{
	// 그레이스케일 값(0~255)의 빈도를 계산하여 히스토그램 배열에 저장
	for (int value = 0; value < 256; value++) {
		histogram[value] = GetCount(value, img, height, width);  // 각 값의 빈도 계산***
	}
}

/* 히스토그램 계산 및 출력 */
int ex1008_2(void) {
	int height, width;
	
	// 이미지를 읽어들이는 함수 (ReadImage는 이미지 파일에서 높이, 너비와 데이터를 읽어옴)
	int** img = ReadImage((char*)"./images/lena.png", &height, &width);
	
	// 0~255 범위의 값들이 등장한 횟수를 저장할 배열 (히스토그램)
	int histogram[256];

	// 이미지로부터 히스토그램을 계산
	GetHistogram(img, height, width, histogram);

	// 히스토그램을 화면에 출력하는 함수 (DrawHistogram은 그래프를 그리는 역할)
	DrawHistogram((char*)"histo", histogram);

	// 이미지를 화면에 출력하는 함수 (사용하지 않음, 주석 처리됨)
	// ImageShow((char*)"input", img, height, width);

	return 0;
}

```

![image.png](/images/2024-12-06-stretching-histogram/image 6.png)

이번에는 Getcount함수를 사용하지 않고, 이미지의 모든 픽셀을 한번에 순회하면서, 각 픽셀의 값을 바로 히스토그램 배열에 업데이트한다.

```cpp
void GetHistogram2(int** img, int height, int width, int* histogram)
{
	for (int y = 0; y < height; y++) {
		for (int x = 0; x < width; x++) {
			histogram[img[y][x]]++;
		}
	}

}

int main(void)
{
	int height, width;
	int** img = ReadImage((char*)"./images/lena.png", &height, &width);
	int histogram[256] = { 0 };

	GetHistogram2(img, height, width, histogram);

	DrawHistogram((char*)"histo", histogram);

}

```

![image.png](/images/2024-12-06-stretching-histogram/image 7.png)

# 2. 히스토그램 평활화

- 밝기의 분포를 균일분포가 되도록 분포를 변환하는 처리
- 히스토그램이 특정 밝기값에 집중되어 있는 경우 효과적이다.
- 스트레칭의 일종으로서, `‘정규화된 누적히스토그램’을 스트레칭함수로 사용`.
- 누적히스토그램은 히스토그램의 적분이다.


🦒


### 2.1 원본 히스토그램 계산

먼저 이미지의 각 **픽셀 값(밝기 값, 0~255)**에 대한 빈도를 계산하여 **히스토그램**을 만든다. 이 히스토그램은 이미지에서 각 밝기 값이 얼마나 많이 등장하는지를 보여준다. 예를 들어, 어두운 이미지라면 0에 가까운 값들이 많이 등장할 것이고, 밝은 이미지라면 255에 가까운 값들이 집중될 것이다.

### 2.2 누적 히스토그램 계산

히스토그램 평활화의 핵심은 **누적 분포 함수(Cumulative Distribution Function, CDF)**를 이용하는 것이다. **누적 히스토그램**은 각 밝기 값에 대해 그 값보다 작거나 같은 픽셀 값의 누적 빈도를 계산한 것이다. 이를 통해 밝기 값의 분포를 파악하고, 이를 새로운 값으로 변환할 수 있게 된다.

### 2.3 정규화 및 매핑

![image.png](/images/2024-12-06-stretching-histogram/image 8.png)

누적 히스토그램 값을 0~255 사이의 범위로 **정규화**한다. 이를 통해 각 밝기 값이 새로운 밝기 값으로 **매핑**된다. 이 과정에서 원래 집중되어 있던 픽셀 값들을 더 넓은 범위로 분산시키게 된다. 결과적으로, 어두운 영역과 밝은 영역 모두 **더 넓은 명암 영역**에 걸쳐 나타나게 된다.


![image.png](/images/2024-12-06-stretching-histogram/image 9.png)

- `정규화된 누적히스토그램`을 함수로 하여 `스트레칭하는 것을 히스토그램 평활화`라고 한다.

  ![image.png](/images/2024-12-06-stretching-histogram/image 10.png)

가로: img in, 세로: img_out

### 누적 히스토그램 계산하기

```cpp
/* 누적 히스토그램을 계산하는 함수 */
void GetChistogram(int** img, int height, int width, int* chist)
{
	// 1. 먼저 일반 히스토그램을 계산하기 위해 임시 배열 선언
	int histogram[256] = { 0 };

	// 2. 이미지로부터 히스토그램을 계산 (0~255 값이 몇 번 등장하는지)
	GetHistogram2(img, height, width, histogram);

	// 3. 누적 히스토그램 계산
	// 누적 히스토그램의 첫 번째 값은 일반 히스토그램의 첫 번째 값과 동일
	chist[0] = histogram[0];

	// 4. 그 후, 각 그레이스케일 값에 대해 이전 값의 누적 합을 계산
	for (int n = 1; n < 256; n++)
	{
		// 현재 값은 이전 값의 누적 합에 현재 히스토그램 값을 더한 것
		chist[n] = chist[n - 1] + histogram[n];
	}
}

int main(void)
{
	int height, width;

	// 1. 이미지를 읽어오고 높이와 너비 값을 받음
	int** img = ReadImage((char*)"./images/lena.png", &height, &width);

	// 2. 누적 히스토그램을 저장할 배열 선언 및 초기화
	int chist[256] = { 0 };

	// 3. 누적 히스토그램 계산
	GetChistogram(img, height, width, chist); // 누적 히스토그램 구하기

	// 4. 정규화된 누적 히스토그램을 저장할 배열 선언 및 초기화
	int norm_chist[256] = { 0 };

	// 5. 누적 히스토그램을 정규화하여 norm_chist에 저장
	// 누적 히스토그램의 값을 0~255 범위로 변환하기 위해 정규화 작업을 진행
	for (int n = 0; n < 256; n++) {
		// 정규화된 값을 계산하여 0~255 범위로 변환 (0.5를 더해 반올림)
		norm_chist[n] = (float)chist[n] / (width * height) * 255 + 0.5;
	}

	// 6. 누적 히스토그램을 화면에 출력 (DrawHistogram은 누적 히스토그램을 그래프 형태로 그리는 함수)
	DrawHistogram((char*)"chisto", chist);

	return 0;
}

```

![image.png](/images/2024-12-06-stretching-histogram/image 11.png)

### 정규화된 누적히스토그램을 이용한 스트레칭

```cpp
/* 누적 히스토그램을 계산하는 함수 */
void GetChistogram(int** img, int height, int width, int* chist)
{
	// 1. 히스토그램 배열을 선언하고, 먼저 일반 히스토그램을 계산
	int histogram[256] = { 0 };
	GetHistogram2(img, height, width, histogram);  // 히스토그램 계산

	// 2. 누적 히스토그램 계산
	// 누적 히스토그램의 첫 번째 값은 일반 히스토그램의 첫 번째 값과 동일
	chist[0] = histogram[0];

	// 3. 그 후, 각 그레이스케일 값에 대해 이전 값의 누적 합을 계산
	for (int n = 1; n < 256; n++)
	{
		// 현재 값은 이전 값의 누적 합에 현재 히스토그램 값을 더한 것
		chist[n] = chist[n - 1] + histogram[n];
	}
}

int main(void)
{
	int height, width;

	// 1. 이미지를 읽어오고 높이와 너비 값을 받음
	int** img = ReadImage((char*)"./images/lena.png", &height, &width);

	// 2. 결과 이미지 저장을 위한 공간 할당
	int** img_out = (int**)IntAlloc2(height, width);  // img_out은 평활화된 이미지가 저장될 공간

	// 3. 누적 히스토그램을 저장할 배열 선언 및 초기화
	int chist[256] = { 0 };

	// 4. 누적 히스토그램 계산
	GetChistogram(img, height, width, chist);  // 이미지로부터 누적 히스토그램 계산

	// 5. 정규화된 누적 히스토그램을 저장할 배열 선언 및 초기화
	int norm_chist[256] = { 0 };

	// 6. 누적 히스토그램을 정규화하여 0~255 범위로 변환
	for (int n = 0; n < 256; n++) {
		// 정규화된 값을 계산하여 0~255 범위로 변환 (0.5를 더해 반올림)
		norm_chist[n] = (float)chist[n] / (width * height) * 255 + 0.5;
	}

	// 7. 정규화된 누적 히스토그램을 사용하여 이미지 매핑(mapping)
	// 원본 이미지의 각 픽셀 값을 정규화된 값으로 변환해 출력 이미지(img_out)에 저장
	for (int y = 0; y < height; y++)
	{
		for (int x = 0; x < width; x++)
		{
			img_out[y][x] = norm_chist[img[y][x]];  // 원본 픽셀을 정규화된 값으로 변환
		}
	}

	// 8. 원본 이미지와 평활화된 결과 이미지를 화면에 출력
	ImageShow((char*)"input", img, height, width);   // 원본 이미지 출력
	ImageShow((char*)"output", img_out, height, width);  // 평활화된 이미지 출력

	// 9. 원본 및 결과 이미지의 히스토그램을 계산
	int hist_input[256] = { 0 };
	int hist_output[256] = { 0 };
	
	GetHistogram2(img, height, width, hist_input);    // 원본 이미지의 히스토그램 계산
	GetHistogram2(img_out, height, width, hist_output); // 평활화된 이미지의 히스토그램 계산

	// 10. 원본 및 평활화된 이미지의 히스토그램을 화면에 출력
	DrawHistogram((char*)"input_hist", hist_input);    // 원본 이미지 히스토그램 출력
	DrawHistogram((char*)"output_hist", hist_output);  // 평활화된 이미지 히스토그램 출력

	return 0;
}

```

![image.png](/images/2024-12-06-stretching-histogram/image 12.png)

![image.png](/images/2024-12-06-stretching-histogram/image 13.png)

### (함수화해본다.)

```cpp
void HistogramEqualiztion(int** img, int height, int width, int** img_out)
{

	/*1. chist*/
	int chist[256] = { 0 };

	// 누적 히스토그램 계산
	GetChistogram(img, height, width, chist); //누적히스토그램구하기

	/*2. normalization*/
	int norm_chist[256] = { 0 };
	for (int n = 0; n < 256; n++) {

		norm_chist[n] = (float)chist[n] / (width * height) * 255 + 0.5;

	}

	//3. mapping usin 'norm_chist[]'
	for (int y = 0; y < height; y++)
	{
		for (int x = 0; x < width; x++)
		{
			img_out[y][x] = norm_chist[img[y][x]];
		}

	}

}

int main(void)
{
	int height, width;

	// 이미지 읽어오기
	int** img = ReadImage((char*)"./images/lena.png", &height, &width);
	int** img_out = (int**)IntAlloc2(height, width);

	HistogramEqualiztion(img, height, width, img_out);

	// 누적 히스토그램 출력
	ImageShow((char*)"input", img, height, width);
	ImageShow((char*)"output", img_out, height, width);
	
	int hist_input[256] = { 0 };
	int hist_output[256] = { 0 };
	
	GetHistogram2(img, height, width, hist_input);
	GetHistogram2(img_out, height, width, hist_output);

	DrawHistogram((char*)"input_hist", hist_input);
	DrawHistogram((char*)"output_hist", hist_output);

	return 0;
}

```

![image.png](/images/2024-12-06-stretching-histogram/image 13.png)

# 문제풀이

 ## (문제) 히스토그램 평활화는 스트레칭의 일종으로서, 이를 위해 사용되는 변환함수는?

 (답) 정규화된 누적 히스토그램

**설명**:

히스토그램 평활화에서 사용하는 변환 함수는 정규화된 누적 히스토그램(Cumulative Distribution Function, CDF)이다. 평활화 과정은 원본 히스토그램에서 각 밝기 값의 누적 분포를 이용해 픽셀 값을 새롭게 매핑하는 방식으로 이루어진다. 이때, 누적 히스토그램을 정규화하여 0에서 255까지의 값으로 변환하면 원본 이미지에서 자주 등장하는 밝기 값들이 이미지 전체에서 더 균일하게 분포되도록 매핑된다. 이러한 변환 과정을 통해 **밝기 값이 적절하게 분산**되어 더 명확한 대비를 갖춘 이미지를 얻게 된다.

- **누적 히스토그램**: 주어진 값보다 작은 모든 픽셀 값들의 빈도를 누적한 값이다.
- **정규화**: 누적 히스토그램을 정규화하여 0~255 범위로 변환함으로써 각 픽셀 값을 새로운 값으로 변환한다.

## (문제) 히스토그램 평활화 처리 후의 결과영상의 히스토그램 분포는 이론적으로 어떤 분포인가?

(답) 균일 분포

**설명**:

히스토그램 평활화 후 이론적으로 얻고자 하는 목표는 균일한 분포(Uniform Distribution)이다. 원본 이미지의 히스토그램이 특정 밝기 값에 몰려 있을 경우, 히스토그램 평활화는 이를 전체 밝기 범위로 고르게 분산시켜, 모든 밝기 값이 비슷한 빈도로 등장하도록 한다. 결과적으로, 이미지의 히스토그램은 평활화 과정 후 **모든 밝기 값이 비슷한 빈도를 가지는 균일 분포**에 가까워지게 된다.

이론적으로는 히스토그램이 평활화되면 모든 밝기 값이 동일한 확률로 나타나는 균일 분포를 이루지만, 실제로는 이미지의 특성에 따라 완전히 균일한 분포를 이루지는 않을 수 있다. 그러나 저대비 문제를 해결하고, 전체적인 대비를 개선하는 데는 매우 효과적이다.