## 環境変数の設定
![alt text](image.png)

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
  }
]
```
![alt text](image-1.png)


## 診断設定

## App ServiceのマネージドIDにACRへのアクセス権を付与
AppServiceのマネージドIDをオンにする
![alt text](image-7.png)

ACRへの権限付与
![alt text](image-8.png)

![alt text](image-9.png)

![alt text](image-10.png)



```bash
az webapp config container set --name udemytodoapp-app --resource-group rg-dev-todo --container-image-name udemytodoappacr01.azurecr.io/udemy-todoapp:dev
```

Portalで継続的デプロイをONに

AzurePortalで確認
![alt text](image-13.png)

デプロイログの確認
![alt text](image-12.png)

```bash
az webapp config appsettings set --resource-group rg-dev-todo --name udemytodoapp-app --settings TEST="TEST"
```

