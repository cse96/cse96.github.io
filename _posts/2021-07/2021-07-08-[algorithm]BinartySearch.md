# BinarySearch

### 이진탐색 알고리즘

- 오름차순으로 정렬된 리스트에서 특정한 값의 위치를 찾는 알고리즘이다.
- 정렬된 상태의 리스트의 중간 값을 임의로 선택해서 그 값과 찾는 값의 대소를 비교하는 방식
- 반복문, 재귀를 사용해 구현한다.

---

### 분할 & 정복

- 분할(Divide):  
  리스트를 중간값을 기준으로 두 개의 서브 리스트로 나눈다.
- 정복(Conquer):
  - 검색할 수 < 중간 값 : 앞 부분의 서브리스트에서 다시 검색
  - 검색할 수 > 중간 값: 뒷 부분의 서브리스트에서 다시 검색
  - 검색할 수 = 중간 값: 해당 값 리턴

```

```

---

n개의 리스트를 매번 2로 나누어 1이 될때까지 비교 연산을 진행한다.  
시간복잡도는 O(logn)이다.