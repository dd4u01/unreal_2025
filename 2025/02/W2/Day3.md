# 2025_02_07 (8주차 Day 5)

### 챌린지반
---

<numeric> 헤더 기반 함수에 대해 알아보는 시간을 가졌다. <br>

연속 구간의 합을 계산하기 위한 **partial_sum** 예제<br>

```ruby
#include <iostream>
#include <numeric>
#include <vector>
using namespace std;

int main() {
    vector<int> v = {1, 2, 3, 4};
    vector<int> psum(4);

    partial_sum(v.begin(), v.end(), psum.begin());

    cout << psum[0] << endl; // 1
    cout << psum[1] << endl; // 3 (1 + 2)
    cout << psum[2] << endl; // 6 (1 + 2 + 3)
    cout << psum[3] << endl; // 10 (1 + 2 + 3 + 4)
}
```

**unique** <br>
> 연속된 중복 원소를 제거하여 (끝 위치) iterator를 반환하는 함수

```ruby
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

int main() {
    vector<int> v = {1, 4, 4, 2, 2, 2, 5, 1};
    sort(v.begin(), v.end()); // 정렬 필수! {1, 1, 2, 2, 2, 4, 4, 5} 이렇게 되겠죠?

    auto newEnd = unique(v.begin(), v.end()); // 이건 아래에서 자세히 설명
    v.erase(newEnd, v.end()); // 중복되는 구간 전부 삭제!

    for (int x : v) cout << x << " ";
}
```

**accumulate** 추가
```ruby
#include <iostream>
#include <numeric>
#include <vector>
using namespace std;

int main() {
    vector<int> v = {1, 2, 3, 4};

		// 곱셈이니 초기값은 1로 시작
    int product = accumulate(v.begin(), v.end(), 1, multiplies<int>());
    cout << "총곱: " << product << endl;
}
```
다음과 같이 4번째 인자를 추가로 설정해주면 원하는 계산이 가능해진다! (예시는 곱셈) <br>

### 과제 관련(8번)
---

강의를 보며 간단한 레벨 디자인을 목표로 처음부터 설계를 진행하는 순서를 정리하려 하였으나 생각에 그쳤다.. <br>

코드에 대한 이해는 주석으로 달아두었고 코드 외적인 부분에 대한 메모를 적어두려 한다. <br>

#### Collision 프리셋 설정 관련 개념 정리 <Br>

**NoCollision**
> 충돌무시.
> 하늘 등 배경에 적용하는 듯.

**BlockAll**
> 전부 막힘.
> 벽 등에 사용

**OverlapAll**
> 이벤트 트리거용

**BlockAllDynamic**
> 움직이는 객체(Dynamic)만 막겠다.
> 플레이어 상호작용

**OverlapAllDynamic**
> 움직이는 객체만 오버랩 처리 하겠다.

**Pawn**
> pawn타입 객체를 대상으로 충돌을 감지 혹은 막겠다.


#### HasTag 태그 설정

**액터-플레이어** 를 기준으로 설정법은 다음과 같다.

1.언리얼엔진 상에서 캐릭터 블루프린트( ex: BP_SpartaCharacter)를 연다. <br>
2. 디테일 창에서 액터>고급>태그를 확인. <br>
3. 추가해서 태그 설정 (ex: Player)
