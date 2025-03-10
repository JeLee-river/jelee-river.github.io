---
layout: post
title: "공식문서 읽기: Conditional Rendering"
date: 2023-06-25
categories: [React]
toc: true
toc_sticky: true
math: true
mermaid: true
---

## 톺아보기

---

### **컴포넌트의 조건부 렌더링**

1. 조건부 렌더링에서 컴포넌트가 `null`을 return하는 것은 일반적인 경우가 아니다.  
   \* 부모 컴포넌트의 JSX에서 컴포넌트를 포함하거나 제외하도록 하자.

<br>

2. 직접 JSX를 분기처리하여 **유사한 output을 중복 작성하는** 방법은 코드 유지보수에 어려울 수 있다.  
    \* 코드를 작성할 때 **DRY 원칙**을 지키자!

   ```javascript
   if (isCheck) {
     return <li className="item">{name} ✔</li>;
   }
   return <li className="item">{name}</li>; //output이 거의 유사함.
   ```

<br>

3. 조건부 렌더링에서 `&&` 연산자를 사용할 떄, 좌항에 숫자를 넣지 않도록 유의하자. **숫자값을 조건으로 설정해야 하는 경우, boolean을 반환하도록 만들자**  
   \* 좌항이 `0`일 경우 false 대신 0을 return 한다.  
   \* `0` 대신 `> 0`과 같이 직접적으로 boolean을 반환하도록 만들자.  
   \* `0` 을 불가피하게 사용해야 하면 `!` 논리 연산자를 두 번 사용하여 truthy, falsy한 값을 boolean으로 반환하도록 할 수 있다.

<br>
<br>

## 고찰

---

<br>
그동안 숫자 0이 JS에서 falsy한 값인 점만 주목하고 있었다. 이로 인해 `&&` 연산자를 이용했을 때 실질적인 값 '0'을 그대로 반환할 수 있음을 미처 생각해보지 못했다. 새삼스럽게 `null`과 `undefined`만 체크해주는 `??` 연산자가 등장한 이유를 체감할 수 있었다.   
 
`!!`을 이용한 이중부정으로 0 대신 false를 반환하도록 할 수 있다는 점도 새롭게 알게된 사실이다. 0의 숫자값을 배재하고 falsy한 특성만 취할 수 있도록 하는 방법으로 보이는데, 되도록 숫자값을 좌항에 두진 않겠지만 알아두면 좋은 tip으로 보인다.
