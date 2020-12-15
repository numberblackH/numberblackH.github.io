---
title: "BAEKJOON Dynamic Programming 1965"
date: 2013-03-01 08:26:28 -0400
categories: baekjoon, dynamic programming
---

> [상자넣기](https://www.acmicpc.net/problem/1965)

## IDEA
dynamic programming
: 1) 최장 증가 부분 수열을 각각 구함
: 2) dp[i] : 좌측부터 i번재 항을 마지막으로 사용하는 수열 중 최대 길이

	|-|1|2|3|4|5|6|7|8|
	|-|-|-|-|-|-|-|-|-|
	|dp|1|2|2|3|4|3|4|5|

## INPUT && OUTPUT
```
상자개수
상자정보

8
1 6 2 5 7 3 5 6

5
```

## CODE
```java
public class Main {
	public static void main(String[] args) throws Exception {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int n = Integer.parseInt(br.readLine());
		int[] numbers = new int[n];
		int[] dp = new int[n];
		StringTokenizer st = new StringTokenizer(br.readLine());
		for (int i = 0; i < n; i++) {
			numbers[i] = Integer.parseInt(st.nextToken());
			dp[i] = 1;
		}
		int max = 1;
		for (int i = 1; i < n; i++) {
			for (int j = 0; j < i; j++) {
				if(numbers[j] < numbers[i]) {
					dp[i] = Math.max(dp[i], dp[j] + 1);
				}
			}
			max = (dp[i] > max) ? dp[i] : max;
		}
		System.out.println(max);
	}
}
```

## COMMENT
- Longest Increasing Subsequence 대표 문제
