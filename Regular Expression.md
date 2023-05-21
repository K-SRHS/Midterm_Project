# 정규 표현식 (Regular Expression)<hr>
## 정규표현식 정의<br>
정규 표현식(Regular Expressions)은 복잡한 문자열을 처리할 때 사용하는 기법으로, 파이썬만의 고유 문법이 아니라 문자열을 처리하는 모든 곳에서 사용한다. (정규 표현식은 줄여서 간단히 "정규식"이라고도 말한다.)

## 정규표현의 필요성
### 1. 문자열 검색 및 매칭
정규 표현식은 특정 패턴을 가진 문자열을 검색하고 매칭할 수 있다. 이는 텍스트 데이터에서 원하는 정보를 추출하거나 특정 패턴을 찾는 데 매우 유용하다.<br>

### 2. 데이터 유효성 검증
정규 표현식을 사용하여 입력된 데이터의 유효성을 검증할 수 있다. 데이터의 형식이 특정한 규칙을 따르는지 확인하거나, 특정한 조건을 만족하는지 확인하는 등의 작업에 사용된다.

### 3. 문자열 처리 및 대체
정규 표현식은 문자열 처리와 대체 작업에 유용하다. 텍스트 데이터에서 특정 패턴을 다른 문자열로 대체하거나, 특정 패턴에 맞는 문자열을 추출하거나 분리할 수 있다. 이를 통해 데이터를 변형하거나 필요한 정보를 추출하는 작업을 수행할 수 있다.<br>

## 메타문자란?
원래 그 문자가 가진 뜻이 아닌 특별한 용도로 사용하는 문자 
* . ^ $ * + ? { } [ ] \ | ( )
정규 표현식에 위 메타 문자를 사용하면 특별한 의미를 갖게 된다.
### 문자 클래스 [ ]
- 문자 클래스로 만들어진 정규식은 "[ ] 사이의 문자들과 매치"라는 의미를 갖는다.
- 문자 클래스를 만드는 메타 문자인 [ ] 사이에는 어떤 문자도 들어갈 수 있다.
- 정규 표현식이 [abc]라면 이 표현식의 의미는 "a, b, c 중 한 개의 문자와 매치"를 뜻한다.
    * "a"는 정규식과 일치하는 문자인 "a"가 있으므로 매치
    * "before"는 정규식과 일치하는 문자인 "b"가 있으므로 매치
    * "dude"는 정규식과 일치하는 문자인 a, b, c 중 어느 하나도 포함하고 있지 않으므로 매치되지 않음

- [  ]안의 두 문자 사이에 하이픈(-)을 사용하면 두 문자 사이의 범위(From - To)를 의미
    * [a-c] = [abc]
    * [0-5] = [012345]
    * [a-zA-Z] : 알파벳 모두
    * [0-9] : 숫자
- ※문자 클래스 안에 ^ 메타 문자를 사용할 경우에는 반대(not)라는 의미를 갖는다.
    * [^0-9] = 숫자가 아닌 문자만 매치

자주 사용하는 문자 클래스

    \d - 숫자와 매치, [0-9]와 동일한 표현식
    \D - 숫자가 아닌 것과 매치, [^0-9]와 동일한 표현식
    \s - whitespace 문자와 매치, [ \t\n\r\f\v]와 동일한 표현식, 맨 앞의 빈 칸은 공백문자(space)를 의미
    \S - whitespace 문자가 아닌 것과 매치, [^ \t\n\r\f\v]와 동일한 표현식
    \w - 문자+숫자(alphanumeric)와 매치, [a-zA-Z0-9_]와 동일한 표현식
    \W - 문자+숫자(alphanumeric)가 아닌 문자와 매치, [^a-zA-Z0-9_]와 동일한 표현식
    대문자로 사용된 것은 소문자의 반대임을 추측할 수 있다.

