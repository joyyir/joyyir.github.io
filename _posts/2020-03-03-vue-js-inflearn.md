---
title: "[Front-end] 인프런 강의 '실습 UI 개발로 배워보는 순수 javascript 와 VueJS 개발' 정리"
description: VueJS 첫 걸음!
excerpt : VueJS 첫 걸음!
date: 2020-03-03 00:00:00 -0400
categories:
  - Front-end
tags:
  - VueJS
---

## 강의 내용 간단 정리

- 실습 예제 : 쇼핑몰 사이트 검색 페이지

![](/assets/images/vue-js-inflearn.png)

1. pure javascript로 MVC 패턴을 이용하여 구현
    - 개발 : [github compare](https://github.com/joyyir/lecture-vue/compare/1-vanilla/scafolding...joyyir:1-vanilla/jyjang)
    - 특징
        - dom을 직접 조작하는 로직이 주를 이룸
        - Model(데이터 조작), View(말그대로 화면), Controller(Model과 View 사이를 중재)
        - 대략적인 flow : View를 통해 사용자 입력이 들어옴 -> View는 이벤트를 발생시킴(.emit) -> Controller는 이 이벤트를 받아서(.on) Model을 통해 데이터를 조작하거나 다른 View에 일을 시킴
    - View에서 이벤트를 발생시키고, 이벤트 핸들러를 등록하는 메소드

    ```javascript
    // View.js의 일부
    on(event, handler) {
        this.el.addEventListener(event, handler)
        return this
    },

    emit(event, data) {
        const evt = new CustomEvent(event, { detail: data })
        this.el.dispatchEvent(evt)
        return this
    }
    ```
2. Vue.js로 MVVM 패턴을 이용하여 구현
    - 개발 : [github compare](https://github.com/joyyir/lecture-vue/compare/2-vue/scafolding...joyyir:2-vue/jyjang)
    - 특징
        - 1번 구현에 비해 소스 양이 현저하게 줄어듦 (dom을 직접 조작하는 소스가 사라지고 데이터만 조작함)
        - 그러나 아직 탬플릿이 컴포넌트 단위로 쪼개져있지 않고 통으로 합쳐져있음 (index.html)
        - 마찬가지로 데이터와 메소드도 컴포넌트 단위로 쪼개져있지 않고 통으로 합쳐져있음 (app.js)
3. Vue.js 컴포넌트로 구현
    - 개발 : [github compare](https://github.com/joyyir/lecture-vue/compare/3-component/scafolding...joyyir:3-component/jyjang)
    - 특징
        - 탬플릿을 컴포넌트 단위로 쪼개긴 했으나 하나의 파일에 모아져있음 (index.html)
        - 데이터와 메소드는 컴포넌트 단위로 쪼개짐 (XXXComponent.js)
        - app.js가 각 컴포넌트를 통합하는 역할을 함 (1번 구현의 MainController.js와 역할이 비슷)
4. Vue.js 단일 파일 컴포넌트로 구현 (.vue 파일)
    - 개발 : [github compare](https://github.com/joyyir/lecture-vue/compare/4-singleFileComponent/scafolding...joyyir:4-singleFileComponent/jyjang)
    - 특징
        - 탬플릿까지 컴포넌트 단위의 .vue 파일로 관리됨


## pure javascript로 MVVM을 구현한 예시

```javascript
const h1 = document.createElement('h1')
document.body.appendChild(h1)
const viewModel = {}
let model = ''
Object.defineProperty(viewModel, 'model', {
    get() { return model; },
    set(val) {
        model = val;
        h1.innerHTML = model;
    }
});
viewModel.model = 'hello world' // viewModel.model 값을 바꿀 때마다 h1의 텍스트가 변경된다.
```


## 유용한 툴

- lite-server [https://www.npmjs.com/package/lite-server](https://www.npmjs.com/package/lite-server) : 경량의 노드 웹 서버를 띄운다. 로컬 파일의 변경사항을 자동으로 싱크해준다. 파일을 수정할 때마다 일일이 브라우저를 새로고침할 필요 없다. 매우 편리함!!!
- vue cli [https://cli.vuejs.org/guide/installation.html](https://cli.vuejs.org/guide/installation.html) : Vue 프로젝트 뼈대를 손쉽게 만들어준다. Spring Boot Cli와 비슷한 느낌


## 생각

- pure javascript로 MVC 패턴만 잘 지켜도 꽤 유지보수하기 좋은 코드가 되겠다. 난 여태까지 엄청 근본도 없이 프론트엔드를 짰구나싶다
- vue로 짜면 학습비용이 있긴 하지만 훨씬 코드가 간결해지고 유지보수하기 쉬워지고 생산성도 높아지겠다는 생각이 들었다.
- view model 덕분에 자질구래한 dom 조작을 할 필요가 없어서 너무 편하다. dom을 조작하면서 생기는 버그도 현저하게 줄어들 것 같다.
- vue에서는 template, data, methods까지 하나의 파일로 묶어서 관리하니까 모듈화의 이점을 잘 살릴 수 있을 것 같다. 재사용성도 늘어나고, 해보진 않았지만 테스트도 가능해지지 않을까 싶다. 객체지향에서의 객체 개념에 template이 추가된 느낌이다.