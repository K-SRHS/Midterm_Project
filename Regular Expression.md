# 정규 표현식 (Regular Expression)

## 목차
1. [정규표현식 정의](#정규표현식-정의)
2. [정규표현의 필요성](#정규표현의-필요성)
    - [문자열 검색 및 매칭](#문자열-검색-및-매칭)
    - [데이터 유효성 검증](#데이터-유효성-검증)
    - [문자열 처리 및 대체](#문자열-처리-및-대체)
3. [메타문자란?](#메타문자란)
    - [문자 클래스 [ ]](#문자-클래스--)
    - [Dot(.)](#dot)
    - [반복(*)](#반복-)
    - [반복(+)](#반복--1)
    - [반복 ({m,n}, ?)](#반복-mn-)
    - [메타문자 |](#메타문자)
    - [메타문자 ^](#메타문자-1)
    - [메타문자 $](#메타문자-2)
  

## 정규표현식 정의
정규 표현식(Regular Expressions)은 복잡한 문자열을 처리할 때 사용하는 기법으로, 파이썬만의 고유 문법이 아니라 문자열을 처리하는 모든 곳에서 사용한다. (정규 표현식은 줄여서 간단히 "정규식"이라고도 말한다.)

## 정규표현의 필요성
### 문자열 검색 및 매칭
정규 표현식은 특정 패턴을 가진 문자열을 검색하고 매칭할 수 있다. 이는 텍스트 데이터에서 원하는 정보를 추출하거나 특정 패턴을 찾는 데 매우 유용하다.

### 데이터 유효성 검증
정규 표현식을 사용하여 입력된 데이터의 유효성을 검증할 수 있다. 데이터의 형식이 특정한 규칙을 따르는지 확인하거나, 특정한 조건을 만족하는지 확인하는 등의 작업에 사용된다.

### 문자열 처리 및 대체
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
    + ab?c = abc, ac

### 메타문자 |
| 메타 문자는 or과 동일한 의미로 사용된다. A|B라는 정규식이 있다면 A 또는 B라는 의미가 된다.
```python
>>> p = re.compile('Crow|Servo')
>>> m = p.match('CrowHello')
>>> print(m)
<re.Match object; span=(0, 4), match='Crow'>
```
### 메타문자 ^
^는 문자열의 맨 처음과 일치함을 의미한다.
```python
print(re.search('^Life', 'Life is too short'))
<re.Match object; span=(0, 4), match='Life'>
print(re.search('^Life', 'My Life'))
None
```
^Life 정규식은 Life 문자열이 처음에 온 경우에는 매치하지만 처음 위치가 아닌 경우에는 매치되지 않음

### 메타문자 $
$는 문자열의 끝과 매치함을 의미한다.
```python
>>> print(re.search('short$', 'Life is too short'))
<re.Match object; span=(12, 17), match='short'>
>>> print(re.search('short$', 'Life is too short, you need python'))
None
```
short$ 정규식은 검색할 문자열이 short로 끝난 경우에는 매치되지만 그 이외의 경우에는 매치되지 않음

### \A
\A는 문자열의 처음과 매치됨을 의미한다. ^ 메타 문자와 동일한 의미이지만 re.MULTILINE 옵션을 사용할 경우 줄과 상관없이 전체 문자열의 처음하고만 매치된다.(^은 각 줄의 문자열의 처음과 매치)

### \Z
\Z는 문자열의 끝과 매치됨을 의미한다. $ 메타 문자와 동일한 의미이지만 re.MULTILINE 옵션을 사용할 경우 줄과 상관없이 전체 문자열의 끝하고만 매치된다.($은 각 줄의 문자열의 처음과 매치)

### \b
단어 구분자(Word boundary)로 보통 단어는 whitespace에 의해 구분된다.
```python
>>> p = re.compile(r'\bclass\b')
>>> print(p.search('no class at all'))  
<re.Match object; span=(3, 8), match='class'>
```
\bclass\b 정규식은 앞뒤가 whitespace로 구분된 class라는 단어와 매치됨을 의미한다. 따라서 no class at all의 class라는 단어와 매치됨을 확인할 수 있다.
```python
>>> print(p.search('the declassified algorithm'))
None
>>> print(p.search('one subclass is'))
None
```
위 예제들의 문자열 안에도 class 문자열이 포함되어 있긴 하지만 whitespace로 구분된 단어가 아니므로 매치되지 않는다   

\b는 파이썬 리터럴 규칙에 의하면 백스페이스(BackSpace)를 의미하므로 백스페이스가 아닌 단어 구분자임을 알려 주기 위해 r'\bclass\b'처럼 Raw string임을 알려주는 기호 r을 반드시 붙여 주어야 한다.

### \B
\B 메타 문자는 \b 메타 문자와 반대로 whitespace로 구분된 단어가 아닌 경우에만 매치된다.
```python
>>> p = re.compile(r'\Bclass\B')
>>> print(p.search('no class at all'))  
None
>>> print(p.search('the declassified algorithm'))
<re.Match object; span=(6, 11), match='class'>
>>> print(p.search('one subclass is'))
None
```
class 단어의 앞뒤에 whitespace가 하나라도 있는 경우에는 매치가 안 되는 것을 확인할 수 있다.

## 파이썬에서 정규 표현식을 지원하는 re 모듈
파이썬은 정규 표현식을 지원하기 위해 re(regular expression의 약어) 모듈을 제공한다. re 모듈은 파이썬을 설치할 때 자동으로 설치되는 표준 라이브러리이다.

ex) 정규표현식'ab*'을 컴파일하여 패턴 객체 'p'를 생성하는 예제
```python
>>> import re
>>> p = re.compile('ab*')
```
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
```python
>>> import re
>>> p = re.compile('[a-z]+')
```
### match
```python
>>> m = p.match("python")
>>> print(m)
<re.Match object; span=(0, 6), match='python'>
```   
"python" 문자열은 [a-z]+ 정규식에 부합되므로 match 객체를 돌려준다.
```python
>>> m = p.match("3 python")
>>> print(m)
None
```
"3 python" 문자열은 처음에 나오는 문자 3이 정규식 [a-z]+에 부합되지 않으므로 None을 돌려준다.

### search
```python
>>> m = p.search("python")
>>> print(m)
<re.Match object; span=(0, 6), match='python'>
```
"python" 문자열에 search 메서드를 수행하면 match 메서드를 수행했을 때와 동일하게 매치된다.
```python
>>> m = p.search("3 python")
>>> print(m)
<re.Match object; span=(2, 8), match='python'>
```
"3 python" 문자열의 첫 번째 문자는 "3"이지만 search는 문자열의 처음부터 검색하는 것이 아니라 문자열 전체를 검색하기 때문에 "3 " 이후의 "python" 문자열과 매치된다.

### findall
```python
>>> result = p.findall("life is too short")
>>> print(result)
['life', 'is', 'too', 'short']
```
findall은 패턴([a-z]+)과 매치되는 모든 값을 찾아 리스트로 리턴한다.

### finditer
```python
>>> result = p.finditer("life is too short")
>>> print(result)
<callable_iterator object at 0x01F5E390>
>>> for r in result: print(r)
...
<re.Match object; span=(0, 4), match='life'>
<re.Match object; span=(5, 7), match='is'>
<re.Match object; span=(8, 11), match='too'>
<re.Match object; span=(12, 17), match='short'>
```
finditer는 findall과 동일하지만 그 결과로 반복 가능한 객체(iterator object)를 리턴한다. 그리고 반복 가능한 객체가 포함하는 각각의 요소는 match 객체이다.

## match 객체의 메서드
|Method|목적|
|-----|-----|
|group()|매치된 문자열을 리턴한다.|
|start()|매치된 문자열의 시작 위치를 리턴한다.|
|end()|매치된 문자열의 끝 위치를 리턴한다.|
|span()|매치된 문자열의 (시작, 끝)에 해당하는 튜플을 리턴한다.|

match 메서드
```python
>>> m = p.match("python")
>>> m.group()
'python'
>>> m.start()
0
>>> m.end()
6
>>> m.span()
(0, 6)
```
search 메서드
```python
>>> m = p.search("3 python")
>>> m.group()
'python'
>>> m.start()
2
>>> m.end()
8
>>> m.span()
(2, 8)
```
### 모듈 단위로 수행하기
```python
>>> p = re.compile('[a-z]+')
>>> m = p.match("python")
```
위 코드가 축약된 형태는 다음과 같다.
```python
>>> m = re.match('[a-z]+', "python")
```
위 예처럼 사용하면 컴파일과 match 메서드를 한 번에 수행할 수 있다. 보통 한 번 만든 패턴 객체를 여러번 사용해야 할 때는 이 방법보다 re.compile을 사용하는 것이 편하다.

## 컴파일 옵션
- DOTALL(S) : . 이 줄바꿈 문자를 포함하여 모든 문자와 매치할 수 있도록 한다.
- IGNORECASE(I) : 대소문자에 관계없이 매치할 수 있도록 한다.
- MULTILINE(M) : 여러줄과 매치할 수 있도록 한다. (^, $ 메타문자의 사용과 관계가 있는 옵션이다)
- VERBOSE(X) : verbose 모드를 사용할 수 있도록 한다. (정규식을 보기 편하게 만들수 있고 주석등을 사용할 수 있게된다.)

### DOTALL, S
. 메타 문자는 줄바꿈 문자(\n)를 제외한 모든 문자와 매치되는 규칙이 있지만, \n 문자도 포함하여 매치하고 싶다면 re.DOTALL 또는 re.S 옵션을 사용해 정규식을 컴파일하면 된다.
```python
>>> import re
>>> p = re.compile('a.b')
>>> m = p.match('a\nb')
>>> print(m)
None
```
정규식이 a.b인 경우 문자열 a\nb는 매치되지 않음을 알 수 있다. 왜냐하면 \n은 . 메타 문자와 매치되지 않기 때문이다. \n 문자와도 매치되게 하려면 다음과 같이 re.DOTALL 옵션을 사용해야 한다.
```python
>>> p = re.compile('a.b', re.DOTALL)
>>> m = p.match('a\nb')
>>> print(m)
<re.Match object; span=(0, 3), match='a\nb'>
```
보통 re.DOTALL 옵션은 여러 줄로 이루어진 문자열에서 줄바꿈 문자에 상관없이 검색할 때 많이 사용한다.

### IGNORECASE, I
re.IGNORECASE 또는 re.I 옵션은 대소문자 구별 없이 매치를 수행할 때 사용하는 옵션
```python
>>> p = re.compile('[a-z]+', re.I)
>>> p.match('python')
<re.Match object; span=(0, 6), match='python'>
>>> p.match('Python')
<re.Match object; span=(0, 6), match='Python'>
>>> p.match('PYTHON')
<re.Match object; span=(0, 6), match='PYTHON'>
```
[a-z]+ 정규식은 소문자만을 의미하지만 re.I 옵션으로 대소문자 구별 없이 매치된다.

### MULTILINE, M
re.MULTILINE 옵션은 ^, $ 메타 문자를 문자열의 각 줄마다 적용해 주는 것
- ^python = 문자열의 처음은 항상 python으로 시작
- python$ = 문자열의 마지막은 항상 python으로 끝
```python
import re
p = re.compile("^python\s\w+")

data = """python one
life is too short
python two
you need python
python three"""

print(p.findall(data))
```
^python\s\w+은 python이라는 문자열로 시작하고 그 뒤에 whitespace, 그 뒤에 단어가 와야 한다는 의미   
실행결과 : ['python one']   
(^ 메타 문자에 의해 python이라는 문자열을 사용한 첫 번째 줄만 매치되었기 때문)
```python
import re
p = re.compile("^python\s\w+", re.MULTILINE)

data = """python one
life is too short
python two
you need python
python three"""

print(p.findall(data))
```
실행결과 : ['python one', 'python two', 'python three']

re.MULTILINE 옵션으로 인해 ^ 메타 문자가 문자열 전체가 아닌 각 줄의 처음이라는 의미를 갖게 됨

### VERBOSE, X
정규식을 주석 또는 줄 단위로 구분
```python
charref = re.compile(r'&[#](0[0-7]+|[0-9]+|x[0-9a-fA-F]+);')
```
위 정규식을 쉽게 이해하기 위해 re.VERBOSE 옵션을 사용
```python
charref = re.compile(r"""
&[#]                # Start of a numeric entity reference
 (
     0[0-7]+         # Octal form
   | [0-9]+          # Decimal form
   | x[0-9a-fA-F]+   # Hexadecimal form
 )
 ;                   # Trailing semicolon
""", re.VERBOSE)
```
첫 번째와 두 번째 예를 비교해 보면 컴파일된 패턴 객체인 charref는 모두 동일한 역할을 한다. 하지만 정규식이 복잡할 경우 두 번째처럼 주석을 적고 여러 줄로 표현하는 것이 훨씬 가독성이 좋다는 것을 알 수 있다.

## 백슬래시 문제
"\section" 문자열을 찾기 위한 정규식을 만든다고 가정했을때 정규식 \section은 \s 문자가 whitespace로 해석되어 의도한 대로 매치가 이루어지지 않는다. 
위 표현은 다음과 동일한 의미이다.

    [ \t\n\r\f\v]ection
    
의도한 대로 매치하고 싶다면 다음과 같이 변경해야 한다.

    \\section

즉 위 정규식에서 사용한 \ 문자가 문자열 자체임을 알려 주기 위해 백슬래시 2개를 사용하여 이스케이프 처리를 해야 한다.

따라서 위 정규식을 컴파일하려면 다음과 같이 작성해야 한다.
```python
>>> p = re.compile('\\section')''
```
위처럼 정규식을 만들어서 컴파일하면 실제 파이썬 정규식 엔진에는 파이썬 문자열 리터럴 규칙에 따라 \\이 \로 변경되어 \section이 전달되어 정규식 엔진에 \\ 문자를 전달하려면 파이썬은 \\\\처럼 백슬래시를 4개나 사용해야 한다.
```python
>>> p = re.compile('\\\\section')
```
위와 같이 \를 사용한 표현이 계속 반복되는 정규식같은 문제를 해결하려면 Raw String을 사용해야 한다. 그 방법은 다음과 같다.
```python
>>> p = re.compile(r'\\section')
```
위와 같이 정규식 문자열 앞에 r 문자를 삽입하면 이 정규식은 Raw String 규칙에 의하여 백슬래시 2개 대신 1개만 써도 2개를 쓴 것과 동일한 의미를 갖게 된다.(백슬래시를 사용하지 않는 정규식이라면 r의 유무에 상관없이 동일한 정규식이 될 것이다.)

## 그루핑
ABC 문자열이 계속해서 반복되는지 조사하는 정규식
```python
>>> p = re.compile('(ABC)+')
>>> m = p.search('ABCABCABC OK?')
>>> print(m)
<re.Match object; span=(0, 9), match='ABCABCABC'>
>>> print(m.group())
ABCABCABC
```
\w+\s+\d+[-]\d+[-]\d+은 이름 + " " + 전화번호 형태의 문자열을 찾는 정규식
```python
>>> p = re.compile(r"\w+\s+\d+[-]\d+[-]\d+")
>>> m = p.search("park 010-1234-1234")
```
이렇게 매치된 문자열 중에서 이름만 뽑아내고 싶을 때
```python
>>> p = re.compile(r"(\w+)\s+\d+[-]\d+[-]\d+")
>>> m = p.search("park 010-1234-1234")
>>> print(m.group(1))
park
```
이름에 해당하는 \w+ 부분을 그룹 (\w+)으로 만들면 match 객체의 group(인덱스) 메서드를 사용하여 그루핑된 부분의 문자열만 뽑아낼 수 있다.

group 메서드의 인덱스는 다음과 같은 의미를 갖는다.
|group(인덱스)|설명|
|-----|-----|
|group(0)|매치된 전체 문자열|
|group(1)|첫 번째 그룹에 해당되는 문자열|
|group(2)|두 번째 그룹에 해당되는 문자열|
|group(n)|n 번째 그룹에 해당되는 문자열|
```python
>>> p = re.compile(r"(\w+)\s+(\d+[-]\d+[-]\d+)")
>>> m = p.search("park 010-1234-1234")
>>> print(m.group(2))
010-1234-1234
```
전화번호 부분을 추가로 그룹 (\d+[-]\d+[-]\d+)로 만들었다. 이렇게 하면 group(2)처럼 사용하여 전화번호만 뽑아낼 수 있다.

전화번호 중에서 국번만 뽑아내고 싶으면 국번 부분을 또 그루핑하면 된다.
```python
>>> p = re.compile(r"(\w+)\s+((\d+)[-]\d+[-]\d+)")
>>> m = p.search("park 010-1234-1234")
>>> print(m.group(3))
010
```
위 예에서 볼 수 있듯이 (\w+)\s+((\d+)[-]\d+[-]\d+)처럼 그룹을 중첩되게 사용하는 것도 가능하다. 그룹이 중첩되어 있는 경우는 바깥쪽부터 시작하여 안쪽으로 들어갈수록 인덱스가 증가한다.

### 그루핑된 문자열 재참조하기
한 번 그루핑한 문자열은 재참조(Backreferences)할 수 있다
```python
>>> p = re.compile(r'(\b\w+)\s+\1')
>>> p.search('Paris in the the spring').group()
'the the'
```
정규식 (\b\w+)\s+\1은 (그룹) + " " + 그룹과 동일한 단어와 매치됨을 의미한다. 이렇게 정규식을 만들게 되면 2개의 동일한 단어를 연속적으로 사용해야만 매치된다. 이것을 가능하게 해주는 것이 바로 재참조 메타 문자인 \1이다. \1은 정규식의 그룹 중 첫 번째 그룹을 가리킨다. (두번째 그룹은 \2)

### 그루핑된 문자열에 이름 붙이기
정규식 안에 그룹이 무척 많아진다고 가정해 보자. 예를 들어 정규식 안에 그룹이 10개 이상만 되어도 매우 혼란스러울 것이다. 거기에 더해 정규식이 수정되면서 그룹이 추가, 삭제되면 그 그룹을 인덱스로 참조한 프로그램도 모두 변경해 주어야 하는 위험도 갖게 된다.

이러한 이유로 정규식은 그룹을 만들 때 그룹 이름을 지정할 수 있게 했다. 그 방법은 다음과 같다.

    (?P<name>\w+)\s+((\d+)[-]\d+[-]\d+)

위 정규식은 앞에서 본 이름과 전화번호를 추출하는 정규식이다. 기존과 달라진 부분은 다음과 같다.

    (\w+) --> (?P<name>\w+)

그룹에 이름을 지어 주려면 다음과 같은 확장 구문을 사용해야 한다.

    (?P<그룹명>...)

그룹에 이름을 지정하고 참조하는 다음 예
```python
>>> p = re.compile(r"(?P<name>\w+)\s+((\d+)[-]\d+[-]\d+)")
>>> m = p.search("park 010-1234-1234")
>>> print(m.group("name"))
park
```
name이라는 그룹 이름으로 참조할 수 있다.

## 전방 탐색
- 긍정형 전방 탐색((?=...)) - ... 에 해당되는 정규식과 매치되어야 하며 조건이 통과되어도 문자열이 소비되지 않는다.
- 부정형 전방 탐색((?!...)) - ...에 해당되는 정규식과 매치되지 않아야 하며 조건이 통과되어도 문자열이 소비되지 않는다.

### 긍정형 전방 탐색
```python
>>> p = re.compile(".+(?=:)")
>>> m = p.search("http://google.com")
>>> print(m.group())
http
```
정규식 중 :에 해당하는 부분에 긍정형 전방 탐색 기법을 적용하여 (?=:)으로 변경하였다. 이렇게 되면 기존 정규식과 검색에서는 동일한 효과를 발휘하지만 : 에 해당하는 문자열이 정규식 엔진에 의해 소비되지 않아(검색에는 포함되지만 검색 결과에는 제외됨) 검색 결과에서는 :이 제거된 후 돌려주는 효과가 있다.

### 부정형 전방 탐색

    .*[.](?!bat$).*$
    확장자가 bat가 아닌 경우에만 통과된다는 의미

    .*[.](?!bat$|exe$).*$
    exe도 제외하라는 조건이 추가

## 문자열 바꾸기
sub 메서드를 사용하면 정규식과 매치되는 부분을 다른 문자로 쉽게 바꿀 수 있다.
```python
>>> p = re.compile('(blue|white|red)')
>>> p.sub('colour', 'blue socks and red shoes')
'colour socks and colour shoes'
```
blue 또는 white 또는 red라는 문자열이 colour라는 문자열로 바뀜   

sub 메서드의 첫 번째 인수는 "바꿀 문자열(replacement), 두 번째 인수는 "대상 문자열"
```python
>>> p.sub('colour', 'blue socks and red shoes', count=1)
'colour socks and red shoes'
```
바꾸기 횟수를 제어하려면 다음과 같이 세 번째 인수에 count 값을 설정한다

- sub 메서드와 유사한 subn 메서드
  subn 역시 sub와 동일한 기능을 하지만 반환 결과를 튜플로 리턴한다는 차이가 있다. 리턴된 튜플의 첫 번째 요소는 변경된 문자열이고, 두 번째 요소는 바꾸기가 발생한 횟수이다.
```python
>>> p = re.compile('(blue|white|red)')
>>> p.subn( 'colour', 'blue socks and red shoes')
('colour socks and colour shoes', 2)
```
### sub 메서드 사용 시 참조 구문 사용하기
sub 메서드를 사용할 때 참조 구문을 사용할 수 있다.
```python
>>> p = re.compile(r"(?P<name>\w+)\s+(?P<phone>(\d+)[-]\d+[-]\d+)")
>>> print(p.sub("\g<phone> \g<name>", "park 010-1234-1234"))
010-1234-1234 park
```
이름 + 전화번호의 문자열을 전화번호 + 이름으로 바꾸는 예
   
그룹 이름 대신 참조 번호를 사용해도 마찬가지 결과를 돌려준다.
```python
>>> p = re.compile(r"(?P<name>\w+)\s+(?P<phone>(\d+)[-]\d+[-]\d+)")
>>> print(p.sub("\g<2> \g<1>", "park 010-1234-1234"))
010-1234-1234 park
```
### sub 메서드의 매개변수로 함수 넣기
sub 메서드의 첫 번째 인수에 함수를 전달할 수도 있다.

hexrepl = match 객체(위에서 숫자에 매치되는)를 입력으로 받아 16진수로 변환하여 돌려주는 함수
```python
>>> def hexrepl(match):
...     value = int(match.group())
...     return hex(value)
...
>>> p = re.compile(r'\d+')
>>> p.sub(hexrepl, 'Call 65490 for printing, 49152 for user code.')
'Call 0xffd2 for printing, 0xc000 for user code.'
```
sub의 첫 번째 인수로 함수를 사용할 경우 해당 함수의 첫 번째 매개변수에는 정규식과 매치된 match 객체가 입력된다. 그리고 매치되는 문자열은 함수의 리턴 값으로 바뀌게 된다.

## Non-Greedy
non-greedy 문자인 ?는 *?, +?, ??, {m,n}?와 같이 사용할 수 있다.
```python
>>> s = '<html><head><title>Title</title>'
>>> len(s)
32
>>> print(re.match('<.*>', s).span())
(0, 32)
>>> print(re.match('<.*>', s).group())
<html><head><title>Title</title>
```
non-greedy 문자를 사용하여 html 문자열을 돌려받는법
```python
>>> print(re.match('<.*?>', s).group())
<html>
```