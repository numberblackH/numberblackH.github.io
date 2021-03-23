---
layout: post
title: "Regular Expression"
date: 2021-03-22 08:26:28 -0400
noindex: true
---

## Regular Expression(정규 표현식)
```
regex : Regular Expression; 정규 표현식
- boolean matches(String regex)
  인자로 주어진 정규식에 매칭되는 값이 있는지 확인

- String replaceAll(String regex, String replacement)
  문자열내에 있는 정규식 regex와 매치되는 모든 문자열을 replacement문자열로 바꾼 문자열을 반환

- String[] split(String regex)
  인자로 주어진 정규식과 매치되는 문자열을 구분자로 분할

숫자로만 구성 : ^[0-9]*$
영문자로만 구성 : ^[a-zA-Z]*$
소문자로만 구성 : ^[a-z]*$
대문자로만 구성 : ^[A-Z]*$
한글로만 구성 : ^[가-힣]*$
E-mail : \\w+@\\w+\\.\\w+(\\.\\w+)?
전화번호 : ^\d{2,3}-\d{3,4}-\d{4}$
휴대폰 전화번호 : ^01(?:0|1|[6-9])-(?:\d{3}|\d{4})-\d{4}$
주민등록번호 : \d{6} \- [1-4]\d{6}
우편번호 : ^\d{3}-\d{2}$

Pattern 클래스 / Matcher 클래스 두가지를 활용한 방법이 있음
```

## CODE 1 : boolean matches(String regex)
```java
public class Main {
	public static void main(String[] args) throws Exception {
		String inp_str = "dddad";
		boolean check1 = Pattern.matches("^(?=.{8,15}$).*", inp_str); // 8 ~ 15 w자리수 문자열

		boolean check2 = Pattern.matches("^[a-zA-Z0-9~!@#$%^&*]*$", inp_str); // rule2. 소문자, 대문자, 숫자, 특수문자로만 구성된 문자열

		boolean check3 = Pattern.matches("^(?=.*[a-z]).*$", inp_str); // 소문자 존재
		boolean check4 = Pattern.matches("^(?=.*[A-Z]).*$", inp_str); // 대문자 존재
		boolean check5 = Pattern.matches("^(?=.*[0-9]).*$", inp_str); // 숫자 존재
		boolean check6 = Pattern.matches("^(?=.*[~!@#$%^&*]).*$", inp_str); // 특수문자 존재
		boolean check7 = Pattern.matches(".*(\\w)\\1\\1\\1.*", inp_str); // 숫자 + 문자 4번 연속 사용 불가
		boolean check8 = Pattern.matches(".*([~!@#$%^&*])\\1\\1\\1.*", inp_str); // 특수 문자 4번 연속 사용 불가

		System.out.println(Pattern.matches(".*(\\w)\\1\\1\\1.*", "aa")); // false
		System.out.println(Pattern.matches(".*(\\w)\\1\\1\\1.*", "aaa")); // false
		System.out.println(Pattern.matches(".*(\\w)\\1\\1\\1.*", "aaaa")); // true
		System.out.println(Pattern.matches(".*(\\w)\\1\\1\\1.*", "aaaaa")); // true
		System.out.println(Pattern.matches(".*(\\w)\\1\\1\\1.*", "aaaaaa")); // true
		System.out.println(Pattern.matches(".*(\\w)\\1\\1\\1.*", "caaa")); // false
		System.out.println(Pattern.matches(".*(\\w)\\1\\1\\1.*", "caaa")); // false
		System.out.println(Pattern.matches(".*(\\w)\\1\\1\\1.*", "aaac")); // false
		System.out.println(Pattern.matches(".*(\\w)\\1\\1\\1.*", "caaac")); // false

		String[] skill_trees = {"BACDE", "CBADF", "AECB", "BDA"};

		for (int i = 0; i < skill_trees.length; i++) {
			skill_trees[i] = skill_trees[i].replaceAll("[EF]", ""); // EF 전부 제거 {"BACD", "CBAD", "ACB", "BDA"};
		}

		for (int i = 0; i < skill_trees.length; i++) {
			skill_trees[i] = skill_trees[i].replaceAll("[^EF]", ""); // EF 말고 전부 제거 {"E", "F", "E", ""};
		}
	}
}
```

