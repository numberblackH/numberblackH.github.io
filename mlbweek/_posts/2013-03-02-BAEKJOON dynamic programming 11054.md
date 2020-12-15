---
title: "BAEKJOON dynamic programming 11054"
date: 2013-03-01 08:26:28 -0400
categories: algo, baekjoon, dynamic programming
---

> [가장 긴 바이토닉 부분 수열](https://www.acmicpc.net/problem/11054)

## IDEA
dynamic programming
: 1) 좌 우에서 최장 증가 부분 수열을 각각 구함
: 2) 좌우의 최장 증가 부분 수열 중 최대값을 구함

 |-|1|5|2|1|4|3|4|5|2|1|
 |-|-|-|-|-|-|-|-|-|-|-|
 |dp|1|2|2|1|3|3|4|5|2|1|
 |dp|1|5|2|1|4|3|3|3|2|1|

## explicit formula
```
dp[0][i] : 좌측부터 i번재 항을 마지막으로 사용하는 수열 중 최대 길이
dp[1][i] : 우측부터 i번재 항을 마지막으로 사용하는 수열 중 최대 길이
```

## INPUT && OUTPUT
```
수열크기
수열정보

10
1 5 2 1 4 3 4 5 2 1

7
```

## CODE
```java
public class Main {
	public static void main(String[] args) throws Exception {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int n = Integer.parseInt(br.readLine());
		int[] numbers = new int[n];
		int[][] dp = new int[2][n];
		StringTokenizer st = new StringTokenizer(br.readLine());
		for (int i = 0; i < n; i++) {
			numbers[i] = Integer.parseInt(st.nextToken());
			dp[0][i] = 1;
			dp[1][i] = 1;
		}
		for (int i = 1; i < n; i++) {
			for (int j = 0; j < i; j++) {
				if (numbers[j] < numbers[i]) {
					dp[0][i] = Math.max(dp[0][i], dp[0][j] + 1);
				}

			}
		}
		for (int i = n - 2; i >= 0; i--) {
			for (int j = n - 1; j > i; j--) {
				if (numbers[j] < numbers[i]) {
					dp[1][i] = Math.max(dp[1][i], dp[1][j] + 1);
				}
			}
		}
		int answer = 0;
		for (int i = 0; i < n; i++) {
			answer = Math.max(answer, dp[0][i] + dp[1][i]);
		}
		System.out.println(answer - 1);
	}
}
```

### COMMENT
* 좌측 우측 따로 생각만 하면 바로 해결가능한 문제
