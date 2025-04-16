---
lab:
  title: 使用 Visual Studio Code 建立 Azure 函式
  description: 瞭解如何使用 HTTP 觸發程式建立 Azure 函式。 在 Visual Studio Code 本機建立及測試程式代碼之後，您會將函式部署至 Azure。
---

# 使用 Visual Studio Code 建立 Azure 函式

在此練習中，您將瞭解如何建立回應 HTTP 要求的 C\# 函式。 在 Visual Studio Code 本機建立及測試程式代碼之後，您會在 Azure 中部署及測試函式。

在這裡練習中執行的工作：

* 建立容器化應用程式的 Azure App 服務 資源
* View the results
* 清除資源

此練習應該需要大約 **30** 分鐘才能完成。

## 在您開始使用 Intune 之前

開始之前，請先確定您具備下列必要項目：

* Azure 訂用帳戶。 如果您沒有免費試用版，則可以[免費註冊](https://azure.microsoft.com/)。

* [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools#installing) 4.x 版。

* 其中一個[支援平台](https://code.visualstudio.com/docs/supporting/requirements#_platforms)上的 [Visual Studio Code](https://code.visualstudio.com/)。

* [.NET 8](https://dotnet.microsoft.com/en-us/download/dotnet/8.0) 是目標架構。

* [適用於 Visual Studio Code 的 C# 開發工具包](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit) 。

* 適用於 Visual Studio Code 的 [Azure Functions 擴充功能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)。

## 建立本機專案

在本區段中，您會使用 Visual Studio Code，以 C# 建立本機 Azure Functions 專案。 稍後在本次練習中會將函式程式碼發佈至 Azure。

1. 在 Visual Studio Code 中，按 F1 開啟命令選擇區、搜尋和執行命令 `Azure Functions: Create New Project...`。

1. 選取專案工作區的目錄位置，並選擇 **[選取]**。 您應建立新資料夾，或為專案工作區選擇空白資料夾。 請勿選擇已屬於工作區一部分的專案資料夾。

1. 提示中會提供下列資訊：

    | 提示 | 動作 |
    |--|--|
    | 選取將包含函式項目的資料夾 | 選取 [ **瀏覽...** ]，為您的應用程式選取資料夾。
    | 選取語言 | 選取 [C#]****。 |
    | 選取 .NET 執行階段 | 選取 **[.NET 8.0 隔離]** |
    | 為專案的第一個函式選取範本 | 選取 **[HTTP 觸發程式**]。<sup>1</sup> |
    | 提供函式名稱 | 輸入 `HttpExample`。 |
    | 提供命名空間 | 輸入 `My.Function`。 |
    | 授權等級 | 選取 [ **匿名**]，可讓任何人呼叫函式端點。 |

    <sup>1</sup> 根據您的 VS Code 設定，您可能需要使用 **[變更範本篩選** ] 選項來查看範本的完整清單。

1. Visual Studio Code 會使用提供的資訊，產生具有 HTTP 觸發程序的 Azure Functions 專案。 您可以在 Explorer 中檢視本機專案檔。

### 在本機執行函式

將 Visual Studio Code 與 Azure Functions Core Tools 整合可讓您在發佈至 Azure 之前，先在本機開發電腦上執行此專案。

1. 請確定終端機已在 Visual Studio Code 中開啟。 您可以依序選取 [終端機]**** 與功能表列中的 [新增終端機]**** 來開啟終端機。 

1. 按 **F5** 以啟動偵錯工具中的函數應用程式專案。 Core Tools 的輸出會顯示在 [終端機]**** 面板中。 您的應用程式會在 [終端]**** 面板中啟動。 您可以查看在本機所執行 HTTP 觸發函式的 URL 端點。

    ![HTTP 觸發函式端點的螢幕快照會顯示在終端機面板中。](./media/06/run-function-local.png)

1. 執行 Core Tools 後，移至 [Azure: Functions]**** 區域。 在 [函式]**** 下方，展開 [本機專案]**** > ****[函式]。 以滑鼠右鍵按兩下 **HttpExample** 函式，然後選取 [ **立即執行函式...**]。

    ![顯示 [立即執行函式] 位置的螢幕快照...步。](./media/06/execute-function-local.png)

1. 在 [ **輸入要求本文** ] 中，輸入的要求訊息本文值 `{ "name": "Azure" }`。 請按 **Enter** 鍵，將此要求訊息傳送至您的函式。 當函式在本機執行並傳回回應時，會在 Visual Studio Code 中引發通知。

    選取通知鈴鐺圖示以檢視通知。 函式的執行資訊會顯示於 [終端機]**** 面板中。

1. 按下 **Shift + F5** 即可停止 Core Tools，並中斷偵錯工具的連線。

確認函式可在本機電腦上正常執行之後，即可使用 Visual Studio Code 將專案直接發佈至 Azure。

## 在 Azure 中部署和執行函式

在本節中，您會建立 Azure 函式應用程式資源，並將函式部署至資源。

### 登入 Azure

在發佈應用程式之前，您必須先登入 Azure。 若您已登入，請移至下一區段。

1. 如果您尚未登入，請選擇活動列中的 Azure 圖示，然後在 [Azure: Functions]**** 區域中選擇 [登入 Azure...]****。

    ![[登入 Azure] 按鈕的螢幕快照。](./media/06/functions-sign-into-azure.png)

1. 當在瀏覽器中收到提示時，請使用您的 Azure 帳戶認證選擇 Azure 帳戶並登入。

1. 成功登入之後，即可關閉新的瀏覽器視窗。 屬於您 Azure 帳戶的訂閱會顯示於提要欄位中。

### 在 Azure 中建立資源

在本區段中，您會建立部署本機函數應用程式所需的 Azure 資源。

1. 選擇 [活動] 列中的 Azure 圖示，然後在 [資源]**** 區域中選取 [建立資源...]****。

    ![[建立資源] 按鈕的螢幕快照。](./media/06/create-resource.png)    

1. 提示中會提供下列資訊：

    | 提示 | 動作 |
    |--|--|
    | 選取要建立的資源 | 選取 [Create Function App in Azure...] (在 Azure 中建立函數應用程式...)**** |
    | 選取訂用帳戶 | 選取要使用的訂用帳戶。 「若您只有一個訂閱，就不會看見此選項。」** |
    | 輸入函數應用程式的全域唯一名稱 | 輸入 URL 路徑中有效的名稱，例如 `myfunctionapp`。 您輸入的名稱會經過驗證，以確保其是唯一的。 |
    | 選取新資源的位置 | 若要獲得較佳的效能，請選取您附近的 [區域]。 |
    | 選取執行階段堆疊 | 選取 **[.NET 8.0 隔離]。** |
    | 選取實例記憶體大小 | 選取 **2048 預設值** |
    | 選取實例計數上限 | 接受預設 **100**。 |

    擴充功能會在終端機視窗的 AZURE** 區域中建立**時，顯示個別資源的狀態。
    
1. 完成時，下列 Azure 資源會使用根據您函數應用程式名稱的名稱，建立於您的訂閱中：

    * 資源群組，這是相關資源的邏輯容器。
    * 標準 Azure 儲存體帳戶，其可維護專案的狀態和其他資訊。
    * Flex 耗用量方案，定義無伺服器函式應用程式的基礎主機。
    * 函式應用程式可提供用來執行函式程式碼的環境。 函數應用程式可讓您將函式以邏輯單位分組，方便您在相同的主控方案中管理、部署及共用資源。
    * 連線至函式應用程式的 Application Insights 執行個體，可追蹤無伺服器函式的使用量。

### 將專案部署至 Azure

> **!重要事項：** 發佈至現有的函式會覆寫任何先前的部署。

1. 在命令選擇區中，搜尋並執行 Azure **Functions：部署至函式應用程式...** 命令。

1. 選取您在建立資源時所使用的訂用帳戶。

1. 選取您建立的函式應用程式。 當系統提示您覆寫先前的部署時，請選取 [部署]****，將函式程式碼部署至新的函數應用程式資源。

1. 部署完成之後，選取 [ **檢視輸出** ] 以檢視部署結果的詳細數據。 如果您錯過通知，請選取右下角的通知鈴鐺圖示，再次看到通知。

    ![[檢視輸出] 按鈕的螢幕快照。](./media/06/function-view-output.png)

### 在 Azure 中執行函數

1. 返回提要欄位中的 [資源]**** 區域，依序展開您的訂閱和新函數應用程式，以及 [函式]****。 **以滑鼠右鍵按兩下** **HttpExample** 函式，然後選擇 [ **立即執行函式...**]。

    ![[立即執行函式] 選項的螢幕快照。](./media/06/execute-function-remote.png)

1. 在 [輸入要求本文]**** 中，您會看到 `{ "name": "Azure" }` 的要求訊息本文值。 請按 Enter 鍵，將此要求訊息傳送至您的函式。

1. 當函式在 Azure 執行並傳回回應時，會在 Visual Studio Code 中引發通知。 選取通知鈴鐺圖示以檢視通知。

## 清除資源

既然您已完成練習，您應該刪除您所建立的雲端資源，以避免不必要的資源使用量。

1. 在您的瀏覽器中，流覽至 Azure 入口網站[https://portal.azure.com](https://portal.azure.com);如果出現提示，請使用您的 Azure 認證登入。
1. 流覽至您建立的資源群組，並檢視此練習中使用的資源內容。
1. 在工具列上，選取 [刪除資源群組]****。
1. 輸入資源群組名稱並確認您想要將其刪除。
