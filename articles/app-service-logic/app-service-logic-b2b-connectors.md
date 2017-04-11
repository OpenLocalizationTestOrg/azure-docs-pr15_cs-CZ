<properties 
    pageTitle="Spojnice Business Business a rozhraní API aplikace v aplikacích pro použití logických operátorů | Microsoft Azure" 
    description="Zjistěte, jak můžete vytvořit a nakonfigurovat upravit, EDIFACT, AS2 a TPM spojnic. microservices architektura" 
    services="logic-apps" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="anneta" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="mandia"/> 

# <a name="business-to-business-connectors-and-api-apps"></a>Business Business spojnicemi a rozhraní API aplikace

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Logika aplikace obsahuje mnoho BizTalk rozhraní API aplikace, které jsou důležité se integrace prostředí. Tyto rozhraní API aplikace založené na koncepty a nástrojích v rámci Admin.systému, ale teď jsou k dispozici jako součást aplikace logiku. 

Jednu kategorii tyto rozhraní API aplikace jsou rozhraní API aplikace Business Business (B2B). Pomocí těchto B2B rozhraní API aplikace, můžete snadno přidat partnerů, vytvořit smlouvami, a proveďte všechno při by místních pomocí upravit, AS2 a EDIFACT.  

Tyto B2B rozhraní API aplikace nabídnout "Aktivační událost" a "Akce". Aktivační události spustí novou instanci na základě zvláštní události jako příchodu X12 zprávy od partnera. Akce je výsledek, jako je po obdržení X12 zprávy po odeslání zprávy pomocí AS2.


## <a name="what-is-a-business-to-business-connector-or-api-apps"></a>Co je konektor Business Business a rozhraní API aplikace
Funkce Business Business (B2B) obsahuje existující, předdefinovaných rozhraní API aplikace, pomocí kterých jiné společnosti, dělení nebo aplikace, a tak dále sdělovat pomocí AS2, upravit a EDIFACT. 

Rozhraní API aplikace B2B patří: 

Spojnice a rozhraní API aplikace | Popis
--- | ---
Správa partnera obchodování BizTalk | Rozhraní API aplikaci, která vytvoří business business (B2B) relací pomocí partnery a smlouvami. Tyto vztahy využít AS2 EDIFACT a X12 protokol.<br/><br/>Rozhraní API aplikace TPM je základní požadavku konektoru AS2 a X12 či EDIFACT rozhraní API aplikace. 
AS2 spojnice | Spojnice, který přijme a odešle zprávy používající AS2 transport. Spojnice přenosy protokolů dat bezpečně a problémy se spolehlivým přes Internet.
BizTalk EDIFACT | Rozhraní API aplikaci, která přijímá a odešle zprávy používající EDIFACT. EDIFACT je také označované jako UN/EDIFACT (United Nations/elektronické dat Interchange pro správu, obchod a dopravu) a je velmi sloužícího odvětví.
BizTalk X12 | Rozhraní API aplikaci, která přijímá a odešle zprávy používající X12 protokol. X12 je také označované jako ASC X 12 (akreditovaných standardy výbor X12) a je velmi sloužícího průmysl. 


Pomocí těchto rozhraní API aplikace, musíte udělat jiné upravit zpráv úkoly. Například pomocí konektoru AS2, které můžete bezpečně přijímání a odesílání různé druhy zpráv (upravit, XML, soubor, a tak dál) zákazníka, dělení ve vaší firmě jako je třeba lidských zdrojů nebo každý uživatel, který používá AS2. 

Budete moct vytvářet tolik rozhraní API aplikace a snadno vytvářet. Taky můžete znovu použít typu single rozhraní API aplikace ve více scénáře nebo pracovní postupy.

Můžete to udělat bez psaní kódu. Pusťme se do práce. 


## <a name="requirements-to-get-started"></a>Požadavky na Začínáme
Při vytváření B2B rozhraní API aplikace existují některé požadované zdroje. Tyto položky musí být vytvořené před použitím dalších rozhraní API aplikace. Zahrnout tyto požadavky: 

