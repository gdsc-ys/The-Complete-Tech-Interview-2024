### Q1. 트랜잭션 격리 레벨에 대해 설명해 주세요.
A1. 4단계가 있다.
- READ UNCOMMITED <br>
커밋하지 않은 데이터에 접근할 수 있다. 트랜잭션이 완료되지 않아도 값을 볼 수 있는 Dirty Read 문제가 생긴다. 
- READ COMMITED <br>
    - 커밋된 데이터만 조회할 수 있다. 즉 dirty read가 발생하지 않는다.
    - 대신 Phantom Read와 Non-Repeatable Read 문제가 발생할 수 있다.
        - Non-Repeatable Read; 하나의 row를 조회 후 재조회했을 때 데이터가 동일하다는 보장이 없음. 그 사이 다른 트랜잭션에서 해당 데이터를 갱신하고 커밋하였기 때문이다. (즉, lock도 없고 버전관리도 없기 때문)
        - Tran A가 count를 하고 있는데 Tran B가 계속 레코드를 추가하며 커밋하고 있다면 tran A에서 조회할 때마다 값이 다를 것이다.  
    - 커밋된 데이터만 보여주므로 transaction 밖/안에서 select를 실행하는 것의 차이가 없다.
- REPEATABLE READ <br>
    - 변경 전의 레코드 와 변경전의 값만을 undo 공간에 백업해두고 commit이 되기 전까지는 해당 레코드 조회 요청은 undo공간의 data를 읽는다. 
    - 새로운 레코드가 추가되는 경우의 부정합인 Phantom Read가 생길 수 있다. (* Non-Repeatable과의 차이: 한 트랜잭션에서 select 쿼리를 두번 시행한 건 동일하나 phantom은 새로운 row가 등장했는지/기존 row가 삭제되었는지 여부이고 non repeatable은 row의 값이 바뀌었는지 여부이다.)
    - mysql은 record에 transaction id를 같이 기록하는 MVCC(Multi-Version Concurrency Control)를 통해  Phantom Read를 막을 수 있다(tran id 비교).
- SERIALIZABLE <br>
    - 트랜젝션을 순차적으로 진행한다.
    - 동일한 레코드에 여러 트랜잭션이 동시에 접근할 수 없음. 
    - (MySQL) 원래 select 작업은 아무런 잠금 없이 실행되나, 해당 단계에서는 shard lock을 걸어 데이터 부정합 문제를 방지한다. 따라서 동시 처리 성능이 떨어진다.

### Q2. 모든 DBMS가 4개의 레벨을 모두 구현하고 있나요? 그렇지 않다면 그 이유는 무엇일까요?
A2. 아닙니다. PostgreSQL에서는 Read Uncommitted 단계를 지원하지 않습니다.

### Q3. 만약 MySQL을 사용하고 있다면, (InnoDB 기준) Undo 영역과 Redo 영역에 대해 설명해 주세요.
A3. InnoDB는 격리 수준과 롤백을 보장하기 위해 insert/update/delete로 변경되기 전의 데이터를 Undo log에 보관합니다. Redo log는 비정상적으로 종료된 트랜잭션에 의해 변경된 데이터를 복구하기 위해 사용됩니다. MySQL은 변경 내용을 바로 디스크에 저장하지 않고 InnoDB buffer pool에 먼저 저장하기 때문에 디스크에 저장되지 않은 데이터에 대한 처리가 필요하기 때문입니다.

트랜잭션이 커밋되면 버퍼 풀에 있는 내용을 디스크에 저장하며 롤백할 시 undo log에 기록된 내용을 다시 복원합니다.

### Q4. 그런데, 스토리지 엔진이 정확히 무엇을 하는 건가요?
A4. 스토리지 엔진 (데이터베이스 엔진)은 데이터베이스에서 데이터를 어떻게 물리적으로 저장하고 접근할 것인지를 담당하는 컴포넌트다. 물리적 데이터 관리로부터 시작된 데이터 삽입, 업데이트, 삭제 요청등을 처리하며 메모리, 인덱스 등의 관리를 통해 운영된다. 따라서 엔진마다 접근 속도, 트랜잭션 기능 지원 정도에 차이가 있다.

### References
https://mangkyu.tistory.com/299
https://zzang9ha.tistory.com/381
https://blog.ex-em.com/1680