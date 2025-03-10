---
layout: post
title: "공식문서 읽기: Your First Component"
date: 2023-06-22
categories: [React]
toc: true
toc_sticky: true
math: true
mermaid: true
---

## 톺아보기

---

<br>

### **리액트의 의의**

1. 리액트는 컨텐츠를 마크업(mark up)하고 이에 JS를 곁들여서 상호작용이 가능하게 만드는 전통적인 웹 페이지 기술을 동일하게 사용한다.  
   \* 리액트 컴포넌트는 이 기술에서 JS 함수에 해당한다.  
   \* 컴포넌트가 return하는 것은 HTML로 마크업된 tag들이지만, **내부적으로는 JS로서 동작하고 있으며 이를 JSX 문법이라고 한다.**

<br>

### **컴포넌트**

1. 컴포넌트명은 반드시 대문자로 시작한다. **이는 React가 HTML tag와 컴포넌트를 구분하는 중요한 단서이다.**  
   \* 소문자로 이루어진 태그는 React가 HTML tag로 인식한다.

2. 컴포넌트는 다른 컴포넌트를 렌더링할 수 있지만, **다른 컴포넌트에서 정의되어서는 안된다.**  
   \* 소문자로 이루어진 태그는 React가 HTML tag로 인식한다.

3. React 기반의 프레임워크는 React가 JS로 페이지를 관리하게 하는 것을 넘어서, React 컴포넌트를 통해 HTML을 자동 생성하게 할 수 있다.  
   = **JS 코드가 load되기 전에 컨텐츠를 보여줄 수 있다.**
