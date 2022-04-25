# HashTable.md
작성자 : 김다인

---
### Hash Table, 해시 테이블
해시 테이블의 특징은 '빠른 검색'이다.  
먼저 테이블 자료구조에 대해 알아보자면, 관계형 데이터베이스에서 자주 쓰이는 것이 테이블이다. key-value 쌍으로 이뤄져 있어 특정 프로그래밍 언어에서는 맵Map이나 딕셔너리Dictionary 자료형으로 불리기도 한다.  
테이블 자료구조에서 key는 원하는 value를 찾는 검색 도구가 되기 때문에 의미 있는 데이터로 정의하는 것이 좋다. 테이블 자료구조에서 탐색 연산은 O(1) 시간 복잡도를 보인다.  
일반 테이블에서 더 나아간 해시 테이블은 내부적으로 배열을 사용해 데이터를 저장한다. 특정 값을 검색하는 데 있어 중복되지 않는 고유한 인덱스를 갖고 있기 때문에 해시 테이블에서 평균적인 검색의 경우 O(1) 시간 복잡도를 갖게 된다. 이 때, 일반 테이블과 달리 해시 테이블이 '평균적인 경우' O(1)이라고 하는 것은 해시 함수와 충돌 문제 때문이다.  

### 해시 함수
만약 해시 함수가 없다면 모든 데이터에게 따로 자리를 줘야 할 텐데, 그렇다면 검색은 굉장히 빠르지만 데이터가 많아진다면 공간 역시 많이 필요하게 된다. 이런 공간 문제 해결을 위해 해시 함수hash function를 사용하는데, 이 해시 함수는 저장할 value와 관련된 고유 숫자를 만들어 그것을 인덱스로 사용할 수 있도록 한다. 이 때 해시 함수에 의해 만들어진 숫자값은 해시 코드hashcode라고 부른다.  
```python
def hashFunc(k): 
  return k % 100
``` 
위의 함수도 해시 함수가 될 수 있다. 하지만 매우 단순하기 때문에 별로 좋은 해시 함수가 될 것 같지는 않다.  
좋은 해시 함수가 되려면 그 함수를 거치고 난 다음의 해시코드가 거의 제각각인 것이 좋다. 데이터들이 적당히 분산된 상태로 테이블에 존재하는 게 좋다는 이야기인데, 데이터가 특정 위치에 몰려 있다면 충돌 발생 확률이 높아지기 때문이다.  
좋은 해시 함수는 key의 일부를 참조하지 않고 key 전체를 참조해서 해시코드를 만든다. 이것이 충돌 발생 확률을 줄이기 때문이다.  

### 충돌 문제 해결책
1. 선형 조사법 (Linear Probing)
- 선형 조사법은 순차적으로 탐색하며, 비어 있는 버킷Bucket(공간)을 찾을 때까지 N칸 옆 슬롯을 계속 검사하는 것이다. 선형 조사법은 단순하지만, 충돌 횟수가 증가하면서 특정 영역에 데이터가 몰리는 클러스터 현상이 발생해 충돌 횟수가 늘어날수록 비효율적이다.
```
f(k)+1 -> f(k)+2 -> f(k)+3 -> ...
```
2. 이차 조사법
- 이차 조사법은 선형 조사법보다 더 멀리서 빈자리를 찾는다. 선형 조사법과 방식은 비슷하지만 더 멀리서 찾는 것이다.
```
f(k)+1^2 -> f(k)+2^2 -> f(k)+3^2 -> ...
```
3. 이중 해시 사용
- 해시코드가 같으면 충돌이 발생할 때 빈 버킷을 찾기 위한 접근 위치가 늘 동일하다. 이 문제를 해결하기 위해 2개의 해시 함수를 활용한다. 하나의 해시 함수에서 충돌이 발생하면, 2차 해시 함수를 이용해 새로운 인덱스를 할당하는 방법이다. 
- 이 방법은 클러스터 현상을 줄일 수 있지만 연산량이 많이 필요하다는 단점이 있다.

위의 1~3번까지는 개방 주소 모델이다.  

4. 체이닝 (Closed Addressing)
- 체이닝의 경우 개방 주소 모델과 달리 해시 함수에 의해 만들어진 해시코드 자리에 데이터를 저장할 수 있도록 충돌을 해결한다. 즉, 한 해시값에 다수의 데이터를 저장할 수 있도록 배열을 2차원 형태로 구성하거나 Node들을 포함하는 배열을 선언해 연결 리스트 방식으로 구현한다.  

### 해시 테이블을 사용하는 이유
* 충돌의 위험이 있지만, 비교적 적은 자원으로 데이터를 효율적으로 관리할 수 있다. 
* 또한 하드디스크, 클라우드의 데이터들을 유한 개수의 해시값으로 맵핑해 메모리를 조금 쓰고도 프로세스 관리가 가능하다.
--- 

* 접근 : O(1)
* 삽입 : O(1)
* 삭제 : O(1)
* 검색 : O(1)
 ---
* [참고 자료 : gyoogle tech interview for developer](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Data%20Structure/Hash.md)
* 2학년 때 들은 자료구조와 알고리즘 수업자료 