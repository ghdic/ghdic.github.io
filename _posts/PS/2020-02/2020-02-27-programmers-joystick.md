---
title: 프로그래머스 조이스틱 풀이
# 오버레이 되는 이미지 및 글
excerpt: 머리쓰는 재밌는 문제였다.

classes: wide

#toc: true
#toc_label: "목차"
#toc_icon: "cog" # 내 컨텐츠에 대한 목차를 오른쪽에 띄워줌

#last_modified_at: 2019-12-08T00:00:00+09:00 # 마지막 업데이트 날짜를 보여줌

tags: [ProblemSolving,]
categories: [PS, greedy]
---

## [조이스틱 문제링크](https://programmers.co.kr/learn/courses/30/lessons/42860?language=cpp)

# 문제
```
조이스틱으로 알파벳 이름을 완성하세요. 맨 처음엔 A로만 이루어져 있습니다.
ex) 완성해야 하는 이름이 세 글자면 AAA, 네 글자면 AAAA

조이스틱을 각 방향으로 움직이면 아래와 같습니다.

▲ - 다음 알파벳
▼ - 이전 알파벳 (A에서 아래쪽으로 이동하면 Z로)
◀ - 커서를 왼쪽으로 이동 (첫 번째 위치에서 왼쪽으로 이동하면 마지막 문자에 커서)
▶ - 커서를 오른쪽으로 이동
예를 들어 아래의 방법으로 JAZ를 만들 수 있습니다.

- 첫 번째 위치에서 조이스틱을 위로 9번 조작하여 J를 완성합니다.
- 조이스틱을 왼쪽으로 1번 조작하여 커서를 마지막 문자 위치로 이동시킵니다.
- 마지막 위치에서 조이스틱을 아래로 1번 조작하여 Z를 완성합니다.
따라서 11번 이동시켜 "JAZ"를 만들 수 있고, 이때가 최소 이동입니다.
만들고자 하는 이름 name이 매개변수로 주어질 때, 이름에 대해 조이스틱 조작 횟수의 최솟값을 return 하도록 solution 함수를 만드세요.

제한 사항
name은 알파벳 대문자로만 이루어져 있습니다.
name의 길이는 1 이상 20 이하입니다.
입출력 예
name	return
JEROEN	56
JAN	23
출처

※ 공지 - 2019년 2월 28일 테스트케이스가 추가되었습니다.
```

# 요약
* 조이스틱을 규칙에 따라 움직여 A로만 이루어진 문자열을 목표 문자열로 최소한의 움직임으로 만든다.

# 접근
* 먼저 A->name[i] 로 전환하는데 드는 비용은 일정하다. 하드코딩을 할 수도 있지만, 간단한 공식을 세워주면 해결된다. `name[i] - 'A'` 라고 했을때 `num > 13 ? 26 - num : num;`이라는 식을 세워줄 수 있다. 실제 하드코딩하면 아래와 같다. N을 기점으로 전환되는걸 알 수 있다.

|A|B|C|D|E|F|G|H|I|J|K|L|M|N|O|P|Q|R|X|T|U|V|W|X|Y|Z|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|0|1|2|3|4|5|6|7|8|9|10|11|12|13|12|11|10|9|8|7|6|5|4|3|2|1|

* 오른쪽으로 가다가 왼쪽으로 전환하는 비용이 더 저렴한 경우를 구해준다.

# 풀이
1. 문자열에 알맞은 문자로 변환하는 비용을 전부 더해준다.(문자를 변환하는 최소 비용)
    ```
    int n = name.length(), answer = 0, arr[20] = {};
        for(int i = 0; i < n; ++i){
            int num = name[i] - 'A';
            answer += num > 13 ? 26-num : num;
        }
    ```
1. 역으로부터 해당 배열에 연속하는 `A`의 개수가 몇개인지 세준다. 이 센 개수가 반대 방향으로 갔을때의 디메리트와 상쇄되면 그냥 계속 진행하는것이고, 상쇄되지 않고 음수일 경우 반대 방향으로 전환하는 것이다.
    ```
    arr[n-1] = name[n-1] == 'A' ? -1:0;
        for(int i = n - 2; i >= 0; --i)
            if(name[i] == 'A')arr[i] = arr[i + 1] - 1;
            else arr[i] = 0;
    ```
1. `arr[i + 1] + i`의 최솟값을 구해준다. `arr[i+1]`은 현재 위치에서 다음 위치까지 연속할 `A`의 개수이며, `i`는 되돌아가는 비용이다. 두개가 더 해짐으로서 상쇄될것이다. 만약 되돌아갈일이 없으면 rest는 0이 될것이다.
1. 정방향으로 갔을때 `n - 1` 번이면 도착하기 때문에, 역방향으로 갔을때 메리트 값인 rest를 더한 `n - 1 + rest`를 결과에 더해준다(좌우로 움직일때 최소비용)
    ```
    int rest = 1e9;
        for(int i = 0; i < n - 1; ++i)
            rest = min(rest, arr[i + 1] + i);
        answer += n - 1 + rest;
        return answer;
    ```

# 소스
{% highlight c++ %}
#include <string>
#include <algorithm>
using namespace std;

int solution(string name) {
    int n = name.length(), answer = 0, arr[20] = {};
    for(int i = 0; i < n; ++i){
        int num = name[i] - 'A';
        answer += num > 13 ? 26-num : num;
    }
    arr[n-1] = name[n-1] == 'A' ? -1:0;
    for(int i = n - 2; i >= 0; --i)
        if(name[i] == 'A')arr[i] = arr[i + 1] - 1;
        else arr[i] = 0;
    int rest = 1e9;
    for(int i = 0; i < n - 1; ++i)
        rest = min(rest, arr[i + 1] + i);
    answer += n - 1 + rest;
    return answer;
}
{% endhighlight %}