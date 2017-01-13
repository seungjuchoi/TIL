## Kramdown Syntax
- 문단 바꾸기는 Enter 두번이다. 한번은 개행문자 역할도 못하고 무시된다.
- 번호 문단 사이에 다른 문단이 등장하면 2번째 문단은 번호가 초기화 된다.
- 문단의 시작이 \`\`\`가 아니면 code block으로 인식하지 않는다. 중간에 써봤자 inline화 된다.
- Bold를 의미하는 문자 후에는 한칸 띈다.
- 줄바꿈 문자는 space 두번 후 Enter다. GFM에서는 Enter 한번으로 가능하다.
- Code block을 indent하고 싶다면 4개의 extra space를 앞쪽에 두자.

## Github Flavored Markdown
- 줄바꿈을 한번만 해도 강제개행을 할 수 있다.
- do_this_and_do_that과 같은 형태의 단어를 기울임꼴 글자로 처리하지 않는다.
- URL을 자동으로 링크로 변환해준다.
- 코드를 입력할 때 ` ``` ` 로 감싸주는 문법이 추가되었다.
- 문법 강조가 적용이 된다. Linguist를 이용하여 처리한다.
- 작업 목록(Task lists) 문법이 추가되었다.
- Git관련 링크를 자동으로 처리해준다. (SHA, 사용자, 이슈 등)
