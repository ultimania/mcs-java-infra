# recuruit_config

recuruitのconfig用リポジトリ

## 各種アクセス

|サービス|URL|
|---|---|
|tomcat|http://dockeronec2-public-alb-816863126.us-east-2.elb.amazonaws.com|
|apache|http://dockeronec2-public-alb-816863126.us-east-2.elb.amazonaws.com:10080|


## プロビジョニング（ＡＷＳ基盤）
`aws/cloud_formation_template.yml`をデプロイ

## セットアップ手順（EC2プロビジョニング後）
以降はEC2にログイン後に実施

### DockerCompose導入
```
$ sudo curl -L https://github.com/docker/compose/releases/download/1.27.2/docker-compose-`uname -s`-`uname -m` -o /usr/bin/docker-compose
$ sudo chmod +x /usr/bin/docker-compose
$ sudo docker-compose --version
```

### コンテナ環境デプロイ
`docker-compose.yml`をカレントディレクトリに用意

```
$ sudo docker-compose up -d
```

### postgreSQLログイン（DBメンテナンス用）
```
$ sudo docker exec -it postgres hostname -i
$ sudo docker exec -it postgres psql -U postgres
```

# tomcat設定（メンテナンス用、コンテナ再作成時）
`username`と`password`は任意に設定
```
$ sudo docker exec -it tomcat sh
# cp -rp ./webapps.dist/* ./webapps/

# cat <<EOF > conf/tomcat-users.xml
<tomcat-users>
  <role rolename="manager-gui"/>
  <role rolename="admin-gui"/>
  <user username="********" password="************" roles="manager-gui,admin-gui"/>
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

### コンテナ環境削除
```
# sudo docker-compose down
```
