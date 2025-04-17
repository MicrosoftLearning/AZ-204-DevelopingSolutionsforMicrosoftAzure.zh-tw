---
lab:
  title: 交換 Azure App 服務 中的部署位置
  description: 瞭解如何在 Azure App 服務 中交換部署位置。 在此練習中，您會：將簡單的應用程式部署至 App Service;對應用程式進行小變更，並將該變更部署到預備位置;最後交換位置，讓更新的應用程式在生產環境中。
---

# 交換 Azure App 服務 中的部署位置

在本練習中，您將藉由使用 Azure CLI **az webapp up**，來將基本的 HTML+CSS 網站部署至 Azure App Service。 接下來，您會更新程序代碼，並將變更部署至預備位置。 最後，您會交換位置。

在這裡練習中執行的工作：

* 下載範例應用程式並將其部署至 Azure App 服務。
* 建立預備部署位置。
* 對範例應用程式進行變更，並將其部署至預備位置。
* 交換預備和默認生產位置，以將變更移至生產位置。

此練習大約需要 **20** 分鐘才能完成。

## 在您開始使用 Intune 之前

若要完成練習，您需要：

* Azure 訂用帳戶。 如果您還沒有帳戶，您可以註冊一個 [https://azure.microsoft.com/](https://azure.microsoft.com/)。

## 下載並部署範例應用程式

在本節中，您會下載範例應用程式並設定變數，讓命令更容易輸入，然後使用 Azure CLI 命令建立 Azure App 服務 資源並部署靜態 HTML 網站。

1. 在您的瀏覽器中，流覽至 Azure 入口網站[https://portal.azure.com](https://portal.azure.com);如果出現提示，請使用您的 Azure 認證登入。

1. **使用頁面頂端搜尋列右邊的 [\>_]** 按鈕，在 Azure 入口網站 中建立新的 Cloud Shell，然後選取 ***Bash*** 環境。 Cloud Shell 會在 Azure 入口網站底部的窗格顯示命令列介面。

    > **注意**：如果您先前已建立使用 *PowerShell 環境的 Cloud Shell* ，請將它切換至 ***Bash***。

1. 在 Cloud Shell 工具列中，在**設定**功能表中，選擇**轉到經典版本**（這是使用程式碼編輯器所必需的）。

1. 執行下列 **git** 命令以複製範例應用程式存放庫。

    ```bash
    git clone https://github.com/Azure-Samples/html-docs-hello-world.git
    ```

1. 執行下列命令，以設定變數來保留資源群組和應用程式名稱。 請記下命令執行之後所顯示的 appName** 值**，稍後在本練習中將會用到它。

    ```bash
    resourceGroup=rg-mywebapp

    appName=mywebapp$RANDOM
    echo $appName
    ```

1. 流覽至包含範例程式代碼的目錄，然後執行 **az webapp up** 命令。 **注意：** 此命令可能需要幾分鐘的時間才能執行。

    ```bash
    cd html-docs-hello-world

    az webapp up -g $resourceGroup -n $appName --sku P0V3 --html
    ```

    現在您的部署已完成檢視 Web 應用程式的時間。

1. 在 Azure 入口網站 瀏覽至您部署的 Web 應用程式。 您可以在搜尋資源、服務和檔 （G +/）** 搜尋列中輸入您稍早**記下的名稱，然後從清單中選取資源。

1. 選取位於 [基本資訊 **] 區段中 [**預設網域**] 字段**的 Web 應用程式連結。 連結會在新的索引標籤中開啟網站。

## 將更新的程式代碼部署到部署位置

在本節中，您會建立部署位置、修改應用程式中的 HTML，並將更新的程式代碼部署到新的部署位置。

### 建立部署位置 

1. 返回具有 Azure 入口網站和 Cloud Shell 的索引標籤。

1. 在 Cloud Shell 中輸入下列命令，以建立名為 *預備*的部署位置。

    ```bash
    az webapp deployment slot create -n $appName -g $resourceGroup --slot staging
    ```

1. 等候命令完成，然後選取 **左側功能表中的 [部署>部署位置** ]，以檢視 Web 應用程式的部署位置。 請注意，新位置的名稱包含*附加至 Web 應用程式名稱的 -staging*

### 更新程式代碼並部署至預備位置

1. 在 Cloud Shell 中，輸入 `code index.html` 以開啟編輯器。 在標題標籤中`<h1>`，將 Azure App 服務 - 範例靜態 HTML 網站*變更*為 *Azure App 服務 預備位置* - 或您想要的任何其他專案。

1. 使用 ctrl-s** 命令**來儲存，並**用 ctrl-q** 結束。

1. 在 Cloud Shell 中，執行下列命令來建立更新專案的 zip 檔案。 下一個步驟需要 zip 或 Web 應用程式資源 （WAR） 檔案。

    ```bash
    zip -r stagingcode.zip .
    ```

1. 在 Cloud Shell 中執行下列命令，將您的更新部署至預備位置。

    ```bash
    az webapp deploy -g $resourceGroup -n $appName --src-path ./stagingcode.zip --slot staging
    ```

1. 選取 **Web 應用程式左側選單中的 [部署>部署位置** ]，然後選取您稍早建立的預備位置。

1. 在 [基本資訊 **] 區段中，選取 [預設網域 **] 字段中**的連結**。 連結會在新的索引標籤中開啟預備位置的網站。

## 交換預備和生產位置

您可以使用工具列中的 [交換 **] 選項，在 Azure 入口網站 **中執行交換。 **如果您選取**左側功能表中的 [概觀**] 或 **[部署>部署位置**]，[交換**] 選項就會出現在工具列中。

1. 在 Azure 入口網站 中，選取**工具列中的 [交換**] 以開啟 [**交換**] 面板。

1. 檢閱交換面板中的設定。 [ **來源** ] 應該會顯示 **-staging** 位置，而 **[目標** ] 應該會顯示預設的生產位置。

    ![[交換] 面板的螢幕快照。](./media/02/app-service-swap-panel.png)

1. 選取 [ **開始交換** ]，然後等候作業完成。 您可以在 [通知 **] 面板中追蹤完成**，方法是選取入口網站頂端的鈴鐺圖示。

1. 若要確認交換流覽至您部署的 Web 應用程式。 在搜尋資源、服務和檔 （G +/） 搜尋列中輸入您稍早建立的 Web 應用程式名稱（例如 *mywebapp12360***），** 然後從清單中選取資源。

1. 選取位於 [基本資訊 **] 區段中 [**預設網域**] 字段**的 Web 應用程式連結。 連結會在新的索引標籤中開啟網站（生產位置）。

1. 確認您的變更，您可能需要重新整理頁面，才能顯示這些變更。

## 清除資源

既然您已完成練習，您應該刪除您所建立的雲端資源，以避免不必要的資源使用量。

1. 流覽至您建立的資源群組，並檢視此練習中使用的資源內容。
1. 在工具列上，選取 [刪除資源群組]****。
1. 輸入資源群組名稱並確認您想要將其刪除。
