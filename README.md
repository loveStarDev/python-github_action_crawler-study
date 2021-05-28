# Github_Action_Crawler
###### 깃허브 액션을 활용한 자동 크롤러

## Why Github Action?
개발자 워크 플로우(work flow)를 위한 플랫폼으로 오케스트레이션, 즉 github에서 제공되어지는 컴퓨터 시스템 및 소프트웨어의 자동화를 제공하는 도구입니다. 쉽게 말해 github에서 제공하는 컴퓨터입니다. 2019년에 github에서 CI/CD 기능을 추가하여 Build/Test/Coverage/Release까지 진행할 수 있게 되었습니다.

<div>
  
      CI/CD란?
      
      - CI 는 지속적인 통합(continuous integration)으로 개발하면서 코드 통합을 지속적으로 진행하면서 품질을 확인
      - 모든 프로젝트가 끝난 후에 코드의 품질을 관리하는 단점을 해소하기 위해 나타난 개념
      - 여러 명의 개발자가 한 프로젝트를 진행 할 때 수시로 각자의 작업들을 확인하며 협업
      - CD는 지속적인 배포(continuous deploy/delivery)로 소프트웨어가 항상 신뢰 가능한 수준에서 배포될 수 있도록 지속적으로 관리
      - 즉, CI 과정을 통해 개발 중에 지속적으로 코딩 빌드와 테스트를 하고 이를 거친 코드는 CD 과정으로 배포에 반영
  
</div>

조직에서는 반복적인 수작업을 통해 일어나는 일에 대한 모든 자동화 방안을 강구해야할 필요가 있습니다. 단순히 속도를 빠르게 하겠다는 것이 아니라 수동으로 할경우에 일어날 수 있는 복합적인 문제를 자동화함으로 해소하겠다는 이점도 있습니다.

## Yml 파일 이란?
#### 기본 구성

`yaml`, `yml` (야믈이라 읽습니다) 2개의 확장자를 사용하고 있습니다.

**키와 값**으로 매핑된 데이터를 주고 받기 위한 여러가지 양식들이 있습니다. 아래 3개의 코드를 비교해보세요.

```xml
--- XML ---

<?xml version="1.0" encoding="UTF-8"?>
<name>최주은</name>
<age>25</name>
```

```json
--- JSON ---
{
    "name" : "최주은",
    "age" : 25
}
```

```yaml
--- YAML ---
name: 최주은
age: 25
```

YAML은 YAML Ain't Markup Language라는 다소 장난스러운 유래를 가지고 있습니다. 가독성이 좋아 가벼운 마크업언어로 사용하고 있습니다. JSON 형식에서 바로 YAML 형식을 사용하고 싶다고 한다면 아래와 같은 사이트를 이용할 수 있습니다.
<div align=center>
  
[JSON to YAML](https://www.json2yaml.com/)
  
</div>

#### 기본 문법

- 주석은 `#`
- 문서의 시작은 `---`
- 문서의 끝은 `...`
- int, str, bool 형을 지원합니다. str만 큰 따옴표를 붙입니다.
- 줄바꿈을 지원합니다. 주로 파이프(`|`)를 사용합니다. `>`를 사용하면 줄바꿈을 무시합니다.
- 여러 key와 value로 나뉠경우 아래와 같은 문법 사용합니다. 부모노드와 자식 노드는 들여쓰기로 구분합니다. 리스트 구조를 표현할 때에는 `-`를 사용합니다. 대쉬 뒤에 띄어쓰기 한 칸 해주셔야 합니다.

```yaml
key: 
  key1:
    key2:
      key3:
        key4: value
```

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
```

- 중괄호 2개로 변수를 사용할 수도 있습니다.

```yaml
foo: "{{ variable }}"
```

- 조건문 사용도 가능합니다.

```yaml
steps:
  ...
  - name: The job has succeeded
    if: ${{ success() }}
```

보다 자세한 문법은 아래 문서를 확인해주세요.
<div align=center>

[Github Docs](https://docs.github.com/en/actions/reference/context-and-expression-syntax-for-github-actions)
  
</div>
