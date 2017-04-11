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
    ms.date="10/12/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-security-and-identity"></a>Azure Government zabezpečení a Identity

##  <a name="key-vault"></a>Klíčové trezoru
Informace o této službě a jak se používá, najdete v článku <a href="https://azure.microsoft.com/documentation/services/key-vault">veřejné dokumentace Azure klíč trezoru.</a>

### <a name="data-considerations"></a>Důležité údaje související data
Tyto informace identifikuje hranici Azure vláda pro trezoru Azure klíč:

| Upraveno/řízená data povolené. | Upraveno/řízená data není povoleno |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Všechna data zašifrovaných klíčem Azure klíč trezoru může obsahovat Regulated/řízená data. | Azure klíč trezoru metadat není povoleno má být řízené exportovat data. Tato metadata zahrnuje všechna data konfigurace zadané při vytváření a udržování trezoru klíče.  Nezadáte Regulated/řízená data do následujících polí: pole názvy zdrojů skupina, klíč trezoru názvy, názvu předplatného. |

V Azure Government obecně neexistuje klíč trezoru. Jako veřejnosti je bez přípony tak, aby klíč trezoru dostupné prostřednictvím Powershellu a rozhraní příkazového řádku pouze.

## <a name="next-steps"></a>Další kroky

Doplňující informace a aktualizace se přihlásit k odběru <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government blogu.</a>
