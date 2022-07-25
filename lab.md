---
ms.openlocfilehash: 57e7101c824903545932fce809cd5661f95cd3dd
ms.sourcegitcommit: d2d374fffa4fcbf92b9c4bdc9c9ecc470152e033
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/16/2021
ms.locfileid: "145196125"
---
# <a name="lab-virtual-machine-setup"></a>實驗室虛擬機器設定

## <a name="installed-software"></a>已安裝的軟體

| 軟體 | 連結 |
| --- | --- |
| Windows 10 (版本 2004) | <https://www.microsoft.com/software-download/windows10> |
| Visual Studio Code | <https://code.visualstudio.com> |
| Visual Studio Code Azure 帳戶延伸模組 | <https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account> |
| Visual Studio Code Azure Functions 延伸模組 | <https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions> |
| Visual Studio Code Azure Resource Manager 工具延伸模組 | <https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools> |
| Visual Studio Code Azure CLI 工具延伸模組 | <https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli> |
| Visual Studio Code PowerShell 延伸模組 | <https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell> |
| Visual Studio Code C# 延伸模組 | <https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp> |
| PowerShell 7 | <https://github.com/PowerShell/PowerShell/releases/tag/v7.0.3> |
| .NET Core 3.1 SDK | <https://dotnet.microsoft.com/download/dotnet-core/3.1> |
| Azure PowerShell | <https://docs.microsoft.com/powershell/azure/install-az-ps> |
| Azure CLI | <https://docs.microsoft.com/cli/azure/install-azure-cli> |
| Azure 儲存體總管 | <https://azure.microsoft.com/features/storage-explorer> |
| .NET 工具 - HttpRepl | <https://github.com/dotnet/HttpRepl> |
| Azure Functions Core Tools | <https://docs.microsoft.com/azure/azure-functions/functions-run-local#v3> |
| Windows 終端機 | <https://aka.ms/terminal> |
| Edge (Chromium) | <https://www.microsoft.com/edge> |

## <a name="additional-configuration"></a>其他組態

- 啟用 ClearType
  
- 將 Microsoft Edge 設為預設瀏覽器

- 更新 VSCode 設定

  ```json
  {
    "editor.fontFamily": "'Cascadia Code', Consolas, 'Courier New', monospace",
    "update.enableWindowsBackgroundUpdates": false,
    "update.mode": "manual",
    "terminal.integrated.shell.windows": "C:\\Program Files\\PowerShell\\7\\pwsh.exe",
    "workbench.startupEditor": "none",
    "terminal.integrated.rendererType": "dom",
    "csharp.suppressDotnetInstallWarning": true,
    "csharp.suppressDotnetRestoreNotification": true,
    "csharp.supressBuildAssetsNotification": true,
    "azureFunctions.showProjectWarning": false
  }
  ```

- 更新 Windows 終端機設定

  ```json
  {
    "$schema": "https://aka.ms/terminal-profiles-schema",
    "defaultProfile": "{574e775e-4f2a-5b96-ac1e-a2962a402336}",
    "profiles": [
      {
        "guid": "{574e775e-4f2a-5b96-ac1e-a2962a402336}",
        "useAcrylic": true,
        "acrylicOpacity": 0.85,
        "colorScheme": "Campbell",
        "fontFace": "Cascadia Code",
        "hidden": false,
        "name": "PowerShell",
        "source": "Windows.Terminal.PowershellCore"
      },
      {
        "guid": "{b453ae62-4e3d-5e58-b989-0a998ec441b8}",
        "hidden": false,
        "name": "Azure Cloud Shell",
        "source": "Windows.Terminal.Azure"
      }
    ],
    "schemes": [],
    "keybindings": []
  }
  ```

- 設定只包含下列圖示的 [開始] 功能表和工作列：
  - 檔案總管
  - Edge
  - Windows 終端機
  - Visual Studio Code
  - Azure 儲存體總管

- 停用 PowerShell 7 更新通知

  1. [建立名為 ``POWERSHELL_UPDATECHECK`` 的環境變數](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_update_notifications?view=powershell-7)
  
  1. 將環境變數的值設為 ``Off`` (區分大小寫)

- 至少執行一次 Azure Functions Core Tools 以設定Windows 防火牆

  ```bash
  func init test --worker-runtime dotnet
  cd test
  func new --template 'HTTP trigger' --name web
  func start --build
  ```
