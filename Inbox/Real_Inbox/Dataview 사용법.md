---
tags: [dataview, metadata]
---

결과물 출력 형태 : tabel, list - 이 후 메타데이터를 사용해 표의 열정보 표현 가능
어디서 가지고 올 것인지 : from "folder" 폴더나 태그/ and, or도 가능
정렬방법 : sort file.size desc/asc - 파일 사이즈 기준으로 내림차순
가지고 올 노트의 조건 : where
```
table
	file.size as 크기,
	file.cday as 생성일
from "Inbox" and #hashtag1 
sort file.cday asc
```

```
list
from "Inbox"
where this.file.ctime - file.ctime <= dur(7 days)
sort file.cday desc
```

```dataview
list
from "Inbox"
where this.file.ctime - file.ctime <= dur(7 days)
sort file.cday desc
```
# 옵시디언 내장 필드 목록

- file.name:  파일명  
- file.folder: 해당 파일이 속한 폴더명
- file.path: 해당 파일이 속한 전체 경로  
- file.link: 해당 파일의 링크  
- file.size: 해당 파일의 크기  
- file.ctime: 해당 파일이 만들어진 시간(시간 + 날짜)  
- file.cday: 해당 파일이 만들어진 날짜  
- file.mtime: 해당 파일이 수정된 시간(시간 + 날짜)  
- file.mday: 해당 파일이 수정된 날짜  
- file.tags: 해당 파일에 존재하는 모든 태그에 대한 배열   
- file.inlinks: 해당 파일을 참조하는 다른 노트들 목록  
- file.outlinks: 해당 파일이 참조하는 다른 노트들 목록  
- file.aliases: 해당 노트의 alias  
- file.tasks: 해당 파일에 존재하는 모든 할일목록(체크리스트)

Dataview를 사용하는 노트의 내장필드를 사용하기 위해서는 앞에 this를 붙이면 된다.
ex) this.file.name

