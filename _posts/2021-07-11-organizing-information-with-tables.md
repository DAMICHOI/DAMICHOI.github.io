---
title : Markdown(마크다운) - 테이블(표) 작성
date : 2021-07-11 00:00:00+0900
tags : [markdown]
---

# 테이블(표) 작성

> 다음 내용들은 Github Help를 참조하여 작성했습니다.

[1. 테이블 만들기](#1-테이블-만들기-)   
[2. 테이블 내에서 컨텐츠 포맷](#2-테이블-내에서-컨텐츠-포맷-)   
[3. 텍스트 정렬](#3-텍스트-정렬-)   

<br/><br/>

---

## 1. 테이블 만들기 [&#128285;](#테이블표-작성)
파이프 `|` 와 하이픈 `-` 으로 테이블을 만들 수 있습니다. 하이픈은 각 열의 머릿글을 만들기 위해 사용되며, 파이프는 각 열을 구분합니다. 테이블이 올바르게 렌더링 되려면 테이블 앞에 빈 줄을 포함해야 합니다.

머릿글 행의 각 열에는 하이픈을 최소 3개 이상 사용해야 합니다.

```
| First Header  | Second Header |
| ---           | ---           |
| First Content | Second Content|
| Content Cell  | Content Cell  |
```

| First Header  | Second Header |   
| ---           | ---           |   
| First Content | Second Content|   
| Content Cell  | Content Cell  |

<br/><br/>

---

## 2. 테이블 내에서 컨텐츠 포맷 [&#128285;](#테이블표-작성)
테이블 내에서 링크, 인라인 코드 블록 및 텍스트 스타일과 같은 형식을 사용할 수 있습니다.

```
| Command | Description |
| --- | --- |
| `git init` | **git 생성**하기 |
| `git clone git_path` | *git_path*에서 **코드 가져오기** |
```

| Command | Description |   
| --- | --- |   
| 'git init' | **git 생성**하기 |   
| 'git clone git_path' | *git_path*에서 **코드 가져오기** |

<br/><br/>

---

## 3. 텍스트 정렬 [&#128285;](#테이블표-작성)
머리글 행 내에서 하이픈의 왼쪽/오른쪽 또는 양쪽에 콜론 `:` 을 포함시켜 텍스트를 좌/우/가운데 정렬을 할 수 있습니다.

```
| 왼쪽 | 가운데 | 오른쪽 |   
| :--- | :---: | ---: |
| 왼쪽 정렬 | 가운데 정렬 | 오른쪽 정렬 |
| left-aligned | center-aligned | right-aligned |
```

| 왼쪽 | 가운데 | 오른쪽 |   
| :--- | :---: | ---: |
| 왼쪽 정렬 | 가운데 정렬 | 오른쪽 정렬 |   
| left-aligned | center-alined | right-aligned |

