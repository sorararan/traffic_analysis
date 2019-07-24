## 環境構築
`docker-compose build`
`docker-compose up -d`
以上で、attacker(kali-linux)とpot(centos)コンテナが立ち上がります。
ちょっと自動化できなかった部分があるので、各コンテナで以下のファイルのコマンドを実行してください。

### attacker
`commands/db_init`

### pot
`commands/vsftpd_start`

`docker-compose exec コンテナ名 bash`
を実行すると各コンテナのbashに入れます

## 狙いの脆弱性
`exploit/unix/ftp/vsftpd_234_backdoor`
(http://www.intellilink.co.jp/article/vulner/110706.html)

