---
layout: post
title: "공식문서 읽기: Passing Props to a Component"
date: 2023-06-24
categories: [React]
toc: true
toc_sticky: true
math: true
mermaid: true
---

## 톺아보기

---

<br>

### **컴포넌트 props**

1. 컴포넌트는 props 객체 하나만을 인자로 받을 수 있다.  
   \* 필요한 props attribute만 선별해서 가져오고 싶을 땐, 구조분해할당을 이용하자.

2. props도 default value를 지정할 수 있다. **단, null, 0은 개발자가 직접 지정한 값이므로, default value가 적용되지 않는 점을 상기하자**

3. props에도 spread 연산자를 사용할 수 있다. **그러나 이를 무분별하게 사용하고 있다면, 컴포넌트를 분리해서 JSX를 자식으로 전달해야 하는 상황이 아닌지 체크하자**

   ```javascript
   //spread
   <Child {...props} />
   ```

   ```javascript
   //JSX를 props로 전달
   <Parents>
     <Child />
   </Parents>
   ```

4. 컴포넌트의 props로 컴포넌트(JSX)를 전달할 수 있다.

   \* 이때, **parents 컴포넌트는 child 컴포넌트가 무엇을 렌더링하는지 내부구조를 알 필요가 없다.(관심사 분리)**

5. **props는 불변하는 객체이다.** 상호작용이나 새로운 데이터로 인해 값이 변경되어야 할 경우, state를 관리하자.

   \* props 변경은 불가능하다. 값에 변화가 필요하다면 부모가 다른 props를 내려주어야 한다.

<br>
<br>

## 고찰

---

<br>
자식 컴포넌트에 props를 내려주는 것을 컴포넌트 분리의 필요성과 연관지어 본 적이 없었다.  
단순히 필요한대로 props를 내리기 보다는 컴포넌트를 분리하여 부모 컴포넌트에서 자식 컴포넌트를 렌더링하는 방식을 고려할 필요성을 느꼈다.

props가 불변 객체(immutable)인 점도 새롭게 알게된 사실이다. props를 변경하려는 시도를 해본 적은 없지만, props는 부모 컴포넌트가 새로운 props를 내려주지 않는 한 임의로 변경할 수 없음을 배웠다. 이 구절에서 React의 state를 가장 먼저 떠올렸다.

state도 마찬가지로 immutable하므로 기존 state를 그대로 변경하여 사용할 수 없기 때문이다. state를 자식으로 전달하는 방식이 props이니 어찌보면 당연한 말일 수도 있을 것 같다. props와 state는 엄연히 다른 개념이지만..

props의 불변성이 객체형(Object type) 데이터의 특성에서 온 것인지, 단순히 React에서 부여한 props의 성질인지 궁금하여 추가 조사를 해봤다.  
결론은 **'JavaScript가 객체지향언어이며, React props는 객체지향언어에서 불변 객체의 특성을 따른다.'** 이다. 같은 맥락의 개념을 내가 분리해서 생각했던 것이다.. React가 JavaScript 기반의 라이브러리인 점을 항시 잊지 말자.
