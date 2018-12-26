### git

### [git](https://git-scm.com/book/ko/v1/Git%EB%A7%9E%EC%B6%A4-Git-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)

- `Git`은 분산형버전관리시스템(DVCS - Distributed Version Control System)이다.

  소스코드의 버전 관리를 할 수 있고, 이력이 관리된다.

- 사용자 정보 설정

> 메일주소는 반드시 GitHub 계정과 동일해야하고 유저네임도 맞춰주는 것이 좋다.
>
>  `--global` 옵션으로 설정하는 것은 딱 한 번만 하면 된다. 해당 시스템에서 해당 사용자가 사용할 때는 이 정보를 사용한다. (만약 프로젝트마다 다른 이름과 이메일 주소를 사용하고 싶으면 `--global` 옵션을 빼고 명령을 실행한다.)

```bash
$ git config --global user.name "###"
$ git config --global user.email "test@example.com"
```

- git 명령어 자동 색칠

```bash
$ git config --global color.ui true
```

> Mac 에서 한글파일명 깨짐 및 commit 오류 문제 해결
>
> ```bash
> $ git config --global core.precomposeunicode true
> $ git config --global core.quotepath false
> ```

- 설정 확인

```bash
$ git config --global --list
```

---

#### 기초 명령어 정리

```bash
$ git init
$ git add .
$ git commit -m "example"
$ git remote add origin https://github.com/example		# remote 는 한번만
$ git push -u origin master							# 첫 push 이후로는 git push 만 입력
```

##### 1. git 저장소 설정

```bash
$ git init
student@DESKTOP MINGW64 ~/Desktop/TIL (master)
```

**주의!! 반드시 현재 디렉토리에 git을 사용하고 있는지, (master) 표기가 생겼는지 확인 할 것**

#### 2. git add

`git add`는 현재 `working directory` 에서 `commit` 할 목록에 담아놓는 것이다.

그리고 그 목록은 `INDEX(staging area)` 라고 한다.

```bash
$ touch a.txt
$ git add .
```

- `git add a.txt`를 해도 되지만, 우선 `git add .` 을 하자!!
- `.` 은 리눅스 상에서 현재 디렉토리를 뜻한다.

#### 3. git commit

`git commit`은 현재 소스코드 상태를 **스냅샷**을 찍는 것과 동일하다.

`staging area`에 담겨 있는 내용을 이력으로 기록한다.

```bash
$ git status
$ git commit -m "커밋메시지"
```

#### 4. git status

git의 현재 상태를 확인한다. **자주자주 입력하면서 확인하는 습관을 만들어 보자!**

```bash
$ git status
```

------

#### 원격 저장소로 보내기(git push)

사전에 github 에 저장소(repository)를 만들어 놓는다.

1. github 저장소(원격저장소) `url`를 `origin` 이라는 이름으로 설정한다.

```bash
$ git remote add origin https://github.com/example/TIL.git
```

2. 원격 저장소로 보낸다.(push)

