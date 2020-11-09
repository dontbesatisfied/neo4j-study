## Docker Clustering

https://neo4j.com/docs/operations-manual/4.0-preview/docker/clustering/

https://neo4j.com/docs/operations-manual/current/docker/clustering/#docker-cc

현재 sharding은 Enterprise feature ...

## 메모리 관리

Total Physical Memory = Heap + Page Cache + OS Memory

### Heap size

JVM Heap memory는 자바의 메모리 영역으로 모든 클래스 인스턴스 및 배열에 대한 메모리가 할당되는 런타임 데이터 영역

사용가능한 힙사이즈의 양은 성능에 아주 중요한 부분이다.

힙사이즈는 너무 크게 설정하면 GC가 일어날때 시스템전체가 멈출수가 있으므로 16GB를 넘지 않는것이 좋다.

8GB~16GB가 안정적으로 실행하기에 충분하다

dbms.memory.heap.initial_size 와 dbms.memory.heap.max_size 같게 설정하자

For a more thorough discussion on this topic, refer to the heap memory configuration section in the Neo4j Operations Manual. That section also contains information about heap memory distribution and gabarge collection tuning.
(https://neo4j.com/docs/operations-manual/current/performance/memory-configuration/#heap-sizing)

### Page cache size

디스크에 저장된 데이터를 캐시하는데 사용된다. 디스크 액세스는 비용이 많이 들기때문에 메모리에 캐시하면 속도이점이 있다.
(현재 저장량 + 늘어날양) \* 1.2 정도가 적당하다.
만약 설정하지 않으면 기본적으로 현재 사용가능한 메모리의 50%가 할당된다.

### OS memory

운영체제 자체의 프로세스를 실행하기위해 일부 메모리를 예약해야한다.
이것은 디비가 구성된 뒤에 사용해야하기때문에 명시적으로 설정은 불가능하지만 보통 1GB면 충분하다
혹시모르니 전체 메모리 - (힙 + 페이지캐시) = 2이상되게하자

### LOG size

Logical transaction logs는 비정상 종료 후 복구할때 사용된다.
온라인 백업작업, incremental 백업에도 사용된다.

dbms.tx_log.rotation.retention_policy 을 7일로 잡는것을 추천한다.
default value = 7 days

===

https://neo4j.com/developer/kb/how-to-estimate-initial-memory-configuration/

https://neo4j.com/developer/guide-performance-tuning/
