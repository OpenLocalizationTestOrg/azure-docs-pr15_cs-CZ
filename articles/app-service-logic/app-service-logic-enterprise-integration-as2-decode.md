<properties 
    pageTitle="Další informace o podnikové integrace Pack dekódovat AS2 zprávy Connctor | Služba Microsoft Azure aplikací | Microsoft Azure" 
    description="Naučte se používat partnery v aplikacích Enterprise integrace Pack a logika" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="padmavc" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="padmavc"/>

# <a name="get-started-with-decode-as2-message"></a>Začínáme s dekódovat AS2 zprávy

Připojení k dekódovat AS2 zprávy stanovit zabezpečení a spolehlivosti při předávání zprávy. Poskytuje digitální podepisování dešifrování a potvrzení prostřednictvím zprávy dispozice oznámení (MDN).

## <a name="create-the-connection"></a>Vytvoření připojení

### <a name="prerequisites"></a>Zjistit předpoklady pro

* Účet Azure. můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)

* Integrace účet je potřeba používat dekódovat AS2 zprávy spojnice. Zobrazit podrobnosti o tom, jak vytvořit [Účet integrace](./app-service-logic-enterprise-integration-create-integration-account.md), [partnery](./app-service-logic-enterprise-integration-partners.md) a [AS2 smlouva](./app-service-logic-enterprise-integration-as2.md)

### <a name="connect-to-decode-as2-message-using-the-following-steps"></a>Připojení k dekódovat AS2 zprávu pomocí následujících kroků:

1. [Vytvoření aplikace použití logických operátorů](./app-service-logic-create-a-logic-app.md) poskytuje příklad.

2. Tato spojnice není k dispozici aktivační události. Spusťte použití logických operátorů, jako je aktivační žádost o pomocí dalších aktivačních událostí.  V Návrháři logiky aplikaci přidat aktivační událost a přidat akci.  Vyberte Zobrazit Microsoft spravované rozhraní API v rozevíracím seznamu seznam a do vyhledávacího pole zadejte "AS2".  Vyberte AS2 – dekódovat AS2 zprávy

    ![Hledání AS2](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage1.png)

3. Pokud jste ještě nevytvořili všechna připojení k účtu integrace, zobrazí se výzva k podrobnosti o připojení

    ![Vytvoření připojení integrace](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage2.png)

4. Zadejte podrobnosti o Integration účtu.  Vlastnosti hvězdičkou jsou potřeba

  	| Vlastnost   | Podrobnosti |
  	| --------   | ------- |
  	| Název připojení *    | Zadejte libovolnou název připojení |
  	| Integrace účtu * | Zadejte název účtu integrace. Ujistěte se, že váš účet integrace a logika aplikace jsou ve stejném umístění Azure |

    Po dokončení podrobnosti o připojení vypadat podobně jako tento

    ![Integrace připojení](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage3.png)

5. Vyberte možnost **vytvořit**
    
6. Všimněte si, že byl vytvořen připojení.  Teď pokračujte dalšími kroky v aplikaci použití logických operátorů

    ![Integrace připojení vytvořili](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage4.png) 

7. Výběr textu a záhlaví z výstupy požadavek

    ![poskytnutí povinná](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage5.png) 

## <a name="the-as2-decode-does-the-following"></a>AS2 dekódovat dělá toto

* Zpracuje záhlaví AS2/HTTP
* Ověří podpis (Pokud je nakonfigurované)
* Dešifruje zprávy (Pokud je nakonfigurované)
* Dekomprimuje zprávy (Pokud je nakonfigurované)
* Sloučí přijaté MDN s původním odchozích zpráv
* Aktualizace a koreluje záznamů v databázi není odmítnutí
* Zapíše záznamy AS2 odesílání zpráv o stavu
* Obsah výstupní data jsou ve formátu Base 64 kódování
* Určuje, zda MDN je potřeba a zda MDN by měl být synchronní nebo asynchronní na základě konfigurace AS2 smlouvy
* Vygeneruje synchronní nebo asynchronní MDN (podle konfigurace smlouvy)
* Nastaví vlastnosti tokenů korelace a v MDN

##<a name="try-it-for-yourself"></a>Vyzkoušejte to pro sebe

Proč není vyzkoušejte si to. Klikněte na [zde](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/) nasazení aplikace plně funkční logiky vlastní použití AS2 aplikace logických funkcí 

## <a name="next-steps"></a>Další kroky

[Další informace o Enterprise Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Další informace o Enterprise Integration Pack") 