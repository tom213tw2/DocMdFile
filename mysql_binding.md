當您遇到錯誤 "Host '192.168.1.103' is not allowed to connect to this MySQL server"，這意味著從 IP 地址 192.168.1.103 的試圖連線被 MySQL 服務器拒絕。這通常是由於 MySQL 服務器的設置未授權該 IP 地址的用戶訪問。要解決這個問題，您需要在 MySQL 服務器上創建或修改一個用戶帳戶，允許從該 IP 地址連線。

### 解決步驟

1. **登入 MySQL 服務器**
   首先，您需要使用具有管理權限的帳戶（如 root）登入 MySQL 服務器。

   ```bash
   mysql -u root -p
   ```

2. **檢查現有的用戶帳戶**
   確認是否已有可以從該 IP 地址連線的用戶帳戶。執行以下 SQL 查詢：

   ```sql
   SELECT user, host FROM mysql.user;
   ```

   這將列出所有用戶及其相應的許可主機。

3. **創建或修改用戶帳戶**
   根據查詢結果，您可能需要創建一個新用戶或修改現有用戶的許可。

   - **創建新用戶**（如果需要）：

     ```sql
     CREATE USER 'username'@'192.168.1.103' IDENTIFIED BY 'password';
     GRANT ALL PRIVILEGES ON database_name.* TO 'username'@'192.168.1.103';
     FLUSH PRIVILEGES;
     ```

   - **修改現有用戶**（如果用戶已存在但主機不正確）：

     ```sql
     GRANT ALL PRIVILEGES ON database_name.* TO 'username'@'192.168.1.103' IDENTIFIED BY 'password';
     FLUSH PRIVILEGES;
     ```

   替換 `username`、`password` 和 `database_name` 為實際的用戶名、密碼和數據庫名。

4. **重新啟動 MySQL**（可選）
   在某些情況下，您可能需要重新啟動 MySQL 服務器以使更改生效。

   ```bash
   sudo systemctl restart mysql
   ```

5. **從客戶端嘗試再次連線**
   在完成上述步驟後，從 IP 地址 192.168.1.103 嘗試再次連線到 MySQL 服務器。

### 注意事項

- 確保 MySQL 服務器的防火牆設置允許從外部 IP 地址連線。
- 如果您的 MySQL 服務器設置了 `bind-address` 為 `127.0.0.1`（只允許本地連線），您需要將其更改為 `0.0.0.0` 或注釋該行以允許遠程連線。
- 出於安全考慮，只給予必要的許可權，並為不同的應用或服務創建獨立的用戶帳戶。