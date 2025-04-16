---
lab:
  title: 使用 .NET 建立適用於 NoSQL 的 Azure Cosmos DB 資源
  description: 瞭解如何使用 Microsoft .NET SDK v3 在 Azure Cosmos DB 中建立資料庫和容器資源。
---

# 使用 .NET 建立適用於 NoSQL 的 Azure Cosmos DB 資源

在此練習中，您會建立主控台應用程式，以在 Azure Cosmos DB 中建立容器、資料庫和專案。

在這裡練習中執行的工作：

* 建立 Azure Cosmos DB 帳戶
* 建立主控台應用程式
* 執行主控台應用程式並驗證結果

此練習應該需要大約 **30** 分鐘才能完成。

## 在您開始使用 Intune 之前

開始之前，請先確定您具備下列必要項目：

* Azure 訂用帳戶。 如果您沒有免費試用版，則可以[免費註冊](https://azure.microsoft.com/)。

## 建立 Azure Cosmos DB 帳戶

在練習的本節中，您會建立資源群組和 Azure Cosmos DB 帳戶。 您也會記錄端點，以及帳戶的存取金鑰。

1. 在瀏覽器中，流覽至 Azure 入口網站[https://portal.azure.com](https://portal.azure.com);如果出現提示，請使用您的 Azure 認證登入。

1. **使用頁面頂端搜尋列右邊的 [\>_]** 按鈕，在 Azure 入口網站 中建立新的 Cloud Shell，然後選取 ***Bash*** 環境。 Cloud Shell 會在 Azure 入口網站底部的窗格顯示命令列介面。

    > **注意**：如果您先前已建立使用 *PowerShell 環境的 Cloud Shell* ，請將它切換至 ***Bash***。

1. 在 Cloud Shell 工具列中，在**設定**功能表中，選擇**轉到經典版本**（這是使用程式碼編輯器所必需的）。

1. 針對本練習所需的資源建立資源群組。 將取代 **\<myResourceGroup>** 為您想要用於資源群組的名稱。 您可以視需要將 useast** 取代**為您附近的區域。 

    ```
    az group create --location useast --name <myResourceGroup>
    ```

1. 執行下列命令來建立 Azure Cosmos DB 帳戶。 將 **\<myCosmosDBacct>** 取代為識別您 Azure Cosmos DB 帳戶的唯一名稱。 將取代 **\<myResourceGroup>** 為您稍早選擇的名稱。

    >**注意：** 帳戶名稱只能包含小寫字母、數位和連字元 （-） 字元。 其長度必須介於 3 到 31 個字元之間。 *此命令需要幾分鐘的時間來完成。*

    ```
    az cosmosdb create -n <myCosmosDBacct> -g <myResourceGroup>
    ```

1.  執行下列命令來擷取 **Azure Cosmos DB 帳戶的 documentEndpoint** 。 從命令結果記錄端點，稍後在練習中需要此端點。 將和****\<myResourceGroup>取代**\<myCosmosDBacct>** 為您建立的名稱。

    ```bash
    az cosmosdb show -n <myCosmosDBacct> -g <myResourceGroup> --query "documentEndpoint" --output tsv
    ```

1. 使用下列命令擷取帳戶的主鍵。 **從命令結果記錄 primaryMasterKey**，以在程式代碼中使用。 將和****\<myResourceGroup>取代**\<myCosmosDBacct>** 為您建立的名稱。

     ```
    # Retrieve the primary key
    az cosmosdb keys list -n <myCosmosDBacct> -g <myResourceGroup>
    ```

## 建立主控台應用程式

既然所需的資源已部署至 Azure，下一個步驟是使用 Visual Studio Code 中的同一個終端視窗來設定主控台應用程式。

1. 建立專案的資料夾並進行變更。

    ```bash
    mkdir cosmosdb
    cd cosmosdb
    ```

1. 建立 .NET 主控台應用程式。

    ```dotnetcli
    dotnet new console --framework net8.0
    ```

1. 執行下列命令，將 Microsoft.Azure.Cosmos** 套件新增**至專案，以及支援的 **Newtonsoft.Json** 套件。

    ```dotnetcli
    dotnet add package Microsoft.Azure.Cosmos --version 3.*
    dotnet add package Newtonsoft.Json --version 13.*
    ```

現在可以使用 cloud Shell 中的 **編輯器，取代 Program.cs** 檔案中的範本程式代碼。

### 新增專案的起始程序代碼

1. 在 Cloud Shell 中執行下列命令，以開始編輯應用程式。

    ```bash
    code Program.cs
    ```

1. 將任何現有的程式代碼取代為using**語句之後**的下列代碼段。 請務必遵循程式碼註解中的指示取代 **\<documentEndpoint>** 和 **\<primaryKey>** 的預留位置值。

    程式代碼提供應用程式的整體結構，以及一些必要的元素。 檢閱程式代碼中的批註，您可以在指示中指定的區域中新增程序代碼。 

    ```csharp
    using Microsoft.Azure.Cosmos;
    
    namespace CosmosExercise
    {
        // This class represents a product in the Cosmos DB container
        public class Product
        {
            public string? id { get; set; }
            public string? name { get; set; }
            public string? description { get; set; }
        }
    
        public class Program
        {
            // Cosmos DB account URL - replace with your actual Cosmos DB account URL
            static string cosmosDbAccountUrl = "<documentEndpoint>";
    
            // Cosmos DB account key - replace with your actual Cosmos DB account key
            static string accountKey = "<primaryKey>";
    
            // Name of the database to create or use
            static string databaseName = "myDatabase";
    
            // Name of the container to create or use
            static string containerName = "myContainer";
    
            public static async Task Main(string[] args)
            {
                // Create the Cosmos DB client using the account URL and key
    
    
                try
                {
                    // Create a database if it doesn't already exist
    
    
                    // Create a container with a specified partition key
    
    
                    // Define a typed item (Product) to add to the container
    
    
                    // Add the item to the container
                    // The partition key ensures the item is stored in the correct partition
    
    
                }
                catch (CosmosException ex)
                {
                    // Handle Cosmos DB-specific exceptions
                    // Log the status code and error message for debugging
                    Console.WriteLine($"Cosmos DB Error: {ex.StatusCode} - {ex.Message}");
                }
                catch (Exception ex)
                {
                    // Handle general exceptions
                    // Log the error message for debugging
                    Console.WriteLine($"Error: {ex.Message}");
                }
            }
        }
    }
    ```

### 新增程式代碼以建立客戶端並執行作業 

在練習的本節中，您會在專案的指定區域中新增程序代碼，以建立：客戶端、資料庫、容器，以及將範例專案新增至容器。

1. 使用帳戶 URL 和金鑰**批注建立 Cosmos DB 用戶端之後**，在空間中新增下列程式代碼。 此程式代碼會定義用來連線到 Azure Cosmos DB 帳戶的用戶端。

    ```csharp
    CosmosClient client = new(
        accountEndpoint: cosmosDbAccountUrl,
        authKeyOrResourceToken: accountKey
    );
    ```

    >注意：最佳做法是從 Azure 身分識別連結庫使用 **DefaultAzureCredential***。* 視訂用帳戶的設定方式而定，這可能需要 Azure 中的一些額外設定要求。 

1. 如果資料庫不存在**批註，請在建立資料庫之後**的空格中新增下列程序代碼。 

    ```csharp
    Microsoft.Azure.Cosmos.Database database = await client.CreateDatabaseIfNotExistsAsync(databaseName);
    Console.WriteLine($"Created or retrieved database: {database.Id}");
    ```

1. 在建立具有指定分割區索引鍵**批注的容器之後**，於空間中新增下列程序代碼。 

    ```csharp
    Microsoft.Azure.Cosmos.Database database = await client.CreateDatabaseIfNotExistsAsync(databaseName);
    Console.WriteLine($"Created or retrieved database: {database.Id}");
    ```

1. 在定義具型別專案 （Product） 之後 **，在空間中新增下列程序代碼，以新增至容器** 批注。 這會定義新增至容器的專案。

    ```csharp
    Product newItem = new Product
    {
        id = Guid.NewGuid().ToString(), // Generate a unique ID for the product
        name = "Sample Item",
        description = "This is a sample item in my Azure Cosmos DB exercise."
    };
    ```

1. 在將專案新增至容器**批注之後**，在空間中新增下列程序代碼。 

    ```csharp
    ItemResponse<Product> createResponse = await container.CreateItemAsync(
        item: newItem,
        partitionKey: new PartitionKey(newItem.id)
    );
    ```

1. 現在程序代碼已完成，請儲存進度使用 **ctrl + q** 來儲存盤案，而 **ctrl + q** 則結束編輯器。

1. 在 Cloud Shell 中執行下列命令，以測試專案中是否有任何錯誤。 如果您看到錯誤，請在編輯器中開啟 *Program.cs* 檔案，並檢查是否有遺漏的程式代碼或貼上錯誤。

    ```bash
    dotnet build
    ```

現在專案已完成執行應用程式，並在 Azure 入口網站 中確認結果。

## 執行應用程式並驗證結果

1. `dotnet run`如果在 Cloud Shell 中執行 命令。 輸出應該類似下列範例。

    ```
    Created or retrieved database: myDatabase
    Created or retrieved container: myContainer
    Created item: c549c3fa-054d-40db-a42b-c05deabbc4a6
    Request charge: 6.29 RUs
    ```

1. 在 Azure 入口網站 中，流覽至您稍早建立的 Azure Cosmos DB 資源。 在左側導覽中選取 **[數據總管** ]。 在 **[數據總管] 中**，選取 **[myDatabase** ]，然後展開 **[myContainer**]。 您可以選取 **[專案**] 來檢視您所建立的專案。

    ![顯示數據總管中專案位置的螢幕快照。](./media/09/cosmos-data-explorer.png)

## 清除資源

既然您已完成練習，您應該刪除您所建立的雲端資源，以避免不必要的資源使用量。

1. 流覽至您建立的資源群組，並檢視此練習中使用的資源內容。
1. 在工具列上，選取 [刪除資源群組]****。
1. 輸入資源群組名稱並確認您想要將其刪除。
