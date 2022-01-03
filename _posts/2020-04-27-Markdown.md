---
title: Markdown
date: 2020-04-27 13:00:00 +0900
categories: [Grind, Stub]
tags: [markdown]
seo:
  date_modified: 2020-04-29 17:30:19 +0900
---

## Markdown

-  마크업 언어의 일종으로, 존 그루버(John Gruber)와 아론 스워츠(Aaron Swartz)가 만들었다고 함. 아래 공식사이트도 저사람꺼인듯.
- 처음엔 뭔가 했는데 요즘엔 .txt보다 .md가 편하다.
    - 쓰는곳이 많기도하고
    - 대부분의 툴에서 알아먹기도 하고 
    - 쓰기쉬운데 이쁘게 써지기까지함.
- 공홈가보면 뭐 설치하고 어쩌고 내가 알던것보다 훨 복잡한것같은데 그냥 vs code깔고 행복하게 써야겠다.
- 써보면 편한데 쓰면서 문법 찾기가 귀찮음. 은 이 블로그 만든 목적이니 정리해보기로함.
- html 이랑 스까쓰는것같은데 몰라그런건. 나중에 필요하면 추가해봄.
- 문법은 공홈을 기본으로 하고 필요한거 그때그때 추가.
- 근데 로컬에서 쓰면서 볼때랑 웹에서 볼때랑 결과가 좀 다름 ... 이건 왜이러는지 모르겠음

## 단락, 헤더, 인용(Paragraphs, Headers, Blockquotes)
- 단락, 헤더는 비슷한 목적으로 쓰이는것같음     
    ```    
    <h1 data-toc-skip>H1</h1>

    <h2 data-toc-skip>H2</h2>

    <h3 data-toc-skip>H3</h3>

    #### H4

    ##### H5

    ###### H6

    ####### plane text

    ```
<h1 data-toc-skip>H1</h1>

<h2 data-toc-skip>H2</h2>

<h3 data-toc-skip>H3</h3>

#### H4

##### H5

###### H6

####### plane text

- 인용은 그저 인용
    ```
    > 인용 쓸때 
    ```
    >인용 쓸때

- 블록처리는 한텝 들어가면 해줌 아니면  
    ```
    내용
        
        블록내용

    다시 내용
    ```
    내용
        
        블록내용

    다시 내용

- 줄바꿈은 spacebar 두번

## Line
- 말그대로 선긋기
- 위 header에서 H1은 지절로 따라옴 나머지는 ㄴㄴ
- *, - 최소 3개면 되고 띄어써도 알아먹음. 
    ```
        * * *

        ***

        *****

        - - -

        ---------------------------------------
    ```

    * * *

    ***

    *****

    - - -

    ---------------------------------------

## 강조(PHRASE EMPHASIS)
- Markdown uses asterisks(*) and underscores(_) to indicate spans  
    ```
    *기울임*
    _기울임_

    **Bold**
    __Bold__

    ***스까***
    ___스까___
    ```
    *기울임*  
    _기울임_  

    **Bold**  
    __Bold__  

    ***스까***  
    ___스까___  

## LISTS
- Unordered (bulleted) lists use asterisks, pluses, and hyphens (*, +, and -) as list markers. These three markers are interchangable; this:
- 아래 같은식으로 위 문자 어떤걸 쓰던 지가 알아서 바꿔줌
- tab으로 알아서 들어가면서 또 바뀜
    
    ```
    * 집에 
    + 가고 
    - 싶다
        - 지금
            - 당장
                - 빠르게
    
    ```
    
    * 집에 
    + 가고 
    - 싶다
        - 지금
            + 당장
                - 빠르게
        

- Ordered (numbered) lists use regular numbers, followed by periods, as list markers:
    
    ``` 
    1. 집에
    2. 가면
    3. 도타

        오지게  
        해야지

    1. ezwp
    ```

    1. 집에
    2. 가면
    3. 도타

        오지게  
        해야지

    1. ezwp
    - 특이한건 번호 순서 상관없이 순차로 써짐. 사실 앞이 전부 1. 이어도 알아서 해줌
    - 또 한 항목이 여러 문단으로 된 경우 저 렇게 써야함 위는 "도타" 랑 "오지게 해야지"랑 같은항목 다른 문단임.

<!-- If you put blank lines between items, you’ll get <p> tags for the list item text. You can create multi-paragraph list items by indenting the paragraphs by 4 spaces or 1 tab:

*   A list item.

    With multiple paragraphs.

*   Another item in the list. -->
<!-- 
Output:

<ul>
<li><p>A list item.</p>
<p>With multiple paragraphs.</p></li>
<li><p>Another item in the list.</p></li>
</ul> -->

