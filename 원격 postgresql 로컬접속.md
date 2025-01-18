### 원격 postgresql 로컬접속

우분투에서 PostgreSQL 설치

```bash
sudo apt install postgresql postgresql-contrib
```

PostgreSQL 서비스가 실행 중인지 확인

```bash
sudo systemctl status postgresql
```

실행 중이 아니면 시작합니다.

```bash
sudo systemctl start postgresql
```

postgresql.conf 파일 편집:

```bash
sudo nano /etc/postgresql/16/main/postgresql.conf
listen_addresses = '*'
```

pg_hba.conf 파일 편집:

```bash
sudo nano /etc/postgresql/16/main/pg_hba.conf
host    all             all             xxx.123.xxx/32            scram-sha-256
```

그리고 오라클 클라우드에서 인바운드 특정 ip xxx.123.xxx/32 5432 포트 열어줘야한다.

반영후 재기동

```bash
sudo systemctl restart postgresql
```

유저명 생성

```bash
sudo -i -u postgres
psql

CREATE DATABASE custom_database;
CREATE USER custom WITH PASSWORD 'password';
GRANT ALL PRIVILEGES ON DATABASE database_name TO custom;
```

접속확인

```bash
원격에서 직접
psql -h localhost -U custom -d custom_database

로컬에서 원격
psql -h xxx.179.xxx.117 -U custom -d custom_database

혹은 pgadmin4 이용
```
