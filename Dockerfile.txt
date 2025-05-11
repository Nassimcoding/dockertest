# 使用 .NET 8 SDK 作為建置階段
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /app

# 複製整個專案檔到容器中
COPY . .

# 使用 .sln 檔案建置並發佈
RUN dotnet publish dockertest.sln -c Release -o /app/out

# 使用 ASP.NET 執行階段來執行應用程式
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime
WORKDIR /app

# 複製建置結果
COPY --from=build /app/out .

# 執行專案主 DLL
ENTRYPOINT ["dotnet", "dockertest.dll"]