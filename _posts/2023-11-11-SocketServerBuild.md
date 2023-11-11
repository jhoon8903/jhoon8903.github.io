---
layout: post
title: 개인과제 프로젝트 3
subtitle: WebSocket 채팅 구현
author: Daniel
categories: Project
tags: 
 - Project
 - Network
 - Game
 - NestJS
 - Socket

banner:
  image:
---

Nest JS를 이용한 소켓 서버 구축
--

- 채팅 기능을 만들기 위한 Nest JS 소켓 서버 구축
- 오랜만에 백엔드 소켓 서버를 구축하는데 행한 과정들에 대한 레포트

## 1. Nest JS Project Create
- 오랜만의 VS CODE 실행 

1. Nest Js 업데이트

```javascript
npm install -g @nestjs/cli
```

2. 새로운 프로젝트 생성

```javascript
nest new DungeonRPG-SERVER
```

```javascript
daniel@Danielui-MacBookPro GitHub % nest new DungeonRPG-SERVER

⚡  We will scaffold your app in a few seconds..

? Which package manager would you ❤️  to use? npm
CREATE dungeon-rpg-server/.eslintrc.js (663 bytes)
CREATE dungeon-rpg-server/.prettierrc (51 bytes)
CREATE dungeon-rpg-server/README.md (3340 bytes)
CREATE dungeon-rpg-server/nest-cli.json (171 bytes)
CREATE dungeon-rpg-server/package.json (1949 bytes)
CREATE dungeon-rpg-server/tsconfig.build.json (97 bytes)
CREATE dungeon-rpg-server/tsconfig.json (546 bytes)
CREATE dungeon-rpg-server/src/app.controller.spec.ts (617 bytes)
CREATE dungeon-rpg-server/src/app.controller.ts (274 bytes)
CREATE dungeon-rpg-server/src/app.module.ts (249 bytes)
CREATE dungeon-rpg-server/src/app.service.ts (142 bytes)
CREATE dungeon-rpg-server/src/main.ts (208 bytes)
CREATE dungeon-rpg-server/test/app.e2e-spec.ts (630 bytes)
CREATE dungeon-rpg-server/test/jest-e2e.json (183 bytes)

✔ Installation in progress... ☕

🚀  Successfully created project dungeon-rpg-server
👉  Get started with the following commands:

$ cd dungeon-rpg-server
$ npm run start

                                         
                          Thanks for installing Nest 🙏
                 Please consider donating to our open collective
                        to help us maintain this package.                        
```

## 2. 웹소켓 라이브러리 설치

1. 소켓 라이브러리 설치

```javasript
npm install @nestjs/websockets @nestjs/platform-socket.io socket.io socket.io-client
```

**📍 설치 실패**

메세지 

- 해당 Nest의 종속성에 대한 버전 에러 발생

```javascript
npm ERR! code ERESOLVE
npm ERR! ERESOLVE unable to resolve dependency tree
npm ERR! 
npm ERR! While resolving: dungeon-rpg-server@0.0.1
npm ERR! Found: @nestjs/common@9.4.3
npm ERR! node_modules/@nestjs/common
npm ERR!   @nestjs/common@"^9.0.0" from the root project
npm ERR! 
npm ERR! Could not resolve dependency:
npm ERR! peer @nestjs/common@"^10.0.0" from @nestjs/websockets@10.2.8
npm ERR! node_modules/@nestjs/websockets
npm ERR!   @nestjs/websockets@"*" from the root project
npm ERR! 
npm ERR! Fix the upstream dependency conflict, or retry
npm ERR! this command with --force or --legacy-peer-deps
npm ERR! to accept an incorrect (and potentially broken) dependency resolution.
npm ERR! 
npm ERR! 
npm ERR! For a full report see:
npm ERR! /Users/daniel/.npm/_logs/2023-11-11T06_04_35_179Z-eresolve-report.txt
```

- package.json에서 해당 common의 버전 호환성을 강제로 수정
=> 싫패하여 버전을 확인 하였는데 
기존 Node Version : 21.1.0;
LTS Node Version : 20.9.0;
이어서 nvm을 이용하여 LTS 버전으로 다운그레이드 하여 설치 완료


Socket IO 문제
--

### Nest JS의 Socket.io Gateway
- Node 로도 구현이 가능하지만, Socket 자체를 사용하는 것은 Nest가 지원하는 Socket이 더 편하기 때문에 Nest Socket gateway를 이용하여 간단한 Socket 이벤트를 구성

```typescript
// websockets.gateway.ts
import 
{
	WebSocketGateway,
	SubscribeMessage,
	MessageBody,
	WebSocketServer,
	OnGatewayConnection,
	OnGatewayDisconnect,

} from '@nestjs/websockets';
import { Server, Socket } from 'socket.io';

@WebSocketGateway({ path: '/chat' })
export class WebsocketsGateway
implements OnGatewayConnection, OnGatewayDisconnect
{
	@WebSocketServer()
	server: Server;

	handleConnection(client: Socket, ...args: any[]) 
	{
		console.log(`Client connected: ${client.id}`);
	}

	handleDisconnect(client: Socket) 
	{
		console.log(`Client disconnected: ${client.id}`);
	}
	
	
	@SubscribeMessage('message')
	handleMessage(client: Socket, payload: string): void 
	{
		this.server.emit('message', payload);
		console.log(payload);
	}
}
```

