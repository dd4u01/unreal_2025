
한 주간 배운 것을 정리하는 시간을 가져보자. <br>

우선 알고리즘 적으로 정렬 함수에 대해 직접 구현해봤었는데 퀵 정렬이 굉장히.. 어려웠다. <br>

벌써 가물가물한 기억을 되짚어보면 정렬에 필요한 반복자와 그에 대한 조건을 순회에 맞춰 계속 설정해주는 것이 정말 머리가 아팠다. <br>

키포인트는 "**정렬을 함수로 만들어서 재귀로 호출하는 것**". <br>

프로젝트 진행 중 알게된 꿀팁은 뭐가 있었던가. <br>

첫째로 git 최신화를 통해 현재 branch에서의 진행상황을 stash로 넘기고 최신화된 데이터만 pull 받아오는 방법. <br>

branch를 옮기면서 pull 및 merge만 제대로 해주면 되기에 어렵지도 않았고. 덕분에 팀원들의 진행상황을 쉽게 확인할 수 있었다. <br>

두 번째로는 프로젝트에서 가장 중요한 '발사' 시스템, 그중에서도 총알의 궤적 및 방향 계산에 대한 내용이었다. <br>

카메라로부터 레일을 화면 정중앙의 조준점 좌표까지 발사하여 궤적을 계산, 총구 끝으로부터 조준점까지의 forward?를 사용하여 방향을 측정하면 된다고 알려주셨다. <br>

조준점 같은 경우에는 이후 담당 팀원이 화면 정중앙에 박아버리는 것으로 떼워버렸는데 계산에는 큰 차이가 없지 싶다. <br>

![image](https://github.com/user-attachments/assets/845d0cbf-81a1-44d3-b9cd-0226242402f3) <br>

그 외에는 레벨디자인 및 타 클래스와의 병합 후 동작 테스트를 진행하며 시간을 보냈다.
