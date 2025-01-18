Certbot 설치 (Let's Encrypt 사용)

```bash
sudo apt install certbot python3-certbot-nginx
```

Nginx 사용 시

```bash
sudo certbot --nginx
```

SSL 인증서 자동 갱신 설정 테스트

```bash
sudo certbot renew --dry-run
```

Crontab 편집기 열기

```bash
sudo crontab -e
```

Let's Encrypt 인증서는 90일 동안 유효하므로 매월 자동 갱신을 실행명령어 저장

(0 3 1 \* \* : 매달 1일 오전 3시를 의미합니다)

```bash
0 3 1 * * certbot renew --quiet
```
