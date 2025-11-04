- Android ANR Reponse
- Application Not Responding
- Rendering 중 추가적인 Request로 인한 Chash
- ANR / Ceash
	- ANR 
		- 메인스레드 차단으로 인해 (너무 오래)
	- Crash 
		- 처리 되지 못한 예외처리 문제 
- 빌드 작동 방식
	1. APK, ABB 파일 생성
	2. Unity Player 안드로이드 라이브러리 생성
	3. Unity Player 인스턴스를 생성하는 Unity Activity 생성 (23 버전 부터는 Game Activity)
	4. 유니티 어플리케이션 로직이 실행되는 별도 Thread 생성 (Responding 5초)
	5. MonoBehaviour 업데이트, 사용자 인터렉션 등 해당하는 Thread 처리 
- Unity Thread 의 경우 5초의 대기 시간을 가지며, 이것이 차단 되면 ANR 발생
- 일반적 원인
	- Ads
	- Third Party
	- Native Code
	- Low Device
	- WebView
- 
# ANR Log 분석

![[Screenshot 2024-07-25 at 2.15.12 PM.jpg]]

### FAQ

![[Screenshot 2024-07-25 at 2.19.24 PM.jpg]]

- Addressable 에서 async await 등으로 백그라운드 작업
- 초기화 진행 시 많은 초기화 금물

### Optimize

- 패키지를 한꺼번에 초기화 하지 말고, 하나씩 초기화
- Third Package 점검
- onPause관련 긴 실행 프로세스를 주의
- CPU, Memory Usage 확인
- UnityPayerForActivityOrService.SynchronizationTimeout.setTimeoutForAll 설정 확인
- 저사양 기기 지원 중단

### Debugging Strategy

![[Screenshot 2024-07-25 at 2.31.35 PM.jpg]]

### Usage Tool

![[Screenshot 2024-07-25 at 2.34.59 PM.jpg]]

### FAQ

![[Screenshot 2024-07-25 at 2.46.27 PM.jpg]]
![[Screenshot 2024-07-25 at 2.47.56 PM.jpg]]