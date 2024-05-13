如果您想通過 Homebrew 在 macOS 上安裝 MySQL 並配置它以允許來自特定 IP（如 '192.168.1.103'）的遠程連接，您可以按照以下步驟操作：

### 安裝 MySQL

1. **安裝 MySQL**：
   首先，如果您還沒有安裝 MySQL，可以使用 Homebrew 來安裝它。打開終端並執行以下命令：

   ```bash
   brew install mysql
   ```

2. **啟動 MySQL 服務**：
   安裝完畢後，使用 Homebrew 啟動 MySQL 服務：

   ```bash
   brew services start mysql
   ```

3. **設置初始密碼**（如果需要）：
   對於新安裝的 MySQL，您可能需要運行初始化腳本來設置 root 用戶的密碼：

   ```bash
   mysql_secure_installation
   ```

### 配置 MySQL 接受遠程連接

1. **編輯 MySQL 配置文件**：
   - 找到 MySQL 的配置文件 `my.cnf`。這個文件的位置可能因安裝方式和系統不同而異。您可以使用 `brew info mysql` 命令查看配置文件的位置。
   - 使用文本編輯器打開 `my.cnf`，例如：

     ```bash
     nano /usr/local/etc/my.cnf
     ```

   - 添加或更改 `bind-address` 配置行為：

     ```
     bind-address = 0.0.0.0
     ```

     這樣設置允許 MySQL 接受來自所有 IP 地址的連接。

2. **重啟 MySQL 服務**：
   配置更改後，重啟 MySQL 服務使更改生效：

   ```bash
   brew services restart mysql
   ```

### 設置用戶權限

1. **登錄到 MySQL**：
   使用終端登錄到 MySQL：

   ```bash
   mysql -u root -p
   ```

   輸入您的 root 密碼。

2. **創建或更新用戶以允許遠程連接**：
   - 如果您需要為特定 IP 創建一個新用戶：

     ```sql
     CREATE USER 'username'@'192.168.1.103' IDENTIFIED BY 'password';
     GRANT ALL PRIVILEGES ON *.* TO 'username'@'192.168.1.103' WITH GRANT OPTION;
     FLUSH PRIVILEGES;
     ```

   - 如果您想更新現有用戶的許可權：

     ```sql
     GRANT ALL PRIVILEGES ON *.* TO 'existing_user'@'192.168.1.103' IDENTIFIED BY 'password' WITH GRANT OPTION;
     FLUSH PRIVILEGES;
     ```

3. **退出 MySQL**：

   ```sql
   EXIT;
   ```

### 安全性和防火牆設置

- 確保您的 macOS 防火牆設置允許外部設備通過端口 3306（MySQL 的默認端口）連接。
- 出於安全考慮，僅為需要遠程訪問的用戶賦予必要的權限，並使用強密碼。
- 如果您的網絡環境允許，考慮限制能夠訪問 MySQL 服務器的 IP 地址範圍。