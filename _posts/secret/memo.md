# 예상 면접 질문

MSA로 만들 때 고려사항
- 서비스 신뢰성을 위해 쪼개서 만드는것
- 유지보수 측면에서도 선택과 집중을 하게끔 단위 나누기

아무리 모놀리식이라도 MSA로 전환하기 위해 고려하는게 있는지

서버가 터졌을 때, 분산돼있는 서비스 중에 한 군데에서 장애나면 전파되지 않도록 어떻게 제어하는가

트래픽이 늘어날 경우 고려할 사항
- 써킷브레이커

메시지 큐에서 중복 메시지가 올 경우
- 받는 쪽에서 고려해서 짜야한다

TPS?
- Transaction Per Second

스케일 업을 꼭 해야하는 장비
- 마스터가 하나만 지원되는 DB