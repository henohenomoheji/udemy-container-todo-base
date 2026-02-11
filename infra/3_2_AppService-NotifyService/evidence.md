
## 環境変数の設定

```json
[
    {
    "name": "POSTGRES_DB",
    "value": "todos",
    "slotSetting": false
  },
  {
    "name": "POSTGRES_HOST",
    "value": "udemytodoapp-pg.postgres.database.azure.com",
    "slotSetting": false
  },
  {
    "name": "POSTGRES_PASSWORD",
    "value": "Knmt1234!",
    "slotSetting": false
  },
  {
    "name": "POSTGRES_PORT",
    "value": "5432",
    "slotSetting": false
  },
  {
    "name": "POSTGRES_USER",
    "value": "pgadmin",
    "slotSetting": false
  },
  {
    "name": "POSTGRES_SSLMODE",
    "value": "require",
    "slotSetting": false
  },
  {
    "name": "SENDER_EMAIL",
    "value": "kttxoww@gmail.com",
    "slotSetting": false
  },
  {
    "name": "EMAIL_PASSWORD",
    "value": "jejfbjrwvhdawwdd",
    "slotSetting": false
  },
  {
    "name": "RECIPIENT_EMAIL",
    "value": "kttxoww@gmail.com",
    "slotSetting": false
  },
  {
    "name": "SLEEP_SECONDS",
    "value": "60",
    "slotSetting": false
  }
]
```

![alt text](image.png)


## システムマネージドID
有効化
![alt text](image-1.png)

![alt text](image-2.png)

## 診断設定
![alt text](image-4.png)

## コンテナデプロイ設定

```bash
az webapp config container set --name udemytodoapp-notify --resource-group rg-dev-todo --container-image-name udemytodoappacr01.azurecr.io/udemy-notify-service:dev
```

継続的デプロイをONに
![alt text](image-3.png)

![alt text](image-5.png)