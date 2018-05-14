<a name="cli"></a>
## <a name="perform-initial-migration"></a>執行初始移轉

從命令列中，執行下列 .NET Core CLI 命令：

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

`dotnet ef migrations add InitialCreate` 命令會產生程式碼來建立初始資料庫結構描述。 結構描述以 `DbContext`(位在 *Models/MovieContext.cs* 檔案內) 中指定的模型為基礎。 `Initial` 引數用來命名移轉。 您可以使用任何名稱，但依照慣例，會選擇描述移轉的名稱。 如需詳細資訊，請參閱[移轉簡介](xref:data/ef-mvc/migrations#introduction-to-migrations)。

`dotnet ef database update` 命令會執行 *Migrations/*\<時間戳記>_InitialCreate.cs 檔案中的 `Up` 方法，以建立資料庫。
