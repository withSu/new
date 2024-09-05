---
layout: single
title:  "Opencv 최신 버전(4.10.0) 환경설정하기 "
categories: OpenCV
tag: [c++]
toc: true
toc_label: "table of content" # toc 이름 설정
toc_icon: "bars" # 아이콘 설정
toc_sticky: true # 마우스 스크롤과 함께 내려갈 것인지 설정
typora-root-url: ../

---





# 1. Opencv 최신 버전(4.10.0) 환경설정 

## 1.1 프로젝트 만들기

먼저 opencv를 설치합니다.

https://opencv.org/





visual studio의 새 프로젝트 만들기에서 콘솔앱을 선택합니다.

![image-20240905102739896](/images/2024-09-05-opencv-settings/image-20240905102739896.png)





visualstudio에서 새프로젝트 만들기

세프로젝트를 만들떼 솔루션을 같은 디렉토리에 설정 체크가 되어있어야합니다.

![image-20240905102807206](/images/2024-09-05-opencv-settings/image-20240905102807206.png)





## 1.2 프로젝트 속성 환경설정

창 상단에 다음 상태임을 확인합니다.

<img src="/images/2024-09-05-opencv-settings/image-20240905102823492.png" alt="image-20240905102823492" style="zoom:150%;" />

프로젝트폴더의 오른쪽클릭 → 속성

![image-20240905102916221](/images/2024-09-05-opencv-settings/image-20240905102916221.png)

다음이 설정되어있는지 확인합니다.

![image-20240905102936649](/images/2024-09-05-opencv-settings/image-20240905102936649.png)

### 1.2.1 c++ 일반 메뉴 선택

왼쪽의 C/C++ 메뉴 클릭

![image-20240905102953679](/images/2024-09-05-opencv-settings/image-20240905102953679.png)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/28371a76-45b8-4a6b-b260-a8589b339055/d47a92b1-557f-4aec-9b5f-988d0f82d9e1/image.png)

다음을 입력합니다,

F:\Program Files\opencv\build\include



### 1.2.2 왼쪽의 링커-일반 메뉴 선택

![image-20240905103027273](/images/2024-09-05-opencv-settings/image-20240905103027273.png)

추가 라이브러리 디렉터리에 다음을 입력합니다.

F:\Program Files\opencv\build\x64\vc16\lib

![image-20240905103106020](/images/2024-09-05-opencv-settings/image-20240905103106020.png)





### 1.2.3 링커-입력 메뉴 클릭

![image-20240905103143866](/images/2024-09-05-opencv-settings/image-20240905103143866.png)

추가 종속성에 다음을 입력합니다.

opencv_world4100d.lib

> (F:\Program Files\opencv\build\x64\vc16\lib 경로의 파일을 확인후 복붙합니다)

> [//d.lib](http://d.lib) 파일임을 주의합니다.

그리고 코드를 실행해봅니다.ㅁ

![image-20240905103201619](/images/2024-09-05-opencv-settings/image-20240905103201619.png)





### 1.2.4  d.dll파일 복사붙여넣기

F:\Program Files\opencv\build\x64\vc16\bin 경로에서

다음  d.dll파일을

![image-20240905103240856](/images/2024-09-05-opencv-settings/image-20240905103240856.png)

프로젝트 파일에 붙여 넣기 합니다.

F:\2024_selfcode\cpp_study_vsstudio\opencv_study_Project\opencv_study_Project\x64\Debug

기본 환경설정은 완료되었습니다.





# 2. 테스트하기

```json
#include <iostream>
#include <opencv2/opencv.hpp>

int main()
{
    // 프로그램 시작 메시지
    std::cout << "프로그램이 시작되었습니다." << std::endl;

    // OpenCV 버전 출력 (OpenCV 설치 여부 확인)
    std::cout << "OpenCV 버전: " << CV_VERSION << std::endl;

    // 더미 이미지 생성 (imread와 유사한 방식으로 Mat 객체 생성 테스트)
    cv::Mat dummyImage = cv::Mat::zeros(100, 100, CV_8UC3); // 100x100 크기의 검정색 이미지

    // dummyImage의 생성 여부 확인
    if (dummyImage.empty()) {
        std::cerr << "Error: 더미 이미지를 생성하는 데 실패했습니다." << std::endl;
        return -1;
    }
    else {
        std::cout << "더미 이미지가 성공적으로 생성되었습니다!" << std::endl;
        std::cout << "이미지 크기: " << dummyImage.cols << " x " << dummyImage.rows << std::endl;
    }

    // 더미 이미지(검정색 화면)를 표시
    cv::imshow("검정색 화면", dummyImage);

    // 키보드 입력 대기 (아무 키나 누르면 창이 닫힘)
    cv::waitKey(0);

    // imread 함수 테스트 (빈 문자열 전달하여 함수 오류 처리 확인)
    cv::Mat testMat = cv::imread(""); // 잘못된 경로로 imread 테스트

    if (testMat.empty()) {
        std::cout << "imread가 빈 파일을 처리할 때 올바르게 실패했습니다." << std::endl;
    }
    else {
        std::cerr << "Error: imread가 빈 파일 경로를 처리하는데 실패했습니다." << std::endl;
        return -1;
    }

    // 프로그램 정상 종료 메시지
    std::cout << "프로그램이 정상적으로 종료되었습니다." << std::endl;

    return 0;
}
```

다음과 같이 정상적으로 작동합니다.

![image-20240905103302824](/images/2024-09-05-opencv-settings/image-20240905103302824.png)



# 3. 상대경로 이미지 불러와서 열어보기

프로젝트 폴더안에 images라는 폴더를 만들고 sample.jpg파일을 넣습니다.

![image-20240905103324895](/images/2024-09-05-opencv-settings/image-20240905103324895.png)



다음과 같이 sample.jpg가 뜨는것을 확인할 수 있습니다.

![스크린샷 2024-09-05 103347](/images/2024-09-05-opencv-settings/스크린샷 2024-09-05 103347.png)

