# 데이터베이스 테스트: DbUnit

## DbUnit의 장점

DB를 사용하는 부분은 프로그래밍 언어 외적인 부분이 상당 부분 포함되기 때문에 TDD를 적용하기가 종종 쉽지 않다. 특히 데이터베이스의 상태를 일정하게 유지하면서 테스트를 지속적으로 수행 가능하게 만드는 데는 고통스러운 작업이 필요하다. 또한 테스트 전후의 데이터베이스 상태를 비교하는 것도 손쉬운 일은 아니다.

이럴 때 도움을 받을 수 있는 대표적인 유틸리티로 DbUnit(www.dbunit.org)이 있다. DbUnit은 다음과 같은 기능을 제공함으로써 DB를 사용하는 테스트 케이스 작성 시에 도움을 준다.

- #### 독립적인 데이터베이스 연결을 지원한다.

   JDBC, DataSource, JNDI 방식 지원

- ####  데이터베이스의 특정 시점 상태를 쉽게 내보내거나(export) 읽어들일(import) 수 있다.

  xml 파일이나 csv 파일 형식을 지원한다. import 계열 동작일 경우에는 엑셀 파일도 지원한다.

- ####  테이블이나 데이터셋을 서로 쉽게 비교할 수 있다.

보통 DbUnit은 독립적으로 사용하기보다는 JUnit 등의 테스트 프레임워크 등과 함께 사용한다. 그래서 DbUnit은 테스트 프레임워크라기보다는 테스트 지원 라이브러리에 더 가깝다. DbUnit을 알면 테스트 케이스 작성 시 DB 관리가 매우 편리해진다. 하지만 그러기 위해서는 제일 먼저 알아야 하는 DbUnit의 개념이 하나 있다. 바로 데이터셋(Dataset)이다.