<properties
    pageTitle="Azure přečtěte následující dokumentaci pro státní správu | Microsoft Azure"
    description="To poskytuje srovnání funkcí a pokyny pro na vývoj aplikací pro státní správu Azure"
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/18/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-databases"></a>Azure Government databází

##  <a name="sql-database"></a>Databáze SQL

Další informace o konfigurace viditelnost metadat a osvědčené postupy ochrany v nápovědě k<a href="https://msdn.microsoft.com/en-us/library/bb510589.aspx"> Centrum zabezpečení aplikace Microsoft pro databázový stroj SQL</a> a [Veřejné dokumentace databáze SQL Azure](https://azure.microsoft.com/documentation/services/sql-database/) .

### <a name="variations"></a>Varianty

V Azure Government obecně neexistuje V12 databáze SQL.

Adresa pro SQL Azure servery v Azure Government se liší:

Typ služby|Azure veřejné|Azure Government
---|---|---
Databáze SQL|*. database.windows.net|*. database.usgovcloudapi.net

### <a name="considerations"></a>Co byste měli zvážit

Tyto informace identifikuje hranici Azure Government pro Azure SQL:

| Upraveno/řízená data povolené. | Upraveno/řízená data není povoleno |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Všechna data uložená a zpracovávají v Microsoft Azure SQL mohou obsahovat upraveno Azure Government data. Je nutné použít databázové nástroje pro přenos dat Azure Government upraveno dat. | Azure SQL metadat není povoleno má být řízené exportovat data. Tato metadata zahrnuje všechna data konfigurace zadané při vytváření a udržování produkt úložiště.  Nezadáte upraveno/řízená data do následujících polí: databáze název, název předplatného, skupiny zdrojů, název serveru, přihlášení správce serveru, nasazení jménech názvy zdrojů, značky zdroje

## <a name="azure-redis-cache"></a>Azure Redis mezipaměti

Informace o této službě a jak ji používat dokumentaci [Azure Redis mezipaměti veřejné](https://azure.microsoft.com/documentation/services/redis-cache/).

### <a name="variations"></a>Varianty

Adresy URL pro přístup k a Správa mezipaměti Redis Azure v Azure Government se liší:

Typ služby|Azure veřejné|Azure Government
---|---|---
Koncový bod mezipaměti|*. redis.cache.windows.net|*. redis.cache.usgovcloudapi.net
Azure portálu|https://Portal.Azure.com|https://Portal.Azure.us

>[AZURE.NOTE] Všechny skripty a kód je potřeba počítat prostředí a odpovídající koncové body. Další informace najdete v tématu [jak připojit ke cloudové Government Azure](../redis-cache/cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud).


### <a name="considerations"></a>Co byste měli zvážit

Tyto informace identifikuje hranici Azure Government Azure Redis mezipaměti:

| Upraveno/řízená data povolené. | Upraveno/řízená data není povoleno |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Všechna data uložená a zpracovávají v mezipaměti Redis Azure mohou obsahovat upraveno Azure Government data. | Azure metadata Redis mezipaměti není povoleno má být řízené exportovat data. Nezadáte upraveno/řízená data do následujících polí: mezipaměti jméno, jméno VNET, název předplatného, skupiny zdrojů, značky zdroje, Redis vlastnosti.  

##  <a name="next-steps"></a>Další kroky

Pro přihlášení k odběru doplňující informace a aktualizace <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government blogu.</a>