### Dot(.)
줄바꿈 문자인 \n을 제외한 모든 문자와 매치됨을 의미
a.b = "a + 모든문자 + b"
   - "aab"는 가운데 문자 "a"가 모든 문자를 의미하는 .과 일치하므로 정규식과 매치됨
   - "a0b"는 가운데 문자 "0"가 모든 문자를 의미하는 .과 일치하므로 정규식과 매치됨
   - "abc"는 "a"문자와 "b"문자 사이에 어떤 문자라도 하나는있어야 하는 이 정규식과 일치하지 않으므로 매치되지 않음
a[.]b = "a + Dot(.)문자 + b"
   - 정규식 a[.]b는 "a.b" 문자열과 매치되고, "a0b" 문자열과는 매치되지 않는다.
   - 문자 클래스([]) 내에 Dot(.) 메타 문자가 사용된다면 이것은 "모든 문자"라는 의미가 아닌 문자 . 그대로를 의미

### 반복 (*)
ca*t = * 바로 앞에 있는 문자 a가 0부터 무한대로 반복될 수 있다는 의미
 - ex) ct, cat, caaat 등

### 반복 (+)
ca+t = 최소 1번 이상 반복될 때 사용(*가 반복 횟수 0부터라면 +는 반복 횟수 1부터인 것)
 - ex) cat, caaat 등 (ct는 "a"가 0번 반복되어 매치되지 않음)

### 반복 ({m,n}, ?)
- { } 메타 문자를 사용하면 반복 횟수를 고정할 수 있다. {m, n} 정규식을 사용하면 반복 횟수가 m부터 n까지 매치할 수 있다. 또한 m 또는 n을 생략할 수도 있다. 만약 {3,}처럼 사용하면 반복 횟수가 3 이상인 경우이고 {,3}처럼 사용하면 반복 횟수가 3 이하를 의미한다. 생략된 m은 0과 동일하며, 생략된 n은 무한대(2억 개 미만)의 의미를 갖는다. ({1,}은 +와 동일하고, {0,}은 *와 동일하다.)
- 반복예제
  * ca{2}t = caat
  * ca{2,5}t = caat, caaaaat 등
  * ? (ab?c = "a + b(있어도 되고 없어도 된다) + c")
    + ex) ab?c = abc, ac

### |
| 메타 문자는 or과 동일한 의미로 사용된다. A|B라는 정규식이 있다면 A 또는 B라는 의미가 된다.

    >>> p = re.compile('Crow|Servo')
    >>> m = p.match('CrowHello')
    >>> print(m)
    <re.Match object; span=(0, 4), match='Crow'>

### ^
^는 문자열의 맨 처음과 일치함을 의미한다.

    >>> print(re.search('^Life', 'Life is too short'))
    <re.Match object; span=(0, 4), match='Life'>
    >>> print(re.search('^Life', 'My Life'))
    None

^Life 정규식은 Life 문자열이 처음에 온 경우에는 매치하지만 처음 위치가 아닌 경우에는 매치되지 않음

### $
$는 문자열의 끝과 매치함을 의미한다.

    >>> print(re.search('short$', 'Life is too short'))
    <re.Match object; span=(12, 17), match='short'>
    >>> print(re.search('short$', 'Life is too short, you need python'))
    None

short$ 정규식은 검색할 문자열이 short로 끝난 경우에는 매치되지만 그 이외의 경우에는 매치되지 않음

### \A
\A는 문자열의 처음과 매치됨을 의미한다. ^ 메타 문자와 동일한 의미이지만 re.MULTILINE 옵션을 사용할 경우 줄과 상관없이 전체 문자열의 처음하고만 매치된다.(^은 각 줄의 문자열의 처음과 매치)

### \Z
\Z는 문자열의 끝과 매치됨을 의미한다. $ 메타 문자와 동일한 의미이지만 re.MULTILINE 옵션을 사용할 경우 줄과 상관없이 전체 문자열의 끝하고만 매치된다.($은 각 줄의 문자열의 처음과 매치)

