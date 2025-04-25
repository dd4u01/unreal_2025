# 2025_04_25

AI_내비게이션 메시 생성과 경로 찾기 알고리즘 활용

1. 프로젝트 세팅 - Runtime Setting - Runtime Generation 옵션 Dynamic으로 설정

> 왜 해야 하는가?

![image](https://github.com/user-attachments/assets/2a36f9e7-66f3-4031-921d-0d63e6299e69)

해당 예시는 저 큐브가 volume의 범위를 왔다갔다 드나들게끔 설정되어있다. <br>
플레이 도중 volume 내에서 일어난 지형적 변화에 대처할 수 있게 만들기 위해서 해당 옵션을 사용한다. <br>

이번에는 NavMeshVolume 자체의 동적 생성에 대해 알아보자. <br>

AI를 맵 전체에 풀어놔야 하는 레벨 디자인이라면, 네비메시를 레벨 전체에 적용하는 비효율적인 방식을 택해야 하는걸까? <br>
적어도 나는 그렇다고 알고있었는데.. 아니었다;; <br>
**Navigation Invoker** 라는 기능을 사용하면 이 컴포넌트를 가진 액터의 주변 영역만 연산하여 리소르를 아끼고 플레이가 가능하게 된다고 하는데.. <br>

우선 BP_AI의 c++ 클래스 헤더 파일에 추가할 코드를 보자. <br>
```ruby
// Invoker 세팅
	UPROPERTY(BlueprintReadWrite, Category = Navigation, meta = (AllowPrivateAccess = "true"))
	UNavigationInvokerComponent* NavInvoker;


// 첫 번째 Public: 아래에 구현
	/** 네비게이션 메시 생성 반경 */
	float NavGenerationRadius;

	/** 네비게이션 메시 제거 반경 */
	float NavRemovalRadius;


// 두 번째 Public: 아래에 구현
/** Returns NavInvoker subobject **/
FORCEINLINE class UNavigationInvokerComponent* GetNavInvoker() const { return NavInvoker; }
```

다음은 cpp 파일이다. <br>

```ruby
// 활성화될 범위 설정. 테스트를 위해 매우 작은 값 부여.
NavGenerationRadius = 10.0f;
NavRemovalRadius = 15.0f;

// Navigation Invoker 컴포넌트 생성 및 초기값 셋업.
NavInvoker = CreateDefaultSubobject<UNavigationInvokerComponent>(TEXT("NavInvoker"));
// SetGenerationRadii 함수를 사용하여 생성 반경과 제거 반경 설정. Protected 멤버변수이므로 함수를 통해 수정 필요.
NavInvoker->SetGenerationRadii(NavGenerationRadius, NavRemovalRadius); 
```

**빌드 파일에 "NavigationSystem" 모듈을 추가해주는 것도 잊으면 안된다!** <br>

![image](https://github.com/user-attachments/assets/287008ab-6abd-4c73-b674-ef336dfc19ca)

프로젝트 세팅에도 추가로 작업을 해줘야 한다. <br>

Navigation System -> Generate Navigation Only arounds Navigation Invokers -> **True** <br>

이렇게 하면 해당 AI 주변에만 NavMesh가 활성화 되어있는 것을 확인할 수 있게된다. <br>

여기에 추가로 PathFinding이나 장애물 회피 관련 기능을 설정해주는 것으로 AI의 움직임은 더욱 정교해지고 자연스러움을 갖출 수 있게 된다.

어떤 알고리즘이 있을지 이 또한 살펴보자. <br>

1. **Nav Modifier** <br>
> Area Class를 활용하여 특정 구역별 비용을 차등설정하면 AI가 원하는 경로로 이동하도록 설계할 수 있다고 한다. <br>

다음 예시는 
1. 아키텍쳐적 한계점을 회피하면서 <br>
2. NavMeshBoundsVolume을 보다 정교하게 다듬고, AI의 경로탐색(Pathfinding)을 응용하여 AI의 이동 경로를 레벨 내 특정 영역으로 임의 제한하기 <br>

에 초점을 맞춘 예시이다. 친절하게 설명되어있는 것을 차근차근 따라가보자. <br>
![image](https://github.com/user-attachments/assets/66e78d75-ae47-49d0-b167-65ab77794d58)
![image](https://github.com/user-attachments/assets/40da9529-3e8b-496f-b05d-713a2ffb5913)
![image](https://github.com/user-attachments/assets/9d5a9e6c-75ff-41c5-b17b-89223fa9743d)
이후에는 캐릭터 C++ 클래스를 수정해주자. ~~차마 코드 전문은 가져오기가 좀 그래서..~~ ~~다른건 괜찮고?~~ <br>
![image](https://github.com/user-attachments/assets/c4ef44f3-1fb9-40fc-8d15-9b94885ac1db)
![image](https://github.com/user-attachments/assets/25885a59-83aa-4c63-9cb6-0223e21efe29)

여기까지 하면 AI의 MoveTo를 활용, Nav Modifier를 임의로 배치하여 AI의 이동경로를 임의로 변경하는 테스트를 진행해 볼 수 있다. <br.

여기서 잠깐. AI의 MoveToLocation 함수에 대해 보다 자세히 알아보는 시간을 가져보자. <br>
```ruby
EPathFollowingRequestResult::Type MoveToLocation(
const FVector& Dest,
float AcceptanceRadius = -1,
bool bStopOnOverlap = true,
bool bUsePathfinding = true,
bool bProjectDestinationToNavigation = true,
bool bCanStrafe = false,
TSubclassOf<UNavigationQueryFilter> FilterClass = nullptr,
bool bAllowPartialPath = true
);
```
- **Dest**: 이동할 목적지 위치 벡터
- **AcceptanceRadius**: AI가 목적지에 도달했다고 판단할 거리 (값이 클수록 더 멀리서 "도착" 처리)
- **bStopOnOverlap**: 목적지와 캐릭터의 충돌 영역이 겹치면 도착으로 간주할지 여부
- **bUsePathfinding**: 경로 탐색 알고리즘을 사용할지 여부 (false면 직선으로 이동 시도)
- **bProjectDestinationToNavigation**: 목적지를 네비게이션 메시에 투영할지 여부
- **bCanStrafe**: AI가 측면 이동을 할 수 있는지 여부
- **FilterClass**: 네비게이션 쿼리에 사용할 필터 클래스
- **bAllowPartialPath**: 완전한 경로를 찾지 못할 경우 부분 경로라도 허용할지 여부 <br>

※※ C++에서는 함수의 필수 파라미터가 아닌 경우 임의 누락이 가능하며 누락된 경우 함수원형의 기본값이 사용된다. <br>

아하. 그렇다면 MoveToActor는 이와 어떤 차이가 있는걸까? <br>

**공통점:** AI가 특정 오브젝트로 “이동”하게 만드는 함수다.
**차이점:** 타겟 오브젝트가 Actor냐 Location이냐의 차이가 있다. Actor를 타겟 오브젝트로 삼느냐, Location(특정위치)를 타겟 오브젝트로 삼느냐가 핵심 차이점이며 Actor의 경우 Actor가 실시간으로 이동하면 추적이 가능하고, Location의 경우 특정 좌표로만 이동한다. <br>

Actor의 경우 Location을 실시간으로 업데이트하는 방식을 사용하기 때문에 이러한 측면에서 **연산 비용**이 조금 더 발생한다는 차이가 있다고 한다. <br>
~~알아만 두자~~ <br>

