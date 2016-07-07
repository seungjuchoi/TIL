Node.js
=========
### History
* 구글 크롬이 이렇게 무서운 속도로 퍼지게 된 것은 다름아닌 자바스크립트 처리 속도 때문 (V8 자바스크립트 엔진(http://code.google.com/p/v8)
* 기존 자바스크립트 엔진들은 자바스크립트를 바이트코드로 변환하거나 인터프리트하여 처리했지만 V8은 JIT(Just In Time) 컴파일 방식을 사용하여 성능을 획기적으로 개선
* 구글의 V8 자바스크립트 엔진을 웹 브라우저가 아닌 서버로 사용할 수 있도록 만든 것이 Node.js
* 단일 스레드 Non-bloking I/O로 빠른 성능

### **NonBlocking** vs Blocking
* Blocking 방식은 한 줄이 끝나야 다음 줄이 실행
* 함수를 매개변수로 전달하는 부류가 Non-blocking 방식으로 동작하고, 함수 호출 후 리턴값을 받거나 그냥 함수만 호출하는 부류는 Blocking 방식으로 실행


### 참고
* http://pyrasis.com/nodejs/nodejs-HOWTO