### \b
단어 구분자(Word boundary)로 보통 단어는 whitespace에 의해 구분된다.

    >>> p = re.compile(r'\bclass\b')
    >>> print(p.search('no class at all'))  
    <re.Match object; span=(3, 8), match='class'>

\bclass\b 정규식은 앞뒤가 whitespace로 구분된 class라는 단어와 매치됨을 의미한다. 따라서 no class at all의 class라는 단어와 매치됨을 확인할 수 있다.

    >>> print(p.search('the declassified algorithm'))
    None
    >>> print(p.search('one subclass is'))
    None
    위 예제들의 문자열 안에도 class 문자열이 포함되어 있긴 하지만 whitespace로 구분된 단어가 아니므로 매치되지 않는다

\b는 파이썬 리터럴 규칙에 의하면 백스페이스(BackSpace)를 의미하므로 백스페이스가 아닌 단어 구분자임을 알려 주기 위해 r'\bclass\b'처럼 Raw string임을 알려주는 기호 r을 반드시 붙여 주어야 한다.

### \B
\B 메타 문자는 \b 메타 문자와 반대로 whitespace로 구분된 단어가 아닌 경우에만 매치된다.

    >>> p = re.compile(r'\Bclass\B')
    >>> print(p.search('no class at all'))  
    None
    >>> print(p.search('the declassified algorithm'))
    <re.Match object; span=(6, 11), match='class'>
    >>> print(p.search('one subclass is'))
    None

class 단어의 앞뒤에 whitespace가 하나라도 있는 경우에는 매치가 안 되는 것을 확인할 수 있다.

## 파이썬에서 정규 표현식을 지원하는 re 모듈
파이썬은 정규 표현식을 지원하기 위해 re(regular expression의 약어) 모듈을 제공한다. re 모듈은 파이썬을 설치할 때 자동으로 설치되는 표준 라이브러리이다.

    ex) 정규표현식'ab*'을 컴파일하여 패턴 객체 'p'를 생성하는 예제
    >>> import re
    >>> p = re.compile('ab*')

## 정규식을 이용한 문자열 검색
컴파일된 패턴 객체를 사용하여 문자열 검색을 수행하는 4가지 메서드
|Method|목적|
|-----|-----|
|match()|문자열의 처음부터 정규식과 매치되는지 조사한다.|
|search()|문자열 전체를 검색하여 정규식과 매치되는지 조사한다.|
|findall()|정규식과 매치되는 모든 문자열(substring)을 리스트로 리턴한다.|
|finditer()|정규식과 매치되는 모든 문자열(substring)을 반복 가능한 객체로 리턴한다.|

match, search는 정규식과 매치될 때는 match 객체를 리턴하고, 매치되지 않을 때는 None을 리턴한다.


우선 다음과 같은 패턴이 존재한다고 가정한다.

    >>> import re
    >>> p = re.compile('[a-z]+')

### match

    >>> m = p.match("python")
    >>> print(m)
    <re.Match object; span=(0, 6), match='python'>
    
"python" 문자열은 [a-z]+ 정규식에 부합되므로 match 객체를 돌려준다.

    >>> m = p.match("3 python")
    >>> print(m)
    None

"3 python" 문자열은 처음에 나오는 문자 3이 정규식 [a-z]+에 부합되지 않으므로 None을 돌려준다.

### search

    >>> m = p.search("python")
    >>> print(m)
    <re.Match object; span=(0, 6), match='python'>

"python" 문자열에 search 메서드를 수행하면 match 메서드를 수행했을 때와 동일하게 매치된다.

    >>> m = p.search("3 python")
    >>> print(m)
    <re.Match object; span=(2, 8), match='python'>

"3 python" 문자열의 첫 번째 문자는 "3"이지만 search는 문자열의 처음부터 검색하는 것이 아니라 문자열 전체를 검색하기 때문에 "3 " 이후의 "python" 문자열과 매치된다.

### findall

    >>> result = p.findall("life is too short")
    >>> print(result)
    ['life', 'is', 'too', 'short']

findall은 패턴([a-z]+)과 매치되는 모든 값을 찾아 리스트로 리턴한다.

