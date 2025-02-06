# 2025_02_06 (8주차 Day 4)

### CodeKata

![image](https://github.com/user-attachments/assets/dc0f2402-c1a8-4ae2-b206-ad5025613c94) <br>

이틀 동안 두 시간에 걸쳐 씨름한 문제를 겨우 해결한 줄 알았으나.. 고려하지 못한 경우의 수가 있는 모양이었다. <br>

```ruby
#include <string>
#include <vector>
#include <algorithm> // std::find
#include <utility> // std::swap

using namespace std;

int solution(vector<int> number) {
    int answer = 0;
    
    int count = 0;
    
    if((number.size()>=3)&&(number.size()<=13))
    {
         for(int i=0; i<number.size()-2; i++)
        {
            int a = number[i];

            for(int j=i+1; j<number.size()-1; j++)
            {
                int b = number[j];

                // find()로 값이 찾아졌을 경우
                if(find(number.begin()+(j+1), number.end(), -(a+b)) != number.end())
                {
                    count++;
                }
            }
        }   
    }
    
    answer = count;
    
    return answer;
}
```

깔끔하게 해결한 줄 알았는데.. 어떤 부분을 고려하지 못한걸까? <br>

---

### 분반 수업

분반 수업의 과제로 내일까지 다음과 같은 성적 관리 프로그램을 직접 짜보게 되었다. <br>

![image](https://github.com/user-attachments/assets/bbc38e71-d97d-462f-8c97-50807ee48cee) <br>

프로그램 전체의 로직은 미처 작성하지 못했지만 우선 정리해두자면.. <br>

학생 ID, 과목 이름, 점수 세 가지 변수를 입력받아 저장하는 부분은 이중 map으로 구현하기로 했다. <br>

```
map<int, map<string, int>>
```

1번 학생 성적 추가 기능에 대해서는 이렇게 하면 문제없이 요구사항의 기능을 모두 수행할 수 있을것으로 예상된다. <br>

---

### 미해결 과제(7번)

역량 부족으로 미처 시간 내에 끝내지 못한 7번 과제는 코드를 구현도 해보기 전에 막혀버린 상태였다.. <br>

![image](https://github.com/user-attachments/assets/303625eb-2a55-4055-842d-882ac5a78af8) <br>

문제의 이 녀석에 대해 뒤늦게 튜터님을 찾아가 질문드렸고.. 충격적인 진실을 알게되었다. <br>

Move()함수에는 문제가 없었고, 원인은 위에있는 함수에서 중괄호'}'가 제대로 안 닫혀 있었기 때문에 그 아래에 있는 코드들을 visual studio에서 인식하지 못해 발생한 문제였던 것. <br>

삽질한 시간을 떠올리니 허무했으나.. 덕분에 코딩을 하면서 몸으로 체감하는 수 밖에 없는 노하우적인 측면에 대해 튜터님께 뼈가되고 살이 될 조언들을 들을 수 있는 좋은 기회가 됐다.. <br>
