---
title: 在 ASP.NET Core 中使用 Grunt
author: rick-anderson
description: 在 ASP.NET Core 中使用 Grunt
ms.author: riande
ms.date: 12/05/2019
no-loc:
- cookie
- Cookie
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: client-side/using-grunt
ms.openlocfilehash: 019f31c1a6fa3a33783f78f2fee71f710642ce6d
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88013199"
---
# <a name="use-grunt-in-aspnet-core"></a>在 ASP.NET Core 中使用 Grunt

Grunt 是一項 JavaScript 工作執行程式，可將腳本縮制、TypeScript 編譯、程式碼品質「不起毛」的工具、CSS 前置處理器，以及任何需要執行以支援用戶端開發的重複性工作自動化。 Visual Studio 中完全支援 Grunt。

這個範例會使用空的 ASP.NET Core 專案做為其起點，以示範如何從頭開始自動執行用戶端組建程式。

完成後的範例會清除目標部署目錄、結合 JavaScript 檔案、檢查程式碼品質、總是 JavaScript 檔案內容，並部署至 web 應用程式的根目錄。 我們將使用下列套件：

* **grunt**： grunt 工作執行器套件。

* **grunt-contrib-clean**：移除檔案或目錄的外掛程式。

* **grunt-contrib-jshint**：用來審查 JavaScript 程式碼品質的外掛程式。

* **grunt-contrib-concat**：將檔案加入單一檔案的外掛程式。

* **grunt-contrib-uglify**：縮短 JavaScript 以減少大小的外掛程式。

* **grunt-contrib-watch：監看**檔案活動的外掛程式。

## <a name="preparing-the-application"></a>準備應用程式

若要開始，請設定新的空白 web 應用程式，並新增 TypeScript 範例檔案。 TypeScript 檔案會使用預設的 Visual Studio 設定自動編譯成 JavaScript，而我們將會使用 Grunt 來處理我們的原始內容。

1. 在 Visual Studio 中，建立新的 `ASP.NET Web Application` 。

2. 在 [**新增 ASP.NET 專案**] 對話方塊中，選取 ASP.NET Core**空白**範本，然後按一下 [確定] 按鈕。

