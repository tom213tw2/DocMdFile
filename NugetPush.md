# Nuget部署步驟

* 該類別庫專案檔(csproj) 需加以下步驟
```
<PropertyGroup>
        <TargetFramework>net6.0</TargetFramework>
        <!-- 確保每個建置配置如 Debug 或 Release 都包含以下行 -->
        <GenerateDocumentationFile>true</GenerateDocumentationFile>
        <!-- 可選：指定生成的 XML 文件名稱和位置 -->
        <DocumentationFile>$(OutputPath)$(AssemblyName).xml</DocumentationFile>
</PropertyGroup>
```

* 對該專案先發佈，確認要執行的路徑
EX:
```
bin\Release\net6.0\publish\*.dll
```

* 透過cmd 到該路徑 cd 專案檔的路徑
```
cd {專案檔路徑}
```

## 打包專案檔 .nuspec
```
 nuget spec {專案導.nuspec}
```

# `.nuspec` 檔案說明

`.nuspec` 檔案包含以下主要元素：

## `<package>`
根元素，所有其他元素都應該包含在此元素內。

## `<metadata>`
包含套件的核心資訊，必須的元素。

### `<id>`
**必需**。套件的唯一識別碼，通常與套件名稱相同。

### `<version>`
**必需**。套件的版本號。

### `<description>`
**必需**。描述套件的詳細資訊。

### `<authors>`
**必需**。套件作者的名字，多個作者以逗號分隔。

### `<owners>`
套件擁有者的名字，多個擁有者以逗號分隔。

### `<license>`
指定套件授權的方式。可以直接放置授權表達式，如 `Apache-2.0`，或者是一個授權檔案的路徑。

### `<projectUrl>`
套件專案的 URL。

### `<icon>`
一個圖標檔案的路徑，用於顯示套件的圖標。

### `<requireLicenseAcceptance>`
如果設置為 `true`，則使用者在安裝套件時需要接受授權條款。

### `<releaseNotes>`
包含此版本的套件的更新和變更說明。

### `<copyright>`
套件的版權聲明。

### `<tags>`
用於搜尋和分類套件的標籤。

### `<dependencies>`
列出此套件依賴的其他套件。

## `<files>`
用於指定包含在套件中的檔案和目錄。

### `<file src="源路徑" target="目標路徑" />`
指定一個檔案或多個檔案（可以使用萬用字元），從源路徑包到目標路徑。

## 示例

以下是一個 `.nuspec` 檔案的示例：

```xml
<?xml version="1.0"?>
<package>
    <metadata>
        <id>ExamplePackage</id>
        <version>1.0.0</version>
        <authors>John Doe</authors>
        <owners>John Doe</owners>
        <license type="expression">MIT</license>
        <projectUrl>http://example.com/</projectUrl>
        <icon>icon.png</icon>
        <requireLicenseAcceptance>false</requireLicenseAcceptance>
        <description>This is an example package.</description>
        <releaseNotes>Initial release</releaseNotes>
        <tags>example demo</tags>
    </metadata>
    <files>
        <file src="lib/net40/*.dll" target="lib/net40" />
        <file src="content/*.txt" target="content" />
    </files>
</package>
```

### 在File 標籤內要加入.xml擋，原因是這樣註解才可以正確顯示出來
```xml
<files>
    <file src="bin\Release\net6.0\publish\*.dll" target="lib\net6.0" />
    <file src="bin\Release\net6.0\publish\*.pdb" target="lib\net6.0" />
    <file src="bin\Release\net6.0\publish\*.xml" target="lib\net6.0" />    
</files>
```

## dependencies 要對該專案有引用的第三方 也要設定，這樣在還原才可以抓到對的資訊，需加在**metadata**中 
```xml
<dependencies>
	<group targetFramework="net6.0">
		<dependency id="Microsoft.AspNetCore.Mvc" version="2.2.0" />
		<dependency id="Microsoft.Extensions.Configuration" version="8.0.0" />
		<dependency id="Newtonsoft.Json" version="13.0.3" />
		<dependency id="StackExchange.Redis" version="2.7.33" />
		<!-- 其他需要的依賴 -->
	</group>
</dependencies>
```

### id 和version 可看專案檔中的PackageReference
```xml
<ItemGroup>
      <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="2.2.0" />
      <PackageReference Include="Microsoft.Extensions.Configuration" Version="8.0.0" />
      <PackageReference Include="Newtonsoft.Json" Version="13.0.3" />
      <PackageReference Include="StackExchange.Redis" Version="2.7.33" />
</ItemGroup>
```

* 打包成套件檔nupkg
```
nuget pack {nuspec路徑}     
```

* 發佈到nuget server ,需要cd到該路徑  
**可在機器上找到該key ```root/.aspnet/DataProtection-Keys```**
```
nuget push your_package.nupkg -ApiKey {your_api_key} -Source {Your service index}

```  
* 不帶Key的方式，這樣比較不安全
```
nuget push your_package.nupkg  -Source {Your service index}
```

