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
    ms.date="09/30/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-data-and-storage"></a>Azure Government dat a úložiště

##  <a name="azure-storage"></a>Azure úložiště

Informace o této službě a jak ji používat dokumentaci [úložišti Azure veřejné](https://azure.microsoft.com/documentation/services/storage/).

### <a name="variations"></a>Varianty

Adresy URL pro účty úložiště v Azure Government se liší:

Typ služby|Azure veřejné|Azure Government
---|---|---
Úložiště objektů BLOB|*. blob.core.windows.net|*. blob.core.usgovcloudapi.net
Úložiště fronty|*. queue.core.windows.net|*. queue.core.usgovcloudapi.net
Úložiště tabulek|*. table.core.windows.net| *. table.core.usgovcloudapi.net

>[AZURE.NOTE] Všechny skripty a kód je potřeba účet pro jednotlivé koncové body.  V tématu [Konfigurace Azure úložiště připojovací řetězec](../storage-configure-connection-string.md#creating-a-connection-string-to-the-explicit-storage-endpoint). 

Další informace o rozhraní API najdete v článku <a href="https://msdn.microsoft.com/en-us/library/azure/mt616540.aspx">Cloudové úložiště účtu konstruktor</a>.

Přípona koncový bod pro použití v těchto přetížení je core.usgovcloudapi.net 

### <a name="considerations"></a>Co byste měli zvážit

Tyto informace identifikuje hranici Azure Government Azure úložištěm:

| Upraveno/řízená data povolené. | Upraveno/řízená data není povoleno |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Data zadaná, ukládají a zpracovat v úložišti Azure produktu může obsahovat řízené exportovat data. Statická ověřovací moduly, jako jsou hesla a PIN kódy čipové karty pro přístup k součástí Azure platformy. Privátních klíčů certifikátů sloužící ke správě Azure platformu součásti. Další zabezpečení informací/tajemství, například certifikáty, šifrovacího klíče, hlavní klíče a úložiště klíče uložené v Azure služby. | Azure úložiště metadat není povoleno má být řízené exportovat data. Tato metadata zahrnuje všechna data konfigurace zadané při vytváření a udržování produkt úložiště.  Nezadáte Regulated/řízená data do následujících polí: skupiny zdrojů, názvy nasazení, názvy zdrojů, značky zdroje  

##  <a name="premium-storage"></a>Úložiště Premium

Informace o této službě a jak se používá, najdete v článku [Premium úložiště: výkonné úložiště pro Azure virtuálního počítače úloh](../storage/storage-premium-storage.md).

###  <a name="variations"></a>Varianty

Premium úložiště je všeobecně dostupná v Virginie USGov. Platí to i pro řadu DS virtuálních počítačích. 

### <a name="considerations"></a>Co byste měli zvážit

Ukládání dat úvahy výše uvedené použít u účtů úložiště premium. 

##  <a name="sql-database"></a>Databáze SQL

Další informace o konfigurace viditelnost metadat a osvědčené postupy ochrany v nápovědě k<a href="https://msdn.microsoft.com/en-us/library/bb510589.aspx"> Centrum zabezpečení aplikace Microsoft pro databázový stroj SQL</a> a [Veřejné dokumentace databáze SQL Azure](https://azure.microsoft.com/documentation/services/sql-database/) .

### <a name="variations"></a>Varianty

V Azure Government obecně neexistuje V12 databáze SQL.

Adresa pro SQL Azure servery v Azure Government se liší:

Typ služby|Azure veřejné|Azure Government
---|---|---
Databáze SQL|*. database.windows.net|*. database.usgovcloudapi.net

### <a name="considerations"></a>Co byste měli zvážit

Tyto informace identifikuje hranici Azure Government Azure úložištěm:

| Upraveno/řízená data povolené. | Upraveno/řízená data není povoleno |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Všechna data uložená a zpracovávají v Microsoft Azure SQL mohou obsahovat upraveno Azure Government data. Je nutné použít databázové nástroje pro přenos dat Azure Government upraveno dat. | Azure SQL metadat není povoleno má být řízené exportovat data. Tato metadata zahrnuje všechna data konfigurace zadané při vytváření a udržování produkt úložiště.  Nezadáte upraveno/řízená data do následujících polí: databáze název, název předplatného, skupiny zdrojů, název serveru, přihlášení správce serveru, nasazení jménech názvy zdrojů, značky zdroje

##  <a name="next-steps"></a>Další kroky

Pro přihlášení k odběru doplňující informace a aktualizace <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government blogu.</a>
