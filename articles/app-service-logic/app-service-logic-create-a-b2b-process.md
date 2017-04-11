<properties 
   pageTitle="Vytvoření procesu B2B v aplikaci služby Azure | Microsoft Azure" 
   description="Základní informace o tom, jak vytvořit obchodní obchodních procesů" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="04/20/2016"
   ms.author="rajram"/>

# <a name="creating-a-b2b-process"></a>Vytvoření procesu B2B

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]


## <a name="business-scenario"></a>Obchodní scénářů 
Contoso a Northwind se dvěma obchodními partnery. Contoso (prodejce) odešle prostřednictvím odvětví úrovně dopravní třeba AS2 nákupní objednávky Northwind (poskytovatel). Northwind ukládá všechny příchozí objednávky cloudového úložiště. Nákupní objednávky jsou zprávy XML mezi těmito dvěma partnery. Až zprávu uložený ve společnosti Northwind cloudového úložiště vnitřní procesy společnosti Northwind zpracování pořadí od tohoto okamžiku na.
 
Cílem tohoto kurzu je vytvořit jak Northwind můžete vytvořit obchodní proces, přes který může přijímat zprávy (nákupních objednávek ve formátu XML) od partnera společnosti Contoso prostřednictvím AS2 a potom přetrvávají v jeho cloudového úložiště.


## <a name="capabilities-demonstrated"></a>Funkce prokázáno 
Tento kurz pomáhá prezentovat následující možnosti: 

- **Zpráva dopravy**: obchodě a dodavatele lze na různé platformy, ale pořád dosáhnete komunikace mezi nimi. V tomto kurzu se komunikace přes AS2 (použitelnost údajů 2). AS2 je Oblíbené způsob, jak přenos dat mezi obchodními partnery v business business komunikace.
- **Trvání dat**: když se zpráva byla přijata přes AS2 pak Northwind chce přetrvávají před další zpracování. Spojnice může použít pro zachování zpráv v jeho cloudového úložiště. V tomto kurzu se právě využít jako cloudového úložiště objektů BLOB Azure pro Northwind.
- **Vytváření obchodních procesů**: V toku, více rozhraní API aplikace může být stříhají společně dosáhnout výsledek firmy, jak je znázorněno zde.


## <a name="before-you-begin"></a>Než začnete
Tento kurz předpokládá základní porozumět aplikace služby Azure, neví, jak vytvořit rozhraní API aplikace a panoramatické tok.


## <a name="steps-to-achieve-the-business-scenario"></a>Kroky k dosažení obchodních scénářů
**Vytváření a konfigurace vyžaduje rozhraní API aplikace**

1. Vytvoření instanci **Spojnice úložiště objektů Blob Azure**. Tento postup vyžaduje přihlašovací údaje pro účet Azure úložiště. Zajistila možnost je připraven před vytvořením to.
2. Vytvoření instance **BizTalk obchodování partnera správy**. Při této akci musí prázdná databáze SQL funkce. Ujistěte se, že je připraven před vytvořením to.
3. Vytvoření instanci **AS2 spojnice**. To je třeba prázdná databáze SQL funkce. Ujistěte se, že je připraven před vytvořením to. Navíc pokud budete chtít archivace zpráv v rámci AS2 zpracování, můžete nastavit přihlašovací údaje k objektů Blob Azure během jejího vytvoření.
4. Konfigurace služby TPM (obchodování partnera správy), která se vytvoří:  
    1. Přejděte na instance služby TPM vytvořen jako součást výše uvedené kroky.
    2. Použití **partnery** možnosti v části *součásti* pro **Přidání** nového partnera s názvem **Contoso** a v jeho profilu přidejte požadovaných AS2 identity.
    3. Použití **partnery** možnosti v části *součásti* **Přidat** nového partnera s názvem **Northwind** a v jeho profilu přidejte požadované identity AS2.
    4. Volba **smlouvami** ve skupinovém rámečku *součásti* na Smlouvu o poskytování **Přidat** nové AS2 mezi Northwind a Contoso. Northwind bude hostovanou partnera a Contoso bude mít partner Host. Podle potřeby konfigurace podepisování šifrování, komprese a potvrzení při vytváření této smlouvy. V případě certifikáty nechcete používat, bylo možné odeslat prostřednictvím možnost **certifikátů** při procházení TPM služba, která se vytvoří.


## <a name="create-a-flow--business-process"></a>Vytvoření tok / firmy zpracovat
1. Vytvořte nový tok jejímž cílem prvního kroku je AS2. Přetahování **AS2 spojnici** a zvolte instanci vytvořen. Klikněte na aktivační událost jako funkce:  
    ![][1]  
2. Další přetahování **Spojnice úložiště objektů Blob Azure** a zvolte instanci vytvořen. Vyberte akci jako funkci a ve které, vyberte **Nahrát objektů Blob** jako požadované funkce. Konfigurace podle potřeby.
3. Nyní vytvořte/nasaďte tok.


## <a name="message-processing--troubleshooting"></a>Zpracování zprávy a řešení potíží
1. Je potřeba vyzkoušejte toku, kterou jsme nasadili. Odesláno: XML zprávy zalomený v AS2 (podle smlouva AS2 vytvořili výše) na koncový bod AS2 vytažená AS2Connector instanci, kterou jste vytvořili. Budete muset konfigurace ověřování koncového bodu tak, aby byla veřejně přístupný.
2. Spuštění informace o tok vytažená tak, že přejdete tok a potom krokování instance toku, který máte spouštět
3. AS2 zpracování informace vyhledejte souvisejících AS2Connector instance a postupujte podle krokování do části sledování. Můžete použít filtry souvisejících chcete omezit zobrazení na informace, které chcete obnovit.

![][2]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-b2b-process/Flow.png
[2]: ./media/app-service-logic-create-a-b2b-process/Tracking.png
 
