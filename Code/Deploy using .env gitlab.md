<span style="font-size:12px; color:#888888;">Created: 07.01.2025 17:31</span>

https://forum.gitlab.com/t/gitlab-pipeline-environment-variables-how-to-transmission/76995/2

```shell
deploy: stage: deploy before_script: - chmod 400 $SSH_KEY script: - ssh to deploy server " docker login ... && do something to pull source code including docker-compose.yml , .env && echo "DB_PASSWORD=$DB_PASSWORD" >> .env && docker-compose down && docker-compose up -d && echo "" > .env"
```

Генерируем .env и заполняем переменными из gitlab
После поднятия очищаем файл, для безопасности не 


Другой подход 
https://stackoverflow.com/questions/62137000/how-to-add-variables-from-gitlab-settings-to-docker-compose-file-for-my-containe

You can send compose time variable into docker compose by using the '--build-arg' flag. You can access it into the Dockerfile as '$'. You can store the variables in the gitlab ci/cd variable section and access them via ${variable_name} and construct the compose command in the .gitlab-ci.yml.

```yaml
docker-compose build --build-arg db_username="${db_username}" --build-arg db_password="${db_password}" 
```

is a sample docker compose command to be added to .gitlab-ci.yml while the variables are from the gitlab ci/cd variables.