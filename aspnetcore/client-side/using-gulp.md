---
title: 在 ASP.NET Core 中使用 Gulp
author: rick-anderson
description: 了解如何在 ASP.NET Core 中使用 Gulp。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/28/2017
uid: client-side/using-gulp
ms.openlocfilehash: 35f62bf276d3708df0e2c8b56a44c34c178d8ff8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278248"
---
# <a name="use-gulp-in-aspnet-core"></a><span data-ttu-id="53bbd-103">在 ASP.NET Core 中使用 Gulp</span><span class="sxs-lookup"><span data-stu-id="53bbd-103">Use Gulp in ASP.NET Core</span></span>

<span data-ttu-id="53bbd-104">由[Erik Reitan](https://github.com/Erikre)， [Scott Addie](https://scottaddie.com)，[奧 Roth](https://github.com/danroth27)，和[Shayne Boyer](https://twitter.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="53bbd-104">By [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Daniel Roth](https://github.com/danroth27), and [Shayne Boyer](https://twitter.com/spboyer)</span></span>

<span data-ttu-id="53bbd-105">在典型的現代 web 應用程式中，可能會在建置程序：</span><span class="sxs-lookup"><span data-stu-id="53bbd-105">In a typical modern web app, the build process might:</span></span>

* <span data-ttu-id="53bbd-106">包裝並最小化 JavaScript 和 CSS 的檔案。</span><span class="sxs-lookup"><span data-stu-id="53bbd-106">Bundle and minify JavaScript and CSS files.</span></span>
* <span data-ttu-id="53bbd-107">每次建置前執行工具以呼叫包裝與最小化工作。</span><span class="sxs-lookup"><span data-stu-id="53bbd-107">Run tools to call the bundling and minification tasks before each build.</span></span>
* <span data-ttu-id="53bbd-108">編譯 LESS 或 SASS 檔案為 CSS 。</span><span class="sxs-lookup"><span data-stu-id="53bbd-108">Compile LESS or SASS files to CSS.</span></span>
* <span data-ttu-id="53bbd-109">編譯 CoffeeScript 或 TypeScript 檔案為 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="53bbd-109">Compile CoffeeScript or TypeScript files to JavaScript.</span></span>

<span data-ttu-id="53bbd-110">「工作執行器」是一種自動執行例行性開發工作等的工具。</span><span class="sxs-lookup"><span data-stu-id="53bbd-110">A *task runner* is a tool which automates these routine development tasks and more.</span></span> <span data-ttu-id="53bbd-111">Visual Studio 為兩個熱門的 JavaScript 型工作執行器提供內建支援: [Gulp](https://gulpjs.com/)和[Grunt](using-grunt.md)。</span><span class="sxs-lookup"><span data-stu-id="53bbd-111">Visual Studio provides built-in support for two popular JavaScript-based task runners: [Gulp](https://gulpjs.com/) and [Grunt](using-grunt.md).</span></span>

## <a name="gulp"></a><span data-ttu-id="53bbd-112">gulp</span><span class="sxs-lookup"><span data-stu-id="53bbd-112">Gulp</span></span>

<span data-ttu-id="53bbd-113">Gulp 是以 JavaScript 為基礎的資料流建置工具組，用於用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="53bbd-113">Gulp is a JavaScript-based streaming build toolkit for client-side code.</span></span> <span data-ttu-id="53bbd-114">當在建置環境中觸發特定事件時，通常會使用它透過一系列的程序串流用戶端檔案。</span><span class="sxs-lookup"><span data-stu-id="53bbd-114">It's commonly used to stream client-side files through a series of processes when a specific event is triggered in a build environment.</span></span> <span data-ttu-id="53bbd-115">例如，在新的建置之前，Gulp 可用於自動化[包裝及縮小](bundling-and-minification.md)或開發環境清理。</span><span class="sxs-lookup"><span data-stu-id="53bbd-115">For instance, Gulp can be used to automate [bundling and minification](bundling-and-minification.md) or the cleansing of a development environment before a new build.</span></span>

<span data-ttu-id="53bbd-116">在 *gulpfile.js* 中定義一組 Gulp 工作。</span><span class="sxs-lookup"><span data-stu-id="53bbd-116">A set of Gulp tasks is defined in *gulpfile.js*.</span></span> <span data-ttu-id="53bbd-117">下列 JavaScript 包含 Gulp 模組，並指定要在即將到來的工作中引用的文件路徑：</span><span class="sxs-lookup"><span data-stu-id="53bbd-117">The following JavaScript includes Gulp modules and specifies file paths to be referenced within the forthcoming tasks:</span></span>

```javascript
/// <binding Clean='clean' />
"use strict";

var gulp = require("gulp"),
  rimraf = require("rimraf"),
  concat = require("gulp-concat"),
  cssmin = require("gulp-cssmin"),
  uglify = require("gulp-uglify");

var paths = {
  webroot: "./wwwroot/"
};

paths.js = paths.webroot + "js/**/*.js";
paths.minJs = paths.webroot + "js/**/*.min.js";
paths.css = paths.webroot + "css/**/*.css";
paths.minCss = paths.webroot + "css/**/*.min.css";
paths.concatJsDest = paths.webroot + "js/site.min.js";
paths.concatCssDest = paths.webroot + "css/site.min.css";
```

<span data-ttu-id="53bbd-118">上述程式碼會指定哪一個節點模組所需。</span><span class="sxs-lookup"><span data-stu-id="53bbd-118">The above code specifies which Node modules are required.</span></span> <span data-ttu-id="53bbd-119">`require`函式匯入每個模組，以便的相依工作可以使用其功能。</span><span class="sxs-lookup"><span data-stu-id="53bbd-119">The `require` function imports each module so that the dependent tasks can utilize their features.</span></span> <span data-ttu-id="53bbd-120">每個匯入的模組會指派給變數。</span><span class="sxs-lookup"><span data-stu-id="53bbd-120">Each of the imported modules is assigned to a variable.</span></span> <span data-ttu-id="53bbd-121">模組可以位於依名稱或路徑。</span><span class="sxs-lookup"><span data-stu-id="53bbd-121">The modules can be located either by name or path.</span></span> <span data-ttu-id="53bbd-122">在此範例中，將模組命名為`gulp`， `rimraf`， `gulp-concat`， `gulp-cssmin`，和`gulp-uglify`會依名稱擷取。</span><span class="sxs-lookup"><span data-stu-id="53bbd-122">In this example, the modules named `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, and `gulp-uglify` are retrieved by name.</span></span> <span data-ttu-id="53bbd-123">此外，能夠重複使用及工作內所參考的 CSS 和 JavaScript 檔案的位置，會建立一系列的路徑。</span><span class="sxs-lookup"><span data-stu-id="53bbd-123">Additionally, a series of paths are created so that the locations of CSS and JavaScript files can be reused and referenced within the tasks.</span></span> <span data-ttu-id="53bbd-124">下表提供的模組中包含描述*gulpfile.js*。</span><span class="sxs-lookup"><span data-stu-id="53bbd-124">The following table provides descriptions of the modules included in *gulpfile.js*.</span></span>

| <span data-ttu-id="53bbd-125">模組名稱</span><span class="sxs-lookup"><span data-stu-id="53bbd-125">Module Name</span></span> | <span data-ttu-id="53bbd-126">描述</span><span class="sxs-lookup"><span data-stu-id="53bbd-126">Description</span></span> |
| ----------- | ----------- |
| <span data-ttu-id="53bbd-127">gulp</span><span class="sxs-lookup"><span data-stu-id="53bbd-127">gulp</span></span>        | <span data-ttu-id="53bbd-128">Gulp 串流建置系統。</span><span class="sxs-lookup"><span data-stu-id="53bbd-128">The Gulp streaming build system.</span></span> <span data-ttu-id="53bbd-129">如需詳細資訊，請參閱[gulp](https://www.npmjs.com/package/gulp)。</span><span class="sxs-lookup"><span data-stu-id="53bbd-129">For more information, see [gulp](https://www.npmjs.com/package/gulp).</span></span> |
| <span data-ttu-id="53bbd-130">rimraf</span><span class="sxs-lookup"><span data-stu-id="53bbd-130">rimraf</span></span>      | <span data-ttu-id="53bbd-131">節點刪除模組。</span><span class="sxs-lookup"><span data-stu-id="53bbd-131">A Node deletion module.</span></span> <span data-ttu-id="53bbd-132">如需詳細資訊，請參閱[rimraf](https://www.npmjs.com/package/rimraf)。</span><span class="sxs-lookup"><span data-stu-id="53bbd-132">For more information, see [rimraf](https://www.npmjs.com/package/rimraf).</span></span> |
| <span data-ttu-id="53bbd-133">gulp concat</span><span class="sxs-lookup"><span data-stu-id="53bbd-133">gulp-concat</span></span> | <span data-ttu-id="53bbd-134">模組可串連作業系統的新行字元為基礎的檔案。</span><span class="sxs-lookup"><span data-stu-id="53bbd-134">A module that concatenates files based on the operating system's newline character.</span></span> <span data-ttu-id="53bbd-135">如需詳細資訊，請參閱[gulp concat](https://www.npmjs.com/package/gulp-concat)。</span><span class="sxs-lookup"><span data-stu-id="53bbd-135">For more information, see [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span></span> |
| <span data-ttu-id="53bbd-136">gulp-cssmin</span><span class="sxs-lookup"><span data-stu-id="53bbd-136">gulp-cssmin</span></span> | <span data-ttu-id="53bbd-137">縮短 CSS 檔案的模組。</span><span class="sxs-lookup"><span data-stu-id="53bbd-137">A module that minifies CSS files.</span></span> <span data-ttu-id="53bbd-138">如需詳細資訊，請參閱[gulp cssmin](https://www.npmjs.com/package/gulp-cssmin)。</span><span class="sxs-lookup"><span data-stu-id="53bbd-138">For more information, see [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin).</span></span> |
| <span data-ttu-id="53bbd-139">gulp uglify</span><span class="sxs-lookup"><span data-stu-id="53bbd-139">gulp-uglify</span></span> | <span data-ttu-id="53bbd-140">縮短模組 *.js*檔案。</span><span class="sxs-lookup"><span data-stu-id="53bbd-140">A module that minifies *.js* files.</span></span> <span data-ttu-id="53bbd-141">如需詳細資訊，請參閱[gulp uglify](https://www.npmjs.com/package/gulp-uglify)。</span><span class="sxs-lookup"><span data-stu-id="53bbd-141">For more information, see [gulp-uglify](https://www.npmjs.com/package/gulp-uglify).</span></span> |

<span data-ttu-id="53bbd-142">一旦匯入必要模組，您可以指定工作。</span><span class="sxs-lookup"><span data-stu-id="53bbd-142">Once the requisite modules are imported, the tasks can be specified.</span></span> <span data-ttu-id="53bbd-143">這裡有六個工作註冊中，由下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="53bbd-143">Here there are six tasks registered, represented by the following code:</span></span>

```javascript
gulp.task("clean:js", function (cb) {
  rimraf(paths.concatJsDest, cb);
});

gulp.task("clean:css", function (cb) {
  rimraf(paths.concatCssDest, cb);
});

gulp.task("clean", ["clean:js", "clean:css"]);

gulp.task("min:js", function () {
  return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
    .pipe(concat(paths.concatJsDest))
    .pipe(uglify())
    .pipe(gulp.dest("."));
});

gulp.task("min:css", function () {
  return gulp.src([paths.css, "!" + paths.minCss])
    .pipe(concat(paths.concatCssDest))
    .pipe(cssmin())
    .pipe(gulp.dest("."));
});

gulp.task("min", ["min:js", "min:css"]);
```

<span data-ttu-id="53bbd-144">下表提供上述程式碼中指定之工作的說明：</span><span class="sxs-lookup"><span data-stu-id="53bbd-144">The following table provides an explanation of the tasks specified in the code above:</span></span>

|<span data-ttu-id="53bbd-145">任務名稱</span><span class="sxs-lookup"><span data-stu-id="53bbd-145">Task Name</span></span>|<span data-ttu-id="53bbd-146">描述</span><span class="sxs-lookup"><span data-stu-id="53bbd-146">Description</span></span>|
|--- |--- |
|<span data-ttu-id="53bbd-147">全新： js</span><span class="sxs-lookup"><span data-stu-id="53bbd-147">clean:js</span></span>|<span data-ttu-id="53bbd-148">若要移除 site.js 檔案的縮短的版本使用 rimraf 節點刪除模組的工作。</span><span class="sxs-lookup"><span data-stu-id="53bbd-148">A task that uses the rimraf Node deletion module to remove the minified version of the site.js file.</span></span>|
|<span data-ttu-id="53bbd-149">全新： css</span><span class="sxs-lookup"><span data-stu-id="53bbd-149">clean:css</span></span>|<span data-ttu-id="53bbd-150">若要移除 site.css 檔案的縮短的版本使用 rimraf 節點刪除模組的工作。</span><span class="sxs-lookup"><span data-stu-id="53bbd-150">A task that uses the rimraf Node deletion module to remove the minified version of the site.css file.</span></span>|
|<span data-ttu-id="53bbd-151">清除</span><span class="sxs-lookup"><span data-stu-id="53bbd-151">clean</span></span>|<span data-ttu-id="53bbd-152">呼叫工作`clean:js`工作中，後面接著`clean:css`工作。</span><span class="sxs-lookup"><span data-stu-id="53bbd-152">A task that calls the `clean:js` task, followed by the `clean:css` task.</span></span>|
|<span data-ttu-id="53bbd-153">min:js</span><span class="sxs-lookup"><span data-stu-id="53bbd-153">min:js</span></span>|<span data-ttu-id="53bbd-154">工作並縮短，串連的 js 資料夾中的所有.js 檔案。</span><span class="sxs-lookup"><span data-stu-id="53bbd-154">A task that minifies and concatenates all .js files within the js folder.</span></span> <span data-ttu-id="53bbd-155">。 Min.js 檔案會排除。</span><span class="sxs-lookup"><span data-stu-id="53bbd-155">The .min.js files are excluded.</span></span>|
|<span data-ttu-id="53bbd-156">min:css</span><span class="sxs-lookup"><span data-stu-id="53bbd-156">min:css</span></span>|<span data-ttu-id="53bbd-157">縮短及串連的 css 資料夾中的所有.css 檔案的工作。</span><span class="sxs-lookup"><span data-stu-id="53bbd-157">A task that minifies and concatenates all .css files within the css folder.</span></span> <span data-ttu-id="53bbd-158">。 Min.css 檔案會排除。</span><span class="sxs-lookup"><span data-stu-id="53bbd-158">The .min.css files are excluded.</span></span>|
|<span data-ttu-id="53bbd-159">min</span><span class="sxs-lookup"><span data-stu-id="53bbd-159">min</span></span>|<span data-ttu-id="53bbd-160">呼叫工作`min:js`工作中，後面接著`min:css`工作。</span><span class="sxs-lookup"><span data-stu-id="53bbd-160">A task that calls the `min:js` task, followed by the `min:css` task.</span></span>|

## <a name="running-default-tasks"></a><span data-ttu-id="53bbd-161">執行預設的工作</span><span class="sxs-lookup"><span data-stu-id="53bbd-161">Running default tasks</span></span>

<span data-ttu-id="53bbd-162">如果您尚未建立新的 Web 應用程式，請在 Visual Studio 中建立新的 ASP.NET Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="53bbd-162">If you haven't already created a new Web app, create a new ASP.NET Web Application project in Visual Studio.</span></span>

1.  <span data-ttu-id="53bbd-163">將新的 JavaScript 檔案加入至您的專案並將其命名*gulpfile.js*，然後複製下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="53bbd-163">Add a new JavaScript file to your project and name it *gulpfile.js*, then copy the following code.</span></span>

    ```javascript
    /// <binding Clean='clean' />
    "use strict";
    
    var gulp = require("gulp"),
      rimraf = require("rimraf"),
      concat = require("gulp-concat"),
      cssmin = require("gulp-cssmin"),
      uglify = require("gulp-uglify");
    
    var paths = {
      webroot: "./wwwroot/"
    };
    
    paths.js = paths.webroot + "js/**/*.js";
    paths.minJs = paths.webroot + "js/**/*.min.js";
    paths.css = paths.webroot + "css/**/*.css";
    paths.minCss = paths.webroot + "css/**/*.min.css";
    paths.concatJsDest = paths.webroot + "js/site.min.js";
    paths.concatCssDest = paths.webroot + "css/site.min.css";
    
    gulp.task("clean:js", function (cb) {
      rimraf(paths.concatJsDest, cb);
    });
    
    gulp.task("clean:css", function (cb) {
      rimraf(paths.concatCssDest, cb);
    });
    
    gulp.task("clean", ["clean:js", "clean:css"]);
    
    gulp.task("min:js", function () {
      return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
        .pipe(concat(paths.concatJsDest))
        .pipe(uglify())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min:css", function () {
      return gulp.src([paths.css, "!" + paths.minCss])
        .pipe(concat(paths.concatCssDest))
        .pipe(cssmin())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min", ["min:js", "min:css"]);
    ```

2.  <span data-ttu-id="53bbd-164">開啟*package.json*檔案 (新增如果不存在)，然後將下列。</span><span class="sxs-lookup"><span data-stu-id="53bbd-164">Open the *package.json* file (add if not there) and add the following.</span></span>

    ```json
    {
      "devDependencies": {
        "gulp": "3.9.1",
        "gulp-concat": "2.6.1",
        "gulp-cssmin": "0.1.7",
        "gulp-uglify": "2.0.1",
        "rimraf": "2.6.1"
      }
    }
    ```

3.  <span data-ttu-id="53bbd-165">在**方案總管 中**，以滑鼠右鍵按一下*gulpfile.js*，然後選取**工作執行器總管**。</span><span class="sxs-lookup"><span data-stu-id="53bbd-165">In **Solution Explorer**, right-click *gulpfile.js*, and select **Task Runner Explorer**.</span></span>
    
    ![從 [方案總管] 中開啟工作執行器總管](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    <span data-ttu-id="53bbd-167">**工作執行器總管**顯示 Gulp 工作的清單。</span><span class="sxs-lookup"><span data-stu-id="53bbd-167">**Task Runner Explorer** shows the list of Gulp tasks.</span></span> <span data-ttu-id="53bbd-168">(您可能必須按一下**重新整理**會顯示專案名稱的左邊的按鈕。)</span><span class="sxs-lookup"><span data-stu-id="53bbd-168">(You might have to click the **Refresh** button that appears to the left of the project name.)</span></span>
    
    ![工作執行器總管](using-gulp/_static/03-TaskRunnerExplorer.png)
    
    > [!IMPORTANT]
    > <span data-ttu-id="53bbd-170">**工作執行器總管**才會出現內容功能表項目*gulpfile.js*根專案目錄中。</span><span class="sxs-lookup"><span data-stu-id="53bbd-170">The **Task Runner Explorer** context menu item appears only if *gulpfile.js* is in the root project directory.</span></span>

4.  <span data-ttu-id="53bbd-171">下面**工作**中**工作執行器總管**，以滑鼠右鍵按一下**全新**，然後選取**執行**從快顯功能表。</span><span class="sxs-lookup"><span data-stu-id="53bbd-171">Underneath **Tasks** in **Task Runner Explorer**, right-click **clean**, and select **Run** from the pop-up menu.</span></span>

    ![工作執行器總管清除 」 工作](using-gulp/_static/04-TaskRunner-clean.png)

    <span data-ttu-id="53bbd-173">**工作執行器總管**會建立新的索引標籤，名為**全新**並執行 「 清除 」 工作，如其定義在*gulpfile.js*。</span><span class="sxs-lookup"><span data-stu-id="53bbd-173">**Task Runner Explorer** will create a new tab named **clean** and execute the clean task as it's defined in *gulpfile.js*.</span></span>

5.  <span data-ttu-id="53bbd-174">以滑鼠右鍵按一下**全新**工作，然後選取 **繫結** > **之前建置**。</span><span class="sxs-lookup"><span data-stu-id="53bbd-174">Right-click the **clean** task, then select **Bindings** > **Before Build**.</span></span>

    ![繫結 BeforeBuild 工作執行器總管](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    <span data-ttu-id="53bbd-176">**之前建置**繫結會設定自動每個專案的組建之前執行 「 清除 」 工作。</span><span class="sxs-lookup"><span data-stu-id="53bbd-176">The **Before Build** binding configures the clean task to run automatically before each build of the project.</span></span>

<span data-ttu-id="53bbd-177">繫結的設定**工作執行器總管**頂端的註解的形式儲存您*gulpfile.js*和只適用於 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="53bbd-177">The bindings you set up with **Task Runner Explorer** are stored in the form of a comment at the top of your *gulpfile.js* and are effective only in Visual Studio.</span></span> <span data-ttu-id="53bbd-178">不需要 Visual Studio 的替代方式是設定自動執行的 gulp 工作中您 *.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="53bbd-178">An alternative that doesn't require Visual Studio is to configure automatic execution of gulp tasks in your *.csproj* file.</span></span> <span data-ttu-id="53bbd-179">例如，將此字串放您 *.csproj*檔案：</span><span class="sxs-lookup"><span data-stu-id="53bbd-179">For example, put this in your *.csproj* file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

<span data-ttu-id="53bbd-180">現在當您執行專案，Visual Studio 中，或從命令提示字元使用，會執行 「 清除 」 工作[dotnet 執行](/dotnet/core/tools/dotnet-run)命令 (執行`npm install`第一個)。</span><span class="sxs-lookup"><span data-stu-id="53bbd-180">Now the clean task is executed when you run the project in Visual Studio or from a command prompt using the [dotnet run](/dotnet/core/tools/dotnet-run) command (run `npm install` first).</span></span>

## <a name="defining-and-running-a-new-task"></a><span data-ttu-id="53bbd-181">定義和執行新工作</span><span class="sxs-lookup"><span data-stu-id="53bbd-181">Defining and running a new task</span></span>

<span data-ttu-id="53bbd-182">若要定義新的 Gulp 工作，請修改*gulpfile.js*。</span><span class="sxs-lookup"><span data-stu-id="53bbd-182">To define a new Gulp task, modify *gulpfile.js*.</span></span>

1.  <span data-ttu-id="53bbd-183">結尾加入下列 JavaScript *gulpfile.js*:</span><span class="sxs-lookup"><span data-stu-id="53bbd-183">Add the following JavaScript to the end of *gulpfile.js*:</span></span>

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    <span data-ttu-id="53bbd-184">這項工作中名為`first`，並只會顯示字串。</span><span class="sxs-lookup"><span data-stu-id="53bbd-184">This task is named `first`, and it simply displays a string.</span></span>

2.  <span data-ttu-id="53bbd-185">儲存*gulpfile.js*。</span><span class="sxs-lookup"><span data-stu-id="53bbd-185">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="53bbd-186">在**方案總管 中**，以滑鼠右鍵按一下*gulpfile.js*，然後選取*工作執行器總管*。</span><span class="sxs-lookup"><span data-stu-id="53bbd-186">In **Solution Explorer**, right-click *gulpfile.js*, and select *Task Runner Explorer*.</span></span>

4.  <span data-ttu-id="53bbd-187">在**工作執行器總管**，以滑鼠右鍵按一下**第一個**，然後選取**執行**。</span><span class="sxs-lookup"><span data-stu-id="53bbd-187">In **Task Runner Explorer**, right-click **first**, and select **Run**.</span></span>

    ![執行第一項工作的工作執行器總管](using-gulp/_static/06-TaskRunner-First.png)

    <span data-ttu-id="53bbd-189">輸出文字會顯示。</span><span class="sxs-lookup"><span data-stu-id="53bbd-189">The output text is displayed.</span></span> <span data-ttu-id="53bbd-190">若要查看根據常見的案例的範例，請參閱[Gulp 配方](#gulp-recipes)。</span><span class="sxs-lookup"><span data-stu-id="53bbd-190">To see examples based on common scenarios, see [Gulp Recipes](#gulp-recipes).</span></span>

## <a name="defining-and-running-tasks-in-a-series"></a><span data-ttu-id="53bbd-191">定義並執行序列中的工作</span><span class="sxs-lookup"><span data-stu-id="53bbd-191">Defining and running tasks in a series</span></span>

<span data-ttu-id="53bbd-192">當您執行多個工作時，工作同時執行的預設值。</span><span class="sxs-lookup"><span data-stu-id="53bbd-192">When you run multiple tasks, the tasks run concurrently by default.</span></span> <span data-ttu-id="53bbd-193">不過，如果您需要以特定順序執行的工作，您必須指定當每一項工作完成時，也做為哪些工作相依於另一項工作完成。</span><span class="sxs-lookup"><span data-stu-id="53bbd-193">However, if you need to run tasks in a specific order, you must specify when each task is complete, as well as which tasks depend on the completion of another task.</span></span>

1.  <span data-ttu-id="53bbd-194">若要定義一系列的順序執行的工作，取代`first`工作，您在上面加入*gulpfile.js*為下列：</span><span class="sxs-lookup"><span data-stu-id="53bbd-194">To define a series of tasks to run in order, replace the `first` task that you added above in *gulpfile.js* with the following:</span></span>

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    <span data-ttu-id="53bbd-195">您現在有三個工作： `series:first`， `series:second`，和`series`。</span><span class="sxs-lookup"><span data-stu-id="53bbd-195">You now have three tasks: `series:first`, `series:second`, and `series`.</span></span> <span data-ttu-id="53bbd-196">`series:second`工作包含指定的工作來執行和完成之前陣列的第二個參數`series:second`工作將會執行。</span><span class="sxs-lookup"><span data-stu-id="53bbd-196">The `series:second` task includes a second parameter which specifies an array of tasks to be run and completed before the `series:second` task will run.</span></span> <span data-ttu-id="53bbd-197">如同在上方，唯一的程式碼中指定`series:first`之前必須完成工作`series:second`工作將會執行。</span><span class="sxs-lookup"><span data-stu-id="53bbd-197">As specified in the code above, only the `series:first` task must be completed before the `series:second` task will run.</span></span>

2.  <span data-ttu-id="53bbd-198">儲存*gulpfile.js*。</span><span class="sxs-lookup"><span data-stu-id="53bbd-198">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="53bbd-199">在**方案總管 中**，以滑鼠右鍵按一下*gulpfile.js*選取**工作執行器總管**如果尚未開啟。</span><span class="sxs-lookup"><span data-stu-id="53bbd-199">In **Solution Explorer**, right-click *gulpfile.js* and select **Task Runner Explorer** if it isn't already open.</span></span>

4.  <span data-ttu-id="53bbd-200">在**工作執行器總管**，以滑鼠右鍵按一下**數列**選取**執行**。</span><span class="sxs-lookup"><span data-stu-id="53bbd-200">In **Task Runner Explorer**, right-click **series** and select **Run**.</span></span>

    ![工作執行器總管執行系列的工作](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a><span data-ttu-id="53bbd-202">IntelliSense</span><span class="sxs-lookup"><span data-stu-id="53bbd-202">IntelliSense</span></span>

<span data-ttu-id="53bbd-203">IntelliSense 提供程式碼完成功能、 參數說明和其他功能，以提高生產力，並減少錯誤。</span><span class="sxs-lookup"><span data-stu-id="53bbd-203">IntelliSense provides code completion, parameter descriptions, and other features to boost productivity and to decrease errors.</span></span> <span data-ttu-id="53bbd-204">Gulp 工作 JavaScript 撰寫的。因此，IntelliSense 可提供開發時的協助。</span><span class="sxs-lookup"><span data-stu-id="53bbd-204">Gulp tasks are written in JavaScript; therefore, IntelliSense can provide assistance while developing.</span></span> <span data-ttu-id="53bbd-205">當您使用 JavaScript，IntelliSense 就會列出物件、 函式、 屬性和參數可根據您目前的內容。</span><span class="sxs-lookup"><span data-stu-id="53bbd-205">As you work with JavaScript, IntelliSense lists the objects, functions, properties, and parameters that are available based on your current context.</span></span> <span data-ttu-id="53bbd-206">從 IntelliSense 完成程式碼所提供的快顯清單中選取程式碼撰寫選項。</span><span class="sxs-lookup"><span data-stu-id="53bbd-206">Select a coding option from the pop-up list provided by IntelliSense to complete the code.</span></span>

![IntelliSense 的 gulp](using-gulp/_static/08-IntelliSense.png)

<span data-ttu-id="53bbd-208">如需有關 IntelliSense 的詳細資訊，請參閱[JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense)。</span><span class="sxs-lookup"><span data-stu-id="53bbd-208">For more information about IntelliSense, see [JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense).</span></span>

## <a name="development-staging-and-production-environments"></a><span data-ttu-id="53bbd-209">開發、 預備及生產環境</span><span class="sxs-lookup"><span data-stu-id="53bbd-209">Development, staging, and production environments</span></span>

<span data-ttu-id="53bbd-210">當 Gulp 用來最佳化預備和生產環境的用戶端檔案中時，處理的檔案會儲存至本機臨時區域與生產位置。</span><span class="sxs-lookup"><span data-stu-id="53bbd-210">When Gulp is used to optimize client-side files for staging and production, the processed files are saved to a local staging and production location.</span></span> <span data-ttu-id="53bbd-211">*_Layout.cshtml*檔案使用**環境**標記協助程式提供的 CSS 檔案的兩個不同版本。</span><span class="sxs-lookup"><span data-stu-id="53bbd-211">The *_Layout.cshtml* file uses the **environment** tag helper to provide two different versions of CSS files.</span></span> <span data-ttu-id="53bbd-212">一個版本的 CSS 檔案供程式開發，另一個版本中最適合用於預備和生產環境。</span><span class="sxs-lookup"><span data-stu-id="53bbd-212">One version of CSS files is for development and the other version is optimized for both staging and production.</span></span> <span data-ttu-id="53bbd-213">在 Visual Studio 2017，當您變更**ASPNETCORE_ENVIRONMENT**環境變數，以`Production`，Visual Studio 會建置的 Web 應用程式和連結至最小化的 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="53bbd-213">In Visual Studio 2017, when you change the **ASPNETCORE_ENVIRONMENT** environment variable to `Production`, Visual Studio will build the Web app and link to the minimized CSS files.</span></span> <span data-ttu-id="53bbd-214">下列標記顯示**環境**標記包含連結標記協助程式`Development`CSS 檔案及縮短`Staging, Production`CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="53bbd-214">The following markup shows the **environment** tag helpers containing link tags to the `Development` CSS files and the minified `Staging, Production` CSS files.</span></span>

```html
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/js/site.js" asp-append-version="true"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.2.0.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery"
            crossorigin="anonymous"
            integrity="sha384-K+ctZQ+LL8q6tP7I94W+qzQsfRV2a+AfHIi9k8z8l9ggpc8X+Ytst4yBo/hH+8Fk">
    </script>
    <script src="https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js"
            asp-fallback-src="~/lib/bootstrap/dist/js/bootstrap.min.js"
            asp-fallback-test="window.jQuery && window.jQuery.fn && window.jQuery.fn.modal"
            crossorigin="anonymous"
            integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa">
    </script>
    <script src="~/js/site.min.js" asp-append-version="true"></script>
</environment>
```

## <a name="switching-between-environments"></a><span data-ttu-id="53bbd-215">環境之間切換</span><span class="sxs-lookup"><span data-stu-id="53bbd-215">Switching between environments</span></span>

<span data-ttu-id="53bbd-216">若要編譯不同環境之間切換，請修改**ASPNETCORE_ENVIRONMENT**環境變數的值。</span><span class="sxs-lookup"><span data-stu-id="53bbd-216">To switch between compiling for different environments, modify the **ASPNETCORE_ENVIRONMENT** environment variable's value.</span></span>

1.  <span data-ttu-id="53bbd-217">在**工作執行器總管**，確認**min**工作已設定為執行**之前建置**。</span><span class="sxs-lookup"><span data-stu-id="53bbd-217">In **Task Runner Explorer**, verify that the **min** task has been set to run **Before Build**.</span></span>

2.  <span data-ttu-id="53bbd-218">在**方案總管 中**，以滑鼠右鍵按一下專案名稱，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="53bbd-218">In **Solution Explorer**, right-click the project name and select **Properties**.</span></span>

    <span data-ttu-id="53bbd-219">Web 應用程式的屬性工作表會顯示。</span><span class="sxs-lookup"><span data-stu-id="53bbd-219">The property sheet for the Web app is displayed.</span></span>

3.  <span data-ttu-id="53bbd-220">按一下 [偵錯] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="53bbd-220">Click the **Debug** tab.</span></span>

4.  <span data-ttu-id="53bbd-221">值設定**主控： 環境**環境變數，以`Production`。</span><span class="sxs-lookup"><span data-stu-id="53bbd-221">Set the value of the **Hosting:Environment** environment variable to `Production`.</span></span>

5.  <span data-ttu-id="53bbd-222">按**F5**瀏覽器中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="53bbd-222">Press **F5** to run the application in a browser.</span></span>

6.  <span data-ttu-id="53bbd-223">在瀏覽器視窗中，以滑鼠右鍵按一下 [] 頁面上，選取**檢視原始檔**來檢視 HTML 頁面。</span><span class="sxs-lookup"><span data-stu-id="53bbd-223">In the browser window, right-click the page and select **View Source** to view the HTML for the page.</span></span>

    <span data-ttu-id="53bbd-224">請注意，樣式表連結指向縮短 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="53bbd-224">Notice that the stylesheet links point to the minified CSS files.</span></span>

7.  <span data-ttu-id="53bbd-225">關閉瀏覽器以停止 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="53bbd-225">Close the browser to stop the Web app.</span></span>

8.  <span data-ttu-id="53bbd-226">在 Visual Studio 中，返回 Web 應用程式的屬性工作表，並變更**主控： 環境**環境變數傳回到`Development`。</span><span class="sxs-lookup"><span data-stu-id="53bbd-226">In Visual Studio, return to the property sheet for the Web app and change the **Hosting:Environment** environment variable back to `Development`.</span></span>

9.  <span data-ttu-id="53bbd-227">按**F5**再次在瀏覽器中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="53bbd-227">Press **F5** to run the application in a browser again.</span></span>

10. <span data-ttu-id="53bbd-228">在瀏覽器視窗中，以滑鼠右鍵按一下 [] 頁面上，選取**檢視原始檔**以查看頁面的 HTML。</span><span class="sxs-lookup"><span data-stu-id="53bbd-228">In the browser window, right-click the page and select **View Source** to see the HTML for the page.</span></span>

    <span data-ttu-id="53bbd-229">請注意，樣式表連結指向 unminified CSS 檔案的版本。</span><span class="sxs-lookup"><span data-stu-id="53bbd-229">Notice that the stylesheet links point to the unminified versions of the CSS files.</span></span>

<span data-ttu-id="53bbd-230">如需 ASP.NET Core 的環境相關的資訊，請參閱[使用多個環境](../fundamentals/environments.md)。</span><span class="sxs-lookup"><span data-stu-id="53bbd-230">For more information related to environments in ASP.NET Core, see [Use multiple environments](../fundamentals/environments.md).</span></span>

## <a name="task-and-module-details"></a><span data-ttu-id="53bbd-231">工作和模組的詳細資料</span><span class="sxs-lookup"><span data-stu-id="53bbd-231">Task and module details</span></span>

<span data-ttu-id="53bbd-232">Gulp 工作已註冊的函式名稱。</span><span class="sxs-lookup"><span data-stu-id="53bbd-232">A Gulp task is registered with a function name.</span></span> <span data-ttu-id="53bbd-233">如果目前的工作之前，必須執行其他工作，您可以指定相依性。</span><span class="sxs-lookup"><span data-stu-id="53bbd-233">You can specify dependencies if other tasks must run before the current task.</span></span> <span data-ttu-id="53bbd-234">其他函式可讓您執行和觀賞 Gulp 的工作，以及將來源設定 (*src*) 和目的地 (*目的地*) 正在修改的檔案。</span><span class="sxs-lookup"><span data-stu-id="53bbd-234">Additional functions allow you to run and watch the Gulp tasks, as well as set the source (*src*) and destination (*dest*) of the files being modified.</span></span> <span data-ttu-id="53bbd-235">以下是主要的 Gulp API 函式：</span><span class="sxs-lookup"><span data-stu-id="53bbd-235">The following are the primary Gulp API functions:</span></span>

|<span data-ttu-id="53bbd-236">Gulp 函式</span><span class="sxs-lookup"><span data-stu-id="53bbd-236">Gulp Function</span></span>|<span data-ttu-id="53bbd-237">語法</span><span class="sxs-lookup"><span data-stu-id="53bbd-237">Syntax</span></span>|<span data-ttu-id="53bbd-238">描述</span><span class="sxs-lookup"><span data-stu-id="53bbd-238">Description</span></span>|
|---   |--- |--- |
|<span data-ttu-id="53bbd-239">task</span><span class="sxs-lookup"><span data-stu-id="53bbd-239">task</span></span>  |`gulp.task(name[, deps], fn) { }`|<span data-ttu-id="53bbd-240">`task`函式建立的工作。</span><span class="sxs-lookup"><span data-stu-id="53bbd-240">The `task` function creates a task.</span></span> <span data-ttu-id="53bbd-241">`name`參數會定義工作的名稱。</span><span class="sxs-lookup"><span data-stu-id="53bbd-241">The `name` parameter defines the name of the task.</span></span> <span data-ttu-id="53bbd-242">`deps`參數包含才可以執行此工作已完成工作的陣列。</span><span class="sxs-lookup"><span data-stu-id="53bbd-242">The `deps` parameter contains an array of tasks to be completed before this task runs.</span></span> <span data-ttu-id="53bbd-243">`fn`參數所代表的回呼函式會執行工作的作業。</span><span class="sxs-lookup"><span data-stu-id="53bbd-243">The `fn` parameter represents a callback function which performs the operations of the task.</span></span>|
|<span data-ttu-id="53bbd-244">監看式</span><span class="sxs-lookup"><span data-stu-id="53bbd-244">watch</span></span> |`gulp.watch(glob [, opts], tasks) { }`|<span data-ttu-id="53bbd-245">`watch`函式監視檔案和執行工作，當檔案變更時。</span><span class="sxs-lookup"><span data-stu-id="53bbd-245">The `watch` function monitors files and runs tasks when a file change occurs.</span></span> <span data-ttu-id="53bbd-246">`glob`參數是`string`或`array`，決定要監看的檔案。</span><span class="sxs-lookup"><span data-stu-id="53bbd-246">The `glob` parameter is a `string` or `array` that determines which files to watch.</span></span> <span data-ttu-id="53bbd-247">`opts`參數提供額外監控選項的檔案。</span><span class="sxs-lookup"><span data-stu-id="53bbd-247">The `opts` parameter provides additional file watching options.</span></span>|
|<span data-ttu-id="53bbd-248">src</span><span class="sxs-lookup"><span data-stu-id="53bbd-248">src</span></span>   |`gulp.src(globs[, options]) { }`|<span data-ttu-id="53bbd-249">`src`函式提供符合 glob 值的檔案。</span><span class="sxs-lookup"><span data-stu-id="53bbd-249">The `src` function provides files that match the glob value(s).</span></span> <span data-ttu-id="53bbd-250">`glob`參數是`string`或`array`，決定要讀取的檔案。</span><span class="sxs-lookup"><span data-stu-id="53bbd-250">The `glob` parameter is a `string` or `array` that determines which files to read.</span></span> <span data-ttu-id="53bbd-251">`options`參數提供的其他檔案選項。</span><span class="sxs-lookup"><span data-stu-id="53bbd-251">The `options` parameter provides additional file options.</span></span>|
|<span data-ttu-id="53bbd-252">目的地</span><span class="sxs-lookup"><span data-stu-id="53bbd-252">dest</span></span>  |`gulp.dest(path[, options]) { }`|<span data-ttu-id="53bbd-253">`dest`函式會定義可以寫入檔案的位置。</span><span class="sxs-lookup"><span data-stu-id="53bbd-253">The `dest` function defines a location to which files can be written.</span></span> <span data-ttu-id="53bbd-254">`path`參數是字串或函式，判斷目的地資料夾。</span><span class="sxs-lookup"><span data-stu-id="53bbd-254">The `path` parameter is a string or function that determines the destination folder.</span></span> <span data-ttu-id="53bbd-255">`options`參數是物件，指定輸出資料夾的選項。</span><span class="sxs-lookup"><span data-stu-id="53bbd-255">The `options` parameter is an object that specifies output folder options.</span></span>|

<span data-ttu-id="53bbd-256">其他的 Gulp 應用程式開發介面的參考資訊，請參閱[Gulp 文件 API](https://github.com/gulpjs/gulp/blob/master/docs/API.md)。</span><span class="sxs-lookup"><span data-stu-id="53bbd-256">For additional Gulp API reference information, see [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span></span>

## <a name="gulp-recipes"></a><span data-ttu-id="53bbd-257">Gulp 配方</span><span class="sxs-lookup"><span data-stu-id="53bbd-257">Gulp recipes</span></span>

<span data-ttu-id="53bbd-258">Gulp 社群也提供 Gulp[配方](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md)。</span><span class="sxs-lookup"><span data-stu-id="53bbd-258">The Gulp community provides Gulp [Recipes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span></span> <span data-ttu-id="53bbd-259">這些配方組成 Gulp 工作，以解決常見的案例。</span><span class="sxs-lookup"><span data-stu-id="53bbd-259">These recipes consist of Gulp tasks to address common scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="53bbd-260">其他資源</span><span class="sxs-lookup"><span data-stu-id="53bbd-260">Additional resources</span></span>

* [<span data-ttu-id="53bbd-261">Gulp 文件</span><span class="sxs-lookup"><span data-stu-id="53bbd-261">Gulp documentation</span></span>](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [<span data-ttu-id="53bbd-262">統合及縮製中 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="53bbd-262">Bundling and minification in ASP.NET Core</span></span>](bundling-and-minification.md)
* [<span data-ttu-id="53bbd-263">用於 ASP.NET Core Grunt</span><span class="sxs-lookup"><span data-stu-id="53bbd-263">Use Grunt in ASP.NET Core</span></span>](using-grunt.md)
