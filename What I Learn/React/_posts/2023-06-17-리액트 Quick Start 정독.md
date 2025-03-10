---
layout: post
title: "공식문서 읽기: Quick Start"
date: 2023-06-17
categories: [React]
toc: true
toc_sticky: true
math: true
mermaid: true
---

## 톺아보기

---

<br>

### **컴포넌트**

1. 컴포넌트명은 대문자로 시작하여 HTML 태그와 구분한다. HTML 태그는 소문자로 시작한다.

2. 컴포넌트는 다수의 JSX태그를 갖지 못한다. 반드시 하나의 태그로 감싸준다.  
   \* JSX : React에서 사용되는 자바스크립트 확장 문법

3. 중괄호를 이용해 JavaScript로 escape하여 변수를 삽입할 수 있다.

4. style은 escape을 위한 중괄호, 스타일 지정 객체를 위한 중괄호가 필요하기 때문에 중괄호가 두 번 사용된다.

5. 조건문을 사용하여 렌더링할 컴포넌트를 분기 처리할 수 있다.  
   **\* [이 때, true를 체크할 값이 숫자가 되지 않도록 유의해야 한다.](https://react-ko.dev/learn/conditional-rendering)**  
    \* && 연산자는 앞의 조건이 falsy 한 값이라면, 해당 객체를 반환한다.

```javascript
...
  return (
    <>
      {user.map(
        (userInfo) =>
          userInfo.id && <Profile user={userInfo} key={userInfo.id} />
      )}
    </>
  );
```

6. 컴포넌트 목록을 렌더링할 때, 반드시 key값이 필요하다.  
   \* React는 나중에 **항목을 삽입, 삭제 또는 재정렬할 때** 어떤 일이 일어났는지 이해하기 위해 키를 사용  
   \* input 태그의 defaultValue도 input 태그에 key를 부여해야 변경을 인식한다. (프로젝트에서의 깨달음..)
