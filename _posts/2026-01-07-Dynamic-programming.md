---
layout: post
title: Dynamic programming - 피보나치 함수 문제
author: Minseo Kim
tags: []
date: 2026-01-07 01:00 +0900
---

백준에서 순서대로 문제를 풀다가 오랜만에 피보나치 함수 문제를 마추쳤다. 복기도 할 겸, 까먹지 않도록 정리해보려 한다.

## 피보나치 수열

첫 두 항이 0 1 이고, 그 이후 항부터는 앞의 두 항의 합으로 이루어지는 무한 수열.
{% highlight text %}
0  1  1  2  3  5  8  13  21  34  55  89  144  ....
{% endhighlight %}

## 피보나치 함수

아래는 백준에서 풀었던 피보나치 함수 문제이다.

C++ 로 구현한 피보나치 함수이다. (재귀 함수)
{% highlight python linenos %}
int fibonacci(int n) {
    if (n == 0) {
        printf("0");
        return 0;
    } else if (n == 1) {
        printf("1");
        return 1;
    } else {
        return fibonacci(n‐1) + fibonacci(n‐2);
    }
}
{% endhighlight %}


N이 주어졌을 때, fibonacci(N)을 호출했을 때, 0과 1이 각각 몇 번 출력되는지 구하는 프로그램을 작성하시오.
(첫 번째 줄에 주어지는 숫자는 N의 개수 T이다.)

**예제 입력**
{% highlight python %}
3
0
1
3
{% endhighlight %}

**예제 출력**
{% highlight python %}
1 0
0 1
1 2
{% endhighlight %}

## Dynamic programming (동적 계획법)

동적 계획법은 복잡한 문제를 더 작은 하위 문제로 나누어 해결하는 **알고리즘 설계 기법**이다.
중복되는 계산 결과를 저장하는 Memorization 을 사용하고, 계산한 값을 저장 후 필요 시에
다시 꺼내쓰기 때문에 효율적인 시간 단축을 가져올 수 있다.

피보나치 함수 문제는 이러한 동적 계획법을 사용해야 timeout 없이 문제를 잘 해결할 수 있었다.
아래는 나의 제출 답변이다.
{% highlight python linenos %}
t = int(input())
l = [None] * 41
l[0] = (1, 0)
l[1] = (0, 1)

def fibonacci_count(n):
    if l[n] != None:
        return l[n]
    else:
        a = fibonacci_count(n-1)
        b = fibonacci_count(n-2)
        l[n] = (a[0] + b[0], a[1] + b[1])
        return l[n]

for i in range(t):
    n = int(input())
    tu = fibonacci_count(n)
    print(tu[0], tu[1])
{% endhighlight %}

나의 코드에 대한 설명을 덧붙이자면 이러하다. 
N의 범위가 40이하 이므로, 모든 요소가 None인 41 길이의 리스트를 
만들어 l[N] 위치에 계산한 fibonacci_count(N) 의 값을 저장해둔다. (DP 사용)
이후의 함수 사용에서 이미 계산한 fibonacci_count 함수 값을 다시 사용하려고 한다면, l[n]을 리턴한다.
만일 이전에 계산한 적 없는 N의 함수값이라면 N-1, N-2의 함수값을 각각 더해서 직접 구해준 이후, list에 저장해준다.