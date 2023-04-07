---
title: Nest JS Bulid heap memory 이슈
category: 트러블슈팅
author: "이정훈"
tags: [트러블슈팅, service]
img : https://i.imgur.com/Yfc1IPP.jpg
comments_disable: true
meta_description: "Nest JS Bulid heap memory 이슈"
---

# t2.micro 에서 Nest JS build 실패

Nest Js 에서 빌드 중 -  heap limit Allocation failed - Javascript heap out of memory 문제 발생. 

Cloud watch로 확인하니 리소스 사용률이 천장을 찍고 있는 상황

![](https://i.imgur.com/N4DIjZN.gif)

`free -h` 로 확인하였을때 빌드 중 급격히 감소하는 가용 메모리양이 확인 됨  

![](https://i.imgur.com/XWhjAXq.gif)


![heap3](:heap3.gif)
TS > JS 컴파일 과정 및 JS를 Interpretor 하는 과정에서 인스턴스의 가용 Heap 메모리 부족으로 빌드   
실패한다고 판단.


![](https://i.imgur.com/hbCtXJp.gif)

-   조치사항 :
1.  Instance Memory Swap 
2.  EC2 Scale up 
3.  Local에서 빌드 후 dist만 업로드 하는 방식 적용 
4.  Node JS Memory 증량

***

### 1. Instance Memory Swap : t2.micro의 memory 1gb + swap memory 2gb => 실패. 
|Ram|Swap Space|
|:----:|:----:|
|Ram <= 2gb|Ram 크기의 2배 (최소 32mb)|
|2gb < Ram < 32gb|4gb + (Ram - 2gb)|
|32gb <= Ram|Ram 크기의 1배|

t2.micro 의 Ram 은 1gb 이므로 2배수인 2gb로 스왑 = > 빌드 실패


![](https://i.imgur.com/HVvbusA.gif)


### 2. EC2 Scale Up : 비용이 발생하므로 해결 불가시 적용

### 3. Local Build 후에 dist만 따로 업로드 : 번거로워 사용 안함

### 4. Node JS Memory 증량

Node JS의 경우 버전별로 가용 최대 메모리 사이즈가 다르다
|버전|최대 가용 메모리|
|:---:|:----:|
|v12 이하|1.35gb|
|v13 - 14|2gb|
|v14 이상|4gb|

> 사용하고 있는 버전이 16버전 이므로  4gb 까지 적용 가능 (기본 512mb) 
	(Aws amazon linux에서는 16버전까지만 제공 그 이상을 사용하면 라이브러리 오류 발생)

```javascript
export NODE_OPTIONS=-max_old_space_size=4096
```

환경변수를 적용하여 Node 가용 메모리 확장 후 적용 => 성공

![](https://i.imgur.com/dnU5B71.gif)

의문점은 가용메모리 확장을 하지 않은 Local의 환경에서는 왜 빌드가 가능한 것이냐가 의문이다.