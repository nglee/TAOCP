<script src='https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.4/MathJax.js?config=TeX-MML-AM_CHTML' async></script>

이 포스트에서는 컴퓨터 프로그래밍의 예술 2장 2.6절의 연습문제 6번에 대한 풀이과정을 설명하겠습니다.

먼저 풀이에 반복적으로 사용될 두 가지 규칙을 설명하겠습니다. 첫 번째 규칙은 다음과 같습니다. \\((k \\ge r)\\)

\\[
\\sum_{i=r}^{k}\\binom{i}{r} = \\binom{r}{r} + \\binom{r+1}{r} + \\cdots + \\binom{k}{r} = \\binom{k+1}{r+1}
\\]

수학적 귀납법으로 증명할 수도 있고, 공식 \\(\\binom{n}{r} + \\binom{n}{r+1} = \\binom{n+1}{r+1}\\) 을 반복 적용하는 것으로도 증명 가능합니다.

이를 응용하면 다음이 성립함을 알 수 있습니다. \\((n \\gt m \\gt r)\\)

\\[
\\begin{align}
&\\binom{n}{r} + \\binom{n-1}{r}  + \\cdots + \\binom{m}{r}\\newline
= &\\left\\{\\binom{n}{r} + \\binom{n-1}{r}  + \\cdots + \\binom{m}{r} + \\binom{m-1}{r} + \\binom{m-2}{r} + \\cdots + \\binom{r}{r}\\right\\} - \\left\\{\\binom{m-1}{r} + \\binom{m-2}{r} + \\cdots + \\binom{r}{r}\\right\\}\\newline
= &\\binom{n+1}{r+1} - \\binom{m}{r+1}
\\end{align}
\\]

아래 풀이에서는 위의 두 규칙을 반복해서 사용합니다.

# 파트1

> "4면체 배열" \\(A[i, j, k]\\) 가 \\(0 \\le k \\le j \\le i \\le n\\) 를 만족하고 사전식 순서에 따라 연속적인 메모리 공간에 저장되어 있다고 가정할 때, \\(LOC(A[i, j, k])\\) 를 \\(a_0 + f_1(i) + f_2(j) + f_3(k)\\) 꼴로 나타내어라. (\\(a_0\\) 는 base address, \\(f_1\\), \\(f_2\\), \\(f_3\\) 는 함수)

사전식 순서로 배열한다면 \\((i, j, k)\\) 가 아래 표와 같은 순서로 증가하게 됩니다. 아래 표에서는 \\(i\\) 값이 같은 것들을 하나의 열에 배치하고 \\(j\\), \\(k\\) 값이 같은 것들을 하나의 행에 배치하였습니다.

\\[
\\begin{array}{c|c|c|c|c|c}
(i, j, k) & i = 0 & i = 1 & i = 2 & \\cdots & i = n \\newline
\\hline
j = 0,\\ k = 0 & (0, 0, 0) & (1, 0, 0) & (2, 0, 0) & \\cdots & (n, 0, 0) \\newline
j = 1,\\ k = 0 & \\text{-} & (1, 1, 0) & (2, 1, 0) & \\cdots & (n, 1, 0) \\newline
j = 1,\\ k = 1 & \\text{-} & (1, 1, 1) & (2, 1, 1) & \\cdots & (n, 1, 1) \\newline
j = 2,\\ k = 0 & \\text{-} & \\text{-} & (2, 2, 0) & \\cdots & (n, 2, 0) \\newline
j = 2,\\ k = 1 & \\text{-} & \\text{-} & (2, 2, 1) & \\cdots & (n, 2, 1) \\newline
j = 2,\\ k = 2 & \\text{-} & \\text{-} & (2, 2, 2) & \\cdots & (n, 2, 2) \\newline
\\cdots & \\text{-} & \\text{-} & \\text{-} & \\cdots & \\cdots \\newline
j = n,\\ k = n & \\text{-} & \\text{-} & \\text{-} & \\text{-} & (n, n, n) \\newline
\\end{array}
\\]

#### \\(f_1(i)\\) 구하기

먼저 \\(i\\) 값이 증가함에 따라 \\((i, 0, 0)\\) 의 메모리 주소 값이 base address(\\(a_0\\)) 에서 얼마씩 증가해야 하는지 살펴보겠습니다. 위의 표를 살펴보면,

\\[
\\begin{align}
i &= 0\\text{ 일 때 }0,\\newline
i &= 1\\text{ 일 때 }1,\\newline
i &= 2\\text{ 일 때 }1 + (1 + 2),\\newline
i &= 3\\text{ 일 때 }1 + (1 + 2) + (1 + 2 + 3),\\newline
&\\cdots\\newline
i &= n\\text{ 일 때 }1 + (1 + 2) + (1 + 2 + 3) + \\cdots + (1 + 2 + \\cdots + n)
\\end{align}
\\]

