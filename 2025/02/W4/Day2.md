# 2025_02_25 (11주차 Day 2)

정렬 함수. 그 중에서도 퀵 정렬에 대하여 직접 구현해보는 과제를 정리해보려 한다. <br>

처음에는 인덱스에서 랜덤으로 값을 받아와서 피벗을 정하는 것 부터 시작해보려 했는데, 그마저도 하나의 연산이라고 생각하니 어.. 굳이? 라는 생각이 들어 피벗은 0으로 고정하고 시작했다. 어찌됐든 정렬만 되면
되는거니까. <br>

그렇게 0번째 인덱스는 피벗으로 고정, 1번째 이후의 배열에 대해 다음과 같이 정렬해줬다. <br>

```ruby
for(int i=begin+1; i<end; i++)
{
  if(arr[i] > arr[begin])
  {
    tempI = i;
    break;
  }
}

for(int j=end; j>begin; j--)
{
  if(arr[j] < arr[begin])
  {
    tempJ = j;
    break;
  }
}

swap(arr[tempI], arr[tempJ];
```

정렬 이후에는 arr[tempJ]와 arr[begin](이하 피벗)을 swap 해주면 첫 번째 정렬이 끝이난다. <br>

이렇게 계속 진행을 해보려 했는데.. 계속해서 반복되는 정렬과 그때마다 바뀌어야하는 조건탓에 머리가 깨질 것만 같았다. <br>

결국 혼자 힘으로는 문제를 해결하지 못했고.. 그때 기가막힌 조언을 듣게된다. <br>

'이렇게 하지말고, 일단 정렬을 함수로 만드는게 낫겠다.' <br>

일찍이 강의를 들을적에 이번 퀵 함수 구현의 핵심은 '**재귀**'라고 짚어주셨었다. <br>

그렇다.. 바로 그 재귀를 사용하기 위해서라도, 당연히 정렬은 함수로 구현되어 마땅한 것이었다. <br>

돌파구를 찾은 듯 함수 구현에 들어갔으나, 여전히 퀵정렬의 벽은 높았으니.. <br>

그놈의 조건문 때문에 계속해서 제대로 정렬이 먹혀들질 않았다. <br>

```ruby
if(i > j)
{
  break;
}
```

우선 i와 j, 각 포인터가 교차하는 부분에 대해 정렬 이후 체크해서 빠져나가는 검증 로직을 반복문 직후에 추가했고.. <br>

i와 j를 굳이 temp 변수에 담을 필요도, for문 내에 if까지 돌릴 필요도 없다는 것을 지식의 힘을 빌려서야 알아낸 끝에 수정했다. <br>

```ruby
while (i <= j && arr[i] <= pivot)
{
  i++;
}

while (i <= j && arr[j] >= pivot)
{
  j--;
}
```

for문과 if로 돌렸던 것을 while로 바꿔주었으니 조건문도 반대로.. 'i <= j' 로직 또한 내부 반복문이 돌아가는 동안 범위를 넘지 못하도록 추가해준다. <br>

반복문은 'i > j' 가 되지 않는 동안 돌아가는 것이니 당연히 조건은 'i <= j'가 맞는데, 'i < j'로 둬놓고 왜 정렬이 안되는걸까.. 고민을.. 오래 했다. <br>

마침내 반복문이 제대로 동작해 정렬되는 것을 확인했고.. 대망의 재귀함수 또한 깔끔하게.. 조언을 받아 작성할 수 있었다. (~~**조언을 받아**~~) <br>

그리하여 완성된, 혼자 힘으로는 해결하지 못한 퀵 정렬 함수.. <br>

```ruby
#include <iostream>
#include <algorithm>
#include <cstdlib>
#include <vector>

using namespace std;

int FuncSort(vector<int>& arr, int begin, int end)
{
	int pivot = arr[begin];
	int i = begin+1;
	int j = end;

	while (true)
	{
		while (i <= j && arr[i] <= pivot)
		{
			i++;
		}

		while (i <= j && arr[j] >= pivot)
		{
			j--;
		}
		
		if (i > j)
		{
			break;
		}

		swap(arr[i], arr[j]);
		// 증감 후 반복 돌입
		i++;
		j--;
	}

	swap(arr[begin], arr[j]);

	return j;
}

void FuncQuickSort(vector<int>& arr, int begin, int end)
{

	if (begin < end)
	{
		int pivot = FuncSort(arr, begin, end);

		FuncQuickSort(arr, begin, pivot - 1); // 좌 정렬
		FuncQuickSort(arr, pivot + 1, end); // 우 정렬
	}
}

int main()
{
	vector<int> arr = { 3, 5, 2, 4, 6, 1 };

	int begin = 0;
	int end = arr.size()-1;

	// 퀵 정렬
	FuncQuickSort(arr, begin, end);

	for (auto it : arr)
	{
		cout << it;
	}

}
```
