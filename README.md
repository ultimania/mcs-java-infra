# Javaアプリケーション基盤 on AWS & Docker

config用リポジトリ

## リポジトリ構成

|階層|内容|使用ツール|
|---|---|---|
|aws|AWSでのプロビジョニング構成ファイル|CloudFormation|
|gitlab-runner|gitlab-runnerを使用する場合の構成ファイル|Gitlab CI/CD|
|tomcat|Tomcatの構成ファイル（設定変更対象のみ）|TomcatManager|
|README.md|本ファイル。本リポジトリの取り扱い説明書||
|docker-compose.yml|Javaアプリケーション基盤を構成するコンテナ構成ファイル|docker-compose|


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
$ sudo docker exec -it postgres psql -U postgres
```

### postgreSQLログ確認
```
$ sudo docker logs postgres
```

### tomcatコンテナログイン（メンテナンス用）
```
$ sudo docker exec -it tomcat sh
```

### tomcatサービス再起動
```
$ sudo docker restart tomcat
```

### tomcatログ確認
```
$ sudo docker logs tomcat
```

### tomcat設定（コンテナ再作成時）
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
```

### tomcat設定（EC2作成後初回のみ）
```
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

# exit
```

### コンテナ環境削除
```
$ sudo docker-compose down
```
