## Jupyter
- A web application
- ipython note에서 Jupyter note로 대체되었음.

## LaTeX
- 논문 쓰기용 document system이라고 생각하면 됨. Tex 시스템을 계승하여 Lamport가 만들었기에 La가 추가로 붙음. LaTeX is user-friendly extension of TeX
- A document preparation system for high-quality typesetting.
- It is most often used for medium-to-large technical or scientific documents but it can be used for almost any form of publishing.
- 작성자가 문서의 모양에 너무 많은 신경을 쓰지 않도록 하는 것이 Goal이다. 디자인은 문서 디자인에게 맡기자는 것.
- 예시)
```
\documentclass{article}
\title{Cartesian closed categories and the price of eggs}
\author{Jane Doe}
\date{September 1994}
\begin{document}
   \maketitle
   Hello world!
\end{document}
```
### LaTeX/Mathematics
- Typesetting mathematics is one of LaTeX's greatest strengths
- \(...\) 혹은	\[...\] 와 같은 구분자를 사용한다.
- '$$...$$'의 구분자를 사용하는 것을 피하라고 권장한다.

Type|Inline | Displayed equations
--------|-------|-------
LaTeX shorthand |	'\(...\)' |	'\[...\]'
TeX shorthand |	$...$ |	$$...$$

- 참고: https://en.wikibooks.org/wiki/LaTeX/Mathematics


## Tex
- computer program for typesetting documents이다. Latex의 전신이다.
- 문자 파일을 읽어 print의 형태로 출력해준다.
- Markup language로 Donald Knuth에 의해 1978년에 공개
- 사용예는 $$를 사용하고 역슬래쉬와command를 사용하는 것이 특징
```
The quadratic formula is $-b \pm \sqrt{b^2 - 4ac} \over 2a$ \bye
The quadratic formula is $$-b \pm \sqrt{b^2 - 4ac} \over 2a$$ \bye
```

## WYSIWYG
- An acronym for "what you see is what you get"
- In computing, a WYSIWYG editor is a system in which content (text and graphics) can be edited in a form closely resembling its appearance when printed or displayed as a finished product, such as a printed document, web page, or slide presentation.
- WYSIWYG implies the ability to directly manipulate the layout of a document without having to type or remember names of layout commands.
- 문서를 작성하면서 변경한 Text와 사진 따위를 볼 수 있도록 한 system. 실제 인쇄할 대상을 화면으로 보면서 작업하도록 한다.

## MathJax
MathJax is an open-source JavaScript display engine for LaTeX, MathML, and AsciiMath notation
'$$...$$'와 같은 수학 구분자를 parsing해서 보기좋게 display하는 것이 목적
http://www.mathjax.org/

## kramdown
- Github에서 지원하는 공식 markdown framework이다.
- https://kramdown.gettalong.org

## 정리
- 목적: github page + jekyll로 만든 blog에서 math를 예쁘게 표현하고 싶다.
- 1차 분석
  * latex를 지원하면서 가장 널리 사용되는 mathjax JavaScript engine을 사용해서 표현해야한다.
  * github는 알아시피 jekyll를 지원하는데 jekyll에서는 math support를 지원한다. 링크에 맞춰 환경설정한다. [링크](https://jekyllrb.com/docs/extras/#math-support)
  * 위 내용은 kramdown에서 Latex를 PNG로 lendering하는 것을 지원하는 데 Kramdown documentation에 사용법이 적혀 있다.
- 2차 분석
  * kramdown은 내부적으로 Latex를 지원하는 데 그 사용법은 아래와 같다. 그리고! 이 방법은 표준이 아니라서 헤깔릴 수 있으니 알아둬야 한다!
  * $$로 묶어서 latex 문법을 사용하면 된다.그러면 mathjax가 lendering을 한다.
```
$$
\begin{align*}
  & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
  = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
  & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
      \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
      \vdots & \ddots & \vdots \\
      \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
    \end{array} \right)
  \left( \begin{array}{c}
      y_1 \\
      \vdots \\
      y_n
    \end{array} \right)
\end{align*}
$$
```
$$
\begin{align*}
  & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
  = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
  & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
      \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
      \vdots & \ddots & \vdots \\
      \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
    \end{array} \right)
  \left( \begin{array}{c}
      y_1 \\
      \vdots \\
      y_n
    \end{array} \right)
\end{align*}
$$
  * table 구분자 |를 사용하므로 이 대신 \vert를 사용해아 한다.
  * github page에서는 위 code가 정상적으로 동작하나 github.com에서 직접 파일을 보면 제대로 보이진 않는다. 이 page에서는 mathjax가 동작하지 않는다.
  * 관련 내용: https://kramdown.gettalong.org/syntax.html#math-blocks
