---
title: Versões com suporte no Banco de Dados do Azure para PostgreSQL
description: Descreve as versões com suporte no Banco de Dados do Azure para PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 11/12/2018
ms.openlocfilehash: a03ead5e577b261b99e635addf6f9a98d8f3cadd
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52619684"
---
# <a name="supported-postgresql-database-versions"></a>Versões suportadas do banco de dados do PostgreSQL
A Microsoft pretende suportar versões n-2 do mecanismo PostgreSQL no serviço Banco de Dados do Azure para PostgreSQL. As versões seriam a versão principal atual no Azure (n) e as duas versões principais anteriores (-2).

No momento, o Banco de Dados do Azure para PostgreSQL oferece suporte às seguintes versões:

## <a name="postgresql-version-105"></a>PostgreSQL versão 10.5
Consulte a [documentação do PostgreSQL](https://www.postgresql.org/docs/10/static/release-10-5.html) para saber mais sobre aperfeiçoamentos e correções nesta versão secundária.

## <a name="postgresql-version-9610"></a>PostgreSQL versão 9.6.10
Consulte a [documentação do PostgreSQL](https://www.postgresql.org/docs/9.6/static/release-9-6-10.html) para saber mais sobre aperfeiçoamentos e correções nesta versão secundária.

## <a name="postgresql-version-9514"></a>PostgreSQL Versão 9.5.14
Consulte a [documentação do PostgreSQL](https://www.postgresql.org/docs/9.5/static/release-9-5-14.html) para saber mais sobre aperfeiçoamentos e correções nesta versão secundária.

## <a name="managing-updates-and-upgrades"></a>Gerenciar atualizações e upgrades
O Banco de Dados do Azure para PostgreSQL gerencia automaticamente os patches de versões secundárias. Atualmente, upgrade da versão principal não tem suporte. Por exemplo, não há suporte para atualização do PostgreSQL 9.5 para PostgreSQL 9.6. Se você quiser atualizar para a próxima versão principal, crie um banco de dados [dump e restaure](./howto-migrate-using-dump-and-restore.md) para um servidor que foi criado com a nova versão do mecanismo.

## <a name="next-steps"></a>Próximas etapas
Para obter mais informações sobre o suporte a extensões do PostgreSQL diferentes, veja [Extensões do PostgreSQL](concepts-extensions.md).
