---
title: Azure Database for MySQL 驱动程序和管理工具兼容性
description: 本文說明 MySQL 驅動程式和管理工具與適用於 MySQL 的 Azure 資料庫之相容性。
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 03/19/2019
ms.openlocfilehash: 05f48145973777052590f8d10e1a2ce1fd22ec7a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "60525378"
---
# <a name="mysql-drivers-and-management-tools-compatible-with-azure-database-for-mysql"></a>MySQL 驅動程式和管理工具與適用於 MySQL 的 Azure 資料庫的相容性
本文說明驅動程式和管理工具與適用於 MySQL 的 Azure 資料庫之相容性。

## <a name="mysql-drivers"></a>MySQL 驅動程式
適用於 MySQL 的 Azure 資料庫使用世界上最熱門的 MySQL 資料庫社群版本。 因此，此版本與多種程式設計語言和驅動程式相容。 目標是支援三個最新版本的 MySQL 驅動程式，並繼續與開放原始碼社群作者一起努力，持續改善 MySQL 驅動程式的功能和可用性。 下表提供的驅動程式清單經過測試，證明與適用於 MySQL 5.6 和 5.7 的 Azure 資料庫相容：

| **驅動程式** | **連結** | **相容的版本** | **不相容的版本** | **注意事項** |
| :-------- | :------------------------ | :----------- | :---------------------- | :--------------------------------------- |
| PHP | https://secure.php.net/downloads.php | 5.5、5.6、7.x | 5.3 | 若要連接 PHP 7.0 與 SSL MySQLi，請在連接字串中加入 MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT。 <br> ```mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306, NULL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);```<br> PDO 組：```PDO::MYSQL_ATTR_SSL_VERIFY_SERVER_CERT``` 選項為 false。|
| .NET | [ GitHub 上的 MySqlConnector](https://github.com/mysql-net/MySqlConnector) \(英文\) <br> [來自 Nuget 的安裝套件](https://www.nuget.org/packages/MySqlConnector/) \(英文\) | 0.27 及更新版本 | 0.26.5 及更舊版本 | |
| MySQL Connector/NET | [MySQL Connector/NET](https://github.com/mysql/mysql-connector-net) | 8.0、7.0、6.10 |  | 編碼錯誤 (bug) 可能會導致在某些非 UTF8 Windows 系統上的連線失敗。 |
| Nodejs |  [GitHub 上的 MySQLjs](https://github.com/mysqljs/mysql/) \(英文\) <br> 來自 NPM 的安裝套件：<br> 從 NPM 執行 `npm install mysql` | 2.15 | 2.14.1 及更舊版本 | |
| GO | https://github.com/go-sql-driver/mysql/releases | 1.3、1.4 | 1.2 及更舊版本 | 在版本 1.3 的连接字符串中使用 `allowNativePasswords=true`。 版本 1.4 包含修补程序，不再需要 `allowNativePasswords=true`。 |
| Python | https://pypi.python.org/pypi/mysql-connector-python | 1.2.3、2.0、2.1、2.2 | 1.2.2 及更舊版本 | |
| Java | https://downloads.mariadb.org/connector-java/ | 2.1、2.0、1.6 | 1.5.5 及更舊版本 | |

## <a name="management-tools"></a>管理工具
相容性的優點也會延伸到資料庫管理工具。 只要資料庫操作在使用者權限的範圍內運作，您現有的工具應該就可以繼續搭配適用於 MySQL 的 Azure 資料庫使用。 下表列出的三個常用資料庫管理工具經過測試，證明與適用於 MySQL 5.6 和 5.7 的 Azure 資料庫相容：

|                                     | **MySQL Workbench 6.x 與更新版本** | **Navicat 12** | **PHPMyAdmin 4.x 與更新版本** |
| :---------------------------------- | :----------------------------- | :------------- | :-------------------------|
| 建立、更新、讀取、寫入、刪除 | X | X | X |
| SSL 連線 | X | X | X |
| SQL 查詢自動完成 | X | X |  |
| 匯入和匯出資料 | X | X | X |
| 匯出成多種格式 | X | X | X |
| 備份與還原 |  | X |  |
| 顯示伺服器參數 | X | X | X |
| 顯示用戶端連線 | X | X | X |

## <a name="next-steps"></a>後續步驟

- [針對適用於 MySQL 的 Azure 資料庫連線問題進行疑難排解](howto-troubleshoot-common-connection-issues.md)