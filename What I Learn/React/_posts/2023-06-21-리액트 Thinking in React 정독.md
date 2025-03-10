---
layout: post
title: "공식문서 읽기: Thinking in React"
date: 2023-06-21
categories: [React]
toc: true
toc_sticky: true
math: true
mermaid: true
---

## 톺아보기

---

<br>

### **리액트의 state**

1. state를 구조화할 때 DRY(Don't Repeat Yourself) 원칙을 지키자.  
    \* state는 컴포넌트의 메모리와 같으며, 컴포넌트가 지속 추적하고 변화시킬 수 있다.  
    \* 최소한의 state만 상태관리하고 필요한 것은 직접 계산하여 무분별한 state 선언을 피한다.

   - 이를 위해 컴포넌트 구조를 초반에 어느정도 잡고 가야할 필요가 있음.
   - **기존 state 또는 props로 계산할 수 있다면, state 선언 대상이 아니다.**

<br>

2. state의 위치는 state를 참조하는 컴포넌트의 계층에 기반한다.  
   \* 하나의 state에 대해 다수의 컴포넌트가 렌더링될 수 있다.  
    = state에 영향받는 모든 컴포넌트를 파악하기 쉬워야 유지보수가 용이하다.  
    = **영향범위 내 컴포넌트들의 상위 컴포넌트(부모)에 state를 선언하면 state를 보다 쉽게 관리할 수 있다.**

<br>
<br>

## 고찰

---

<br>
관리하는 state의 수는 그동안 깊게 생각해보지 않은 문제였다. 나는 코드의 가독성이 떨어지는 문제 정도만 인지하고 있었지, 유지보수와 같은 장기적인 문제와 연결짓진 못하였다.  
state가 꼭 필요한 부분인지 고민하고, 기존의 state만으로 계산이 가능하다면 가급적 새로운 state를 지양하는 것이 선언적으로 코드를 작성하는 기본 원칙임을 배우게 되었다.
