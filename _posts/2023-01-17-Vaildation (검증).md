---
layout: post
title: Vaildation (검증)
subtitle: 유저정보 검증하기
categories: Trouble
author: Daniel
tags: 
 - Database
 - Trouble
banner:
 image : assets/images/posts/W3t6zdq.png
---

function is1(value) {
	return value === 1;
}

joi 라이브러리 검증

toHexString ?


mongoose  'Schema.virtual' ?
![](https://i.imgur.com/D07Co2s.png)
3T 에서 조회한 모습 

JSON Type로 mongoose에서 보았을 때
todoId 라는 가상의(virtual) Column을 보여준다 
![](https://i.imgur.com/W3t6zdq.png)


deleteOne(todoId) 

router.delete('/todos/:_id', async (req, res) => {

const { _id } = req.params;

const deleteTodo = await Todo.deleteOne({ _id })

.exec()

.then(res.status(201).json('삭제되었습니다.'))

.catch(res.status(400).json('없는 아이디 입니다.'));

});



 //////////// 체크박스
if (done) {

const doneAt = new Date();

doneAt.setHours(doneAt.getHours() + 9);

currentTodo.doneAt = doneAt;

await currentTodo.save();

res.status(201).json({ status: `${done}`, message: '완료하였습니다.' });

} else {

currentTodo.doneAt = null;

await currentTodo.save();

res

.status(201)

.json({ starus: `${done}`, message: '할 일을 되돌렸습니다,' });

}


mysql 접속 에러