## LINKS
- 그냥 써도 되고 앞에[ ]로 감싸면 별칭가능

    ```
    http://example.com/
    [example](http://example.com/)
    ```

    - http://example.com/
    - [example](http://example.com/)

- 다음처럼 쓰면 마우스 오버 팝업으로 쓸 문구 설정가능

    ```
    http://example.com/
    [example](http://example.com/ "애지간치 사부작대네 씨벌년이")
    ```

    - http://example.com/
    - [example](http://example.com/ "애지간치 사부작대네 씨벌년이")

- 참조형 링크(Reference-style links) 다른곳에 링크 써놓고 그 링크 참조할수 있음. 이렇게 하면 노출이 안되네 오옹

    ```
    I get 10 times more traffic from [Google][1] than from [Yahoo][2] or [MSN][3].

    [1]: http://google.com/        "Google"
    [2]: http://search.yahoo.com/  "Yahoo Search"
    [3]: http://search.msn.com/    "MSN Search"
    ```

    I get 10 times more traffic from [Google][1] than from [Yahoo][2] or [MSN][3].

    [1]: http://google.com/        "Google"
    [2]: http://search.yahoo.com/  "Yahoo Search"  
    [3]: http://search.msn.com/    "MSN Search"

- title 속성은 선택 사항임. 링크 이름에는 문자, 숫자 및 공백이 포함될 수 있지만 대소 문자를 구분 하지 않음.

    ```
    I start my morning with a cup of coffee and
    [The New York Times][NY Times].

    [ny times]: http://www.nytimes.com/
    ```

    I start my morning with a cup of coffee and
    [The New York Times][NY Times].

    [ny times]: http://www.nytimes.com/

- 문서 내 링크 - footnote
    - 각주(footnote) 용도 
    - 공식적으로 되는건 아님(?)
    
        ```
        Bla bla <sup id="a1">[1](#f1)</sup>
        <b id="f1">1</b> Footnote content here. [↩](#a1)
        ```

        Bla bla <sup id="a1">[1](#f1)</sup>  
        <b id="f1">1</b> Footnote content here. [↩](#a1)

    -  Test <sup id="a2">[2](#f2)</sup>
    -  gitlab나 github에는 각각 전용 md 문법이 따로 더 있는것같은데 gitlab는 footnote하는게 있는것같음 근데 github엔없음
    - 위 방법은 html 방법 스까다가 쓸수 있게 만든듯 애초에 이게 html호환 어쩌고 하면서 만들어져서 가능한가본데 암튼 다른 답변들은 footnote로 이동만 써있던데 이건 다시 돌아오는 링크까지 있어서 가져옴

## IMAGES
- 링크랑 비슷함.
- local image

    ```
    ![avatar](../assets/img/sample/avatar.jpg "Title")
    
    Reference-style:
    ![alt text][id]

    [id]: ../assets/img/sample/avatar.jpg "Title"
    ```

    ![avatar](../assets/img/sample/avatar.jpg "Title")  
    Reference-style:

    ![alt text][local]

    [local]: ../assets/img/sample/avatar.jpg "Title"
    - 절대경로는 안되고 상대경로로 해야 알아먹음. 는 웹에서는 또 안보임....
- web image
    ```
    ![avatar](https://www.google.com/images/branding/googlelogo/1x/googlelogo_color_272x92dp.png "Title")
    
    Reference-style:
    ![alt text][id]

    [id]: https://www.google.com/images/branding/googlelogo/1x/googlelogo_color_272x92dp.png "Title"
    ```

    ![avatar](https://www.google.com/images/branding/googlelogo/1x/googlelogo_color_272x92dp.png "Title")  
    Reference-style:

    ![alt text][web]

    [web]: https://www.google.com/images/branding/googlelogo/1x/googlelogo_color_272x92dp.png "Title"

## CODE
- 딴거만 써있길래 .. html 인것같은데 사실 여기 내용 잘 모르겠음
- 코드 쓸때 역따옴표? 세개 가 기본 옆에 언어 이름을 쓰면 그 언어에 맞게 하이라이팅(?) 해줌
- 언어 이름은 소문자로 해야함. 대문자로 해도 그냥 미리보기는 알아먹는것같은데 웹에서는 못하는것같음.
    ```
    (```
    그냥 snipet??

    ```)
    ```
    ```
    그냥 snipet??
    ```

    c# 
    ```
    (```c#
    private void mtd()
    {
        
    }
    ```)
    ```
    ```c#
    private void mtd()
    {
        
    }
    ```

## Table
- vertical bar 로 column 구분, 한줄이 row
- 굳이 column width 맞출 필요는 없음
- 쓸때는 좀 그래보여도 결과는 이쁨
    ```
    |             |ref             |out|
    |:---         |:---            |:---|
    |변수 초기화  | 호출 전 초기화  | 상관없음
    |변수 사용    | 사용 가능      | 사용 불가
    |반환(!return)| 상관없음       | 있어야함
    ```

|             |ref             |out|
|:---         |:---            |:---|
|변수 초기화  | 호출 전 초기화  | 상관없음
|변수 사용    | 사용 가능      | 사용 불가
|반환(!return)| 상관없음       | 있어야함
    
## CheckBox
- 에디터상에서는 안보였는데 웹에서는 보임 vs code로 쓰는데 md 관련 확장을 안깔아서 안보이나 싶기도하고 암튼 유용함
    ```
        - [ ] 가능
        - [x] 쌉가능
    ```
    - [ ] 가능
    - [x] 쌉가능

## 참고
- [공식 사이트](https://daringfireball.net/)  
- [footnote](https://stackoverflow.com/questions/25579868/how-to-add-footnotes-to-github-flavoured-markdown#comment85410120_25579868)
- <b id="f2">2</b> Test content here. [↩](#a2)