만큼 증가해야 함을 알 수 있습니다. 따라서,

\\[
\\begin{align}
f_1(i) &= \\sum_{l = 0}^{i}\\frac{l(l+1)}{2}\\newline
&= \\sum_{l = 0}^{i}\\binom{l+1}{2}\\newline
&= \\binom{2}{2} + \\binom{3}{2} + \\cdots + \\binom{i+1}{2}\\newline
&= \\binom{i+2}{3}
\\end{align}
\\]

#### \\(f_2(j)\\) 구하기

\\(i\\) 값이 고정되어 있을 때 \\(j\\) 값이 증가함에 따라 \\((i, j, 0)\\) 의 메모리 주소 값이 \\((i, 0, 0)\\) 의 메모리 주소에서 얼마씩 증가해야 하는지 살펴보겠습니다. 위의 표를 살펴보면,

\\[
\\begin{align}
j &= 0\\text{ 일 때 }0,\\newline
j &= 1\\text{ 일 때 }1,\\newline
j &= 2\\text{ 일 때 }1 + 2,\\newline
j &= 3\\text{ 일 때 }1 + 2 + 3,\\newline
&\\cdots\\newline
j &= n\\text{ 일 때 }1 + 2 + \\cdots + n
\\end{align}
\\]

만큼 증가해야 함을 알 수 있습니다. 따라서,

\\[
\\begin{align}
f_2(j) &= \\sum_{l = 0}^{j}l\\newline
&= \\frac{j(j+1)}{2}\\newline
&= \\binom{j+1}{2}
\\end{align}
\\]

조금 다른 방식으로 생각하면,

\\[
\\begin{align}
f_2(j) &= \\sum_{l = 0}^{j}l\\newline
&= \\sum_{l = 0}^{j}\\binom{l}{1}\\newline
&= \\binom{1}{1} + \\binom{2}{1} + \\cdots + \\binom{j}{1}\\newline
&= \\binom{j+1}{2}
\\end{align}
\\]

#### \\(f_3(k)\\) 구하기

마지막으로 \\(i\\) 와 \\(j\\) 값이 고정되어 있을 때, \\(k\\) 값이 증가함에 따라 \\((i, j, k)\\) 의 메모리 주소 값이 \\((i, j, 0)\\) 의 메모리 주소에서 얼마씩 증가해야 하는지 살펴보면, 다음과 같음을 알 수 있습니다.

\\[
f_3(k) = k = \\binom{k}{1}
\\]

#### 답

\\[
LOC(A[i, j, k]) = a_0 + \\binom{i+2}{3} + \\binom{j+1}{2} + \\binom{k}{1}
\\]

# 파트2

> "4면체 배열" \\(B[i, j, k]\\) 가 \\(0 \\le i \\le j \\le k \\le n\\) 를 만족하고 사전식 순서에 따라 연속적인 메모리 공간에 저장되어 있다고 가정할 때, \\(LOC(B[i, j, k])\\) 를 \\(b_0 + g_1(i) + g_2(j) + g_3(k)\\) 꼴로 나타내어라. (\\(b_0\\) 는 base address, \\(g_1\\), \\(g_2\\), \\(g_3\\) 는 함수)

사전식 순서로 배열한다면 \\((i, j, k)\\) 가 아래 표와 같은 순서로 증가하게 됩니다. 아래 표에서는 \\(i\\) 값이 같은 것들을 하나의 열에 배치하고 \\(j\\), \\(k\\) 값이 같은 것들을 하나의 행에 배치하였습니다.

\\[
\\begin{array}{c|c|c|c|c|c}
(i,\\ j,\\ k) & i = 0 & i = 1 & i = 2 & \\cdots & i = n \\newline
\\hline
j = 0,\\ k = 0  & (0, 0, 0) & \\text{-} & \\text{-} & \\text{-} & \\text{-} \\newline
j = 0,\\ k = 1 & (0, 0, 1) & \\text{-} & \\text{-} & \\text{-} & \\text{-} \\newline
j = 0,\\ k = 2 & (0, 0, 2) & \\text{-} & \\text{-} & \\text{-} & \\text{-} \\newline
\\cdots & \\cdots & \\text{-} & \\text{-} & \\text{-} & \\text{-} \\newline
j = 0,\\ k = n & (0, 0, n) & \\text{-} & \\text{-} & \\text{-} & \\text{-} \\newline
j = 1,\\ k = 1 & (0, 1, 1) & (1, 1, 1) & \\text{-} & \\text{-} & \\text{-} \\newline
j = 1,\\ k = 2 & (0, 1, 2) & (1, 1, 2) & \\text{-} & \\text{-} & \\text{-} \\newline
\\cdots & \\cdots & \\cdots & \\text{-} & \\text{-} & \\text{-} \\newline
j = 1,\\ k = n & (0, 1, n) & (1, 1, n) & \\text{-} & \\text{-} & \\text{-} \\newline
j = 2,\\ k = 2 & (0, 2, 2) & (1, 2, 2) & (2, 2, 2) & \\text{-} & \\text{-} \\newline
\\cdots & \\cdots & \\cdots & \\cdots & \\cdots & \\text{-} \\newline
j = n,\\ k = n & (0, n, n) & (1, n, n) & (2, n, n) & \\cdots & (n, n, n) \\newline
\\end{array}
\\]