3. 在 [方案總管中，檢查項目結構。 此 `\src` 資料夾包含空的 `wwwroot` 和 `Dependencies` 節點。

    ![空白 web 解決方案](using-grunt/_static/grunt-solution-explorer.png)

4. 將名為的新資料夾新增 `TypeScript` 至您的專案目錄。

5. 在新增任何檔案之前，請確定 Visual Studio 已核取 [在儲存時編譯] 選項。 流覽至 [**工具**] [選項] [  >  **Options**  >  **文字編輯器**] [  >  **Typescript**  >  **專案**]：

    ![設定 TypeScript 檔案自動編譯的選項](using-grunt/_static/typescript-options.png)

6. 在目錄上按一下滑鼠右鍵 `TypeScript` ，然後從內容功能表中選取 [**新增 > 新專案**]。 選取**JavaScript**檔案專案，並將檔案命名為*Tastes* (記下 \*) 的 ts 延伸模組。 將以下的 TypeScript 程式程式碼複製到檔案 (當您儲存時，JavaScript 來源) 會出現新的*Tastes.js*檔案。

    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7. 將第二個檔案新增至**TypeScript**目錄，並將它命名為 `Food.ts` 。 將下列程式碼複製到檔案中。

    ```typescript
    class Food {
      constructor(name: string, calories: number) {
        this._name = name;
        this._calories = calories;
      }

      private _name: string;
      get Name() {
        return this._name;
      }

      private _calories: number;
      get Calories() {
        return this._calories;
      }

      private _taste: Tastes;
      get Taste(): Tastes { return this._taste }
      set Taste(value: Tastes) {
        this._taste = value;
      }
    }
    ```

## <a name="configuring-npm"></a>設定 NPM

接下來，設定 NPM 以下載 grunt 和 grunt 工作。

1. 在 [方案總管中，以滑鼠右鍵按一下專案，然後從內容功能表中選取 [**加入 > 新專案**]。 選取 [ **NPM 設定檔**] 專案，保留預設名稱*package.js*[]，然後按一下 [**新增**] 按鈕。

2. 在 [ *package.json* ] 檔案的 [ `devDependencies` 物件括弧] 中，輸入 "grunt"。 `grunt`從 Intellisense 清單中選取，然後按 enter 鍵。 Visual Studio 會將 grunt 套件名稱加上引號，並新增一個冒號。 在冒號右邊，從 Intellisense 清單的頂端選取最新穩定版本的封裝， (按 [ `Ctrl-Space` 如果 intellisense 未出現) ]。

    ![grunt Intellisense](using-grunt/_static/devdependencies-grunt.png)

    > [!NOTE]
    > NPM 會使用[語義版本](https://semver.org/)設定來組織相依性。 語義版本控制（也稱為 SemVer）會使用編號配置來識別 \<major> 封裝 \<minor> ... \<patch>Intellisense 只會顯示一些常用的選項，以簡化語義版本設定。 在上述範例中， (0.4.5 Intellisense 清單中最上方的專案) 會被視為封裝的最新穩定版本。 插入號 (^) 符號符合最新的主要版本，而波形符 (~) 符合最新的次要版本。 如需 SemVer 所提供完整表現度的指南，請參閱[NPM semver version parser reference](https://www.npmjs.com/package/semver) 。

3. 新增更多相依性以載入 grunt-contrib- \* 適用于*clean*、 *jshint*、 *concat*、 *uglify*和*watch*的封裝，如下列範例所示。 版本不需要符合範例。

    ```json
    "devDependencies": {
      "grunt": "0.4.5",
      "grunt-contrib-clean": "0.6.0",
      "grunt-contrib-jshint": "0.11.0",
      "grunt-contrib-concat": "0.5.1",
      "grunt-contrib-uglify": "0.8.0",
      "grunt-contrib-watch": "0.6.1"
    }
    ```

4. 儲存 package.json 檔案。

每個專案的套件 `devDependencies` 將會下載，以及每個套件所需的任何檔案。 您可以在**方案總管**中啟用 [**顯示所有**檔案] 按鈕，以在*node_modules*目錄中尋找封裝檔案。

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> 如有需要，您可以在**方案總管**中手動還原相依性，方法是以滑鼠右鍵按一下 `Dependencies\NPM` ，然後選取 [**還原封裝**] 功能表選項。

![還原套件](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a>設定 Grunt

Grunt 是使用名為*Gruntfile.js*的資訊清單來設定，其會定義、載入及註冊可以手動執行或設定為根據 Visual Studio 中的事件自動執行的工作。

1. 以滑鼠右鍵按一下專案，然後選取 [**加入**  >  **新專案**]。 選取 [ **JavaScript**檔案專案] 範本，將名稱變更為*Gruntfile.js*，然後按一下 [**新增**] 按鈕。

1. 將下列程式碼新增至*Gruntfile.js*。 函式會 `initConfig` 設定每個封裝的選項，而模組的其餘部分則會載入和註冊工作。

   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

1. 在函式內 `initConfig` ，新增工作的選項， `clean` 如下列範例所示*Gruntfile.js* 。 此工作會 `clean` 接受目錄字串陣列。 此工作會從*wwwroot/lib*中移除檔案，並移除整個 */temp*目錄。

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

1. 在函式底下 `initConfig` ，新增對的呼叫 `grunt.loadNpmTasks` 。 這會讓工作從 Visual Studio 執行。

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

1. 儲存*Gruntfile.js*。 檔案看起來應該類似下面的螢幕擷取畫面。

    ![初始 gruntfile.js](using-grunt/_static/gruntfile-js-initial.png)

1. 以滑鼠右鍵按一下*Gruntfile.js* ，然後從內容功能表中選取 [工作執行**器 Explorer** ]。 [工作執行**器 Explorer** ] 視窗隨即開啟。

    ![工作執行器 explorer 功能表](using-grunt/_static/task-runner-explorer-menu.png)

1. 確認 `clean` 顯示在工作**Tasks**執行**器 Explorer**的 [工作] 底下。

    ![工作執行器 explorer 工作清單](using-grunt/_static/task-runner-explorer-tasks.png)

1. 以滑鼠右鍵按一下清除工作，然後從內容功能表中選取 [**執行**]。 命令視窗會顯示工作的進度。

    ![工作執行器 explorer 執行清除工作](using-grunt/_static/task-runner-explorer-run-clean.png)

    > [!NOTE]
    > 尚無任何要清除的檔案或目錄。 如果您想要的話，可以在方案總管中手動建立它們，然後以測試的形式執行清理工作。

1. 在函 `initConfig` 式中，使用下列程式碼新增的專案 `concat` 。

    `src`屬性陣列會列出要合併的檔案，以其應合併的順序排列。 `dest`屬性會指派所產生之合併檔案的路徑。

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```

    > [!NOTE]
    > `all`上述程式碼中的屬性是目標的名稱。 目標會在某些 Grunt 工作中用來允許多個組建環境。 您可以使用 IntelliSense 來查看內建目標，或指派自己的目標。

1. `jshint`使用下列程式碼新增工作。

    Jshint `code-quality` 公用程式會針對在*暫存*目錄中找到的每個 JavaScript 檔案執行。

    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > 當 JavaScript 使用括弧語法來指派屬性而非點標記法（例如，而不是）時，選項 "-W069" 是 jshint 所產生的錯誤。 `Tastes["Sweet"]` `Tastes.Sweet` 選項會關閉警告，讓程式的其餘部分繼續進行。

1. `uglify`使用下列程式碼新增工作。

    此工作會縮短在臨時目錄中找到的*combined.js*檔案，並遵循標準命名慣例* \<file name\>.min.js*在 wwwroot/lib 中建立結果檔案。

    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

1. 在該負載的呼叫 `grunt.loadNpmTasks` `grunt-contrib-clean` 底下，請使用下列程式碼，針對 jshint、concat 和 uglify 加入相同的呼叫。

    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

1. 儲存*Gruntfile.js*。 檔案看起來應該類似下列範例。

    ![完整的 grunt 檔案範例](using-grunt/_static/gruntfile-js-complete.png)

1. 請注意，[工作執行**器**] 的 [工作清單] 會包含 `clean` 、 `concat` 和工作 `jshint` `uglify` 。 依序執行每個工作，並觀察**方案總管**中的結果。 每項工作都應該執行，而不會發生錯誤。

    ![工作執行器 explorer 執行每項工作](using-grunt/_static/task-runner-explorer-run-each-task.png)

    Concat 工作會建立新的*combined.js*檔案，並將它放入 temp 目錄中。 此工作 `jshint` 只會執行，而且不會產生輸出。 此工作 `uglify` 會建立新的*combined.min.js*檔案，並將其放入*wwwroot/lib*中。 完成時，解決方案看起來應該像下面的螢幕擷取畫面：

    ![所有工作之後的方案瀏覽器](using-grunt/_static/solution-explorer-after-all-tasks.png)

    > [!NOTE]
    > 如需每個套件選項的詳細資訊，請造訪 [https://www.npmjs.com/](https://www.npmjs.com/) 主頁面上 [搜尋] 方塊中的套件名稱並加以查閱。 例如，您可以查詢 grunt-contrib-clean 封裝，以取得說明其所有參數的檔連結。

### <a name="all-together-now"></a>摘要

使用 Grunt `registerTask()` 方法，以特定循序執行一系列的工作。 例如，若要執行上述的範例步驟，請 > concat-> jshint-> uglify 中，將下列程式碼新增至模組。 程式碼應該加入與 loadNpmTasks ( # A1 呼叫相同的層級，而不是在 initConfig 外部。

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

新工作會顯示在 [工作執行器] 中的 [別名工作] 底下。 您可以用滑鼠右鍵按一下並執行，就如同其他工作一樣。 工作 `all` 會依 `clean` 序執行 `concat` 、 `jshint` 和 `uglify` 。

![別名 grunt 工作](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a>監看變更

工作 `watch` 可讓您留意檔案和目錄。 [監看式] 會在偵測到變更時自動觸發工作。 將下列程式碼新增至 initConfig，以監看 \* TypeScript 目錄中的 .js 檔案是否有變更。 如果 JavaScript 檔案已變更， `watch` 將會執行工作 `all` 。

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

新增對的呼叫 `loadNpmTasks()` ，以在 [工作執行 `watch` 器] Explorer 中顯示工作。

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

以滑鼠右鍵按一下工作執行器 Explorer 中的 [監看式] 工作，然後從內容功能表中選取 [執行]。 顯示執行中監看工作的命令視窗會顯示「正在等候 ...」消息。 開啟其中一個 TypeScript 檔案，加入一個空格，然後儲存檔案。 這會觸發監看式工作，並觸發其他工作依序執行。 下列螢幕擷取畫面顯示範例執行。

![執行工作輸出](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a>系結至 Visual Studio 事件

除非您想要在每次使用 Visual Studio 時手動啟動工作，否則請**在組建、****清除**和**專案開啟**事件**之後**，將工作系結至。

[系結]， `watch` 讓它在每次開啟時執行 Visual Studio。 在 [工作執行器] 中，以滑鼠右鍵按一下 [ **Bindings**監看式] 工作，然後  >  從內容功能表中選取 [系**結專案**]。

![將工作系結至專案開啟](using-grunt/_static/bindings-project-open.png)

卸載並重載專案。 當專案再次載入時，[監看式] 工作會自動開始執行。

## <a name="summary"></a>總結

Grunt 是功能強大的工作執行器，可以用來將大部分的用戶端組建作業自動化。 Grunt 會利用 NPM 來傳遞其套件，並使用與 Visual Studio 的功能工具整合。 Visual Studio 的工作執行器 Explorer 會偵測設定檔案的變更，並提供便利的介面來執行工作、查看執行中的工作，以及將工作系結至 Visual Studio 的事件。
