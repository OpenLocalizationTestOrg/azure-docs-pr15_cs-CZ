<properties 
    pageTitle="Další informace o podnikové integrace Pack kódovat X12 zpráva Connctor | Služba Microsoft Azure aplikací | Microsoft Azure" 
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

# <a name="get-started-with-encode-x12-message"></a>Začínáme s kódovat X12 zprávy

Ověří, upravit a partnera specifické vlastnosti, převede kódovaný XML zprávy upravit transakce sady v výměnu a žádosti o potvrzení technické a/nebo funkční

## <a name="create-the-connection"></a>Vytvoření připojení

### <a name="prerequisites"></a>Zjistit předpoklady pro

* Účet Azure. můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)

* Integrace účet je potřeba používat kódovat x12 zprávy spojnice. Zobrazit podrobnosti o tom, jak vytvořit [Účet integrace](./app-service-logic-enterprise-integration-create-integration-account.md), [partnery](./app-service-logic-enterprise-integration-partners.md) a [X12 smlouva](./app-service-logic-enterprise-integration-x12.md)

### <a name="connect-to-encode-x12-message-using-the-following-steps"></a>Připojení k kódovat X12 zprávu pomocí následujících kroků:

1. [Vytvoření aplikace použití logických operátorů](./app-service-logic-create-a-logic-app.md) poskytuje příklad

2. Tato spojnice není k dispozici aktivační události. Spusťte použití logických operátorů, jako je aktivační žádost o pomocí dalších aktivačních událostí.  V Návrháři logiky aplikaci přidat aktivační událost a přidat akci.  Vyberte Zobrazit Microsoft spravované rozhraní API v rozevíracím seznamu seznam a potom zadejte "x12" do vyhledávacího pole.  Vyberte buď X12-kódovat X12 zprávu podle názvu smlouva nebo X12-kódovat X 12 zprávu identity.  

    ![hledání x12](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage1.png) 

3. Pokud jste ještě nevytvořili všechna připojení k účtu integrace, zobrazí se výzva k podrobnosti o připojení

    ![Integrace připojení účtu](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage1.png) 


4. Zadejte podrobnosti o Integration účtu.  Vlastnosti hvězdičkou jsou potřeba

  	| Vlastnost | Podrobnosti |
  	| -------- | ------- |
  	| Název připojení * | Zadejte libovolnou název připojení |
  	| Integrace účtu * | Zadejte název účtu integrace. Ujistěte se, že váš účet integrace a logika aplikace jsou ve stejném umístění Azure |

    Po dokončení podrobnosti o připojení vypadat podobně jako tento

    ![Vytvoření připojení účtu integrace](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage2.png) 


5. Vyberte možnost **vytvořit**

6. Všimněte si, že byl vytvořen připojení.

    ![Podrobné informace o připojení integrace účtu](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage3.png) 

#### <a name="x12---encode-x12-message-by-agreement-name"></a>X12-kódovat X12 zprávy podle názvu smlouva

7. Vyberte X12 smlouva z rozevíracího seznamu a jazyk xml zprávy kódování.

    ![poskytnutí povinná](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage4.png) 

#### <a name="x12---encode-x12-message-by-identities"></a>X12-kódovat X12 zprávu pomocí identity

7.  Zadejte identifikátor odesílatele, kvalifikátor odesílatele, příjemce identifikátor a příjemce kvalifikátor nakonfigurovaná v X12 smlouvy.  Vyberte zprávu xml kódování

    ![poskytnutí povinná](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage5.png) 

## <a name="x12-encode-does-following"></a>Kódování X12 slouží následující:

* Smlouva řešení porovnáním odesílatele a příjemce vlastnosti kontextu.
* Jsou výměny upravit převod kódovaný XML zprávy upravit transakce sady v výměnu.
* Slouží k použití transakce nastavit záhlaví a návěs segmentů
* Generuje číslo výměny ovládací prvek, číslo skupiny ovládací prvek a číslo ovládacího prvku nastavit transakce pro každý odchozí výměny
* Slouží k nahrazení oddělovače datové části dat
* Ověří partnera specifické vlastnosti a upravit
    * Schémat prvků transakce sadu dat proti zprávy schématu
    * Upravit ověření provádět transakce sadu datové prvky.
    * Rozšířené ověření provádějí prvky transakce sadu dat
* Žádosti o potvrzení technické a/nebo funkční (Pokud je nakonfigurované).
    * Technická potvrzení vygeneruje důsledku záhlaví ověření. Technická potvrzení sestav stavu zpracování výměny záhlaví a přívěsu adresa příjemce
    * Funkční potvrzení vygeneruje důsledku ověřovací text. Funkční potvrzení sestavy každý došlo k chybě při zpracování přijatých dokumentu

## <a name="next-steps"></a>Další kroky

[Další informace o Enterprise Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Další informace o Enterprise Integration Pack") 

