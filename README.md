# Schedule CLI (PyMySQL)

간단한 콘솔 기반 일정 관리 프로그램입니다. MySQL에 일정을 저장하고, 추가 / 조회 / 완료 처리 기능을 제공합니다. 포트폴리오용으로 정리한 예시 프로젝트입니다.

---

## 주요 기능

* 일정 추가: 제목, 내용, 시작/종료 시간 입력 후 DB에 저장
* 일정 조회: 등록된 일정 목록 확인
* 일정 완료: 특정 일정의 완료 상태 변경

## 사용 기술 스택

* Python 3.8+
* PyMySQL (MySQL / MariaDB 연동)
* MySQL / MariaDB

## 파일 구조 예시

```
schedule-cli/
├─ README.md
├─ schedule.py   # 제공된 메인 코드 (콘솔 인터페이스)
├─ requirements.txt
└─ .env.example
```

## 요구 사항

* Python 3.8 이상
* MySQL 또는 MariaDB 서버
* Python 패키지: `pymysql`

`requirements.txt` 예시:

```
pymysql
```

## 데이터베이스 설정

아래 SQL을 사용하여 `schedule_db` 데이터베이스와 `schedules` 테이블을 생성하세요.

```sql
CREATE DATABASE IF NOT EXISTS schedule_db;
USE schedule_db;

CREATE TABLE IF NOT EXISTS schedules (
  id INT AUTO_INCREMENT PRIMARY KEY,
  title VARCHAR(255) NOT NULL,
  description TEXT,
  start_datetime DATETIME,
  end_datetime DATETIME,
  is_completed TINYINT(1) NOT NULL DEFAULT 0,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**주의:** 제공된 코드에서는 사용자가 `start_datetime`, `end_datetime`를 `yyyymmddhhmmss` 형식으로 입력하도록 되어 있습니다. 실제로는 `YYYY-MM-DD HH:MM:SS` 형식의 `DATETIME` 컬럼을 사용하는 것이 일반적이며, 입력값을 변환하여 저장하는 것을 권장합니다.

## 환경 변수 구성

프로덕션/개발 환경에서 DB 접속 정보를 코드에 하드코딩하지 않는 것이 안전합니다. 예시 `.env`:

```
DB_HOST=localhost
DB_PORT=3307
DB_USER=root
DB_PASSWORD=1234
DB_NAME=schedule_db
```

`get_db_connection` 함수를 아래처럼 수정하여 환경변수를 사용하도록 권장합니다:

```python
import os
import pymysql

def get_db_connection():
    return pymysql.connect(
        host=os.getenv('DB_HOST', 'localhost'),
        port=int(os.getenv('DB_PORT', 3306)),
        user=os.getenv('DB_USER', 'root'),
        password=os.getenv('DB_PASSWORD', ''),
        database=os.getenv('DB_NAME', 'schedule_db')
    )
```

## 사용법

1. 패키지 설치

```bash
pip install -r requirements.txt
```

2. 데이터베이스 생성 및 테이블 준비 (위 SQL 참고)

3. 스크립트 실행

```bash
python schedule.py
```

4. 콘솔에서 메뉴를 선택해 일정 추가/조회/완료 기능 사용

### 예시 상호작용

```
선택: 1
제목: 회의
내용: 프로젝트 킥오프
시작 시간(yyyymmddhhmmss): 20250830140000
종료 시간(yyyymmddhhmmss): 20250830143000
일정 추가 완료.
```

작성일: 2025-08-29
