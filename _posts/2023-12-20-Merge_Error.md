---
layout: post
title: Unable to merge unrelated histories in this repository 
subtitle: 히스토리 문제로 머지가 되지 않을 때
author: Daniel
categories: Github
tags: 
 - Github
banner:
  image: https://i.imgur.com/VS2kqTp.jpg
---


![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

![](https://i.imgur.com/VS2kqTp.jpg)

Unable to merge unrelated histories in this repository
--

- "이 저장소에서 관련되지 않은 기록을 병합할 수 없습니다."
- 중간에 히스토리가 꼬이거나 잘못된 병합으로 인해서 발생
- 이러한 상황은 자체 초기 커밋(예: README 또는 라이선스 파일)을 사용하여 GitHub와 같은 플랫폼에서 새 저장소가 초기화되고 자체 기록이 있는 기존 로컬 저장소가 이미 있는 경우 일반적입니다.
- 이 문제를 해결하려면 `git pull` 또는 `git merge` 명령과 함께 `--allow-un관련-histories` 플래그를 사용할 수 있습니다. 
- 이 플래그는 두 기록이 서로 관련이 없더라도 병합이 진행되도록 Git에 지시합니다.

- 지금은 기록에 상관 없이 브랜치의 변경사항을 Main에 강제로 머지하는 과정입니다.

### Main 브랜치로 전환

```csharp
git checkout main
```


![](https://i.imgur.com/uXJyRIt.jpg)

### 하위 브랜치를 강제로 병합

```bash
git merge --strategy=ours 하위브랜치이름 --no-commit --no-ff
```

![](https://i.imgur.com/6MzH9MN.jpg)

#### error 발생

```bash
fatal: refusing to merge unrelated histories
```

- 히스토리 이슈로 인한 병합 불가 문제
- 히스토리에 상관 없이 강제로 병합 진행 합니다.

```bash
git merge Develop1.1 --allow-unrelated-histories --strategy=ours --no-commit --no-ff
```

![](https://i.imgur.com/DWMmgBP.jpg)

```bash
Automatic merge went well; stopped before committing as requested
```

- 정상적으로 머지가 가능해진 상태를 확인할 수 있습니다.
- 이제 부터는 평소의 머지 방법을 선택하여 머지 합니다.

![](https://i.imgur.com/ePF1wlb.jpg)

- 정상적으로 머지 된 것을 확인 할 수 있습니다.
