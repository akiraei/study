

# 이해관계자, UX 리서처의 첫 연구 대상

**정성 연구를 한다고 하면 제일 먼저 연구해야 하는 참여자는 나의 조직과 이해관계자이다**

### 제품 개발 직군이 일반적으로 생각하는 방식

- 주의: 이는 일반적인 구분이지, 조직이나 회사에 따라 생각하는 방식과 생각은 차이가 있을 수 있다.

| 직군  | 생각하는 방식   | 생각                                                                         |
| --- | --------- | -------------------------------------------------------------------------- |
| UXR | why, who  | - 왜 이런 행동을 하는가?<br>- 왜 이 행동이 중요한가?<br>- 우리의 유저는 누구인가?                      |
| UXD | who, how  | - 누구를 위해 제품을 만드는가?<br>- 누구의 니즈를 우선시할까?<br>- 그 니즈를 어떻게 맞출 수 있는가?            |
| PM  | how, when | - 제품을 어떻게 만들까?<br>- 제품을 언제까지 만들어야 하는가?                                     |
| Eng | what      | - 무엇을 만들까?<br>- 무슨 기능이 있어야 할까?<br>- 무엇이 만들어졌나?<br>- 우리의 데이터가 무엇을 말해주고 있는가? 

#### 내가 생각하는 생각하는 방식

| 직군    | 생각하는 방식    | 생각                                                                   |
| ----- | ---------- | -------------------------------------------------------------------- |
| UXR   | why, who   | - why: 왜 이런 행동을 하는가?<br>- why: 왜 이 행동이 중요한가?<br>- who: 우리의 유저는 누구인가? |
| UXD   | who, what  | - who: 누구를 위해 제품을 만드는가?<br>- what: 그 니즈를 어떻게 맞출 수 있는가?               |
| PM    | what, when | - what: 어떤 제품을 만들까?<br>- when: 제품을 언제까지 만들어야 하는가?                    |
| Eng   | what, how  | - how: 제품을 어떻게 만들까?<br>- what: 무슨 모듈이 있어야 할까?<br>                    |
| Data  | who        | - who: 우리의 데이터가 말하는 고객은 어떤 존재인가?                                     |
| Sales | where      | - where: 어디서(채널) 파는가?                                                |

| 질문    | 직군             | 생각                                                                   |
| ----- | -------------- | -------------------------------------------------------------------- |
| why   | UXR            | - 왜 이런 행동을 하는가?<br>- 왜 이 행동이 중요한가?                                   |
| who   | UXR, UXD, Data | - 누구를 위해 제품을 만드는가?<br>- 우리의 유저는 누구인가?<br>- 우리의 데이터가 말하는 고객은 어떤 존재인가? |
| what  | UXD, PM, ENG   | - 그 니즈를 어떻게 맞출 수 있는가?<br>- 어떤 제품을 만들까?<br>- 무슨 모듈이 있어야 할까?           |
| how   | Eng            | - 제품을 어떻게 만들까?                                                       |
| when  | PM             | - 제품을 언제까지 만들어야 하는가?                                                 |
| where | Sales          | -어디서(채널) 파는가?                                                        |

| 직군    | why | who | what | how | when | where |
| ----- | --- | --- | ---- | --- | ---- | ----- |
| UXR   | 1   | 1   | 0    | 0   | 0    | 0     |
| UXD   | 0   | 1   | 1    | 0   | 0    | 0     |
| PM    | 0   | 0   | 1    | 0   | 1    | 0     |
| Eng   | 0   | 0   | 1    | 1   | 0    | 0     |
| Data  | 0   | 1   | 0    | 0   | 0    | 0     |
| Sales | 0   | 0   | 0    | 0   | 0    | 1     |

```mermaid
    classDiagram

    Why <|.. UXR

    Who <|.. UXR
    Who <|.. UXD
    Who <|.. Data

    What <|.. UXD
    What <|.. PM
    What <|.. Eng

    How <|.. Eng

    When <|.. PM

    Where <|.. Sales

    Why --> Who
    Who --> What
    How <-- What
    When --> What
    When --> How
    What --> Where

    class What {
        +Research research
        +Product product
    }

    class Why {
        +Research research
    }

    class Who {
        +Research research
    }

    class How {
        +Product product
    }

    class When {
        +Product product
    }

    class Eng {
        +Unique how
    }

    class PM {
        +Unique when
    }

    class UXR {
        +Unique why
    }

    class Where {
        +Sales sales
    }

    class Sales {
        +Unique where
    }

```

### 이해관계자 인터뷰

- 조직, 제품, 프로젝트에 대한 이들의 입장과 지식 수준을 파악할 수 있다
- 일반적으로 인터뷰를 진행하면 프로젝트 우선순위나 불확실성에 대해 **대부분 비슷하게** 답한다
- 이해관계자 인터뷰를 통해 얻는 것
	- 조직이 유저에 대해 알고 있는 **핵심 지식**을 빠르게 습득
	- 조직내 **우선 순위**을 파악
	- 전박적인 **조직 분위기**
	- 조직원 개인이 조직을 바라보는 **시선**
- 저자는 새로운 팀에 합류, 새로운 컨설팅 시작에 '이해관계자 인터뷰'를 가장 먼저 시작한다고 함

### 이해관계자 인터뷰 템플릿

- 인터뷰용 템플릿을 제시

| 질문        | 내용                                                                                                                                                          |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 프로젝트 우선순위 | - 현재 가장 우선순위가 높은 프로젝트는 무엇인가요?<br>- 이번 분기와 다음 분기의 우선순위는 무엇인가요?<br>- 이 우선순위 중 불확실성이 높은 것은 무엇인가요?                                                              |
| 지식 & 갭    | - 유저에 대해 어떤 것을 알고 있나요?<br>- [팀명]에서 현재 가장 합의가 안되고 있는 내용은 무엇은가요?                                                                                              |
| 조직에 대한 시각 | - [팀명]/[회사명]에서 일하는 데 가장 큰 장애물이 무엇이라고 생각하나요?<br>- [팀명]/[회사명]을 이해하는 데 도움이 되는 팁은 무엇인가요?                                                                        |
| 관계        | - 제가 [팀명]이나 [이해관계자]님의 일하는 스타일에 대해 알아야 할 점이 있나요?<br>- [이해관계자]님은 이전에 UX리서치를 경험해본 적이 있나요? 있다면 직접 했나요, 리서치팀과 함께 협업했나요?<br>- [이해관계자]님은 UX 리서치에 대해 어떤 기대를 갖고 있나요? |
