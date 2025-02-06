# 2025_02_06 (8주차 Day 4)

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

ㅇ