### finditer

    >>> result = p.finditer("life is too short")
    >>> print(result)
    <callable_iterator object at 0x01F5E390>
    >>> for r in result: print(r)
    ...
    <re.Match object; span=(0, 4), match='life'>
    <re.Match object; span=(5, 7), match='is'>
    <re.Match object; span=(8, 11), match='too'>
    <re.Match object; span=(12, 17), match='short'>

finditer는 findall과 동일하지만 그 결과로 반복 가능한 객체(iterator object)를 리턴한다. 그리고 반복 가능한 객체가 포함하는 각각의 요소는 match 객체이다.

## match 객체의 메서드
|Method|목적|
|-----|-----|
|group()|매치된 문자열을 리턴한다.|
|start()|매치된 문자열의 시작 위치를 리턴한다.|
|end()|매치된 문자열의 끝 위치를 리턴한다.|
|span()|매치된 문자열의 (시작, 끝)에 해당하는 튜플을 리턴한다.|

match 메서드

    >>> m = p.match("python")
    >>> m.group()
    'python'
    >>> m.start()
    0
    >>> m.end()
    6
    >>> m.span()
    (0, 6)

search 메서드

    >>> m = p.search("3 python")
    >>> m.group()
    'python'
    >>> m.start()
    2
    >>> m.end()
    8
    >>> m.span()
    (2, 8)

### 모듈 단위로 수행하기

    >>> p = re.compile('[a-z]+')
    >>> m = p.match("python")

위 코드가 축약된 형태는 다음과 같다.

    >>> m = re.match('[a-z]+', "python")

위 예처럼 사용하면 컴파일과 match 메서드를 한 번에 수행할 수 있다. 보통 한 번 만든 패턴 객체를 여러번 사용해야 할 때는 이 방법보다 re.compile을 사용하는 것이 편하다.

## 컴파일 옵션
- DOTALL(S) : . 이 줄바꿈 문자를 포함하여 모든 문자와 매치할 수 있도록 한다.
- IGNORECASE(I) : 대소문자에 관계없이 매치할 수 있도록 한다.
- MULTILINE(M) : 여러줄과 매치할 수 있도록 한다. (^, $ 메타문자의 사용과 관계가 있는 옵션이다)
- VERBOSE(X) : verbose 모드를 사용할 수 있도록 한다. (정규식을 보기 편하게 만들수 있고 주석등을 사용할 수 있게된다.)

### DOTALL, S
. 메타 문자는 줄바꿈 문자(\n)를 제외한 모든 문자와 매치되는 규칙이 있지만, \n 문자도 포함하여 매치하고 싶다면 re.DOTALL 또는 re.S 옵션을 사용해 정규식을 컴파일하면 된다.

    >>> import re
    >>> p = re.compile('a.b')
    >>> m = p.match('a\nb')
    >>> print(m)
    None

정규식이 a.b인 경우 문자열 a\nb는 매치되지 않음을 알 수 있다. 왜냐하면 \n은 . 메타 문자와 매치되지 않기 때문이다. \n 문자와도 매치되게 하려면 다음과 같이 re.DOTALL 옵션을 사용해야 한다.

    >>> p = re.compile('a.b', re.DOTALL)
    >>> m = p.match('a\nb')
    >>> print(m)
    <re.Match object; span=(0, 3), match='a\nb'>

보통 re.DOTALL 옵션은 여러 줄로 이루어진 문자열에서 줄바꿈 문자에 상관없이 검색할 때 많이 사용한다.

### IGNORECASE, I
re.IGNORECASE 또는 re.I 옵션은 대소문자 구별 없이 매치를 수행할 때 사용하는 옵션

    >>> p = re.compile('[a-z]+', re.I)
    >>> p.match('python')
    <re.Match object; span=(0, 6), match='python'>
    >>> p.match('Python')
    <re.Match object; span=(0, 6), match='Python'>
    >>> p.match('PYTHON')
    <re.Match object; span=(0, 6), match='PYTHON'>

