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

# tomcat設定
```
# docker exec -it tomcat sh
# mv webapps webapps.org
# mv webapps.dist webapps

# cat <<EOF > conf/tomcat-users.xml
<tomcat-users>
  <role rolename="manager-gui"/>
  <role rolename="admin-gui"/>
  <user username="admin" password="asdfqwer1234" roles="manager-gui,admin-gui"/>
</tomcat-users>
EOF


# cat <<EOF > webapps/manager/META-INF/context.xml
<!--
  Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->
<Context antiResourceLocking="false" privileged="true" >
<!--
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1|0.0.0.0" />
-->
  <Manager sessionAttributeValueClassNameFilter="java\.lang\.(?:Boolean|Integer|Long|Number|String)|org\.apache\.catalina\.filters\.CsrfPreventionFilter$LruCache(?:$1)?|java\.util\.(?:Linked)?HashMap"/>
</Context>
EOF


```