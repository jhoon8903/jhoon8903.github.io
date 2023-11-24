---
layout: post
title: NetCoreAudio Handler Events
subtitle: 오디오 플레이어 클래스의 반복 재생 기능 만들기
author: Daniel
categories: Csharp
tags: 
 - Programming
 - Csharp
 - Utility
banner:
  image: https://i.imgur.com/qS9Z1Yc.jpg
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

NetCoreAudio Loop 기능 제작
--

🚨 오디오 플레이어를 제작하고 사요중 중요한 문제가 발생
- 오디오가 재생이 끝났을때 다시 반복하는 기능이 없는 것
- 반복기능이 없다면 추가적으로 실행을 시켜주어야하고 이는 코드의 가독성을 낮추고 복잡성을 높히는 문제로 발전하게 될 것이라고 판단되어 Loop 기능을 만들기로 함

### Event Handler 

![](https://i.imgur.com/qS9Z1Yc.jpg)

- NetCoreAudio에는 `PlaybackFinished`라는 Event Flag가 존재
- 이는 오디오파일의 플레이가 종료 되었을 때 true로 변하며, 플레이중에는 false 상태로 존재
- 해당 Flag 의 상태가 변하면 다시 플레이하는 메서드를 호출하여 반복 재생 기능을 하도록 구현

```csharp
_audioPlayer.PlaybackFinished += OnPlayCompleted;
```

- 추가적인 bool 값으로 loop 변수를 선언하여 반복 재생 여부를 내부적으로 결정
- 플레이시에 꼭 필요한 FilePath의 경우 내부적으로 `_currentFilePath` 라는 변수에 할당

### 코드 전문

```csharp
using NetCoreAudio.Interfaces;  
  
namespace HexaCoreVillage.Utility  
{  
	public class AudioPlayer  
	{  
		public enum PlayOption 
		{ 
			Play, Pause, Resume, Stop, 
			Change, LoopStart, LoopStop
		}  
		
		private static readonly IPlayer _audioPlayer = new NetCoreAudio.Player();  
		
		private static bool _loop = false;  
		
		private static string? _currentFilePath;  
		  
		  
		/// <summary>  
		/// 오디오 플레이어 메서드  
		/// </summary>  
		/// <param name="filePath">Audio File Path</param>  
		/// <param name="playOption">Play : 재생, Pause : 일시정지, Resume : 재실행, Stop : 중지, Change : 파일 바꾸기, LoopStart : 반복 실행, LoopStop : 반복실행취소</param>  
		
		public static void AudioController(string? filePath, PlayOption playOption)  
		{  
			_currentFilePath = filePath;  
			  
			switch (playOption)  
			{  
				case PlayOption.Play:  
				PlayAudio();  
				return;  
				case PlayOption.Pause:  
				_audioPlayer.Pause();  
				return;  
				case PlayOption.Resume:  
				_audioPlayer.Resume();  
				return;  
				case PlayOption.Stop:  
				StopAudio();  
				return;  
				case PlayOption.Change:  
				ChangeAudio(filePath);  
				return;  
				case PlayOption.LoopStart:  
				_loop = true;  
				PlayAudio();  
				return;  
				case PlayOption.LoopStop:  
				_loop = false;  
				StopAudio();  
				return;  
			}  
		}  
		  
		private static void PlayAudio()  
		{  
			_audioPlayer.PlaybackFinished+= OnPlayFinished;  
			_audioPlayer.Play(_currentFilePath);  
		}  
		  
		private static void StopAudio()  
		{  
			_loop = false;  
			_audioPlayer.Stop();  
			_audioPlayer.PlaybackFinished -= OnPlayFinished;  
		}  
		  
		private static void ChangeAudio(string? filePath)  
		{  
			_audioPlayer.Stop();  
			_audioPlayer.Play(filePath);  
		}  
		  
		private static void OnPlayFinished(object sender, EventArgs eventArgs)  
		{  
			if (_loop)  
			{  
				PlayAudio();  
			}  
		}  
	}  
}
```


Process Stop
--

👨🏻‍🚒 프로세스가 종료 될 때 시스템에서 재생중인 오디오가 멈추지 않고 계속 재생되는 문제


마치며
--

기능은 없으면 만들면 되고, 그 중에서 이벤트를 활용하는 방법에 대한 아이디어를 더 생각하면 서비스 친화적인 좋은 개발을 할 수 있을 것 같습니다.

