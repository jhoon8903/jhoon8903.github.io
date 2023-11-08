---
layout: post
title: Mongoose server 11000 Error
subtitle: Database 연결 실패 트러블슈팅
categories: Trouble
author: Daniel
tags: 
 - Trouble
 - Project
 - Network
 - Programming
 - NodeJS
 - Database
img : assets/images/posts/CPCzrTw.png
---

![](https://i.imgur.com/vvjUw24.png)
# ERROR message
	게시판 post 요청시 error 메세지 출력
	Code 11000
	keyPattern : { postId: 1 }
	keyValue : {postId : null} 
- 메세지 해석 : 서버에 중복된 값(postId : 1)이 있어 해당 요청을 받을 수 없고, keyValue 값이 비어있다.

처음 postId 라는 변수를 넣고 개발을 진행하던 중 동료의 피드백으로 해당 변수가 필요가 없다는
것을 알고 해당변수를 삭제 (비활성화:/주석처리) 하고 post 호출을 진행하면 해당 메세지가 계속 나오고 있었다. 

[./routes/posts.js]
![](https://i.imgur.com/6paxbfV.png)
	- 해당 라우터 코드에서 기존에 있던 postsId 부분을 변경
	- 이후 post 작성에는 문제 없는 코드로 확인

[./schemas/posts.js]
![](https://i.imgur.com/gv8to0t.png)
	-  schemas 코드에서도 변경하여 중복 될 값이 없음에도 개속해서 서버에러가 발생


## 해결 Solution
항상 코드와 컴퓨터에는 문제가 없다고 판단. (잘못은 항상 사람이 한다.)
코드 재작성 및 리부팅, 재시작 등 작업을 진행하였지만 별다른 특이사항을 발견하지 못함

프로젝트 엎고 다시해야 하나 생각하고 마지막으로 3T 서버를 확인하였는데 이상한 걸 봐버렸다.
지금 사진에서는 지워져 있지만, _ _id_ _ 밑에 수상한 아이디 요소들이 존재하는 것을 보았고
마지막 단에 postId 요소를 발견하고, 해당 요소를 삭제하니 정상적으로 post요청이 들어가는.....

아마. 처음 postId 의 요소 중 unique : true 설정을 한 것이 app 단에서는 변경 되었지만,
서버단에서 유니크 요소가 심어져 의도치 않은 중복차단을 하게 되고 postId값을 비워서 보내니,
require 설정에서 필수인 항목이 들어오지 않으 에러를 호출한 것으로 보인다.

추후에 동료가 해당문제가 발생하면 스크린샷 찍어 첨부해야겠다.

![](https://i.imgur.com/CPCzrTw.png)


![](https://i.imgur.com/e2KS6Du.png)

아니 그런데 ,,,, content 는 왜 안들어가냐....