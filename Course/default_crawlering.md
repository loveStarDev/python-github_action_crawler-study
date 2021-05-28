지금까지 작성된 YML 파일을 설명해 드리도록 하겠습니다. 뒤에 챕터에서 코드가 더 복잡해지니 이번 챕터에 나오는 코드를 충분히 이해하시고 다음 챕터로 넘어가시기 바랍니다.

```yaml
# gitactions 이름
name: helloGithubAction

# push가 일어났을 경우 아래 jobs 실행
# [push, pull_request] 등과 같이 다중 이벤트 처리 가능(아래 페이지 참고)
on: [push]

# 위 이벤트가 일어났을 경우 실행될 job, 여러개 설정 가능.
jobs:
  # 작업 단위
  build:
    # 실행되는 OS, Window와 Mac OS도 제공합니다. 자세한 version은 document 참고
    runs-on: ubuntu-latest
    steps:
    # 다른 사람이 만들어놓은 action을 가지고 오는 것
    # github.com/actions/checkout 폴더 참고(깃헙에서 만든 것)
    # marketplace에서 여러 uses를 가지고 올 수 있음
    # 아무것도 없는 OS에서 자동으로 우리의 코드를 클론하고 다운받아 실행하게 해주는 소스코드
    - uses: actions/checkout@v2
    # OS에서 실행할 이름
    - name: 1. 파이썬 실행
    # OS에서 실행할 명령어
      run: python test.py
```

- ls -al 명령을 통해 실제 그런지 볼 수 있음

```yaml
name: helloGithubAction

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: 1. 파일 목록 출력
      run: ls
    - name: 2. 파이썬 실행
      run: python test.py
```

[Events that trigger workflows](https://docs.github.com/en/actions/reference/events-that-trigger-workflows)

패키지 확인을 위해 아래 코드를 저장하고 실행해보세요.

```yaml
name: helloGithubAction

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: 1. 파일 목록 출력
      run: ls -al
    - name: 2. 설치되어 있는 패키지 확인
      run: pip list
    - name: 3. 파이썬 실행
      run: python test.py
```

크롤링을 위한 requests는 있지만 bs4가 없습니다.

![image](https://user-images.githubusercontent.com/48270975/119986245-922db700-bffe-11eb-99ea-e7dfb91364fb.png)

추가로 환경설정해주기 위해 `requirements.txt` 파일을 만들어 아래 코드를 입력합니다. request는 있지만 하나 추가해줄게요. 만약 더 필요한 라이브러리가 있다면 엔터 치시고 이어서 쓰시면 됩니다. 뒤에 버전을 명시해줄 수도 있지만 우리는 정확한 버전이 필요한 것은 아니기 때문에 넘어가도록 하겠습니다.

```
beautifulsoup4
requests
```

YAML 파일을 수정해서 commit합니다.

```yaml
name: helloGithubAction

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: 1. 파일 목록 출력
      run: ls -al
    - name: 2. 설치되어 있는 패키지 확인
      run: pip list
    - name: 3. pip 업그래이드
      run: python -m pip install --upgrade pip
    - name: 4. 환경 설정
      run: pip install -r requirements.txt
    - name: 5. 파이썬 실행
      run: python githubactiontest.py
```

문제 없이 빌드되는 것을 확인했으니 크롤링으로 넘어가보도록 하겠습니다.

![image](https://user-images.githubusercontent.com/48270975/119986225-8d690300-bffe-11eb-8dc4-332d0ffa919b.png)

리디북스의 베스트셀러를 크롤링하는 간단한 소스코드입니다.

```python
import requests
from bs4 import BeautifulSoup

url = 'https://ridibooks.com/category/new-releases/2200'
response = requests.get(url)
response.encoding = 'utf-8'
html = response.text

soup = BeautifulSoup(html, 'html.parser')

bookservices = soup.select('.title_text')
for no, book in enumerate(bookservices, 1):
    print(no, book.text.strip())
```

저장하고 commit 하시면 아래와같이 실행된 것을 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/48270975/119986206-87732200-bffe-11eb-9e40-91011abbf85b.png)
