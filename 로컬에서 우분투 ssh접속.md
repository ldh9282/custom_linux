## 우분투(Ubuntu)에서 서버설정하고 로컬 윈도우에서 SSH로 접속해서 사용하려면 다음 단계를 따르면 됩니다.

### 1. SSH 서버 설치

우분투에서 SSH 서버가 기본적으로 설치되어 있지 않을 수 있으므로, OpenSSH 서버를 설치해야 합니다.

```bash
sudo apt update
sudo apt install openssh-server
```

설치가 완료되면 SSH 서버가 자동으로 시작됩니다.

### 2. SSH 서버 상태 확인

```bash
sudo systemctl status ssh
```

"active (running)" 상태인지 확인하세요. 실행 중이 아니면 시작합니다.

```bash
sudo systemctl start ssh
```

### 3. 우분투 IP 확인 (로컬 네트워크에서 SSH 연결을 위해 우분투의 IP 주소를 알아야 합니다.)

데스크탑 네임

```bash
hostname
```

IP

```bash
hostname -I
```

### 4. 로컬 윈도우에서 SSH 연결 (PowerShell 또는 CMD 열기)

```bash
ssh <사용자명>@<데스크탑네임 또는 IP>
```

데스크탑네임

```bash
ssh user@123.456.789.999
```

IP

```bash
ssh user@user-virtual-machine
```

(1) 처음 연결 시, 호스트 키를 확인하라는 메시지가 나타나면 **"yes"**를 입력.

(2) 암호 입력 후 접속 완료.

### 5. GitHub에 가지고 있는 스프링부트 프로젝트 다운받아서 실행

#### (1) Java 설치

```bash
sudo apt install openjdk-17-jdk
```

현재 설치된 Java 버전을 확인하려면:

```bash
java -version
```

기본 Java 버전 설정

```
sudo update-alternatives --config java
```

위 목록에서 Java 17을 선택하려면, 해당하는 번호(예: 2)를 입력하고 Enter를 누릅니다.

#### (2) Git 설치

```bash
sudo apt install git
```

설치 확인:

```bash
git --version
```

GitHub에서 프로젝트를 클론합니다.

```bash
git clone https://github.com/ldh9282/custom_meta.git
```

클론이 완료되면 해당 디렉토리로 이동합니다.

```bash
cd custom_meta
```

.env 파일 생성 (${DATABASE_URL} 등 환경변수설정)

```bash
nano .env
```

#### (3) DB원격접속

PostgreSQL 클라이언트 설치 (우분투 -> 원격DB 로컬인경우)

```bash
sudo apt install postgresql-client
```

로컬 윈도우에서 방화벽 5432 인바운드 open

인바운드 규칙이 작동하지 않으면 게스트 또는 공용네트워크에서 개인 네트워크로 변경

로컬 윈도우에서 PostgreSQL 원격 연결 허용 설정 (모든 ip)

postgresql/data/pg_hba.conf

```
host all all 0.0.0.0/0 scram-sha-256
```

로컬 윈도우에서 PostgreSQL 원격 연결 허용 설정 (특정 ip)

```
host all all 123.456.789.999/32 scram-sha-256
```

ssh 접속후 원격에서 192.123.123.123 ip의 custom_database DB의 custom 유저로 접속여부확인

```bash
psql -h 192.123.123.123 -U custom -d custom_database
```

비밀번호 입력후 접속확인

확인후 나가기

```bash
\q
```

#### (4) 프로젝트 빌드 도구 확인 (Maven 프로젝트인 경우)

메이븐 설치:

```bash
sudo apt install maven
```

설치 확인:

```bash
mvn -version
```

애플리케이션 실행

Maven으로 프로젝트를 빌드합니다:

```bash
mvn clean install
```

빌드가 완료되면 target 디렉토리에 JAR

```bash
java -jar target/met-0.0.1-SNAPSHOT.jar
```

혹은 mvn 이용 (참고로 리눅스에선 mvn, 윈도우에선 mvnw)

```bash
mvn spring-boot:run
```

액티브 프로필을 jvm 환경변수로 넣어줄 경우 (default, dev, prod)

```bash
mvn spring-boot:run -Dspring-boot.run.jvmArguments="-Dspring.profiles.active=default"
```

#### 5. 원격 강제 pull (로컬 강제 푸시했을때)

```bash
cd custom_meta
git fetch origin
git reset --hard origin/master
```

### 6. 리눅스 서버 실행

```bash
cd custom_meta
mvn spring-boot:run -Dspring-boot.run.jvmArguments="-Dspring.profiles.active=default"
mvn spring-boot:run -Dspring-boot.run.jvmArguments="-Dspring.profiles.active=dev"

```
