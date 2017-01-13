## Syntax Tips
- 문단 바꾸기는 Enter 두번이다. 한번은 개행문자 역할도 못하고 무시된다.
- 번호 문단 사이에 다른 문단이 등장하면 2번째 문단은 번호가 초기화 된다.
- 문단의 시작이 \`\`\`가 아니면 code block으로 인식하지 않는다. 중간에 써봤자 inline화 된다.
- Bold를 의미하는 문자 후에는 한칸 띈다.
- 줄바꿈 문자는 space 두번 후 Enter다. GFM에서는 Enter 한번으로 가능하다.
- Code block을 indent하고 싶다면 4개의 extra space를 앞쪽에 두자.
        ```
        1. Working directory에 작업중인 코드 변경 사항에 영향을 주지 않고 다른 작업을 하고 싶을 때

               ```bash
               $ git log
               ```
        ```
