<properties
    pageTitle="Co je Azure klíč trezoru? | Microsoft Azure"
    description="Azure trezoru klíč pomáhá chránit klíče a tajemství používá cloudové aplikací a služeb. Pomocí klávesy trezoru Azure zákazníci zašifrovat lze klíče a tajemství (například ověřovací klíče, klíče účtu úložiště, klíče šifrování dat. Soubory PFX a hesla) pomocí kláves, které jsou chráněny hardwarové zabezpečení moduly (moduly hardwarového zabezpečení)."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="cabailey"/>



# <a name="what-is-azure-key-vault"></a>Co je Azure klíč trezoru?

Azure trezoru klíč je k dispozici ve většině oblastech. Další informace najdete v tématu [klíč trezoru ceny stránky](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Úvod

Azure trezoru klíč pomáhá chránit klíče a tajemství používá cloudové aplikací a služeb. Pomocí klávesy trezoru dá zašifrovat klíče a tajemství (například ověřovací klíče, klíče účtu úložiště, klíče šifrování dat. Soubory PFX a hesla) pomocí kláves, které jsou chráněny hardwarové zabezpečení moduly (moduly hardwarového zabezpečení). Přidané assurance importovat nebo vygenerování klíčů v moduly hardwarového zabezpečení. Pokud jste tuto možnost vyberte, Microsoft procesy v FIPS 140-2 klávesová úrovně 2 ověřený moduly hardwarového zabezpečení (hardware a firmware).  

Klíč trezoru zjednodušuje proces správy klíčů a umožňuje spravovat ovládací prvek zkratek, které se přístup a šifrování vaše data. Vývojáři mohli vytvářet klávesové zkratky pro vývoj a testování v minutách a potom Bezproblémová jejich migraci výrobní klíče. Správci zabezpečení můžete udělit (a odvolání) oprávnění k klávesy, podle potřeby.

Použití v následující tabulce lépe porozumět tomu, jak klíč trezoru vám může potřebám vývojářů a správci zabezpečení.





| Role        | Popis problému           | Vyřešit Azure klíčové trezoru  |
| ------------- |-------------|-----|
| Vývojář aplikace Azure      | "Chci vytvořit aplikaci pro Azure, který používá klíče pro podpis a šifrování, ale chci klávesy jako externí z aplikace tak, aby řešením je vhodné pro aplikace geograficky distribuovaný. <br/><br/>Chci taky tyto klávesy a tajemství zamknutý, aniž byste museli napsat kód Já. Můžu se taky podívat tyto klávesy a tajemství je snadno pomocí aplikace s optimální výkon." | √ Klávesy jsou uložené v trezoru a vyvolat URI v případě potřeby.<br/><br/> √ Klíče jsou chráněny tak, že Azure pomocí standardní algoritmů, délky klíčů a hardwarové zabezpečení moduly (moduly hardwarového zabezpečení).<br/><br/> √ Klávesy se zpracovávají v moduly hardwarového zabezpečení, které se nacházejí ve stejném Azure datacentrech jako aplikace. To umožňuje vyšší spolehlivost a latence nižší než pokud klávesy uložena v samostatném umístění, například místní.|
| Karta Vývojář softwaru jako služba (SaaS)      |"Můžu nechcete, aby zodpovědnost nebo potenciální odpovědnost Moje zákazníci klienta klíče a tajemství. <br/><br/>Chci zákazníků, kteří mají vlastníte a že spravovat legendou tak, aby se můžete Soustřeďte se na udělat, co mám dělat nejlépe, které základní poskytuje funkce softwaru." | √ Zákazníci můžete importovat vlastní klíče do Azure a spravovat. Pokud aplikace SaaS potřebuje k provádění operací, cryptographic pomocí kláves se svým zákazníkům, klíč trezoru provede tyto operace jménem aplikace. Aplikace nezobrazuje zákazníků klíče.|
| Hlavní bezpečnostní úředník (CSO) | "Budu chtít vědět, že naše aplikace v souladu s FIPS moduly 140-2 úrovně 2 hardwarového zabezpečení pro zabezpečené správy klíčů. <br/><br/>Chci, abyste měli jistotu, že organizaci pod kontrolou klíčové životní cyklus a můžete sledovat použití klíče. <br/><br/>A i když používáme více Azure služby a zdroje, chcete správu klíčů z jednoho místa v Azure."     |√ Moduly hardwarového zabezpečení jsou FIPS ověřit 140-2 úrovně 2.<br/><br/>√ Klíč trezoru je navržen tak, že Microsoft najdete v článku nebo extrahovat klíče.<br/><br/>√ Poblíž protokolování v reálném čase použití klíče.<br/><br/>√ trezoru poskytuje jediné rozhraní, bez ohledu na to, kolik trezorů máte v Azure, oblasti, které budou podporu a aplikace, které používají. |


Kdokoli s předplatným Azure vytváření a používání klíčových trezorů. Přestože klíč trezoru výhody vývojáři a správci zabezpečení, může implementovaná a spravuje správce organizace, která spravuje jiných Azure služeb pro organizaci. Tento správce by přihlaste předplatné Azure například vytvořte trezoru pro organizaci, ve kterém chcete ukládat klávesy a potom odpovídá za provozní úkoly, jako například:

+ Možnost vytvořte nebo importujte klíče nebo tajná
+ Odvolání nebo odstranění klíče nebo tajná
+ Povolit uživatelům nebo aplikace pro přístup k klíčové trezoru, abyste mohli potom spravovat a jeho použít a pomocí kláves tajemství
+ Konfigurace použití klíče (třeba podepsat nebo šifrování)
+ Sledování klíčových použití

Tento správce by pak vývojáři poskytnout URI zavolat z jejich aplikací a jejich správce zabezpečení poskytnout informace o použití klíče protokolování. 

   ![Základní informace o Azure klíčové trezoru][1]

Vývojáři můžou taky správu klíčů přímo, pomocí rozhraní API. Další informace najdete v příručce [Vývojář klíč trezoru](key-vault-developers-guide.md).

## <a name="next-steps"></a>Další kroky

Výukové dosáhnout toho, aby Začínáme pro správce najdete v článku [Začínáme s Azure klíč trezoru](key-vault-get-started.md).

Další informace o použití protokolování pro klíč trezoru najdete v tématu [Azure klíč trezoru protokolování](key-vault-logging.md).

Další informace o použití klávesy a tajemství s Azure klíč trezoru najdete v článku [o klíče, tajemství a certifikáty](https://msdn.microsoft.com/library/azure/dn903623\(v=azure.1\).aspx).


<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
