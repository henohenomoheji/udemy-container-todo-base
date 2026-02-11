# 権限関連

App Service→Container Registryへアクセスし、コンテナイメージ取得

1. Azure App Service が
2. Azure Container Registry（ACR） にアクセスして
3. 指定されたコンテナイメージを pull（取得）
4. そのイメージを使って アプリを起動


```
[App Service]
   |
   | ① 私は誰？（Managed Identity）
   |
[Azure AD]
   |
   | ② このIDはACRにアクセスしていい？
   |
[ACR]
   |
   | ③ OKなら image pull
   |
[App Service]
   |
   | ④ コンテナ起動

```

👉 App Service の Managed Identity に
👉 ACR に対して AcrPull ロールを付与する


@Microsoft.KeyVault(SecretUri=https://udemy-todo-kv.vault.azure.net/secrets/POSTGRES-PASSWORD/)

@Microsoft.KeyVault(SecretUri=https://udemy-todo-kv.vault.azure.net/secrets/POSTGRES-PASSWORD/)


```
MacBook-Pro:udemy-container-todo-base kanemotosatoko$ code notify-service/
bash: code: command not found
```

解決方法（macOS）

VS Code を起動

Cmd + Shift + P

Shell Command: Install 'code' command in PATH

ターミナル再起動


以下に **「docker を使わず、`az acr build` で ACR 側にビルド＆push する手順書」**をまとめます。
（注意事項どおり **`az acr login` は使いません**）

---

# 手順書：docker不要で `az acr build` で ACR にビルド＆Push

## 前提

* 開発環境：VS Code devcontainer（docker コマンド無しでもOK）
* `az` が利用できる
* ACR が作成済み（例：`udemytodoappacr01`）
* リポジトリに `Dockerfile` がある
* Azure にログインできる（`az login`）

---

## 1. Azure へログイン（未実施の場合のみ）

```bash
az login
az account show
```

---

## 2. 変数を設定

```bash
ACR_NAME=udemytodoappacr01
IMAGE_NAME=udemy-todoapp
TAG=dev
```

---

## 3. ACR のログインサーバー（FQDN）を取得

```bash
ACR_LOGIN_SERVER=$(az acr show -n $ACR_NAME --query loginServer -o tsv)
echo $ACR_LOGIN_SERVER
```

期待例：`udemytodoappacr01.azurecr.io`

---

## 4. Dockerfile の場所を確認（必要なら）

```bash
find . -maxdepth 3 -name Dockerfile -print
```

* 直下なら：`./Dockerfile`
* サブフォルダなら：`./path/to/Dockerfile`

---

## 5. ACR 側でビルド＆Push（docker不要）

### Dockerfile がリポジトリ直下の場合

```bash
az acr build \
  -r $ACR_NAME \
  -t ${ACR_LOGIN_SERVER}/${IMAGE_NAME}:${TAG} \
  -f Dockerfile \
  .
```

### Dockerfile がサブフォルダの場合（例：`infra/Dockerfile`）

```bash
az acr build \
  -r $ACR_NAME \
  -t ${ACR_LOGIN_SERVER}/${IMAGE_NAME}:${TAG} \
  -f infra/Dockerfile \
  .
```

> ✅ この手順では **ローカルの docker / docker login は一切不要**
> ✅ ACR 側がビルドして、そのまま ACR に push します

---

## 6. Push されたか確認

```bash
az acr repository list -n $ACR_NAME -o table
az acr repository show-tags -n $ACR_NAME --repository $IMAGE_NAME -o table
```

---

# 注意事項（重要）

* **`az acr login` は実行しない**

  * devcontainer に docker がないと `DOCKER_COMMAND_ERROR` になります
* `az acr build` は ACR にソース一式（ビルドコンテキスト）を送るため、以下に注意

  * ビルド対象が重い場合は `.dockerignore` で不要ファイルを除外する（例：`.git`, `.venv`, `node_modules`, `data` など）

---

# トラブルシュート（よくある）

## 権限不足（AcrPush等）

`az acr build` 実行時に権限エラーが出たら、ACR に対して以下のいずれかが必要です：

* 最小：`AcrPush`
* もしくは `Contributor`

---

このまま Markdown で社内手順書に貼れる形にも整形できます。必要なら「コマンドを変数なしで固定値にした版」も作ります。
