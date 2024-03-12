---
title:  "[Rust][Side Project] C로 작성된 Editor를 Rust로 포팅해보기 - 0"
date: 2023-11-25 15:00:00 +0900
categories: [Rust, Side Project]
tags: [Rust, Side Project, C, Editor]
math: true
mermaid: true
---

# 들어가기 전...

### 그래서 C로 작성된 Editor가 뭔데?

`C`로 작성된 Editor가 뭔지부터 얘기를 해야겠다.   
개발 잘하는 친구 녀석이 이런 (Github Repository)[https://github.com/antirez/kilo]와 (사이트)[https://viewsourcecode.org/snaptoken/kilo/]를 보내왔다.   
우리가 매일 쓰는 Editor를 직접 만들어 보는 경험을 언젠가는 해봐야지! 했는데   
1000 lines 언저리에서 동작하는 Editor라니!   
군침이 싸-악 당기는 주제였다.   

한참 걸리기는 했지만, 위 사이트의 `184 steps`를 잘 따라서(사실상 보면서 옮겨 쓰는...)   
동작하는 Editor를 만들었다!   
![Tiny Editor 실행 화면](/assets/media/20231125_rust_tiny_editor_0_0.png)   

C로 작성한 `Tiny Editor`는 이 [Repository](https://github.com/ghkdxofla/tiny-editor/tree/main/c/src)에서 확인할 수 있다.   
Rust로 작성한 Version도 위 Repo에서 작성해나갈 예정이다.   

### 아쉬운 점을 넘어서보기

Step by Step으로 개발하면서 오랜만에 다시 C에 대해 공부한 것 까지는 좋았으나...   
코드를 쭉 보면서 옮겨 적은 느낌이 워낙 강하게 들다보니   
'이 Editor가 내가 만든 것이 맞나?' 싶더라.   
그래서 `Rust` 공부도 요새 멈춰서 아쉬운 겸   
'C로 작성한 Editor를 Rust로 포팅해보면 되겠다'로 결론을 내렸다.   

# Rust Start

### 초기화

우선은 Rust로 초기 환경을 구축한다.   
(Rust 설치 및 Cargo는 생략한다.)

```bash
cargo init
```

`main.rs` 가 생성되고, 다음의 코드 까지 확인한다.   
```rust
fn main() {
    println!("Hello, world!");
}
```

긴 연재글이 될 것 같으니, 아주아주 간단한 `Hello World!` 가 가능한 상태로 이번 글은 마친다.   

# 어떤 글이 연재되는가?

현재까지 완성된 Tiny Editor의 기능을 하나하나 Rust로 바꿀 예정이다.   
최대한 Commit 단위는 작업 단위로 할 예정이며,   
몇 개의 Commit을 묶어서 글 연재를 통해 전달할 생각이다.   

Rust 빠가사리인 상태이므로, 여러 삽질과 러프한 코드를   
작성할 것이니 불편하면 어쩔 수 없다 ㅎ.   
