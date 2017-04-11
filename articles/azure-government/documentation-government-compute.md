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
    ms.date="09/29/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-compute"></a>Pro využití Azure Government

##  <a name="virtual-machines"></a>Virtuálních počítačích

Informace o této službě a jak se používá najdete v článku [Azure virtuálních počítačích velikosti](../virtual-machines/virtual-machines-windows-sizes.md).

### <a name="variations"></a>Varianty

Následující SKU OM je všeobecně dostupná (GA) v Azure Government:

OM SKU|VA Gov USA|I Gov USA|Poznámky
---|---|---|---
A|GA|GA|Žádná
Dv1|GA|-|Žádná
DSv1|GA|-|Žádná
Dv2|CZ|CZ|15 je brzy k dispozici
F|CZ|CZ|Žádná
G|Plánované|-|Žádná

###  <a name="data-considerations"></a>Důležité údaje související data

Tyto informace identifikuje hranici Azure Government pro Azure virtuálních počítačích:

| Upraveno/řízená data povolené. | Upraveno/řízená data není povoleno |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Data zadaná uloženy a zpracovat v rámci virtuálního počítače mohou obsahovat řízené exportovat data. Binární soubory spuštěné v Azure virtuálních počítačích. Statická ověřovací moduly, jako jsou hesla a PIN kódy čipové karty pro přístup k součástí Azure platformy. Privátních klíčů certifikátů sloužící ke správě součástí Azure platformy. Připojovací řetězec SQL.  Další zabezpečení informací/tajemství, například certifikáty, šifrovacího klíče, hlavní klíče a úložiště klíče uložené v Azure služby.  | Metadata není povoleno má být řízené exportovat data. Tato metadata zahrnuje všechna data konfigurace zadané při vytváření a udržování virtuálního počítače Azure.  Nezadáte Regulated/řízená data do následujících polí: klient názvy rolí skupiny zdrojů, názvy nasazení, názvy zdrojů, značky zdroje  

## <a name="next-steps"></a>Další kroky

Doplňující informace a aktualizace se přihlásit k odběru <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government blogu.</a>
