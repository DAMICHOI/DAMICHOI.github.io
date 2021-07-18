# Laura's Tech Blog
> 포스트 작성 규칙은 다음과 같습니다.

<br>

## 글 쓰기

1. `_posts` 디렉토리에 `yyyy-mm-dd-filename.md` 파일을 추가한다.
- filename : 해당 포스트의 고유 키로 url의 일부로 사용. 영문자, 숫자, -(하이푼) 만 사용할 것을 권장.
- yyyy-mm-dd : 발행 년-월-일.
2. 파일 상단에 [Front Mater](https://jekyllrb.com/docs/front-matter/)를 작성한다.
- title: '제목' # 제목(필수)
- date: `YYYY-MM-DD HH:MM:SS` # 발행일(필수)
- tags: `[tag1,tag2,tag3,...]` # 태그 목록(선택). 영소문자, 숫자, -(하이픈), .(점) 만 사용할 것을 권장.
3. 태그가 새로 등장할 경우 태그 등록

## 태그 등록
1. _data 디렉토리에 tag.yml 파일에 태그 추가
- 예시 : `- java`
2. post의 tags 배열의 항목과 매칭(필수). 영소문자, 숫자, -(하이픈), .(점) 만 사용할 것을 권장.
3. format.yml 파일에 태그와 매칭되는 태그명 추가
- 예시 : `java : JAVA`