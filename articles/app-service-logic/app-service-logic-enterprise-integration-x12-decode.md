<properties 
    pageTitle="Další informace o podnikové integrace Pack dekódovat X12 zpráva Connctor | Služba Microsoft Azure aplikací | Microsoft Azure" 
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

# <a name="get-started-with-decode-x12-message"></a>Začínáme s dekódovat X12 zprávy

Ověří, upravit a partnera specifické vlastnosti, vygeneruje dokument ve formátu XML pro každou transakci sadu a vygeneruje potvrzení zpracovaných transakce.

## <a name="create-the-connection"></a>Vytvoření připojení

### <a name="prerequisites"></a>Zjistit předpoklady pro

* Účet Azure. můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)

* Integrace účet je potřeba používat dekódovat X12 zprávy spojnice. Zobrazit podrobnosti o tom, jak vytvořit [Účet integrace](./app-service-logic-enterprise-integration-create-integration-account.md), [partnery](./app-service-logic-enterprise-integration-partners.md) a [X12 smlouva](./app-service-logic-enterprise-integration-x12.md)

### <a name="connect-to-decode-x12-message-using-the-following-steps"></a>Připojení k dekódovat X12 zprávu pomocí následujících kroků:

1. [Vytvoření aplikace použití logických operátorů](./app-service-logic-create-a-logic-app.md) poskytuje příklad

2. Tato spojnice není k dispozici aktivační události. Spusťte použití logických operátorů, jako je aktivační žádost o pomocí dalších aktivačních událostí.  V Návrháři logiky aplikaci přidat aktivační událost a přidat akci.  Vyberte Zobrazit Microsoft spravované rozhraní API v rozevíracím seznamu seznam a potom zadejte "x12" do vyhledávacího pole.  Vyberte X12 – dekódovat X12 zprávy

    ![hledání x12](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage1.png)  

3. Pokud jste ještě nevytvořili všechna připojení k účtu integrace, zobrazí se výzva k podrobnosti o připojení

    ![Integrace připojení účtu](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage4.png)    

4. Zadejte podrobnosti o Integration účtu.  Vlastnosti hvězdičkou jsou potřeba

  	| Vlastnost | Podrobnosti |
  	| -------- | ------- |
  	| Název připojení * | Zadejte libovolnou název připojení |
  	| Integrace účtu * | Zadejte název účtu integrace. Ujistěte se, že váš účet integrace a logika aplikace jsou ve stejném umístění Azure |

    Po dokončení podrobnosti o připojení vypadat podobně jako tento
    
    ![Vytvoření připojení účtu integrace](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage5.png) 

5. Vyberte možnost **vytvořit**
    
6. Všimněte si, že byl vytvořen připojení.

    ![Podrobné informace o připojení integrace účtu](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage6.png) 

7. Vyberte X12 plochá zpráva souboru dekódovat

    ![poskytnutí povinná](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage7.png) 

## <a name="x12-decode-does-following"></a>X12 dekódovat slouží následující

* Ověří obálky proti obchodování partnera smlouva
* Vygeneruje dokumentu XML pro každou transakci sadu.
* Ověří partnera specifické vlastnosti a upravit
    * Upravit strukturální ověření a rozšířenou schémat
    * Ověření struktury výměny obálky.
    * Schémat obálky vzhledem ke schématu ovládacího prvku.
    * Schémat prvků transakce sadu dat vzhledem ke schématu zprávy.
    * Upravit ověření provádějí prvky transakce sadu dat 
* Ověří, že čísla ovládacího prvku nastavit výměny, skupiny a transakce nejsou duplicitní
    * Zkontroluje ovládací prvek číslo výměny proti dříve přijatou křižovatky.
    * Zkontroluje, číslo ovládací prvek skupiny před ostatními čísly skupiny ovládací prvek ve výměnu.
    * Kontroly pro že transakce nastavit ovládací prvek číslo před další čísla transakce sadu ovládací prvek v dané skupině.
* Převede celý výměny XML 
    * Rozdělení výměny jako sady transakce - pozastavení transakce sady při chybě: analyzuje každou transakci nastavení v křižovatky do samostatného dokumentu XML. Pokud jeden nebo více transakce sad v výměnu ověření se nezdařilo:, X12 dekódovat pozastaví jenom ty transakce sady.
    * Rozdělení výměny jako sady transakce - pozastavení výměny při chybě: analyzuje každou transakci nastavení v křižovatky do samostatného dokumentu XML.  Pokud jeden nebo více transakce sad v výměnu ověření se nezdařilo:, X12 dekódovat pozastaví celý výměny.
    * Zachovat výměny - pozastavení transakce sady při chybě: vytvoření dokumentu XML pro celý dávkový výměnu. X12 dekódovat pozastaví jenom ty transakce množinami ověření se nezdařilo:, přičemž ke zpracování jiných transakce nastaví
    * Zachovat výměny - pozastavení výměny při chybě: vytvoření dokumentu XML pro celý dávkový výměnu. Pokud jeden nebo více transakce sad v výměnu ověření se nezdařilo:, X12 dekódovat pozastaví celý výměny 
* Vygeneruje technických a/nebo funkční potvrzování (Pokud je nakonfigurované).
    * Technická potvrzení vygeneruje důsledku záhlaví ověření. Technická potvrzení sestavy stavu zpracování výměny záhlaví a přívěsu adresa příjemce.
    * Funkční potvrzení vygeneruje důsledku ověřovací text. Funkční potvrzení sestavy každý došlo k chybě při zpracování přijatých dokumentu

## <a name="next-steps"></a>Další kroky

[Další informace o Enterprise Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Další informace o Enterprise Integration Pack") 


