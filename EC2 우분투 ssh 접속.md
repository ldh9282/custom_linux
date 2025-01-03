### SSH 접속

```bash
ssh -i "경로/키페어.pem" ubuntu@public_ip
```

### 퍼미션 deny 관련 (powershell 에서)

```bash
icacls "경로/키페어.pem" /inheritance:r
icacls "경로/키페어.pem" /grant:r "$($env:USERNAME):(R)"
```

### 프로세스 백그라운드 실행 nohup (ssh 세션끊어져도 백그라운드 실행)

```bash
nohup mvn spring-boot:run -Dspring-boot.run.jvmArguments="-Dspring.profiles.active=prod" &
```

### 기타 프로세스 명령어

#### 실행중인 서버 프로세스의 pid 확인

```bash
ps -aux
```

#### pid 로 프로세스 강제 킬

```bash
kill -9 [pid]
```

### out of memory 서버오류로 인한 인스턴스 강제종료를 AWS cloud 터미널로 해야할때

```bash
aws ec2 stop-instances --instance-ids i-xx인스턴스idxxxxx --force
```
