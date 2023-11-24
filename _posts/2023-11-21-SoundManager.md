---
layout: post
title: OS API SoundManager
subtitle: C# Mac 과 Window 모두에서 사용 가능한 Audio Package
author: Daniel
categories: C#
tags: 
 - Csharp
 - Programming
 - Audio
 - OS
 - Window
 - Mac
banner:
  image: https://avatars.githubusercontent.com/u/42421108?s=200&v=4
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

NetCoreAudio
--

[NetCoreAudio-Github](https://github.com/mobiletechtracker/NetCoreAudio) - 링크 

- 해당 라이브러리를 사용하면 모든 운영 체제의 .NET Core 환경에서의 오디오 파일 사용이 가능해집니다.

### 메서드

#### Play(string filePath)
- 현재 재생을 중지하고, 지정된 오디오 파일을 재생
- 재생 bool 은 true, 일시 중지는 false

#### Pause()
- 일시 정지
- bool 은 true를 반환

#### Resume()
- 일시 정지 되어있던 재생을 다시 시작
- Pause bool  = true
#### Stop()
- 재생을 정지하고 버퍼를 삭제
- 정지 된 재생을 Resume 처럼 중단 지점에서 시작이 불가능함
- 처음부터만 재생 가능

#### SetVolume()
- 재생 볼륨을 0 - 100 까지 설정 가능

#### Events()
- 이벤트 핸들러 처리 가능


### 해당 라이브러리를 사용하게 된 이유

- 팀 협업시에 Window 사용자의 WaveAudio 라이브러리를 사용하면 간편
- 하지만 Mac 또는 다른 OS 사용자의 경우 해당 메서드를 실행하면 다음과 같은 에러가 발생

```csharp
using NAudio.Wave;

static AudioFileReader startBGM = new AudioFileReader(startBGMPath); 

static LoopStream startLoop = new LoopStream(startBGM); 

static AudioFileReader newWorldBGM = new AudioFileReader(newWorldBGMPath); 

static LoopStream newWorldLoop = new LoopStream(newWorldBGM); 

static WaveOutEvent audioOutput = new WaveOutEvent(); 

static AudioFileReader audioFile = new AudioFileReader(startBGMPath)

public override void Start() 
{ 
	CursorVisible = false; 
	audioOutput.Init(audioFile); 
	audioOutput.Play(); 
}
```

- Exception Error Log

```csharp
Unhandled exception. System.DllNotFoundException: Unable to load shared library 'winmm.dll' or one of its dependencies. In order to help diagnose loading problems, consider setting the DYLD_PRINT_LIBRARIES environment variable: dlopen(/usr/local/share/dotnet/shared/Microsoft.NETCore.App/7.0.5/winmm.dll.dylib, 0x0001): tried: '/usr/local/share/dotnet/shared/Microsoft.NETCore. App/7.0.5/winmm.dll.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/usr/local/share/dotnet/shared/Microsoft.NETCore.App/7.0.5/ winmm.dll.dylib' (no such file), '/usr/local/share/dotnet/shared/Microsoft.NETCore.App/7.0.5/winmm.dll.dylib' (no such file) dlopen(/Users/daniel/Documents/GitHub/HexCore/TeamDungeon/HexaCoreVillage/bin/Debug/net7.0/winmm.dll.dylib, 0x0001): tried: '/Users/daniel/Documents/GitHub/HexCore/TeamDungeon/ HexaCoreVillage/bin/Debug/net7.0/winmm.dll.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/Users/daniel/Documents/GitHub/HexCore/TeamDungeon/HexaCoreVillage/bin/ Debug/net7.0/winmm.dll.dylib' (no such file),
```

> `winnm.dll`을 참조하는 해당 라이브러리들은 Window에 종속이 되어 Mac, Linux 계열에서는 실행이 불가능해짐


### 문제 해결

#### AudioController Class

- 인터페이스에서 인스턴스를 생성 후 해당 인스턴스를 static으로 선언
- 모든 Class 에서 사용 가능하도록 메서드 화

```csharp
using NetCoreAudio.Interfaces;  
  
namespace HexaCoreVillage.Utility  
{  
	public class AudioPlayer  
	{  
		public enum PlayOption { Play, Pause, Resume, Stop, Change}  
		private static readonly IPlayer _audioPlayer = new NetCoreAudio.Player();  
		  
		/// <summary>  
		/// 오디오 플레이어 메서드  
		/// </summary>  
		/// <param name="filePath">Audio File Path</param>  
		/// <param name="playOption">Play : 재생, Pause : 일시정지, Resume : 재실행, Stop : 중지, Change : 파일 바꾸기</param>  
		public static void AudioController(string filePath, PlayOption playOption)  
		{  
			switch (playOption)  
			{  
				case PlayOption.Play:  
				_audioPlayer.Play(filePath);  
				break;  
				case PlayOption.Pause:  
				_audioPlayer.Pause();  
				break;  
				case PlayOption.Resume:  
				_audioPlayer.Resume();  
				break;  
				case PlayOption.Stop:  
				_audioPlayer.Stop();  
				break;  
				case PlayOption.Change:  
				_audioPlayer.Stop();  
				_audioPlayer.Play(filePath);  
				break;  
			}  
		}  
	}  
}
```


#### 사용

```csharp
AudioController(startBGMPath,PlayOption.Play);
```

- 오히려 기존 코드보다 간결해졌으며, 많은 Using 선언 및 필드를 객체화 시킬 필요 없음

#### 기존 Field 구성

```csharp
 static AudioFileReader startBGM = new AudioFileReader(startBGMPath);  
 static LoopStream startLoop = new LoopStream(startBGM);  
 static AudioFileReader newWorldBGM = new AudioFileReader(newWorldBGMPath);  
 static LoopStream newWorldLoop = new LoopStream(newWorldBGM);  
 static WaveOutEvent audioMgr = new WaveOutEvent();
```


마치며
--

자주 사용하거나 이용하게 될 라이브러리는 아니지만, 협업에 있어서 서로 같은 팀이 함께하면서 다른 작업 환경에도 거기서 어쩔수 없다고 하는 것이 아닌 할 수 있는 방법을 찾는 것이 중요
작업 환경 문제로 인한 것들은 충분히 대체할 수 있는 Cross Platform 도구를 찾는 것도 개발자의 역량이라고 생각합니다.