Požadavek | Popis
--- | ---
Databáze Azure SQL | Ukládá B2B položek, včetně partnerů, schémat, certifikáty a agreeements. Tyto B2B rozhraní API aplikace vyžadují vlastní databázi SQL Azure. <br/><br/>**Poznámka:** Zkopírujte připojovací řetězec k této databázi.<br/><br/>[Vytvoření databáze Azure SQL](../sql-database/sql-database-get-started.md)
Kontejner úložiště objektů Blob Azure | Ukládá zprávy vlastnosti při zapnuté funkci AS2 archivaci. Pokud nepotřebujete AS2 zprávy archivace, není potřeba kontejneru úložiště. <br/><br/>**Poznámka:** Chcete-li povolit archivace, zkopírujte připojovací řetězec pro toto úložiště objektů Blob.<br/><br/>[Informace o účtech Azure úložiště](../storage/storage-create-storage-account.md)
Služba Bus Namespace a jeho hodnoty klíče | Ukládá X12 a EDIFACT dávky data. Pokud nepotřebujete dávky, není potřeba obor Bus služby.<br/><br/>**Poznámka:** Chcete-li povolit dávky, zkopírujte následující hodnoty.<br/><br/>[Vytvoření Namespace Bus služby](http://msdn.microsoft.com/library/azure/hh690931.aspx)
TPM instanci | K vytvoření konektoru AS2 a X12 nebo EDIFACT rozhraní API aplikace je potřeba instanci BizTalk obchodování partnera Management (TPM). Při vytváření rozhraní API aplikace TPM vytváříte TPM Instance. <br/><br/>**Poznámka:** Znáte název TPM rozhraní API aplikace. 


## <a name="create-the-api-apps"></a>Vytváření aplikací pro rozhraní API
B2B rozhraní API aplikace se dají vytvořit pomocí portálu Azure nebo pomocí rozhraní REST API. 


### <a name="create-the-api-apps-using-rest-apis"></a>Vytvoření rozhraní API aplikace pomocí rozhraní REST API
[Jak používat rozhraní REST API najdete v dokumentaci.](http://go.microsoft.com/fwlink/p/?LinkId=529766)


### <a name="create-the-b2b-api-apps-in-the-azure-portal"></a>Vytváření aplikací pro rozhraní API B2B na portálu Azure
Na portálu Azure vytvoříte B2B rozhraní API aplikace při vytváření aplikace logiku, webových aplikací nebo mobilní aplikace. Nebo můžete vytvořit pomocí vlastní zásuvné. Oba způsoby, jak je snadné tak záleží na potřeb nebo Předvolby. Někteří uživatelé raději nejprve vytvořit všech aplikací rozhraní API B2B s jejich specifické vlastnosti. Potom vytvářet logiky aplikace nebo webové aplikace/mobilní aplikace a přidání aplikací rozhraní API B2B jste vytvořili.  

Podle těchto kroků vytváření aplikací pro rozhraní API B2B pomocí zásuvné rozhraní API aplikace.


#### <a name="create-the-biztalk-trading-partner-management-tpm-api-apps"></a>Vytvoření BizTalk obchodování partnera Management (TPM) rozhraní API aplikace

> [AZURE.NOTE] K vytvoření konektoru AS2 a X12 nebo EDIFACT rozhraní API aplikace je potřeba instanci BizTalk obchodování partnera Management (TPM). Při vytváření rozhraní API aplikace TPM vytváříte TPM Instance.

Instanci TPM vytvoříte takto:

1. Azure portálu Startboard (domovskou stránku) vyberte **Marketplace**. **Rozhraní API aplikace** obsahuje všechny existující rozhraní API aplikace a spojnic. Také je možné **Hledat** pro konkrétní rozhraní API aplikace B2B.
2. Vyberte **BizTalk obchodování správy partnera**. V nové zásuvné vyberte možnost **vytvořit**. 
3. Zadejte vlastnosti: 

    Vlastnost | Popis
--- | ---
Jméno | Zadejte libovolnou název TPM instance. Například můžete ji nazvat *AccountsPayableTPM*.
Nastavení balíčku | Zadejte ADO.NET **Připojovací řetězec databáze** k databázi SQL Azure jste vytvořili. <br/><br/>Když zkopírujte připojovací řetězec heslo nepřidá připojovací řetězec. Je nutné zadat heslo v připojovacím řetězci.
Plán služeb aplikací | Seznam plánu platby. Můžete ho změnit v případě potřeby více nebo méně zdroje.
Ceny osy | Vlastnost jen pro čtení, který obsahuje kategorii ceny v rámci předplatného Azure. 
Pole Skupina zdroje | Vytvořit novou nebo použít existující skupinu. Rozhraní API aplikace a konektory: pro vaše logiku aplikací Web Apps a mobilní aplikace musí být ve stejné skupině zdroje. <br/><br/>[Pomocí skupin zdrojů](../azure-resource-manager/resource-group-overview.md) vysvětluje tuto vlastnost. 
Předplatné | Jen pro čtení vlastnost, která jsou uvedeny aktuálního předplatného.
Umístění | Zeměpisné polohy, který je hostitelem služby Azure. 
Přidání Startboard | Výběrem této možnosti Přidat aplikaci rozhraní API B2B na pravobok (domovskou stránku).

4. Vyberte možnost **vytvořit**. 

Po vytvoření TPM rozhraní API aplikace (TPM Instance) můžete pak vytvořit konektoru AS2 a/nebo X12 nebo EDIFACT rozhraní API aplikace. 


#### <a name="create-the-as2-connector"></a>Vytvoření AS2 spojnice

1. Azure portálu Startboard (domovskou stránku) vyberte **Marketplace**. **Rozhraní API aplikace** obsahuje všechny existující rozhraní API aplikace a spojnic. Také je možné **Hledat** pro konkrétní rozhraní API aplikace B2B.
2. Vyberte **spojnici AS2**. V nové zásuvné vyberte možnost **vytvořit**. 
3. Zadejte vlastnosti: 

    Vlastnost | Popis
--- | ---
Jméno | Zadejte libovolnou název AS2 spojnice. Například můžete ji nazvat *AS2Connector*.
Nastavení balíčku | Zadejte nastavení specifické pro tuto aplikaci rozhraní API, jako je název TPM Instance. <br/><br/>V tomto tématu specifické vlastnosti najdete v článku [Přidání nastavení balíčku AS2](#AddAS2Conn) . 
Plán služeb aplikací | Seznam plánu platby. Můžete ho změnit v případě potřeby více nebo méně zdroje.
Ceny osy | Vlastnost jen pro čtení, který obsahuje kategorii ceny v rámci předplatného Azure. 
Pole Skupina zdroje | Vytvořit novou nebo použít existující skupinu. [Pomocí skupin zdrojů](../azure-resource-manager/resource-group-overview.md) vysvětluje tuto vlastnost. 
Předplatné | Jen pro čtení vlastnost, která jsou uvedeny aktuálního předplatného.
Umístění | Zeměpisné polohy, který je hostitelem služby Azure. 
Přidání Startboard | Výběrem této možnosti Přidat aplikaci rozhraní API B2B na pravobok (domovskou stránku).

    **<a name="AddAS2Conn"></a>Spojnice AS2 nastavení balíčku**

    Vlastnost | Popis
--- | --- 
Připojovací řetězec databáze | Zadejte připojovací řetězec ADO.NET k databázi SQL Azure jste vytvořili. Když zkopírujte připojovací řetězec heslo nepřidá připojovací řetězec. Ujistěte se, že před vložením zadejte heslo v připojovacím řetězci.
Povolení archivace pro příchozí zprávy | Volitelné. Povolte tuto vlastnost na ukládání vlastností zprávy příchozí zprávy AS2 dostali od partnera. 
Připojovací řetězec úložiště objektů Blob Azure  | Zadejte připojovací řetězec do kontejneru úložišti objektů Blob Azure, kterou jste vytvořili. Když je povolený archivace zprávy zakódovaný a dekódovanou jsou uloženy v tomto kontejneru úložiště.
Název Instance TPM | Zadejte název **BizTalk obchodování partnera správy** rozhraní API aplikace jste dříve vytvořili. Při vytváření konektoru AS2 tato spojnice spustí pouze smlouvami AS2 v této konkrétní instanci TPM.

4. Vyberte možnost **vytvořit**. 


#### <a name="create-the-x12-or-edifact-api-apps"></a>Vytváření aplikací pro rozhraní API X12 nebo EDIFACT

1. Azure portálu Startboard (domovskou stránku) vyberte **Marketplace**. **Rozhraní API aplikace** obsahuje všechny existující rozhraní API aplikace a spojnic. Také je možné **Hledat** pro konkrétní rozhraní API aplikace B2B.
2. Vyberte **BizTalk X 12** nebo **BizTalk EDIFACT**. V nové zásuvné vyberte možnost **vytvořit**. 
3. Zadejte vlastnosti: 

    Vlastnost | Popis
--- | ---
Jméno | Zadejte libovolnou název B2B rozhraní API aplikace. Například můžete ji nazvat *EDI850APIApp*.
Nastavení balíčku | Zadejte nastavení specifické pro tuto aplikaci rozhraní API, jako je název TPM Instance. <br/><br/>V tomto tématu specifické vlastnosti, najdete v článku [X12 nebo EDIFACT nastavení balíčku](#AddX12) . 
Plán služeb aplikací | Seznam plánu platby. Můžete ho změnit v případě potřeby více nebo méně zdroje.
Ceny osy | Vlastnost jen pro čtení, který obsahuje kategorii ceny v rámci předplatného Azure. 
Pole Skupina zdroje | Vytvořit novou nebo použít existující skupinu. [Pomocí skupin zdrojů](../azure-resource-manager/resource-group-overview.md) vysvětluje tuto vlastnost. 
Předplatné | Jen pro čtení vlastnost, která jsou uvedeny aktuálního předplatného.
Umístění | Zeměpisné polohy, který je hostitelem služby Azure. 
Přidání Startboard | Výběrem této možnosti Přidat aplikaci rozhraní API B2B na pravobok (domovskou stránku).

    **<a name="AddX12"></a>Nastavení balíčku aplikace rozhraní API X12 a EDIFACT**  

    Vlastnost | Popis
--- | --- 
Připojovací řetězec databáze | Zadejte připojovací řetězec ADO.NET k databázi SQL Azure jste vytvořili. Když zkopírujte připojovací řetězec heslo nepřidá připojovací řetězec. Ujistěte se, že před vložením zadejte heslo v připojovacím řetězci.
Služba Bus Namespace | Zadejte oboru Bus služby, kterou jste vytvořili. Povinné pouze v případě dávky je povolena. 
Název služby Bus Namespace sdílené přístupová klávesa | Zadejte oboru služby Bus přístupová klávesa jste vytvořili. Povinné pouze v případě dávky je povolena. 
Hodnota služby Bus Namespace sdílené přístupová klávesa | Zadejte oboru služby Bus přístupová klávesa hodnotu, kterou jste vytvořili. Povinné pouze v případě dávky je povolena. 
Název Instance TPM | Zadejte název **BizTalk obchodování partnera správy** rozhraní API aplikace jste dříve vytvořili. Při vytváření X12 nebo EDIFACT rozhraní API aplikace se spustí rozhraní API aplikace X12/EDFIACT dohod do této konkrétní instanci TPM.

4. Vyberte možnost **vytvořit**. 


## <a name="add-your-partners-agreements-certificates-and-schemas"></a>Přidání partnerů, smlouvami, certifikáty a schémat 
Na portálu Azure otevřete TPM rozhraní API aplikace. V části **součásti** přidáte partnerů, smlouvami, certifikáty a schémat. 

Můžete také přidat smlouvami ke spojnicím AS2, X12 rozhraní API aplikace a EDIFACT rozhraní API aplikace. 


## <a name="monitor-your-api-apps"></a>Sledování aplikací rozhraní API
Na portálu Azure otevřete TPM rozhraní API aplikace. V části **operace** můžete zobrazit různé správy operace. Například máte tyto možnosti:

- Zobrazení informační a chyby události
- Zobrazení paměťové použití a vlákna počet pracovního procesu (w3wp)
- Zobrazení protokolů server aplikace a web

Další na [sledování aplikací logiky](app-service-logic-monitor-your-logic-apps.md).


## <a name="add-the-api-apps-to-your-application"></a>Přidání aplikací rozhraní API aplikace 
Microsoft Azure aplikaci služby zpřístupňuje typech jinou aplikací, které můžete použít tyto B2B rozhraní API aplikace. Můžete vytvořit novou nebo můžete přidat existující rozhraní API aplikace B2B logiky aplikací, aplikací Mobile nebo Web Apps. 

V aplikaci jednoduše výběrem B2B rozhraní API aplikace z Galerie automaticky přidá ji do aplikace.  

> [AZURE.IMPORTANT] Přidat spojovací čáry a rozhraní API aplikace jste dříve vytvořili, vytvoření aplikace použití logických operátorů, mobilní aplikace nebo webové aplikace ve stejné skupině zdroje. 

Podle těchto kroků přidat rozhraní API aplikace B2B aplikace použití logických operátorů, mobilní aplikace nebo webové aplikace: 

1. V Azure portálu Startboard (domovskou stránku) přejděte na **Web Marketplace**a vyhledejte použití logických operátorů, mobilní telefon nebo Web Apps. 

    Při vytváření nové aplikace, vyhledejte aplikace použití logických operátorů, mobilní aplikace nebo webové aplikace. Vyberte požadovanou aplikaci a v nové zásuvné, vyberte možnost **vytvořit**. [Vytvoření aplikace použití logických operátorů](app-service-logic-create-a-logic-app.md) jsou uvedené kroky. 

2. Otevřete aplikaci a vyberte **aktivačními událostmi a akce**. 

3. **Galerie**vyberte B2B rozhraní API aplikace, automaticky přidá do aplikace. Můžete taky vytvořit nové rozhraní API aplikace B2B.

    > [AZURE.IMPORTANT] Konektor AS2 a X12 EDIFACT rozhraní API aplikace vyžadují TPM Instance. Pokud vytváříte novou B2B rozhraní API aplikace, nejprve vytvořit aplikaci TPM rozhraní API, takže vytvořte konektoru AS2 X12 rozhraní API aplikace nebo EDIFACT rozhraní API aplikace. 

4. Klikněte na **OK** uložte provedené změny. 

>[AZURE.NOTE] Začínáme s aplikací logiky Azure před registrací Azure účet, [Zkuste aplikace použití logických operátorů](https://tryappservice.azure.com/?appservice=logic). Můžete okamžitě vytvořit aplikaci logiky krátkodobý starter. Žádné povinné; kreditní karty žádné závazky.

## <a name="more-b2b-resources"></a>Další materiály B2B

[Vytvoření procesu B2B](app-service-logic-create-a-b2b-process.md)<br/>
[Vytváření obchodního smlouva partnera](app-service-logic-create-a-trading-partner-agreement.md)<br/>
[Co je spojnic a BizTalk rozhraní API aplikace](app-service-logic-what-are-biztalk-api-apps.md)


## <a name="read-about-logic-apps-and-web-apps"></a>Další informace o použití logických operátorů aplikace a webových aplikací Web Apps
[Jaké aplikace logiky?](app-service-logic-what-are-logic-apps.md)<br/>
[Weby a aplikací Web Apps v aplikaci služby Azure](../app-service-web/app-service-web-overview.md)


## <a name="more-connectors"></a>Další spojnic

[Spojnice a rozhraní API aplikace seznamu](app-service-logic-connectors-list.md)<br/><br/>
[Co je spojnic a BizTalk rozhraní API aplikace](app-service-logic-what-are-biztalk-api-apps.md) 
