---
title: 从 .NET for Apache Spark 应用程序调用 Java UDF
description: 了解如何从 .NET for Apache Spark 应用程序调用 Java UDF。
ms.author: nidutta
author: Niharikadutta
ms.date: 10/09/2020
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: f85ed5585c8daef0ca9a21275b8de71b2eead360
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875481"
---
# <a name="call-a-java-udf-from-your-net-for-apache-spark-application"></a>从 .NET for Apache Spark 应用程序调用 Java UDF

本文介绍如何从 [.NET for Apache Spark](https://github.com/dotnet/spark) 应用程序调用 Java 用户定义的函数 (UDF)。

1. 如何定义 Java UDF 并将其编译成 jar - 如果已在 jar 文件中定义了 UDF，则不需要执行此步骤。 在这种情况下，你只需要包含包的 UDF 函数的全名。
2. 在 .NET for Apache Spark 应用程序中注册并调用 Java UDF。

## <a name="define-and-compile-your-java-udfs"></a>定义和编译 Java UDF

1. 创建 Maven 或 SBT 项目，并将以下依赖项添加到项目配置文件中：
    1. `org.apache.spark.spark-core_2.11.<version>`
    2. `org.apache.spark.spark-sql_2.11.<version>`
2. 定义 Java UDF，具体方法是实现[相关接口](https://github.com/apache/spark/blob/master/sql/core/src/main/java/org/apache/spark/sql/api/java/UDF1.java)（根据 UDF 的签名）并导入相关的包，如下面的简单示例所示

    ```java
    package com.ScalaUdf.app; // Name of package where UDF is defined
    import org.apache.spark.sql.api.java.UDF1; // UDF interface to implement

    public class JavaUdf implements UDF1<Integer, Integer> { // Name of the Java UDF
        private static final int serialVersionUID = 1;
        @Override
        public Integer call(Integer num) throws Exception { // Define logic of UDF
            return (num + 5);
        }
    }
    ```

3. 编译并打包项目，以创建可执行的 jar，如 `UdfApp-0.0.1.jar`。

## <a name="register-and-call-java-udfs-in-net-for-apache-spark"></a>在 .NET for Apache Spark 中注册和调用 Java UDF

1. 使用 [`RegisterJava`](https://github.com/dotnet/spark/blob/8dcdcdc7c60d5f42cba5a90f1346d854ab5bf7bb/src/csharp/Microsoft.Spark/Sql/UDFRegistration.cs#L424) API 将 Java UDF 注册到 Spark SQL 中。
2. 使用 [`CreateOrReplaceTempView`](https://github.com/dotnet/spark/blob/main/src/csharp/Microsoft.Spark/Sql/DataFrame.cs#L982) 函数，注册要将 UDF 作为 SQL 表调用的 `DataFrame`。
3. 使用 `SparkSession.Sql` 调用使用 Spark SQL 的表视图上的 UDF。
说明以上步骤的基本示例：

    ```csharp
    class Program
    {
        static void Main()
        {
            SparkSession spark = SparkSession
                .Builder()
                .AppName("Scala/Java UDFs from .NET for Apache Spark")
                .GetOrCreate();
            spark.Udf().RegisterJava<int>("udfAdd5", "com.ScalaUdf.app.JavaUdf"); // Register your Java UDF as 'udfAdd5'
            DataFrame df = spark.CreateDataFrame(new int[] { 2, 5 });
            df.CreateOrReplaceTempView("numbersData"); // Create an SQL table from the DataFrame `df`
            DataFrame dfUdf = spark.Sql("SELECT udfAdd5(_1) As Result FROM numbersData"); // Call the registered UDF on the table
            dfUdf.Show();
            spark.Stop();
        }
    }
    ```

4. 通过 `--jars` 选项传递先前编译的 Java UDF jar，以使用 `spark-submit` 提交此应用程序：

    ```bash
    spark-submit --master local --jars UdfApp-0.0.1.jar --class org.apache.spark.deploy.dotnet.DotnetRunner microsoft-spark-2-4_2.11-1.0.0.jar InterRuntimeUDFs.exe
    ```

    生成的 `dfUdf` DataFrame 将数字 5 添加到 `JavaUdf` 定义的输入列的每一行：

    ```text
    +-------+
    | Result|
    +-------+
    |      7|
    |     10|
    +-------+
    ```

## <a name="call-net-udf-from-scala-or-python-in-apache-spark"></a>在 Apache Spark 中从 Scala 或 Python 调用 .NET UDF

你还可以使用 [sparkdotnetudf 开源工具](https://github.com/imback82/sparkdotnetudf)从用 Scala 或 Python 编写的 Apache Spark 应用程序注册和调用 C# UDF。
