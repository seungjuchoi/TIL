# Git 고급 Command 정리
##  Advanced Commands

명령어 | 설명
------------ | -------------
git tag <new-tag-name A> <branch-name> | 태그 긋기
git merge --squash <당길 branch-name> | 당길 브랜치의 commit들을 하나의 commit으로 합치고 흡수, Stage에만 update
git log --since="5 hours" | 5시간전 이후로
git log --before="5 hours" | 5시간전
git log -p | commit의 변경사항까지 자세히 출력
git blame -M <파일명> | 이동했거나 같은 파일에 복사된 내용을 찾음.
git revert -n HEAD | 돌려놓기, -n 옵션은 스테이징 후 커밍을 기다림, 사용하지 않는다면 커밋까지 완료
git reset --soft HEAD^ | 헤더 바로 전으로 돌려놓기 (commit 부분만 적용, stage와 작업트리는 그대로)
git reset --hard HEAD^ | 헤더 바로 전으로 돌려놓기 (3부분 모두 적용)
git reset --hard commitID | 지정한 commit ID 이후의 commit들을 제거함. (commit ID는 git log 를 통해 확인)
git add -A | stages All 
git add . | stages new and modified, without deleted 
git add -u | stages modified and updated without new
git rebase --continue | diff merge를 수행하고 정상 종료 시킴
git rebase --skip | 내가 작업한 부분은 취소시키고 Remote의 commit을 반영함
git rebase --abort | diff merge 발생 이전으로 돌아감 (repo sync를 하면 다시 diff merge 발생함)
git diff HEAD..HEAD^ | HEAD와 HEAD 전 commit  비교.
git clean -xdf | Untraced files 삭제
git apply --check	| 실행 전 검사
git am --resolved	| 충돌 해결 후 
git am --skip |	충돌 스킵
git am --abort	| 적용 중단 
git stash | Dirty code 임시저장
git statsh list  | 저장된 리스트 출력
git stash apply --index | 저장된 것 적용
git stash drop stash@{0} | list에서 drop 시키기
git stash pop | (적용 후 삭제)
git push origin :heads/{branch name} | Branch 삭제
git push origin :tags/{tag name} | Branch 삭제

## Gerrit의 업데이트가 되지 않을 경우
```bash
ssh -p <port> <IP> gerrit flush-caches
```

## Git 추가
ssh -p <Port> <IP> gerrit create-project --name <Name> --branch <Branch> --description "'<Description>'"


## 비어 있는 Git Project 생성 후 Empty Commit 생성 
```bash
	git init
	git commit --allow-empty -m "Initial commit with no content"
	git push ssh://xxx.com:12345/platform/xxxx.git HEAD:<Branch>
```

## Thread활용하여 Repo command 사용하기
```bash
repo forall -c 'while [ $(ps -C ssh | wc -l) -gt 10 ]; do sleep 1; done; git push &'
```

## Manifest revision 만들기
```bash
repo manifest -r -o xxx.xml
```

## sed 명령어를 활용한 Local To Remote Push
예
```bash
repo forall -c 'cat -v -t .git/config | grep url | sed -e "s/\^Iurl = ssh:\/\/192.168.1.11:29418\///g"| xargs -i git push ssh://seungju24.choi@111.111.111.111:12345/{} HEAD:refs/heads/<Branch Name>'
```

## "repo sync ."와 "git pull" 차이
repo sync . 은 git fetch 후 git rebase 를 하는것과 동일한 기능을 하지만 git pull은 git fetch 후 git merge를 한다. 
git pull을 사용하여 소스를 가져올경우 merge commit이 생기기 때문에 repo sync . 으로 소스를 가져오는 것을 권장한다. 
이때 주의사항으로는 repo sync . 은 .repo 폴더 내에 manifest.xml을 기준으로 하기때문에 다른 branch의 소스를 가져올경우 manifest.xml 내에 brach의 merge 내용을 확인할 필요가 있다.
git pull에 대한 자세한 내용은 다음 링크 참조 
[Git pull](http://www.kernel.org/pub/software/scm/git/docs/git-pull.html)