[a-z]+ 정규식은 소문자만을 의미하지만 re.I 옵션으로 대소문자 구별 없이 매치된다.

### MULTILINE, M
re.MULTILINE 옵션은 ^, $ 메타 문자를 문자열의 각 줄마다 적용해 주는 것
- ^python = 문자열의 처음은 항상 python으로 시작
- python$ = 문자열의 마지막은 항상 python으로 끝

    import re
    p = re.compile("^python\s\w+")
    
    data = """python one
    life is too short
    python two
    you need python
    python three"""
    
    print(p.findall(data))
    (^python\s\w+은 python이라는 문자열로 시작하고 그 뒤에 whitespace, 그 뒤에 단어가 와야 한다는 의미)
    실행결과 : ['python one']

(^ 메타 문자에 의해 python이라는 문자열을 사용한 첫 번째 줄만 매치되었기 때문)

    import re
    p = re.compile("^python\s\w+", re.MULTILINE)
    
    data = """python one
    life is too short
    python two
    you need python
    python three"""
    
    print(p.findall(data))
    실행결과 : ['python one', 'python two', 'python three']

re.MULTILINE 옵션으로 인해 ^ 메타 문자가 문자열 전체가 아닌 각 줄의 처음이라는 의미를 갖게 됨

### VERBOSE, X
정규식을 주석 또는 줄 단위로 구분

    charref = re.compile(r'&[#](0[0-7]+|[0-9]+|x[0-9a-fA-F]+);')

위 정규식을 쉽게 이해하기 위해 re.VERBOSE 옵션을 사용

    charref = re.compile(r"""
    &[#]                # Start of a numeric entity reference
     (
         0[0-7]+         # Octal form
       | [0-9]+          # Decimal form
       | x[0-9a-fA-F]+   # Hexadecimal form
     )
     ;                   # Trailing semicolon
    """, re.VERBOSE)

첫 번째와 두 번째 예를 비교해 보면 컴파일된 패턴 객체인 charref는 모두 동일한 역할을 한다. 하지만 정규식이 복잡할 경우 두 번째처럼 주석을 적고 여러 줄로 표현하는 것이 훨씬 가독성이 좋다는 것을 알 수 있다.

## 백슬래시 문제
"\section" 문자열을 찾기 위한 정규식을 만든다고 가정했을때 정규식 \section은 \s 문자가 whitespace로 해석되어 의도한 대로 매치가 이루어지지 않는다. 
위 표현은 다음과 동일한 의미이다.

    [ \t\n\r\f\v]ection
    
의도한 대로 매치하고 싶다면 다음과 같이 변경해야 한다.

    \\section

즉 위 정규식에서 사용한 \ 문자가 문자열 자체임을 알려 주기 위해 백슬래시 2개를 사용하여 이스케이프 처리를 해야 한다.

따라서 위 정규식을 컴파일하려면 다음과 같이 작성해야 한다.

    >>> p = re.compile('\\section')''

위처럼 정규식을 만들어서 컴파일하면 실제 파이썬 정규식 엔진에는 파이썬 문자열 리터럴 규칙에 따라 \\이 \로 변경되어 \section이 전달되어 정규식 엔진에 \\ 문자를 전달하려면 파이썬은 \\\\처럼 백슬래시를 4개나 사용해야 한다.

    >>> p = re.compile('\\\\section')

위와 같이 \를 사용한 표현이 계속 반복되는 정규식같은 문제를 해결하려면 Raw String을 사용해야 한다. 그 방법은 다음과 같다.

    >>> p = re.compile(r'\\section')

위와 같이 정규식 문자열 앞에 r 문자를 삽입하면 이 정규식은 Raw String 규칙에 의하여 백슬래시 2개 대신 1개만 써도 2개를 쓴 것과 동일한 의미를 갖게 된다.(백슬래시를 사용하지 않는 정규식이라면 r의 유무에 상관없이 동일한 정규식이 될 것이다.)