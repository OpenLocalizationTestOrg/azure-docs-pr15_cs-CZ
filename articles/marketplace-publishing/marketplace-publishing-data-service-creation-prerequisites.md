<properties
   pageTitle="Technická předpoklady k vytváření datové služby pro Tržiště | Microsoft Azure"
   description="Porozumět tomu, že splníte požadavky pro vytváření datové služby nasadit a prodej na Azure Marketplace"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="hascipio; avikova" />

# <a name="technical-pre-requisites-for-creating-a-data-service-offer-for-the-azure-marketplace"></a>Technické předpoklady pro vytváření datové služby nabídky pro Azure Marketplace

>[AZURE.IMPORTANT] **V současné době jsme už rychlého připojení všechny nové vydavatelé datové služby. Nové dataservices nebude získat schváleno seznam.** Pokud máte s aplikací business SaaS chcete publikovat na AppSource můžete najít další informace [v tomto poli](https://appsource.microsoft.com/partners). Pokud máte IaaS aplikace nebo vývojář služby rádi byste publikovat na webu Azure Marketplace můžete najít další informace [v tomto poli](https://azure.microsoft.com/marketplace/programs/certified/).

Přečtěte si procesu důkladně před zahájením a vysvětlení kdy a proč je provedena každého kroku. Co nejvíce můžete měli připravit údajů o vaší společnosti a dalších dat, stáhněte si potřebné nástroje nebo vytvořit technické součásti před zahájením proces vytváření nabídky.

Byste si měli jste připravení před zahájením procesu následující položky:

## <a name="make-a-decision-on-what-technology-will-be-used-to-publish-your-data-service-offer"></a>Rozhodování na jakou technologii se použijí k publikování vaši nabídku datové služby

Můžete se rozhodnout, vydavateli mezi více technologie při publikování dat služby Azure Marketplace. V hlavním technologiích podporovaných píše níže. Bez ohledu na to jakou technologii se používá pro publikování datové služby, koncový uživatel používá data prostřednictvím **informační kanál OData** zveřejněným službou Azure Marketplace. Úplné informace o službě OData najdete [http://www.odata.org/](http://www.odata.org/)

## <a name="sql-azure-database"></a>SQL Azure databáze

S datovou sadu připravení v SQL Azure odpovídá vydavatele. Budete muset přihlášení k odběru Azure, zřízení odpovídající velikost databáze a odeslání dat do databáze SQL Azure. Vydavatel je také odpovídá vždy aktualizovat jeho data. Další informace o přihlášení k odběru služby Azure najdete [https://azure.microsoft.com/services/sql-database/](https://azure.microsoft.com/services/sql-database/)


Při přesunutí dat do databáze SQL Azure, můžete z Azure Marketplace vystavit tabulek a zobrazení. Publisheru můžete určit, které tabulky a zobrazení a sloupce jsou vystaven koncový uživatel. Dále můžete taky určit poskytovatel obsahu sloupce, které jde zpracovávat koncový uživatel a ty, které vracejí pouze v datové. To vám vysokou úroveň flexibilitu o tom, které by měl zobrazenou po data v databázi. Sloupce, které můžete vyhledávat muset podporovaným jeden nebo více indexy databáze.

## <a name="rest-based-web-service"></a>Webová služba REST na základě

Podporované Protocol (protokol): **pouze HTTPS**

Existujících služeb na základě ZBÝVAJÍCÍ může projevit až Azure Marketplace. Protože datové vystaven vždy koncových uživatelů jako datový kanál OData, na základě potřeb služby Azure Marketplace budete moci služba namapovat OData služby. Tak ZBYTEK na základě koncové body chcete zobrazit všechny parametry jako HTTP parametry.

Datové musí být ve formuláři, který je možné namapovat do ATOM odpověď. Proto odpověď ze služby musí být ve formátu XML a pouze může obsahovat jeden opakující se element obsahující datové hodnoty (třeba sada záznamů). Služby Azure Marketplace namapuje uzel opakující se položku uzel ATOM a datové hodnoty do vlastnosti uzlů v rámci uzel položky.

Informace o ověření (například rozhraní API klíčů, ověřování tokenu, atd.) musí být za předpokladu, že jako parametr HTTP nebo v záhlaví HTTP (dvojice hodnoty klíče) – základní ověřování podporuje i. Platné klíč musí být k dispozici a všechny žádosti o prostřednictvím webu Azure Marketplace provádějí pomocí tohoto klíče. Uživatel sledování a fakturace se stane ve vrstvě Azure Marketplace.

Chyby vrácené službou muset namapovat na HTTP stavů. V případě, že služba vrátí XML, který obsahuje chybu tyto chodí namapovat službou Azure Marketplace s kódy stav HTTP.

## <a name="soap-based-web-services"></a>Na základě SOAP webové služby

Protocol (protokol): **pouze HTTPS**

Požadavky na jsou že stejné jako v ostatních na základě oddíl služby. Jediný rozdíl je, že parametry může být podle textu XML, účtované službě vydavatele s každé žádost prostřednictvím webu Azure Marketplace. To znamená, že se právě HTTP parametrů, které uživatel zadá na front-end převést na XML elementy XML dokumentu odeslaná s žádostí o webové službě poskytovatele obsahu.

## <a name="odata-based-web-services"></a>OData na základě webové služby

Protocol (protokol): **pouze HTTPS**

Data můžete vystavit jako služby OData k webu Azure Marketplace. Systém bude předávat služby prostřednictvím a nahradí kořenovém službu kořenové služby Azure Marketplace – zajistit, aby že všechny další hovory absolvovat Azure Marketplace.

OData služby pouze nemusíte přejděte do back-end v databázi. OData podporuje jakýkoli druh úložiště nebo obchodní logiky řídit službu.


## <a name="next-steps"></a>Další kroky
Teď prohlédnete před požadavky a dokončené potřebné úkoly, můžete se Přechod vpřed s vytvářením vaši nabídku datové služby podle [Data služby publikování Guide](marketplace-publishing-data-service-creation.md).

Nebo pokud jste chtěli: kontrolujte potvrzení celkového procesu a příslušné články pro jednotlivá pole publikování fází, naleznete v článku [Začínáme: jak publikovat nabídka z Azure Marketplace](marketplace-publishing-getting-started.md).

[link-acct]:marketplace-publishing-accounts-creation-registration.md
