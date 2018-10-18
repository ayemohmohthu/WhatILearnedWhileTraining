#### rails server already running when u start rails, kill the process id already running and then re-start.
ps -aef | grep rails
or
lsof -wni tcp:3000
```
~ ❯❯❯ lsof -wni tcp:3000
COMMAND  PID         USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
ruby    4671 ayemohmohthu   10u  IPv4 0x4d493c2ee305c013      0t0  TCP 127.0.0.1:hbci (LISTEN)
ruby    4671 ayemohmohthu   11u  IPv6 0x4d493c2ee0de18cb      0t0  TCP [::1]:hbci (LISTEN)
~ ❯❯❯ kill -9 4671
```
or
①ターミナルを再起動（私はこれで治った。）
or
②server.pidファイルを削除  
　[Railsプロジェクトフォルダ]\tmp\pids\server.pid を削除する。

#### dockerコンテナ上yarn update/bundle install したいとき
```
docker-compose run web yarn install
docker-compose run --rm web bundle install
```
