
지난 2주간 무엇을 했나 점검해보는 시간을 가져보자. <br>

c++ 강의를 마치고 다시 언리얼엔진으로 돌아와 과제 수행을 위한 개념 공부에 들어갔다. <br>

후에 자주 마주치게될 'CDO'에 대해 알아보고, 에셋 명명 규칙에 대해 훑어보며 얼리얼엔진 문법에 적응하는 시간을 가졌다. <br>

앞서 체험했던 1주차 언리얼엔진 강의에서는 에디터 상에서 블루프린트를 활용하여 익숙해졌다면, 이번에는 C++ 지식을 기반으로 visual studio와 연동하여 코드를 직접 짜는 방향으로 강의가 진행됐다. <br>

준비해주신 액터나 오브젝트, 마테리얼을 클릭 한 번으로 만들어낼 수 있었던 과거와는 다르게 직접 코드를 짜보면서 액터의 생명주기, InitializeComponents()부터 EndPlay()에 이르기까지 각 시점에 따라
호출되는 함수와 그 쓰임새에 대해 알아보고 차후 어떻게 활용할 수 있을지 고민할 수 있는 유익한 시간을 보냈다. <br>

```
// -- 이하 생명주기 관련 Flow --
// 생성자 - 메모리에 생김. 딱 한 번 호출.
// PostInitailizeComponents() - 컴포넌트가 완성된 직후 호출. 컴포넌트끼리 데이터 주고받기. 상호작용
// BeginPlay() - 배치 (Spawn) 직후
// Tick(float DeltaTime) - 매 프레임마다 호출됨.
// Destroyed() - 삭제 되기 직전에 호출된다. 리소스 정리
// EndPlay() - 게임 종료, 파괴 (Destroyed()), 레벨 전환
```

언리얼엔진의 핵삼(차별점?) 이라 생각되는 리플렉션에 대해서는 적응해나가고있는 참이다.. 2장에 들어서는 본격적으로 클래스를 활용하여 캐릭터와 캐릭터의 동작을 구현하는 강의가 있는데 이 역시 맛만 봤다. <br>

과제 제출이 얼마 남지 않은 시점에서 현재 첫 번째 과제를 해결하는 단계에 걸쳐있다. 잔병치레에 더불어 긴 연휴동안 너무 풀어져버렸고 그동안 확 뒤쳐진 것이 느껴져 마음이 아프다. <br>
