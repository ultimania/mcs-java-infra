# gitlab-runner

## 起動

``` bash
cd /opt/git
git clone http://192.168.11.22/root/recuruit_config.git && cd ./recuruit_config/gitlab-runner
docker-compose up -d
```

# runner登録

``` bash
docker-compose exec runner gitlab-runner register -n \
   --url "https://gitlab.com/" \
   --registration-token "確認したトークン" \
   --executor "docker" \
   --description "Docker Runner" \
   --docker-image "docker:stable" \
   --tag-list docker \
   --docker-volumes "/var/run/docker.sock:/var/run/docker.sock" \
   --docker-volumes "/builds:/builds"
```