- 이벤트 SubscribeMessage 부분의 이벤트 키워드는 'message'로 약속
- path 의 경우 localhost:3000/chat 으로 설정

### C# WebSocket

 - 처음 시도하였을 때 System.Net.WebSocket을 이용하여 시도를 했는데 빈번히 연결에 실패
 - API 및 Gateway 점검을 하였을 때 Http 통신 자체는 문제가 없이 응답을 받았지만
 - 연결은 계속하여 실패하여 Socket.io와 .Net의 WebSocket은 같은 것인가? 라는 의문이 듬
 - 결과적으로 둘은 완전히 다르다라는 의견에 도달
 - Unity 에서 사용하는 `SocketIOSharp`를 이용해 보았지만, 아래의 Fail Message가 발생
```csharp
Packet decoding failed. 
0{"sid":"56__c7HeAk4XX9b3AAAa","upgrades": ["websocket"],"pingInterval":25000,"pingTimeout":20000,"maxPayload":1000000} : System.FormatException: The input string '0{"sid"' was not in a correct format.
at System.Number.ThrowOverflowOrFormatException(ParsingStatus status, ReadOnlySpan`1 value, TypeCode type) at System.Int32.Parse(String s)
```
- 해당 문제에 대해서도 지속적으로 확인을 해보니 
- Socket.IO 공식 Docs에 비슷한 문제에 대한 내용이 존재하는 것을 발견
- [Socket.IO - The server is not reachable](https://socket.io/docs/v4/troubleshooting-connection-issues/#you-are-trying-to-reach-a-plain-websocket-server)
- 해당 부분은 경험의 문제인 것 같아 질문을 남겨 두었고 답변에 따라서 방향이 정해질 것 같음

코드 전문 

```csharp
using SocketIOSharp.Client;  
using System.Text.Json;  
using EngineIOSharp.Common.Enum;  
  
namespace DungeonTextGame;  
  
public class ChatMessage  
{  
	public string User { get; set; }  
	public string Message { get; set; }  
}  
  
public static class ChatSocket  
{  
	private static SocketIOClient _webSocket;  
	private static string ID { get; set; }  
	  
	public static async Task ChatRoomAsync()  
	{  
		Clear();  
		ID = Account.LoginCharacter.Id;  
		await ConnectToNestServer();  
		  
		while (true)  
		{  
			string inputMessage = ReadLine();  
			await SendMessage(inputMessage);  
		  
			if (inputMessage.Equals("exit", StringComparison.OrdinalIgnoreCase))
			{  
				LogoutChatRoom();  
				break;  
			}  
		}  
	}  
	  
	  
	private static Task ConnectToNestServer()  
	{  
		_webSocket = 
		new SocketIOClient(
		new SocketIOClientOption(
		EngineIOScheme.http, "127.0.0.1", 3000, 843,"/chat"));  
		
		WriteLine($"Socket : {_webSocket}");  
		  
		_webSocket.On("connect", () =>  
		{  
			WriteLine($"{ID}님이 채팅방에 입장하셨습니다.");  
			_webSocket.Emit("join", ID);
		});  
		  
		_webSocket.On("message", data =>  
		{  
			string json = data[0].ToString();  
			// data[0]는 첫 번째 매개변수를 가정합니다.
			ChatMessage message = 
				JsonSerializer.Deserialize<ChatMessage>(data.ToString());  
			ConsoleMessage(message);  
		});  
		  
		_webSocket.Connect();  
		return Task.CompletedTask;  
	}  
	  
	/// <summary>  
	/// 메세지를 보내는 메서드  
	/// </summary>  
	private static Task SendMessage(string inputMessage)  
	{  
		ChatMessage message = new ChatMessage 
		{ 
			User = ID, 
			Message = inputMessage 
		};  
		_webSocket.Emit("message", JsonSerializer.Serialize(message));  
		return Task.CompletedTask;  
	}  
	  
	private static void ConsoleMessage(ChatMessage message)  
	{  
		if (message.User == ID)  
		{  
			ForegroundColor = ConsoleColor.Yellow;  
		}  
		else  
		{  
			ResetColor();  
		}  
		WriteLine($"{message.User} : {message.Message}");  
	}  
	  
	  
	/// <summary>  
	/// 메세지방을 나가는 메서드  
	/// </summary>  
	private static void LogoutChatRoom()  
	{  
		_webSocket.Close();  
		GameManager.GameStart();  
	}  
}
```

> ConnectToNestServer()에서  WriteLine($"Socket : {_webSocket}");  반환값이 
>  > Socket : SocketIOSharp.Client.SocketIOClient 로 출력 되는 것으로 보아 

```csharp
_webSocket.On("connect", () =>  
{  
	WriteLine($"{ID}님이 채팅방에 입장하셨습니다.");  
	_webSocket.Emit("join", ID); // 예시로 'join' 이벤트를 서버에 보냅니다.  
});  
  
_webSocket.On("message", data =>  
{  
	string json = data[0].ToString();  
// data[0]는 첫 번째 매개변수를 가정합니다. 필요에 따라 인덱스를 조정하십시오.  
	ChatMessage message = JsonSerializer.Deserialize<ChatMessage>(json);  
	ConsoleMessage(message);  
});
```

- 이 부분의 데이터 디코딩이 정상적으로 이루어 지지 않는 것이 있지 않은가하고 추측
- 답변이 오는데로 다시 시도 하고 해결하기로 함

