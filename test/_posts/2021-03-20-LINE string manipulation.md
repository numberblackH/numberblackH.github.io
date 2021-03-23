---
layout: post
title: "LINE string manipulation 1"
date: 2021-03-20 08:26:28 -0400
noindex: true
---

LINE 1 &mdash; <2021 상반기 주니어, LINE>
{:.faded}

---

`PROBLEM`
```
ROR 게임은 두 팀으로 나누어서 진행하며, 상대 팀 진영을 먼저 파괴하면 이기는 게임입니다. 따라서, 각 팀은 상대 팀 진영에 최대한 빨리 도착하는 것이 유리합니다.

지금부터 당신은 한 팀의 팀원이 되어 게임을 진행하려고 합니다. 다음은 5 x 5 크기의 맵에, 당신의 캐릭터가 (행: 1, 열: 1) 위치에 있고, 상대 팀 진영은 (행: 5, 열: 5) 위치에 있는 경우의 예시입니다.

게임 맵의 상태 maps가 매개변수로 주어질 때, 캐릭터가 상대 팀 진영에 도착하기 위해서 지나가야 하는 칸의 개수의 최솟값을 return 하도록 solution 함수를 완성해주세요. 단, 상대 팀 진영에 도착할 수 없ㅌ을 때는 -1을 return 해주세요.
```

`제한 사항`
```
maps는 n x m 크기의 게임 맵의 상태가 들어있는 2차원 배열로, n과 m은 각각 1 이상 100 이하의 자연수입니다.
n과 m은 서로 같을 수도, 다를 수도 있지만, n과 m이 모두 1인 경우는 입력으로 주어지지 않습니다.
maps는 0과 1로만 이루어져 있으며, 0은 벽이 있는 자리, 1은 벽이 없는 자리를 나타냅니다.
처음에 캐릭터는 게임 맵의 좌측 상단인 (1, 1) 위치에 있으며, 상대방 진영은 게임 맵의 우측 하단인 (n, m) 위치에 있습니다.
```

## IDEA
string manipulation
: Regular Expression(정규 표현식)

## INPUT && OUTPUT
```
inp_str
result

"AaTa+!12-3"
[2]

"aaaaZZZZ)"
[2, 3, 4]

"CaCbCgCdC888834A"
[1, 4, 5]

"UUUUU"
[1, 3, 4, 5]

"ZzZz9Z824"
[0]
```

## CODE
```java
class Solution {
	public int[] solution(String inp_str) {
		int[] result = new int[6];
		if (!Pattern.matches("^(?=.{8,15}$).*", inp_str)) {
			result[1] = 1;
		}

		if (!Pattern.matches("^[a-zA-Z0-9~!@#$%^&*]*$", inp_str)) {
			result[2] = 2;
		}

		int count = 0;
		if (Pattern.matches("^(?=.*[a-z]).*$", inp_str)) {
			count++;
		}
		if (Pattern.matches("^(?=.*[A-Z]).*$", inp_str)) {
			count++;
		}
		if (Pattern.matches("^(?=.*[0-9]).*$", inp_str)) {
			count++;
		}
		if (Pattern.matches("^(?=.*[~!@#$%^&*]).*$", inp_str)) {
			count++;
		}
		if (count < 3) {
			result[3] = 3;
		}

		if (Pattern.matches(".*(\\w|[~!@#$%^&*])\\1\\1\\1.*", inp_str)) {
			result[4] = 4;
		}

		for (int i = 0; i < inp_str.length(); i++) {
			int temp = 1;
			for (int j = i + 1; j < inp_str.length(); j++) {
				if (inp_str.charAt(i) == inp_str.charAt(j)) {
					temp++;
				}
			}
			result[5] = (temp >= 5) ? 5 : result[5];
		}

		ArrayList<Integer> list = new ArrayList<>();
		for (int i = 1; i < result.length; i++) {
			if (result[i] == i) {
				list.add(i);
			}
		}

		if (list.size() == 0) {
			list.add(0);
		}

		int[] answer = new int[list.size()];
		int index = 0;
		for (int number : list) {
			answer[index++] = number;
		}
		return answer;
	}
}
```

## COMMENT
* Regular Expression(정규 표현식)으로 표현하기에 쉽지 않았음
