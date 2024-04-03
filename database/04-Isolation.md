### Q1. 트랜잭션 격리 레벨에 대해 설명해 주세요.
A1. 4단계가 있다.
- READ UNCOMMITED: 커밋하지 않은 데이터에 접근할 수 있다. 트랜잭션이 완료되지 않아도 값을 볼 수 있는 Dirty Read 문제가 생긴다. 
- READ COMMITED: 커밋된 데이터만 조회할 수 있다. Phantom Read와 Non-Repeatable Read 문제가 발생할 수 있다. undo 로그에서 커밋된 데이터를 반환하며, 1번째 select - record 추가 transaction - 2번쨰 select를 하면 값이 달라지는 문제가 발생할 수 있는데, 이는 금전적 처리와 연결될 때 특히 중요하다. Tran A가 count를 하고 있는데 Tran B가 계속 레코드를 추가하며 커밋하고 있다면 tran A에서 조회할 때마다 값이 다를 것이다. 커밋된 데이터만 보여주므로 transaction 밖/안에서 select를 실행하는 것의 차이가 없다.
- REPEATABLE READ: 변경 전의 레코드를 undo 공간에 transaction 번호와 함께 백업해두어, 자신보다 먼저 실행된 트랜잭션의 데이터만을 조회하여 필요하다면 undo공간의 data를 읽는다. MVCC(Multi-Version Concurrency Control)를 통해 새로운 레코드의 추가로 두번째 select에 추가 레코드가 발견되는 Phantom Read도 막을 수 있는데(tran id 비교), lock이 생길경우에는 발생될 수 있다. 그러나 MySQL은 SELECT FOR UPDATE을 실행하면 gap lock이 있어서 생기지 않는다.
- SERIALIZABLE: 트랜젝션을 순차적으로 진행, 동일한 레코드에 여러 트랜잭션이 동시에 접근할 수 없음. 읽기/쓰기 잠금을 통해 구현됨. SELECT문 작업에서도 Shared Lock으로 걸음.

### Q2. 모든 DBMS가 4개의 레벨을 모두 구현하고 있나요? 그렇지 않다면 그 이유는 무엇일까요?
A2. 아닙니다. 이유는 모릅니다. 

### Q3. 만약 MySQL을 사용하고 있다면, (InnoDB 기준) Undo 영역과 Redo 영역에 대해 설명해 주세요.
A3. InnoDB는 격리 수준과 롤백을 보장하기 위해 insert/update/delete로 변경되기 전의 데이터를 Undo log에 보관합니다. Redo log는 비정상적으로 종료된 트랜잭션에 의해 변경된 데이터를 복구하기 위해 사용됩니다. MySQL은 변경 내용을 바로 디스크에 저장하지 않고 InnoDB buffer pool에 먼저 저장하기 때문에 디스크에 저장되지 않은 데이터에 대한 처리가 필요하기 때문입니다.

트랜잭션이 커밋되면 버퍼 풀에 있는 내용을 디스크에 저장하며 롤백할 시 undo log에 기록된 내용을 다시 복원합니다.

### Q4. 그런데, 스토리지 엔진이 정확히 무엇을 하는 건가요?
A4. 스토리지 엔진 (데이터베이스 엔진)은 데이터베이스에서 데이터를 어떻게 물리적으로 저장하고 접근할 것인지를 담당하는 컴포넌트다. 물리적 데이터 관리로부터 시작된 데이터 삽입, 업데이트, 삭제 요청등을 처리하며 메모리, 인덱스 등의 관리를 통해 운영된다. 따라서 엔진마다 접근 속도, 트랜잭션 기능 지원 정도에 차이가 있다.

### References
https://mangkyu.tistory.com/299
https://zzang9ha.tistory.com/381
https://blog.ex-em.com/1680