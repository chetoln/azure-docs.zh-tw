---
title: 在 Azure HDInsight 中使用互動式 Spark Shell
description: 互動式 Spark Shell 會針對執行 Spark 命令 (一次一個) 及查看結果提供「讀取-執行-列印」的流程。
ms.service: hdinsight
author: maxluk
ms.author: maxluk
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/09/2018
ms.openlocfilehash: 9044ed3ad9cf9ffa2f54d130bb50b37df121b86f
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2019
ms.locfileid: "64696810"
---
# <a name="run-apache-spark-from-the-spark-shell"></a>從 Spark Shell 執行 Apache Spark

互動式 [Apache Spark](https://spark.apache.org/) Shell 會提供 REPL (「讀取、求值、輸出」迴圈) 環境，一次執行一個 Spark 命令並查看結果。 此流程適用於開發和偵錯。 Spark 會為每個支援的語言提供一個殼層：Scala、Python 和 R。

## <a name="get-to-an-apache-spark-shell-with-ssh"></a>透過 SSH 使用 Apache Spark Shell

使用 SSH 連線到叢集的主要前端節點，來存取 HDInsight 上的 Apache Spark Shell：

     ssh <sshusername>@<clustername>-ssh.azurehdinsight.net

您可以從 Azure 入口網站，取得您叢集的完整 SSH 命令：

1. 登入 [Azure 入口網站](https://portal.azure.com)。
2. 瀏覽至您 HDInsight Spark 叢集的窗格。
3. 選取安全殼層 (SSH)。

    ![Azure 入口網站中的 HDInsight 窗格](./media/apache-spark-shell/hdinsight-spark-blade.png)

4. 複製顯示的 SSH 命令，並在您的終端機中執行該命令。

    ![Azure 入口網站中的 HDInsight SSH 窗格](./media/apache-spark-shell/hdinsight-spark-ssh-blade.png)

如需使用 SSH 連線到 HDInsight 的詳細資訊，請參閱[搭配 HDInsight 使用 SSH](../hdinsight-hadoop-linux-use-ssh-unix.md)。

## <a name="run-an-apache-spark-shell"></a>執行 Apache Spark Shell

Spark 會為 Scala (spark-shell)、Python (pyspark) 和 R (sparkR) 提供殼層。 在位於 HDInsight 叢集前端節點的 SSH 工作階段中，請輸入下列命令：

    ./bin/spark-shell
    ./bin/pyspark
    ./bin/sparkR

現在您可以使用適當的語言輸入 Spark 命令。

## <a name="sparksession-and-sparkcontext-instances"></a>SparkSession 和 SparkContext 執行個體

依預設，執行 Spark Shell 時，SparkSession 和 SparkContext 的執行個體會自動具現化。

若要存取 SparkSession 執行個體，請輸入 `spark`。 若要存取 SparkContext 執行個體，請輸入 `sc`。

## <a name="important-shell-parameters"></a>重要的殼層參數

Spark Shell 命令 (`spark-shell`、`pyspark`或 `sparkR`) 支援許多命令列參數。 若要查看完整的參數清單，請使用參數 `--help` 啟動 Spark Shell。 請注意，某些參數可能只適用於 `spark-submit` (由 Spark Shell 包裝)。

| 參數 | description | 範例 |
| --- | --- | --- |
| --master MASTER_URL | 指定主要 URL。 在 HDInsight 中，此值一律為 `yarn`。 | `--master yarn`|
| --jars JAR_LIST | 本機 Jar 的逗號分隔清單，用來包含在驅動程式和執行程式 Classpath 中。 在 HDInsight 中，此清單是由 Azure 儲存體或 Data Lake Storage 中的預設檔案系統路徑組成。 | `--jars /path/to/examples.jar` |
| --packages MAVEN_COORDS | Jar 的 Maven 座標逗號分隔清單，用來包含在驅動程式和執行程式 Classpath 中。 依序搜尋本機 Maven 存放庫、Maven 中心和所有以 `--repositories` 指定的遠端存放庫。 座標格式為 *groupId*:*artifactId*:*version*。 | `--packages "com.microsoft.azure:azure-eventhubs:0.14.0"`|
| --py-files LIST | (僅適用於 Python) 要放在 PYTHONPATH 中的 .zip、.egg 或 .py 檔案的逗號分隔清單。 | `--pyfiles "samples.py"` |

## <a name="next-steps"></a>後續步驟

- 如需概觀，請參閱 [Azure HDInsight 上的 Apache Spark 簡介](apache-spark-overview.md)。
- 請參閱[在 Azure HDInsight 中建立 Apache Spark 叢集](apache-spark-jupyter-spark-sql.md)，搭配使用 Spark 叢集和 SparkSQL。
- 請參閱[什麼是 Apache Spark 結構化串流](apache-spark-streaming-overview.md)，以使用 Spark 撰寫可處理串流資料的應用程式。
