<properties 
    pageTitle="Další informace o podnikové integrace Pack dekódovat EDIFACT zprávy spojnice | Služba Microsoft Azure aplikací | Microsoft Azure" 
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

# <a name="get-started-with-decode-edifact-message"></a>Začínáme s dekódovat EDIFACT zprávy

Ověří, upravit a partnera specifické vlastnosti, vygeneruje dokument ve formátu XML pro každou transakci sadu a vygeneruje potvrzení zpracovaných transakce.

## <a name="create-the-connection"></a>Vytvoření připojení

### <a name="prerequisites"></a>Zjistit předpoklady pro

* Účet Azure. můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)

* Účet integrace je potřeba používat dekódovat EDIFACT zprávy spojnice. Zobrazit podrobnosti o tom, jak vytvořit [Účet integrace](./app-service-logic-enterprise-integration-create-integration-account.md), [partnery](./app-service-logic-enterprise-integration-partners.md) a [EDIFACT smlouva](./app-service-logic-enterprise-integration-edifact.md)

### <a name="connect-to-decode-edifact-message-using-the-following-steps"></a>Připojení k dekódovat EDIFACT zprávu pomocí následujících kroků:

1. [Vytvoření aplikace použití logických operátorů](./app-service-logic-create-a-logic-app.md) poskytuje příklad.

2. Tato spojnice není k dispozici aktivační události. Spusťte použití logických operátorů, jako je aktivační žádost o pomocí dalších aktivačních událostí.  V Návrháři logiky aplikaci přidat aktivační událost a přidat akci.  Vyberte Zobrazit Microsoft spravované rozhraní API v rozevíracím seznamu seznam a do vyhledávacího pole zadejte "EDIFACT".  Vyberte dekódovat EDIFACT zprávy

    ![hledání EDIFACT](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage1.png)
    
3. Pokud jste ještě nevytvořili všechna připojení k účtu integrace, zobrazí se výzva k podrobnosti o připojení

    ![Vytvoření účtu integrace](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage2.png)  

4. Zadejte podrobnosti o Integration účtu.  Vlastnosti hvězdičkou jsou potřeba

  	| Vlastnost | Podrobnosti |
  	| -------- | ------- |
  	| Název připojení * | Zadejte libovolnou název připojení |
  	| Integrace účtu * | Zadejte název účtu integrace. Ujistěte se, že váš účet integrace a logika aplikace jsou ve stejném umístění Azure |

    Po dokončení podrobnosti o připojení vypadat podobně jako tento

    ![integrace účtu](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage3.png)  

5. Vyberte možnost **vytvořit**

6. Všimněte si, že byl vytvořen připojení

    ![Podrobné informace o připojení integrace účtu](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage5.png)  

7. Vyberte zprávu plochému souboru EDIFACT dekódovat

    ![poskytnutí povinná](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage5.png)  

## <a name="edifact-decode-does-following"></a>Dekódovat EDIFACT znamená sledovat

* Řešení smlouvu porovnáním kvalifikátor odesílatele a identifikátor URI a sluchátko kvalifikátor a identifikátor URI
* Rozdělí více křižovatky v jedné zprávě na samostatné.
* Ověří obálky proti obchodování partnera smlouva
* Provede zpětný překlad výměnu.
* Ověří, upravit a partnera specifické vlastnosti obsahuje
    * Ověření struktury výměny obálky.
    * Schémat obálky vzhledem ke schématu ovládacího prvku.
    * Schémat prvků transakce sadu dat vzhledem ke schématu zprávy.
    * Upravit ověření provádějí prvky transakce sadu dat
* Ověří, že čísla ovládacího prvku nastavit výměny, skupiny a transakce nejsou duplicitní (Pokud je nakonfigurované) 
    * Zkontroluje ovládací prvek číslo výměny proti dříve přijatou křižovatky. 
    * Zkontroluje, číslo ovládací prvek skupiny před ostatními čísly skupiny ovládací prvek ve výměnu. 
    * Kontroly pro že transakce nastavit ovládací prvek číslo před další čísla transakce sadu ovládací prvek v dané skupině.
* Vygeneruje dokumentu XML pro každou transakci sadu.
* Převede celý výměny XML 
    * Rozdělení výměny jako sady transakce - pozastavení transakce sady při chybě: analyzuje každou transakci nastavení v křižovatky do samostatného dokumentu XML. Pokud jeden nebo více transakce sad v výměnu ověření se nezdařilo, pak EDIFACT dekódovat pozastaví jenom ty transakce sady. 
    * Rozdělení výměny jako sady transakce - pozastavení výměny při chybě: analyzuje každou transakci nastavení v křižovatky do samostatného dokumentu XML.  Pokud jeden nebo více transakce sad v výměnu ověření se nezdařilo, pak EDIFACT dekódovat pozastaví celý výměny.
    * Zachovat výměny - pozastavení transakce sady při chybě: vytvoření dokumentu XML pro celý dávkový výměnu. Dekódovat EDIFACT pozastaví pouze transakce sady, neúspěšné ověření a pokračujte v procesu u všech ostatních sady transakce
    * Zachovat výměny - pozastavení výměny při chybě: vytvoření dokumentu XML pro celý dávkový výměnu. Pokud jedna nebo více sad transakce v výměnu ověření se nezdařilo, pak EDIFACT dekódovat pozastaví celý výměny 
* Vygeneruje technické (ovládací prvek) nebo funkční potvrzování (Pokud je nakonfigurované).
    * Technická potvrzení nebo CONTRL ACK sestavy výsledky syntaktických zaškrtnutí dokončení přijaté výměny.
    * Funkční potvrzení potvrdí přijmout nebo odmítnout přijaté výměny nebo skupiny

## <a name="next-steps"></a>Další kroky

[Další informace o Enterprise Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Další informace o Enterprise Integration Pack") 