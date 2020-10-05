# recuruit_config

recuruitのconfig用リポジトリ


# アプリアクセス（ルートＵＲＬ）
http://dockeronec2-public-alb-816863126.us-east-2.elb.amazonaws.com

# プロビジョニング（ＡＷＳ基盤）
https://cf-templates-1l7kgoixmy3jv-us-east-2.s3.us-east-2.amazonaws.com/2020272cWa-template118v36ullhkj

# コンテナ構築

```
# docker run -it --rm -p 8080:8080 --name tomcat tomcat &
# docker run -d -v /opt/jenkins_home:/var/jenkins_home -p 8081:8080 -p 50000:50000 --name jenkins jenkins/jenkins:lts &
# docker run -d --name postgres -e POSTGRES_PASSWORD=qwer1234 -e PGDATA=/var/lib/postgresql/data/pgdata -v /opt/postgresql:/var/lib/postgresql postgres
# docker run --name mariadb -e MYSQL_ROOT_PASSWORD=qwer1234 mariadb &
```

# postgreSQLログイン
```
# docker exec -it postgres hostname -i
# docker exec -it postgres psql -U postgres
```