#### \\(g_1(i)\\) 구하기

먼저 \\(i\\) 값이 증가함에 따라 \\((i, 0, 0)\\) 의 메모리 주소 값이 base address(\\(b_0\\)) 에서 얼마씩 증가해야 하는지 살펴보겠습니다. 위의 표를 살펴보면,

\\[
\\begin{align}
i &= 0\\text{ 인 것들은 총 }1 + 2 + \\cdots + (n - 1) + n + (n + 1)\\text{ 개 있습니다.}\\newline
i &= 1\\text{ 인 것들은 총 }1 + 2 + \\cdots + (n - 1) + n\\text{ 개 있습니다.}\\newline
i &= 2\\text{ 인 것들은 총 }1 + 2 + \\cdots + (n - 1)\\text{ 개 있습니다.}\\newline
&\\cdots\\newline
i &= n\\text{ 인 것들은 총 }1\\text{ 개 있습니다.}
\\end{align}
\\]

따라서,

\\[
\\begin{align}
i &= n\\text{ 이면 }&&(1 + 2 + \\cdots + (n + 1)) + \\cdots + (1 + 2 + 3) + (1 + 2)\\text{ 만큼 증가해야 합니다.}\\newline
i &= n - 1\\text{ 이면 }&&(1 + 2 + \\cdots + (n + 1)) + \\cdots + (1 + 2 + 3)\\text{ 만큼 증가해야 합니다.}\\newline
&\\cdots\\newline
i &= 1\\text{ 이면 }&&(1 + 2 + \\cdots + (n + 1))\\text{ 만큼 증가해야 합니다.}
\\end{align}
\\]

따라서, \\(1 + 2 + \\cdots + r = \\frac{r(r+1)}{2} = \\binom{r+1}{2}\\) 임을 이용하면,

\\[
\\begin{align}
g_1(n) &= \\binom{n+2}{2} + \\cdots + \\binom{4}{2} + \\binom{3}{2} &&= \\left\\{\\binom{n+2}{2} + \\cdots + \\binom{2}{2}\\right\\} - \\left\\{\\binom{2}{2}\\right\\}\\newline
g_1(n-1) &= \\binom{n+2}{2} + \\cdots + \\binom{4}{2} &&= \\left\\{\\binom{n+2}{2} + \\cdots + \\binom{2}{2}\\right\\} - \\left\\{\\binom{3}{2} + \\binom{2}{2}\\right\\}\\newline
&\\cdots&&\\cdots\\newline
g_1(1) &= \\binom{n+2}{2} &&= \\left\\{\\binom{n+2}{2} + \\cdots + \\binom{2}{2}\\right\\} - \\left\\{\\binom{n+1}{2} + \\cdots + \\binom{2}{2}\\right\\}
\\end{align}
\\]

따라서,

\\[
\\begin{align}
g_1(i) &= \\left\\{\\binom{n+2}{2} + \\cdots + \\binom{2}{2}\\right\\} - \\left\\{\\binom{n+2-i}{2} + \\cdots + \\binom{2}{2}\\right\\}\\newline
&= \\binom{n+3}{3} - \\binom{n+3-i}{3}
\\end{align}
\\]

#### \\(g_2(j)\\) 구하기

\\(i\\) 값이 고정되어 있을 때 \\(j\\) 값이 증가함에 따라 \\((i, j, j)\\) 의 메모리 주소 값이 \\((i, i, i)\\) 의 메모리 주소에서 얼마씩 증가해야 하는지 살펴보겠습니다. 이 값은 \\(j\\) 뿐만이 아니라 \\(i\\) 에도 의존하기 때문에, \\(j\\) 에 대한 함수로 바로 정리되지 않습니다. 우선 \\(i\\) 와 \\(j\\) 에 대한 함수 \\(\\hat{g}_2(i,j)\\) 로 정리하겠습니다.

