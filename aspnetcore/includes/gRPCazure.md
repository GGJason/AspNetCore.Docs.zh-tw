> [!WARNING]
> Azure App Service 或 IIS 上目前不支援[ASP.NET Core gRPC](xref:grpc/index) 。 Http.Sys 的 HTTP/2 執行不支援 gRPC 所依賴的 HTTP 回應尾端標頭。 如需詳細資訊，請參閱 [此 GitHub 問題](https://github.com/dotnet/AspNetCore/issues/9020)。
