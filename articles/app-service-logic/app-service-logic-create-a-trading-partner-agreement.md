<properties 
   pageTitle="Vytvořit obchodní smlouvu partnera služby Azure aplikace | Microsoft Azure" 
   description="Vytvoření obchodování partnera smlouvami" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
    ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="08/23/2016"
   ms.author="rajram"/>

# <a name="creating-a-trading-partner-agreement"></a>Vytváření obchodního smlouva partnera   

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Obchodní partnery jsou zahrnuté do B2B entity komunikace (Business Business). Když dvěma partnery Vytvořme relaci, to se nazývá *smlouvy*. Smlouva definované je založená na přání partnery komunikace dvou dosáhnout a Protocol (protokol) nebo transport konkrétní. Různé B2B protokoly a palet nepodporuje aplikaci služby Azure patří:

- AS2 (použitelnost výkazu 2)
- EDIFACT (United Nations/elektronické výměny dat pro správu, obchod a dopravu (UN/EDIFACT))
- X12 (ASC X12)

### <a name="biztalk-api-apps-that-support-b2b-scenarios"></a>BizTalk rozhraní API aplikace, které podporují B2B scénáře
Následující aplikace rozhraní API povolení tyto funkce pomocí bohaté a intuitivní prostředí v Azure portálu:


## <a name="biztalk-trading-partner-management-tpm"></a>BizTalk obchodování partnera Management (TPM)
- Vytváření a správu partnerů, profily a identity
- Ukládání a Správa schémat upravit
- Ukládání a správa certifikátů (AS2 protocol)
- Vytváření a správu AS2 smlouvami
- Vytváření a správu EDIFACT dohod (včetně dávky na straně odeslat)
- Vytváření a správu X12 smlouvami (včetně dávky na straně odeslat)

![][1]


## <a name="as2-connector"></a>AS2 spojnice
- Spustí AS2 smlouvami podle související instance TPM rozhraní API aplikace
- Informace o řešení potíží s zpracování/sledování AS2 plochy


## <a name="biztalk-edifact"></a>BizTalk EDIFACT
- Spustí EDIFACT smlouvami podle související instance TPM rozhraní API aplikace
- Informace o řešení potíží s zpracování/sledování EDIFACT plochy
- Poskytuje správu stavu dávkách (spuštění a ukončení) podle EDIFACT smlouvy související instance TPM rozhraní API aplikace


## <a name="biztalk-x12"></a>BizTalk X12
- Spustí X12 smlouvami podle související instance TPM rozhraní API aplikace 
- Plochy X12 zpracování/sledování informace pro řešení potíží
- Poskytuje správu stavu dávkách (spuštění a ukončení) podle X12 smlouvy související instance TPM rozhraní API aplikace

Dříve uvedená je nutné AS2 X 12 a EDIFACT rozhraní API aplikace z TPM rozhraní API aplikace funkce podle očekávání.


## <a name="getting-started"></a>Začínáme
Vytvoření obchodního smlouvami partnera:

1. Vytvoření instance konektoru **BizTalk obchodování partnera správy** . Při této akci musí prázdná databáze SQL funkce. Před spuštěním je potřeba mít prázdnou databázi k dispozici a je připraven k použití.
2. Nahrajte schémata a certifikáty podle potřeby tak, že dohod. Nainstalovat při procházení instance TPM vytvořené a krokování do části "Schémata" nebo "Osvědčení"
3. Vyhledejte instance TPM vytvořili a přesunuli do části **partnery**
4. Vytvoření partnery podle potřeby. Také upravit profilů podle potřeby a přidejte požadované identitami
5. Část **dohod** teď slouží k vytváření smluv. Když vytvoříte dohodu, je nutné vybrat Protocol (protokol), která se použije. Zbývající možnosti konfigurace vycházejí protokol, který jste vybrali.

![][2]

![][3]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-trading-partner-agreement/TPMResourceView.png
[2]: ./media/app-service-logic-create-a-trading-partner-agreement/ProtocolSelection.png
[3]: ./media/app-service-logic-create-a-trading-partner-agreement/X12AgreementCreation.png
 
