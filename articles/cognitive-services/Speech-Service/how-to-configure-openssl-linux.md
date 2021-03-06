---
title: 如何設定適用於 Linux 的 OpenSSL
titleSuffix: Azure Cognitive Services
description: 瞭解如何設定適用于 Linux 的 OpenSSL。
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/16/2020
ms.author: jhakulin
ms.openlocfilehash: ff8772f7c3c3213c010b0bdbd0d0aa8897404bac
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/11/2020
ms.locfileid: "77119991"
---
# <a name="configure-openssl-for-linux"></a>設定適用於 Linux 的 OpenSSL

在1.9.0 之前使用任何語音 SDK 版本時， [OpenSSL](https://www.openssl.org)會以動態方式設定為主機系統版本。 在較新版本的語音 SDK 中，OpenSSL （版本[1.1.1 b](https://mta.openssl.org/pipermail/openssl-announce/2019-February/000147.html)）會以靜態方式連結至語音 sdk 的核心程式庫。

若要確保連線能力，請確認您的系統中已安裝 OpenSSL 憑證。 執行命令：
```bash
openssl version -d
```

以 Ubuntu/Debian 為基礎的系統上的輸出應該是：
```
OPENSSLDIR: "/usr/lib/ssl"
```

檢查 OPENSSLDIR 下是否有 `certs` 子目錄。 在上述範例中，它會是 `/usr/lib/ssl/certs`。

* 如果有 `/usr/lib/ssl/certs`，而且它包含許多個別的憑證檔案（具有 `.crt` 或 `.pem` 延伸模組），則不需要進一步的動作。

* 如果 OPENSSLDIR 不是 `/usr/lib/ssl` 和/或有單一憑證組合檔案，而不是多個個別檔案，您就必須設定適當的 SSL 環境變數，以指出可以找到憑證的位置。

## <a name="examples"></a>範例

- OPENSSLDIR 為 `/opt/ssl`。 有 `certs` 子目錄具有許多 `.crt` 或 `.pem` 檔案。
在執行使用語音 SDK 的程式之前，請將環境變數 `SSL_CERT_DIR` 設定為指向 `/opt/ssl/certs`。 例如，
```bash
SSL_CERT_DIR=/opt/ssl/certs ./helloworld
```

- OPENSSLDIR 為 `/etc/pki/tls`。 有一個憑證配套檔案，例如 `ca-bundle.pem` 或 `ca-bundle.crt`。
在執行使用語音 SDK 的程式之前，請將環境變數 `SSL_CERT_FILE` 設定為指向 `/etc/pki/tls/ca-bundle.pem`。 例如，
```bash
SSL_CERT_FILE=/etc/pki/tls/ca-bundle.pem ./helloworld
```

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [關於語音 SDK](speech-sdk.md)