## CODE 2 : String replaceAll(String regex, String replacement)
```java
public class Main {
	public static void main(String[] args) throws Exception {
		String[] skill_trees = {"BACDE", "CBADF", "AECB", "BDA"};

		for (int i = 0; i < skill_trees.length; i++) {
			skill_trees[i] = skill_trees[i].replaceAll("[EF]", ""); // EF 전부 제거 {"BACD", "CBAD", "ACB", "BDA"};
		}

		for (int i = 0; i < skill_trees.length; i++) {
			skill_trees[i] = skill_trees[i].replaceAll("[^EF]", ""); // EF 말고 전부 제거 {"E", "F", "E", ""};
		}
	}
}
```

## COMMENT
* [링크](https://offbyone.tistory.com/400)
* [링크](https://coding-factory.tistory.com/529)
* [링크](https://hee-kkk.tistory.com/22)
* [링크](https://doorisopen.github.io/developers-library/Java/2020-07-01-java-regular-expression)


```
1. 가장 많이 사용되는 최소 8자리에 숫자, 문자, 특수문자 각각 1개 이상 포함
"^(?=.*[A-Za-z])(?=.*\d)(?=.*[$@$!%*#?&])[A-Za-z\d$@$!%*#?&]{8,}$"

2. 최소 8자리에 대문자 1자리 소문자 한자리 숫자 한자리
"^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)[a-zA-Z\d]{8,}$"

3. 최소 8자리에 대문자 1자리 소문자 1자리 숫자 1자리 특수문자 1자리
"^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[$@$!%*?&])[A-Za-z\d$@$!%*?&]{8,}"

이메일 정규식 예제
"^[A-Z0-9._%+-]+@[A-Z0-9.-]+\\.[A-Z]{2,6}$"

위도, 경도 정규식 예제
위도, 경도 정규식(-90~90,-180~180 소수점 6자리)
^[-+]?([1-8]?\d(\.\d+)?|90(\.0+)?),\s*[-+]?(180(\.0+)?|((1[0-7]\d)|([1-9]?\d))(\.\d+)?)$

IP주소 정규식 예제
"^(([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])\.){3}([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])$"


1. 암호:
조건1. 6~20 영문 대소문자
조건2. 최소 1개의 숫자 혹은 특수 문자를 포함해야 함
/^(?=.*[a-zA-Z])((?=.*\d)|(?=.*\W)).{6,20}$/

2. 전자우편 주소:
/^[a-z0-9_+.-]+@([a-z0-9-]+\.)+[a-z0-9]{2,4}$/

3. URL:
/^(file|gopher|news|nntp|telnet|https?|ftps?|sftp):\/\/([a-z0-9-]+\.)+[a-z0-9]{2,4}.*$/

4. HTML 태그 – HTML tags:
/\<(/?[^\>]+)\>/

5. 전화 번호 – 예, 123-123-2344 혹은 123-1234-1234:
/(\d{3}).*(\d{3}).*(\d{4})/

6. 날짜 – 예, 3/28/2007 혹은 3/28/07:
/^\d{1,2}\/\d{1,2}\/\d{2,4}$/

7. jpg, gif 또는 png 확장자를 가진 그림 파일명:
/([^\s]+(?=\.(jpg|gif|png))\.\2)/

8. 1부터 50 사이의 번호 – 1과 50 포함:
/^[1-9]{1}$|^[1-4]{1}[0-9]{1}$|^50$/

9. 16 진수로 된 색깔 번호:
/#?([A-Fa-f0-9]){3}(([A-Fa-f0-9]){3})?/

전자우편 주소
/^[a-z0-9_+.-]+@([a-z0-9-]+\.)+[a-z0-9]{2,4}$/

URL
/^(file|gopher|news|nntp|telnet|https?|ftps?|sftp):\/\/([a-z0-9-]+\.)+[a-z0-9]{2,4}.*$/

HTML 태그 - HTML tags:
/\<(/?[^\>]+)\>/

전화 번호 - 예, 123-123-2344 혹은 123-1234-1234:
/(\d{3}).*(\d{3}).*(\d{4})/

날짜 - 예, 3/28/2007 혹은 3/28/07:
/^\d{1,2}\/\d{1,2}\/\d{2,4}$/

jpg, gif 또는 png 확장자를 가진 그림 파일명:
/([^\s]+(?=\.(jpg|gif|png))\.\2)/

1부터 50 사이의 번호 - 1과 50 포함:
/^[1-9]{1}$|^[1-4]{1}[0-9]{1}$|^50$/

16 진수로 된 색깔 번호:
/#?([A-Fa-f0-9]){3}(([A-Fa-f0-9]){3})?/

적어도 소문자 하나, 대문자 하나, 숫자 하나가 포함되어 있는 문자열(8글자 이상 15글자 이하)
 - 올바른 암호 형식을 확인할 때 사용될 수 있음:
/(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,15}/

```