\\[
\\begin{align}
j &= 0\\text{ 이면 }&&{i = 0}\\text{ 일 때 }0\\text{ 만큼 증가해야 합니다.}\\newline
j &= 1\\text{ 이면 }&&{i = 0}\\text{ 일 때 }n+1\\text{ 만큼 증가해야 합니다.}\\newline
& &&{i = 1}\\text{ 일 때 }0\\text{ 만큼 증가해야 합니다.}\\newline
j &= 2\\text{ 이면 }&&{i = 0}\\text{ 일 때 }n+(n+1)\\text{ 만큼 증가해야 합니다.}\\newline
& &&{i = 1}\\text{ 일 때 }n\\text{ 만큼 증가해야 합니다.}\\newline
& &&{i = 2}\\text{ 일 때 }0\\text{ 만큼 증가해야 합니다.}\\newline
j &= 3\\text{ 이면 }&&{i = 0}\\text{ 일 때 }(n-1)+n+(n+1)\\text{ 만큼 증가해야 합니다.}\\newline
& &&{i = 1}\\text{ 일 때 }(n-1)+n\\text{ 만큼 증가해야 합니다.}\\newline
& &&{i = 2}\\text{ 일 때 }(n-1)\\text{ 만큼 증가해야 합니다.}\\newline
& &&{i = 3}\\text{ 일 때 }0\\text{ 만큼 증가해야 합니다.}\\newline
&\\cdots
\\end{align}
\\]

다른 방식으로 표현하면,

\\[
\\begin{align}
i &= 0\\text{ 이면 }&&{j = 0}\\text{ 일 때 }0\\text{ 만큼 증가해야 합니다.}\\newline
& &&{j = 1}\\text{ 일 때 }(n+1)\\text{ 만큼 증가해야 합니다.}\\newline
& &&{j = 2}\\text{ 일 때 }(n+1)+n\\text{ 만큼 증가해야 합니다.}\\newline
& &&{j = 3}\\text{ 일 때 }(n+1)+n+(n-1)\\text{ 만큼 증가해야 합니다.}\\newline
& &&\\cdots\\newline
& &&{j = n}\\text{ 일 때 }(n+1)+n+(n-1)+\\cdots+2\\text{ 를 더해줘야 합니다.}\\newline
i &= 1\\text{ 이면 }&&{j = 1}\\text{ 일 때 }0\\text{ 만큼 증가해야 합니다.}\\newline
& &&{j = 2}\\text{ 일 때 }n\\text{ 만큼 증가해야 합니다.}\\newline
& &&{j = 3}\\text{ 일 때 }n+(n-1)\\text{ 만큼 증가해야 합니다.}\\newline
& &&\\cdots\\newline
& &&{j = n}\\text{ 일 때 }n+(n-1)+\\cdots+2\\text{ 를 더해줘야 합니다.}\\newline
&\\cdots
\\end{align}
\\]

따라서, \\(i \\lt j\\) 일 때,

\\[
\\begin{align}
\\hat{g}_2(i,j) &= \\binom{n+1-i}{1} + \\binom{(n+1-i)-1}{1} + \\binom{(n+1-i)-2}{1} + \\cdots + \\binom{(n+1-i)-(j-i-1)}{1}\\newline
&= \\binom{n+1-i}{1} + \\binom{(n+1-i)-1}{1} + \\binom{(n+1-i)-2}{1} + \\cdots + \\binom{n+2-j}{1}\\newline
&= \\left\\{\\binom{n+1-i}{1} + \\cdots + \\binom{1}{1}\\right\\} - \\left\\{\\binom{(n+2-j)-1}{1} + \\cdots + \\binom{1}{1}\\right\\}\\newline
&= \\binom{n+2-i}{2} - \\binom{n+2-j}{2}
\\end{align}
\\]

위 식은 \\(i = j\\) 일 때도 성립함을 알 수 있습니다.

#### \\(g_3(k)\\) 구하기

마지막으로 \\(i\\) 와 \\(j\\) 값이 고정되어 있을 때, \\(k\\) 값이 증가함에 따라 \\((i, j, k)\\) 의 메모리 주소 값이 \\((i, j, j)\\) 의 메모리 주소에서 얼마씩 증가해야 하는지 살펴보면, 다음과 같음을 알 수 있습니다. \\(k\\) 에 대한 함수로 바로 정리되지 않기 때문에, 우선 \\(j\\) 와 \\(k\\) 에 대한 함수 \\(\\hat{g}_3(j,k)\\) 로 정리하겠습니다.

\\[
\\hat{g}_3(j,k) = k-j = \\binom{k-j}{1}
\\]

#### 답

\\[
LOC(B[i, j, k]) = b_0 + \\binom{n+3}{3} - \\binom{n+3-i}{3} + \\binom{n+2-i}{2} - \\binom{n+2-j}{2} + \\binom{k-j}{1}
\\]
