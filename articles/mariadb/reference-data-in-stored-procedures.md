---
title: Procedimentos armazenados de Replicação de Dados do Banco de Dados do Azure para MariaDB
description: Este artigo apresenta todos os procedimentos armazenados usados para Replicação nos dados.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: reference
ms.date: 09/24/2018
ms.openlocfilehash: 87c6fa783964c019841810ec38f342a5a44c0ee3
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46959080"
---
# <a name="azure-database-for-mariadb-data-in-replication-stored-procedures"></a>Procedimentos armazenados de Replicação de Dados do Banco de Dados do Azure para MariaDB

A Replicação de Dados permite sincronizar dados de um servidor do MariaDB executado localmente em máquinas virtuais ou em serviços de banco de dados hospedados por outros provedores de nuvem no serviço do Banco de Dados do Azure para MariaDB.

Os procedimentos armazenados a seguir são usados para configurar ou remover a Replicação de Dados entre um mestre e uma réplica.

|**Nome do procedimento armazenado**|**Parâmetros de entrada**|**Parâmetros de saída**|**Observação de uso**|
|-----|-----|-----|-----|
|*mysql.az_replication_change_master*|master_host<br/>master_user<br/>master_password<br/>master_port<br/>master_log_file<br/>master_log_pos<br/>master_ssl_ca|N/D|Para transferir dados com o modo SSL, passe o contexto do Certificado de Autoridade de Certificação para o parâmetro master_ssl_ca. </br><br>Para transferir dados sem SSL, passe uma cadeia de caracteres vazia para o parâmetro master_ssl_ca.|
|*mysql.az_replication _start*|N/D|N/D|Inicia a replicação.|
|*mysql.az_replication _stop*|N/D|N/D|Para a replicação.|
|*mysql.az_replication _remove_master*|N/D|N/D|Remove o relacionamento de replicação entre o mestre e a réplica.|
|*mysql.az_replication_skip_counter*|N/D|N/D|Ignora um erro de replicação.|

Para configurar a Replicação de Dados entre um mestre e uma réplica no Banco de Dados do Azure para MariaDB, confira [como configurar a Replicação de Dados](howto-data-in-replication.md).