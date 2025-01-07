<span style="font-size:12px; color:#888888;">Created: 07.01.2025 17:31</span>

https://forum.gitlab.com/t/gitlab-pipeline-environment-variables-how-to-transmission/76995/2

```shell
deploy: stage: deploy before_script: - chmod 400 $SSH_KEY script: - ssh to deploy server " docker login ... && do something to pull source code including docker-compose.yml , .env && echo "DB_PASSWORD=$DB_PASSWORD" >> .env && docker-compose down && docker-compose up -d && echo "" > .env"
```

Генерируем .env и заполняем переменными из gitlab
После поднятия очищаем файл, для безопасности не 