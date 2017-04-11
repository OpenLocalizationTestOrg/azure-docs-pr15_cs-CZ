<properties
    pageTitle="Jak migrovat logiky aplikace do schématu verze 2015-08-01 – verze preview | Aplikace služby Microsoft Azure"
    description="Použití logických operátorů aplikace můžete migrovat snadno na nejnovější verzi schéma. Postupujte podle těchto kroků."
    services="logic-apps"
    documentationCenter=""
    authors="MSFTMAN"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="deonhe"/>

# <a name="how-to-migrate-logic-apps-to-schema-version-2015-08-01-preview"></a>Jak migrovat logiky aplikace do schématu verze 2015-08-01 – verze preview

Přesunout stávající aplikace logiky nové schéma, postupujte takto:  
1. Otevřete aplikaci logiku v portálu Azure  
2. Klikněte na schéma aktualizace:

 ![Ikona rozhraní API][step1]   
Na stránce schématu aktualizace slouží k zobrazení a obsahuje odkaz na dokument, který poskytování údajů o vylepšeních v nové schéma: ![rozhraní API ikonu][step2]

>[AZURE.NOTE] Po výběru **Schématu aktualizace**automaticky spustit kroky migrace a poskytovat výstupu kód. Můžete to ale aktualizujte definici vaší, ujistěte se, že které sledujete osvědčených postupů kódování třeba můžou být uvedené v části **Doporučené postupy** .

## <a name="best-practices-when-migrating-your-logic-apps-to-the-latest-schema-version"></a>Osvědčené postupy při migraci na nejnovější verzi schéma aplikace použití logických operátorů:  

- Zkopírování migrované skriptu do nové aplikace použití logických operátorů: nechcete přepsat původní jednu až do dokončení testování a potvrzeno aplikaci migrované funguje očekávaným způsobem.
- Otestujte vaše logiky aplikace **před** přepnutím ve výrobním
- Po dokončení migrace spuštění aktualizace aplikací logiky používat [spravované rozhraní API](./apis-list.md) , pokud je to možné. Například můžete začít používat Dropbox v2 whereever používáte v1 Dropboxu.


## <a name="whats-next"></a>Další krok
-  [Zjistěte, jak ručně migrovat aplikací logiky](../app-service-logic/app-service-logic-schema-2015-08-01.md)


<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png






