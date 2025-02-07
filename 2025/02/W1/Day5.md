# 2025_02_07 (8주차 Day 5)

오늘은 챌린지반의 과제를 수행하는 것에 집중했다(~~따라가기 힘들다..~~) <br>

지난 TIL에서 이중 맵을 통해 학생 정보를 처리하겠다는 기초 설계를 적어놨었다. <br>

'map'안에 map을 또 넣으면 안되나? 하고 단순하게 생각이 들었을 뿐이었는데, 이게 이번 과제의 전부였다고 생각한다. 아주 칭찬해. <br>

```ruby
#include <iostream>
#include <map>
#include <vector>
#include <set>
#include <iomanip>  // 소수점 출력용

using namespace std;

int studentID, score;
string subject;

// 학생별 성적 관리 (학생ID -> (과목명 -> 점수))
map<int, map<string, int>> studentRecords;

// 과목별 최고 점수 관리 (과목명 -> (점수 -> 학생ID 리스트))
map<string, map<int, vector<int>>> topScores;

// 과목 목록 저장
set<string> subjects;

// 성적 추가 함수
void addStudentScore() 
{

    cout << "학생 ID 입력: ";
    cin >> studentID;
    cout << "과목명 입력: ";
    cin >> subject;
    cout << "점수 입력 (0~100): ";
    cin >> score;

    // 점수 유효성 검사
    if (score < 0 || score > 100) 
    {
        cout << "유효하지 않은 점수입니다! (0~100 범위 내에서 입력)\n";
        return;
    }

    subjects.insert(subject);

    // 점수 갱신(왜 해야하나? vector라서 값이 중복됨) 
    int prevScore = studentRecords[studentID][subject];
    studentRecords[studentID][subject] = score;

    // 최고 점수 관리 업데이트
    auto& subjectRanking = topScores[subject];

    // 기존 점수 제거
    if (prevScore > 0 && subjectRanking.count(prevScore)) 
    {
        auto& students = subjectRanking[prevScore];
        students.erase(find(students.begin(), students.end(), studentID));

        // 동일 점수를 가진 학생의 정보 삭제 방지
        if (students.empty())
        {
            subjectRanking.erase(prevScore);
        }
    }

    // 새로운 점수 추가
    subjectRanking[score].push_back(studentID);

    cout << "성적이 추가되었습니다!\n";
}

// 특정 학생의 성적 조회 함수
void showStudentScores() 
{
    int studentID;
    cout << "조회할 학생 ID 입력: ";
    cin >> studentID;

    if (studentRecords.find(studentID) == studentRecords.end()) 
    {
        cout << "해당 학생은 성적 기록이 없습니다!\n";
        return;
    }

    cout << "학생 " << studentID << "의 성적\n";
    for (const auto& record : studentRecords[studentID])
    {
        const string& subject = record.first;
        int score = record.second;
        cout << " - " << subject << ": " << score << "점\n";
    }
}

// 전체 과목별 평균 점수 계산 함수
void showAverageScores() 
{
    if (subjects.empty()) 
    {
        cout << "해당 과목은 등록되어있지 않습니다!\n";
        return;
    }

    map<string, double> subjectAverages; // (과목 -> 총점)

    // 과목별 총점 계산
    for (const auto& record : studentRecords) 
    {

        // map<string,int> (과목-> 점수) 조회
        for (const auto& subjects : record.second) 
        {

            const string& subject = subjects.first;
            int score = subjects.second;

            subjectAverages[subject] += score;
        }
    }

    cout << "과목별 평균 점수\n";
    for (const auto& subject : subjects) 
    {

        // 점수가 없을 경우 루프
        if (subjectAverages.count(subject) == 0)
        {
            continue;
        }

        double avg = subjectAverages[subject] / studentRecords.size();

        // 소수점 둘째 자리까지만 출력
        cout << fixed << setprecision(2);
        cout << " - " << subject << " 평균 점수: " << avg << "점\n";
    }
}

// 과목별로 최고 점수를 받은 학생 조회 함수
void showTopStudents() 
{
    string subject;
    cout << "조회할 과목명 입력: ";
    cin >> subject;

    if (topScores.find(subject) == topScores.end() || topScores[subject].empty()) 
    {
        cout << "해당 과목(" << subject << ")의 성적 기록이 없습니다!\n";
        return;
    }

    // 최고 점수 조회
    auto highestScore = topScores[subject].rbegin(); // map()이 오름차순 정렬이라 rbegin()
    int topScore = highestScore->first;
    const vector<int>& topStudents = highestScore->second;

    cout << subject << " 최고 점수: " << topScore << "점\n";
    cout << " - 학생 ID: ";
    for (int studentID : topStudents)
    {
        cout << studentID << " ";
    }
    cout << "\n";
}

int main() 
{
    while (true) 
    {
        cout << "\n=== 학생 성적 관리 시스템 ===\n";
        cout << "1. 학생 성적 추가\n";
        cout << "2. 특정 학생 성적 조회\n";
        cout << "3. 과목별 평균 점수 조회\n";
        cout << "4. 과목별 최고 점수 학생 조회\n";
        cout << "5. 종료\n";
        cout << "선택: ";

        int choice;
        cin >> choice;

        // 입력 오류 방지
        if (cin.fail()) 
        {
            // 초기화
            cin.clear();
            cin.ignore(10, '\n');
            cout << "잘못된 입력입니다. 숫자(~5)로 다시 입력해주세요.\n";
            continue;
        }

        switch (choice) 
        {
        case 1:
            addStudentScore();
            break;
        case 2:
            showStudentScores();
            break;
        case 3:
            showAverageScores();
            break;
        case 4:
            showTopStudents();
            break;
        case 5:
            cout << "프로그램을 종료합니다.\n";
            return 0;
        default:
            cout << "올바른 번호를 입력해주세요!\n";
        }
    }
}
```

![image](https://github.com/user-attachments/assets/f636b8e8-4b6a-4bbe-b964-347c21cb5e17)
![image](https://github.com/user-attachments/assets/6ae6afa8-f722-4035-8013-f5dd53fe68bd) <br>


나열하고 보니 조금 눈이 아픈것도 같지만.. 어쨌든 의도대로 잘 굴러가는 것을 확인할 수 있었다. <br>

발표를 위해 프로그램에 대한 간단한 설명을 정리해보았다. <br>

### 어떤 컨테이너를 사용했는가?

1. map<int, map<string, int>>

> 이유
레드-블랙 트리 기반의 정렬 컨테이너로 빠른 검색이 가능
자동 정렬(오름차순)이 되어있어 정렬 작업이 필요 없었다.(최고 점수 학생 조회에 사용)

2. set<string>

> 이유
중복 방지
자동 정렬(알파벳 순)


3. vector<int>

> 학생 ID 리스트를 저장하는데 사용함.


### 효율적인 데이터 관리를 위한 설계 방식?

1. 점수 갱신 로직
- vector<int>로 저장되는 최고 점수 학생 리스트의 중복 저장 방지를 위해 기존 점수를 삭제 후
추가하는 방식으로 구현.

2. 최고 점수 조회
데이터를 오름차순으로 정렬해주는 map의 특성을 따라 rbegin()으로 조회함.

### 사용한 STL 알고리즘

1. find()
- 조회, 삭제에 사용

2. rbegin()
- 최고 점수 조회에 사용(map 오름차순 정렬에 기반)

3. erase()
- 중복 저장 방지를 위해 사용
