출처: [노마드코더 - 개발자라면 알아야 하는 Big O](https://www.youtube.com/watch?v=BEVnxbxBqi8)

<BR/>

**알고리즘의 속도**는
컴퓨터 하드웨어 자체와의 속도와는 별개로
그보다는 **‘완료까지 걸리는 절차의 수’**로 정의된다.

예를 들어, 같은 작업을 수행하는 데 5개의 스텝이 걸리는 알고리즘이
10개 스텝이 걸리는 알고리즘보다 우수하다고 말할 수 있다.

시간복잡도 개념을 사용하는 이유는 빠르고 이해하기가 쉽기 때문이다.
사이즈가 N개면 'N 스텝의 알고리즘이 필요하다’고 얘기하는 것보다
그냥 ‘그 동작은 O(N)의 시간복잡도를 갖는다’ 하면 바로 알아들을 수 있다.

<BR/>

Big O를 이해하면 알고리즘의 특징 분석이 빠르고,
언제 무엇을 쓸지도 빠르게 파악이 가능하다.
또 내가 짠 코드가 어떤 시간복잡도를 갖는지 코드의 성능을 평가할 수도 있고, 미래에 어떻게 작동할지 예측할 수도 있다.

# O(1)

상수 시간 (constant time).
**찾는 범위가 아무리 커져도 똑같은 수행 시간**을 가지는 것

예를 들어 배열의 첫 번째 값을 찾는 로직은
배열이 10개든, 100개든 연산 수행에 걸리는 시간이 똑같다.

<img src="https://velog.velcdn.com/images/yena1025/post/809f752a-d906-45c6-93b2-2f841f5988d0/image.png" width="350" />

Big O는 함수의 디테일에는 관심이 없다.
⇒ 러프하게, 인풋 사이즈에 따라 이 함수가 어떻게 작동하는지에만 관심

따라서 똑같은 아이템을 2번 출력해도 상수(1, 2, ...)에는 신경쓰지 않기 때문에
여전히 O(1)로 표시한다.

인풋의 크기와 상관없이 무조건 일정한 시간복잡도를 가지는 O(1)은
매우 선호되는 알고리즘이지만 적용이 가능한 경우가 잘 없다.

# O(N)

인풋과 시간이 정확히 비례하여 증가하는 시간복잡도

예: 배열의 요소를 모두 출력하는 함수 ⇒ 배열 크기에 따라 연산하는 시간도 똑같이 늘어난다.

<img src="https://velog.velcdn.com/images/yena1025/post/8255c9f8-3fc5-4d1d-a9ee-3fb24589012b/image.png" width="350" />

Big O에서 상수는 신경 쓰지 않으므로
O(N) 코드가 두 개라고 해서 O(2N)이 되지 않는다.

다음 코드: for문이 나란히 2개 있어도(이중 for문 x) O(2N)이라 하지 않고 무조건 O(N)의 시간복잡도를 가진다.

<img src="https://velog.velcdn.com/images/yena1025/post/94b45e3e-66b8-47bd-97e6-ad335500d850/image.png" width="350" />

# O(N2) - 2차 시간(제곱, exponent)

**중첩 반복(Nested Loops)**이 있을 때 발생 ⇒ 예: 이중 for문

인풋이 10개라면 연산을 모두 수행하는 데 100번의 스텝 필요

제곱은 input이 조금만 늘어나도 시간이 기하급수적으로 늘어난다. ⇒ O(N)과 비교하면 O(N)이 더 효율적이다.

<img src="https://velog.velcdn.com/images/yena1025/post/2297e303-3296-4e1e-92ed-33f16d1685de/image.png" width="350" />

# O(logN): 로그 시간 (logarithm)

**이진검색 알고리즘**이 대표적이다.

**지수(제곱, exponent)와 정반대의 그래프**를 가진다.

## 지수(exponent)

**2를** **몇 번 곱해야** **32가 되나?** (답은 5)

<img src="https://velog.velcdn.com/images/yena1025/post/63fd18b2-0c74-480f-990c-eb7156c9c7c3/image.png" width="350" />

## 로그(logarithm)

**32를 2로** **몇 번 나눠야 1이 나오나?**

<img src="https://velog.velcdn.com/images/yena1025/post/e2f88100-9015-43e8-9236-018c5688b20e/image.png" width="350" />

32를 2로 1번 나누면 16 (32 ÷ 2 = 16)
32를 2로 2번 나누면 8 (16 ÷ 2 = 8)
32를 2로 3번 나누면 4 (8 ÷ 2 = 4)
32를 2로 4번 나누면 2 (4 ÷ 2 = 2)
32를 2로 5번 나누면 1 (2 ÷ 2 = 1)

마지막 찾는 값(1)이 나올 때까지 계속 절반으로 나눈다. ⇒ 이진검색과 유사

<img src="https://velog.velcdn.com/images/yena1025/post/7d4269ca-332d-4196-9175-c7d20aae1d77/image.png" width="350" />

지수(exponent)와 정반대로, input이 많아질수록 수행시간이

**처음에는 선형처럼 증가하다가 나중으로 갈수록 상수에 수렴**한다.

⇒ 선형시간보다는 빠르고 상수시간보다는 느린 알고리즘

⇒ **input이 많아질수록 수행시간이 감소**하므로 **대량의 데이터를 다룰 때 적합**

이진검색에서는 인풋 10개를 20개로 2배 늘려도

스텝은 1밖에 더 증가하지 않는다. ⇒ 1번만 더 나눠주면 되기 때문

<img src="https://velog.velcdn.com/images/yena1025/post/21e773fa-8d89-4bdf-862e-64b086ca582d/image.png" width="350" />

# 최종 정리

선호되는 순서는 아래부터

<img src="https://velog.velcdn.com/images/yena1025/post/e4c9dfbd-b4de-4570-a7e5-9562eaba7c45/image.png" width="400" />
