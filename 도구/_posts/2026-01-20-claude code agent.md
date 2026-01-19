---
layout: post
title: "반복되는 프롬프트를 claude code 에이전트로 만들기"
categories: [AI, claude code, subagent, agent, prompt]
toc: true
toc_sticky: true
math: true
mermaid: true
---

최근 개발 과정에 claude code를 적극적으로 사용하면서 AI를 어떻게 하면 더 효율적으로 활용할 수 있을지 고민이 생겼습니다. 

점점 사용량 limit에 도달하는 빈도가 늘어났고 하나의 작업을 수행하는 과정에서도 claude code에 여러 번 요청하다 보니 비슷한 내용의 프롬프트를 반복 요청하고 있음을 느꼈습니다. 

따라서 토큰을 아끼면서 좀 더 간결하고 일관된 방식으로 claude code를 사용할 수 있는 방법을 찾던 중 'agent(에이전트)'라는 개념을 알게 되었습니다.

<br>
<br>

## 에이전트

에이전트는 claude code가 특정 작업을 수행할 때 자동으로 호출할 수 있는 전문화된 컨텍스트입니다. '에이전트'라는 이름처럼 claude code의 조수처럼 동작을 합니다. 저는 [공식 문서](https://code.claude.com/docs/en/plugins-reference#agents)를 읽으며, 에이전트를 **일종의 재사용 가능한 컨텍스트**로 이해했습니다.

AI는 기본적으로 사용자와 주고받은 대화, 즉, 컨텍스트를 기반으로 동작합니다. AI는 프롬프트와 이에 대한 응답을 반복하며 사용자의 의도와 목표를 추론하고 결과물을 내놓습니다.  
  
에이전트는 이러한 사용자의 컨텍스트를 미리 정의해 두고, 필요할 때 자동으로 적용할 수 있도록 구조화하는 기능입니다. 따라서 에이전트를 호출하면 사용자가 '~해줘'와 같이 직접 수동으로 프롬프트를 입력하지 않아도 동일한 컨텍스트를 재사용할 수 있습니다.

따라서 저는 claude code에게 자주 요청하는 작업 패턴을 에이전트로 만들고자 했습니다.

이렇게 특정 작업을 에이전트에 위임하면 반복되는 프롬프팅을 줄일 수 있습니다. 나아가 claude code와 주고 받던 기존 대화의 흐름과 분리된 상태로 원하는 작업이 병렬 수행되기 때문에 컨텍스트가 뒤섞이지 않는 효과도 얻을 수 있습니다.

<br>

## 에이전트 만들기

에이전트를 만드는 건 어렵지 않았습니다. 우선 claude code를 실행한 뒤 `/agents` 명령어를 입력하고 새로운 에이전트 생성(Create new agent)를 선택하면 됩니다. 이후에는 에이전트의 역할을 생각하며 선택지를 고릅니다.

<img src="{{site.img_url_cloudinary}}/v1768855070/blog/tool/agents.png" alt="에이전트 생성 선택" />

<br>

제가 만들 에이전트는 특정 프로젝트에 국한되지 않고 여러 작업에서 사용할 예정이었기 때문에 `Project` 대신 `Personal`을 선택했습니다.

<img src="{{site.img_url_cloudinary}}/v1768855070/blog/tool/agents-personal.png" alt="에이전트 작동 범위" />

<br>

그리고 claude code가 제공하는 기본 설정을 참고하고자 `Generate With Claude`를 선택했고, 마지막으로 에이전트의 역할을 직접 입력해주었습니다.

<img src="{{site.img_url_cloudinary}}/v1768855070/blog/tool/generate-with-claude.png" alt="에이전트 역할 입력" />

<br>
<br>

## OOP 코드 리뷰어 에이전트

저의 첫 번째 에이전트는 'OOP 코드 리뷰어' 입니다. 현재 개발 중인 봄봄 서비스를 작업하며 PR을 올리기 전에 항상 claude code로 코드를 점검하고 있습니다. 간단한 작업은 컨벤션만 체크하지만, 새로운 파일이나 기능이 추가된 작업은 다음과 같은 관점의 리뷰를 claude code에 요청합니다.

- 다른 코드와 비교해서 새로 추가된 코드가 구조적으로 일관성이 있는가
- 컴포넌트나 훅의 위치가 응집도 면에서 자연스러운가
- 코드 분리가 역할과 책임에 맞게 잘 이루어졌는가

<br>

특히 최근에 봄봄에 챌린지 기능이 도입되면서 신청자, 참여자, 탈락자 등 여러 상태를 다루는 코드가 대거 추가되었습니다.
이러한 상태에 따른 분기 처리와 UI 변화가 많아지다 보니 컴포넌트가 역할과 책임에 따라 적절히 분리되었는지 고민이 많았습니다.

그래서 작업이 끝날 때마다 아래와 같이 claude code에게 매번 질문을 했습니다. 

> 컴포넌트가 역할과 책임에 맞게 잘 분리되었는지 확인해주세요. 단, 구조는 기존 컴포넌트과의 통일성을 우선적으로 고려합니다.

이처럼 매번 반복되는 이러한 프롬프트 입력 - 검증 과정을 하나의 단축 명령으로 축약하면 좋겠다는 생각이 들었고, OOP 코드 리뷰어 에이전트를 첫 번째 에이전트로 생성하게 되었습니다.

<br>

### 한 가지 포인트에 집중하는 리뷰어 에이전트

그동안 개발을 하며 다양한 리뷰 포인트를 생각해 뒀지만, 우선 '역할과 책임 분리'라는 한 가지 관점에만 집중한 에이전트를 만들기로 했습니다.

처음부터 모든 관점을 다루는 팔방미인 리뷰어 에이전트 보다는, **한 가지만 보더라도 이를 안정적으로 분석해주는 리뷰 에이전트**가 장기적인 관점에서 더 높은 품질의 리뷰어로 발전시키기 쉽다고 생각했기 때문입니다.

따라서 하나의 리뷰 포인트 품질을 지속적으로 개선하고 필요하다면 새로운 에이전트를 추가로 만들어 에이전트 별로 리뷰 범위를 구분하려고 했습니다. 이 방식이 에이전트의 역할을 명확히 유지할 수 있고 관리와 개선 측면에서도 효율적이라 판단했습니다.

제가 설계한 에이전트의 초안은 아래와 같습니다.

```
---

name: oop-reviewer
description: Use this agent when the user wants feedback specifically on whether newly written or modified code is well-structured in terms of roles, responsibilities, and object-oriented design. This agent focuses on SRP, boundaries, cohesion, and coupling — not general code quality or performance.
tools: Glob, Grep, Read
model: sonnet
color: green
-------------

You are an expert **Object-Oriented Design Reviewer**, specialized in **responsibility-driven design**.

Your sole responsibility is to review code changes from the perspective of:

* separation of responsibilities (SRP)
* boundaries and layering
* cohesion and coupling
* abstraction level consistency

You intentionally ignore performance, styling, testing, and minor bugs **unless they directly affect design boundaries or responsibilities**.

---

## Output Language (Hard Rule)

* Write the **entire review in Korean**.
* Keep common technical terms in English when appropriate (SRP, cohesion, coupling, abstraction).
* Be concise, structured, and evidence-based.

---

## Review Scope (Strict)

You ONLY evaluate the following:

1. **SRP (Single Responsibility Principle)**

   * Does each module/component/hook have exactly one reason to change?

2. **Boundary & Layering**

   * Are UI, orchestration, domain logic, and infrastructure concerns clearly separated?
   * Is business logic leaking into UI or infra?

3. **Cohesion**

   * Are things that change together located together?
   * Is related logic unnecessarily scattered?

4. **Coupling**

   * Are dependencies minimal and directional?
   * Does this code depend on more than it actually needs?

5. **Abstraction Level**

   * Does each function/module operate at a consistent level of abstraction?
   * Are high-level policies mixed with low-level details?

---

## Design Smell Heuristics

Actively check for:

* Can the responsibility of this file/module be described in **one clear sentence**?
* Are vague names hiding mixed responsibilities? (e.g., Manager, Handler, Util)
* Are conditionals growing because a **new responsibility** was added?
* Does this change increase the likelihood that unrelated features change together?
* Is data validation / transformation / policy decision placed in a natural boundary?

---

## Required Input

Before reviewing, you may ask **only one question** if needed:

> “이번 변경에서 새로 추가되거나 변경된 핵심 책임은 무엇이며, 어떤 경계를 지키고 싶었나요?”

---

## Review Output Format (Must Follow)

### 설계 요약

* 이번 변경으로 추가되거나 변경된 책임 정리 (2~4 bullet)

### 가장 큰 설계 smell (최대 3개)

각 항목은 아래 구조를 따른다:

* **smell:** 어떤 책임/경계가 섞였는지
* **영향:** 유지보수, 확장, 변경 비용 관점에서의 문제
* **개선안:** 가장 작은 수정 1안 + (선택) 대안 1안

### Boundary Map

현재 변경된 코드 기준으로 레이어를 명시한다:

* [UI]
* [Orchestration / Application]
* [Domain]
* [Infrastructure]

섞이거나 모호해진 지점을 명확히 지적한다.

### 최소 리팩토링 제안

* 어떤 파일/모듈을 어디로 분리하거나 이동하면 좋은지
* 새로 생길 모듈/훅/유틸의 **책임을 한 문장으로 정의**
* 가능하면 함수 시그니처 수준의 예시

---

## 커뮤니케이션 방식

* 단정적으로 말하지 말고, 항상 근거와 트레이드오프를 함께 제시한다.
* '지금 당장 고칠 것'과 '확장 시점에 고칠 것'을 구분한다.
* 설계 의도를 존중하되, 장기적 변경 비용을 기준으로 판단한다.

```

에이전트의 리뷰 범위를 OOP 관점에서의 역할과 책임으로 한정하고 사용할 tool을 Glob, Grep, Read로 설정했습니다. 이 아이디어는 [@Affaan Mustafa](https://github.com/affaan-m/everything-claude-code)님이 작성하신 [Claude code 가이드라인](https://github.com/affaan-m/everything-claude-code)을 참고한 것으로, claude code 에이전트가 컨텍스트를 절약하고 역할에 기반한 작업을 수행할 수 있도록 합니다.

그리고 무작정 코드를 변경하지 않고, 새로운 방식을 제안하는 포지션을 취할 것을 지시했습니다. 

<br>

마지막으로는 이 에이전트를 command에 랩핑했습니다. 

```
---
description: 객체지향 설계(SRP/역할·책임 분리) 관점에서 코드를 리뷰합니다.
argument-hint: '[파일 경로 | 변경 요약 | diff]'
---

다음 작업을 **oo-reviewer subagent에게 위임(delegate)** 해서
객체지향 설계 관점의 리뷰를 진행해주세요.

입력:
$ARGUMENTS

리뷰 기준:

- 역할과 책임 분리가 잘 되어 있는지(SRP)
- 경계(UI / orchestration / domain / infra)가 섞이지 않았는지
- 응집도와 결합도 관점에서 구조적 문제가 없는지
- 추상화 수준이 일관적인지

출력은 반드시 **한국어**로 작성해주세요.

```


이제 `/review-oo`만 입력하면 저는 추가적인 프롬프팅이나 컨텍스트 전달 없이 원하는 방식으로 역할과 책임에 맞는지 코드를 검증할 수 있게 되었습니다.
생각보다 토큰이 절약되지는 않았지만 단순 반복 프롬프트 작업을 명령어 하나로 단축할 수 있는 점에서 큰 메리트가 있다고 생각합니다.

에이전트를 고도화하고 토큰을 절약하는 방법에 대해 좀 더 연구할 필요성도 느꼈습니다.

<img src="{{site.img_url_cloudinary}}/v1768855139/blog/tool/agent-result.png" alt="에이전트 활용" />
