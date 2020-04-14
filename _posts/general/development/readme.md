# Code Review

코드 리뷰의 주요 포인트
- 하루 안으로 Reviewer가 진행
- 주요 로직은 Unit test가 있어야 함
- Prototyping code 등 중요도가 낮은 코드는 느슨한 리뷰도 필요
  * 단, 이 경우에는 Repository를 분리
- 스타일 가이드는 반드시 지키기
  * Lint로 대부분 거르기
- 불필요한 공백줄 제거
  * Editor에서 Extra Whitespace 없애는 옵션을 키면 된다
- 코드 리뷰의 중요한 요소
  * 더 단순한 디자인은 없는지
  * 이해하기 쉬한 구조인지
  * 확장성, 유연성은 좋은지
  * 예외 처리가 잘 되어 있는지
  * 버그가 발생할 가능성이 있는지
  * 병목이 될 가능성이 있는지
  * 테스트는 쉬운지(*)
- 하나의 기능만 코드 리뷰 대상으로
- 다른 범위까지 수정해서 보내지 말기
- 너무 먼 미래에 대한 방어 코드는 오히려 비효율적

스타트업은 어떻게 리뷰를 해야 할까?
```
아직 제품이 시장에서 검증이 안 된 스타트업의 경우, 전략적으로 상당한 기술적 빚 (technical debt)를 안고 가는 것도 괜찮은 방법입니다. 이 경우 code quality는 어느 정도 포기하고, 코드 리뷰에 들어가는 노력을 작게 가져 갑니다.
```

좋지 않은 코드 리뷰의 경험
- 오프라인 코드 리뷰
- 프로젝터로 볼 때 눈아픔
- 분위기 타는 리뷰
- 도구의 부실함
- 담당 서비스가 모두 다를 때
- 주니어 코드의 리뷰 대상
  * 일방적인 가이드가 되는 경우가 많다
  * 시니어들에겐 도움이 잘 안됨

# Boilerplate 목록

추천 목록 모음 페이지
- [Node.js](https://medium.com/better-programming/best-node-js-boilerplate-to-speed-up-your-project-development-a9eca7b07f90)
- [Django](https://dev.to/sm0ke/django-boilerplate-code-open-source-and-free-2aa5)

# 참조 링크

[실리콘 벨리 회사의 코드리뷰 이야기](https://xyz37.blog.me/221147928687)

[카카오스토리 팀의 코드 리뷰 도입 사례](https://tech.kakao.com/2016/02/04/code-review/)