```bash
$ git push -u origin master
```
> 한번 push 한 이후로는 `git push` 까지만 입력해도 된다.
- git 학습 사이트
  - [git 입문](https://backlog.com/git-tutorial/kr/intro/intro1_1.html)
  - [git 간편안내서](https://rogerdudler.github.io/git-guide/index.ko.html)
  
#### 웹 크롤링
### 코스피 정보 가져오기

Beautiful Soup library 설치 [doc]
```
$ pip install bs4
```

Beautiful Soup 체험해보기

```python
from bs4 import BeautifulSoup
url = 'https://www.google.co.kr/'
html_doc = requests.get(url).text
soup = BeautifulSoup(html_doc, 'html.parser')

print(soup.prettify())
```
# 코스피 가져오기
```python
import requests
from bs4 import BeautifulSoup
import os

token = os.getenv("TELEGRAM_BOT_TOKEN")
method_name = "getUpdates"
url = f'https://api.telegram.org/bot{token}/{method_name}'
update = requests.get(url).json()


chat_id = update["result"][0]["message"]["from"]["id"]
method_name = "sendmessage"

url_cos = "https://finance.naver.com/sise/"
html_cos = requests.get(url_cos).text
soup = BeautifulSoup(html_cos, 'html.parser')
select = soup.select_one('#KOSPI_now') 
msg = "코스피: " + select.text
msg_url = f'https://api.telegram.org/bot{token}/{method_name}?chat_id={chat_id}&text={msg}'

print(msg_url)
print(requests.get(msg_url))
```

### Telegram Chatbot

### 1. 텔레그램 설치

- Telegram 가입 및 로그인
- <https://telegram.org/>
- 데스크탑 앱 or 웹 버전 사용 권장 (폰에서도 알림 받아볼 수 있도록 로그인 해놓기)
- 전화번호 인증 통해서 가입을 해야 이용 가능

---

### 2. 봇 생성

- `@Botfather` 검색

- `시작` 버튼 클릭

- `/newbot` 입력

- 봇 이름 생성 (중복 가능)

- username 생성

  > 중복되지 않는 유니크한 이름 & bot 으로 끝나는 이름

- `token`, `url` 주소 얻기

  > **API Token**은 비밀번호 같은 것 / 절대 노출되지 않도록 관리
  >
  > **보안 설정을 하지 않고 github 에 공유하지 않도록 주의 또 주의한다.**

- url 클릭하여 봇 활성화 후 시작 버튼 클릭

- [텔레그램 봇 api](https://core.telegram.org/bots)

---

### 3. 챗봇 만들기

- **requests** 모듈 [[doc]](http://docs.python-requests.org/en/master/)

  - requests.get(url) 함수를 사용하면 해당 웹페이지 호출 결과를 가진 `Response 객체`를 리턴한다.

    > Response 객체는 HTML Response 와 관련된 여러 attribute들을 가지고 있는데, 예를 들어, Response의 status_code 속성을 체크하여 HTTP Status 결과를 체크할 수 있고, Response 에서 리턴된 데이터를 문자열로 리턴하는 text 속성 등이 있다.

- requests 체험해보기

```python
import requests

url = 'https://api.github.com/'
response = requests.get(url)
print(response)
print(response.status_code)
print(response.text)
print(response.json())
```

#### 3.1 chat_id 가져오기

- 나에게 메세지를 보내려면, telegram에서 지정한 내 고유 `chat id` 값을 알아야 함.
- 우리는 bot 의 관리자이니 해당 bot 으로 누군가가 message 를 보내면, 그 사람의 chat_id 를 알아낼 수 있음. 그래야 bot 이 그 사람에게 메세지를 보내줄수 있다.
- 텔레그램 클라이언트에서 내가 생성한 bot 을 이름으로 검색해서 찾은 다음 start 버튼을 누르고 (그러면 `start` 라는 메세지가 전송 됨.
- `getUpdates` 는 bot 이 받은 모든 메세지를 확인 가능한 정해진 `method` 들 중 하나. 

```python
import requests

token = ""
method_name = "getUpdates"
url = f'https://api.telegram.org/bot{token}/{method_name}'

print(url)
print(requests.get(url))
```

#### 3.2 나에게 메세지 보내기

```python
import requests

token = ""
method_name = "getUpdates"
url = f'https://api.telegram.org/bot{token}/{method_name}'


chat_id = 
method_name = "sendmessage"
msg = "안녕하세요ㅎㅎ"
msg_url = f'https://api.telegram.org/bot{token}/{method_name}?chat_id={chat_id}&text={msg}'

print(msg_url)
print(requests.get(msg_url))
```

#### 3.3 chat_id 를 json 에서 가져오기

```python
import requests

token = ""
method_name = "getUpdates"
url = f'https://api.telegram.org/bot{token}/{method_name}'
update = requests.get(url).json()

chat_id = update["result"][0]["message"]["from"]["id"]
method_name = "sendmessage"
msg = "안녕하세요ㅎㅎ"
msg_url = f'https://api.telegram.org/bot{token}/{method_name}?chat_id={chat_id}&text={msg}'

print(msg_url)
print(requests.get(msg_url))
```

#### 3.4 token 보안설정 - 환경변수 세팅

- `.bash_profile`

```bash
# [주의] export 이후로는 공백이 없어야한다.
export TELEGRAM_BOT_TOKEN='...'
```

```bash
$ source ~/.bash_profile
```

```python
import requests
import os

token = os.getenv("TELEGRAM_BOT_TOKEN")
method_name = "getUpdates"
url = f'https://api.telegram.org/bot{token}/{method_name}'
update = requests.get(url).json()

chat_id = update["result"][0]["message"]["from"]["id"]
method_name = "sendmessage"
msg = "안녕하세요ㅎㅎ"
msg_url = f'https://api.telegram.org/bot{token}/{method_name}?chat_id={chat_id}&text={msg}'

print(msg_url)
print(requests.get(msg_url))
```

#### 챗봇 퀘스트 회고

- 스펙

날씨와 미세먼지를 알려준다/ 매일 아침 사용자에게 정보전달 

- 회고

Heroku를 이용해 배포를 할 때 급하게함.

크롤링할 정보를 가져올 때 어느 부분을 긁어야할지 헷갈림.

- 보완 계획

가능하다면 삼성화재 연수원의 점심메뉴를 불러와 11시 50분마다 알림이 올 수 있게 기능 변환,

날씨정보도 아침마다 업데이트하며 자기 맞춤형 알람 봇으로 만듬
