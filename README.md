# Azure MVC CI/CD Demo

ASP.NET Core MVC (.NET 8) 空白專案，搭配 GitHub Actions 自動部署到 Azure App Service。

Push 到 `main` branch 後，CI/CD 流程自動觸發，無需手動操作。

## 技術棧

- **Framework**: ASP.NET Core MVC (.NET 8)
- **CI/CD**: GitHub Actions
- **部署目標**: Azure App Service

---

## 部署前置設定

首次部署前，請完成以下設定步驟：

### 1. 建立 Azure App Service

1. 前往 [Azure Portal](https://portal.azure.com)
2. 搜尋列輸入「**App Service**」→ 點選「**建立**」→「**Web 應用程式**」
3. 填寫設定：
   - **訂用帳戶**：選擇你的訂用帳戶
   - **資源群組**：新建或選擇現有群組
   - **名稱**：輸入全球唯一的名稱（例如 `my-webapp-123`），這會成為網址 `https://my-webapp-123.azurewebsites.net`
   - **執行階段堆疊**：選擇「**.NET 8 (LTS)**」
   - **作業系統**：Linux 或 Windows 皆可
   - **區域**：選擇離你最近的區域（例如 East Asia）
4. 點選「**檢閱 + 建立**」→「**建立**」

### 2. 下載 Publish Profile

1. 前往 [Azure Portal](https://portal.azure.com)
2. 找到你的 App Service
3. 點選左側選單「**概觀**」→ 上方工具列「**取得發佈設定檔**」
4. 下載 `.PublishSettings` 檔案，記下其內容（整個 XML）

### 3. 設定 GitHub Secrets

前往你的 GitHub Repository → **Settings** → **Secrets and variables** → **Actions** → **New repository secret**，新增以下兩個 secret：

| Secret 名稱 | 值 |
|---|---|
| `AZURE_WEBAPP_NAME` | 你的 App Service 名稱（例如 `my-webapp-123`） |
| `AZURE_WEBAPP_PUBLISH_PROFILE` | 步驟 2 下載的 `.PublishSettings` 檔案完整內容（貼上整個 XML） |

---

## CI/CD 觸發流程

```
Codex / 開發者 完成任務
       ↓
   git push → main
       ↓
GitHub Actions 自動觸發 deploy.yml
       ↓
  dotnet restore → build → publish
       ↓
  azure/webapps-deploy 部署到 Azure
       ↓
  https://<your-app-name>.azurewebsites.net ✓
```

Workflow 檔案位於 `.github/workflows/deploy.yml`。

---

## 本機執行

```bash
dotnet restore
dotnet run --project src/WebApp
```

開啟瀏覽器訪問 `https://localhost:5001`。
