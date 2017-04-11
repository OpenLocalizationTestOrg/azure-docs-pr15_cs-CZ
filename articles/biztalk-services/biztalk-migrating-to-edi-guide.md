<properties   
    pageTitle="Migrace Admin.systému upravit řešení BizTalk služby technické příručky | Microsoft Azure"
    description="Migrace upravit do MABS; BizTalk služby Microsoft Azure"
    services="biztalk-services"
    documentationCenter="na"
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags 
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="mandia"/>


# <a name="migrating-biztalk-server-edi-solutions-to-biztalk-services-technical-guide"></a>Migrace Admin.systému upravit řešení ke službám BizTalk: technická příručka

Autor: Tim Wieman a Nitin Mehrotra

Recenzentů: Karthik Bharthy

Napsané pomocí: Microsoft Azure BizTalk služby – únor 2014 uvolněte.

## <a name="introduction"></a>Úvod

Elektronické výměny dat (Upravit) je jedním z nejčastěji rozšířené prostředků, do kterého firmy exchange elektronicky, také označují jako Business Business nebo B2B transakce. Admin.systému byla upravit podpora přes deset let od první vydání BizTalk Server. S BizTalk služby Microsoft budou dál problémy podpory pro řešení upravit na platformu Microsoft Azure. Transakce B2B nepracují převážně organizace a proto je snadněji implementovat, pokud bylo provedeno na platformě cloudu. Tuto možnost prostřednictvím služby BizTalk poskytuje Microsoft Azure.

Někteří uživatelé prohlédnout BizTalk služby jako "greenfield" platformu pro nová upravit řešení, množství zákazníků mít aktuální BizTalk serveru upravit řešení, které se chcete migrovat Azure. Protože je navržen BizTalk služby upravit podle stejných klíčové entit jako BizTalk serveru upravit architektura (obchodní partnerů, entity, smlouvami), je možné k migraci BizTalk serveru upravit artefakty BizTalk služeb.

Tento dokument pojednává o rozdílech souvisejících se migrace artefakty BizTalk serveru upravit BizTalk služeb. Tento dokument předpokládá praxi BizTalk serveru upravit zpracování a obchodování smlouvami partnera. Další informace o BizTalk serveru upravit najdete v článku [Obchodování partnera pomocí BizTalk Server pro správu](https://msdn.microsoft.com/library/bb259970.aspx).

## <a name="which-version-of-biztalk-server-edi-artifacts-can-be-migrated-to-biztalk-services"></a>Jakou verzi systému BizTalk serveru upravit artefakty můžete migrovat BizTalk služeb?

Modul BizTalk serveru upravit byl výrazně rozšířeného BizTalk Server 2010, pokud byla změna modelu zahrnout partnerů, profily a dohod. Přihlašovacích údajů BizTalk používá stejný model uspořádat obchodními partnery a obchodního oddělení tyto obchodními partnery. Migrace upravit artefakty z BizTalk Server 2010 a novějších verzích BizTalk služeb, jako výsledek je mnohem víc přímo přesměrování proces. Migrace upravit artefaktům spojeným s verzí před BizTalk serveru 2010, musíte nejdřív upgradovat na BizTalk Server 2010 a migrace upravit artefakty do služby BizTalk.

## <a name="scenariosmessage-flow"></a>Tok scénáře/zprávy

Jako BizTalk Server upravit zpracování služby BizTalk vytvořené kolem řešení obchodování správy partnera (TPM). Řešení TPM má tyto klíčové prvky:

- Obchodních partnerů, které představují organizace B2B transakce.
- Profily, které představují oddělení obchodního partnera.
- Obchodování smluv partnera (nebo dohod), které představují obchodní smlouva mezi dvěma partnery/profily.

Následující obrázek znázorňuje podobnosti a rozdíly mezi BizTalk serveru upravit řešení a služeb upravit BizTalk řešení:

![][EDImessageflow]

Důležité rozdíly a podobnosti upravit řešení tok BizTalk serveru a služby BizTalk jsou:

- Stejně jako admin.systému používá EDIReceive kanálu přijímat zprávy upravit a EDISend kanálu odeslat zprávu upravit, přihlašovacích údajů BizTalk používá upravit přijímání konferenčního mostu pro příjem a odeslat upravit most k odeslání zprávy upravit. BizTalk serveru kanály jsou přidružené k dohodu pomocí odesílání a přijímání porty. V dialogovém okně BizTalk služby samotnou dohodu označuje posílat nebo přijímat konferenčního mostu.
- Na serveru BizTalk po kanálu EDIReceive zpracuje ji upravit zprávu vypsáním k databázi SQL serveru. Profilace EdiSend potom vyzvedne zprávu z databáze SQL serveru, zpracuje ji a potom ji pošle obchodního partnera.

    V dialogovém okně BizTalk služby po dostávat upravit mostu zpracuje ji upravit, přesměrovává zprávu na externí proces. Externí proces může běžet na Microsoft Azure nebo místní. Externí proces by měl směrovat zprávu odeslat mostu upravit; Odeslat most není si ho naimportovat podstatě zprávu. Po zpracování zprávy, přesměruje mostu upravit odeslat zprávu obchodního partnera.

Služby BizTalk poskytuje možnosti konfigurace snadno použitelné rychle vytvářet a publikovat B2B smlouva mezi obchodních partnerů bez konfigurace všechny Microsoft Azure provést výpočet instance (role webu nebo pracovního), žádné databáze Microsoft Azure SQL nebo z účtů Microsoft Azure úložiště. Složitější scénáře budou vyžadovat, čímž propojíte v pracovních postupech nebo jiné služby zpracování "kolem okrajů" obchodování partnera smlouvy před datem start_date nebo po zpracování mostu obchodování upravit smlouva partnera. Píše následující posloupnosti událostí během zpracování služby BizTalk zprávy upravit.

1. Zprávu upravit přijetí z obchodních partnera, Fabrikam.  Pro příjem upravit zpráv od obchodní partnerů, BizTalk Services podporuje protokoly dopravní třeba FTP, SFTP, AS2 a HTTP/S.

2. Obchodní zpracování přijímání straně smlouva partnera provede zpětný překlad upravit zprávy ve formátu XML.  Směrování bylo upravit zprávy (ve formátu XML) do služby Bus koncové body například Service Bus Relay koncový bod, služby Bus téma, Bus frontě nebo Most BizTalk služby.

3. Bylo zprávy XML může potom dostali z koncový bod pro další zpracování vlastní.  Tyto koncové body může zpracovat komponentu místní nebo instanci Microsoft Azure výpočet pro další zpracování zpráv ve službě Windows pracovního postupu (Bránu) nebo Windows Communication Foundation (WCF), například.

4. "Odeslat straně zpracování" obchodního partnera smlouva potom překládá zprávy XML do formátu upravit a odešle obchodního partnera, Contoso.  Pro odesílání zpráv upravit obchodním partnerům, BizTalk Services podporuje stejné protokoly jako klávesovými zkratkami pro příjem zpráv upravit.

Další tento dokument obsahuje koncepční pokyny na některé z různých artefakty BizTalk serveru upravit migrace BizTalk služeb.

## <a name="sendreceive-ports-to-trading-partners"></a>Odesílání a přijímání porty k obchodování partnery

V Admin.systému nastavíte zobrazit umístění a přijímání porty zprávy upravit/XML z obchodními partnery a nastavit porty odeslat upravit/XML zprávy odešlete obchodního partnera. Můžete pak pokračovat v práci, tyto porty obchodní smlouva partnera pomocí konzoly pro správu serveru BizTalk. V dialogovém okně BizTalk služby do umístění, které dostáváte zprávy od obchodních partnerů, a pokud odesíláte zprávy obchodních partnerů nakonfigurované jako součást obchodní partnera samotnou dohodu (jako součást dopravy nastavení) na portálu BizTalk služby.  Takže nemáte skutečně pojem "Odeslat porty" a "získáte umístění" sobě, služby BizTalk. Další informace najdete v tématu [Vytvoření smlouvy](https://msdn.microsoft.com/library/windowsazure/hh689908.aspx).

## <a name="pipelines-bridges"></a>Potrubí (mosty)

Upravit serveru BizTalk potrubí způsoby entity zpracování zpráv, které mohou také obsahovat vlastní logiky pro konkrétní zpracování možnosti podle aplikace. Ekvivalent BizTalk státní, by mostu upravit. Ale služby BizTalk prozatím mosty upravit "uzavřeny".  To znamená nemůžete přidat vlastní aktivit do mostu upravit. Vlastní zpracování musí udělat mimo mostu upravit v aplikaci před nebo po zprávu zadá mostu nakonfigurování jako součást smlouva obchodování partnera. EAI mosty mít možnost vlastní zpracování. Pokud chcete vlastní zpracování, můžete před nebo po zpracování zprávy pomocí konferenčního mostu upravit EAI mosty. Další informace najdete v tématu [jak zahrnout vlastní kód v mosty](https://msdn.microsoft.com/library/azure/dn232389.aspx).

Můžete vložit publikovat nebo přihlášení k odběru toku s vlastní kód nebo pomocí služby Bus zpráv fronty a témata před obchodní smlouva partnera, zobrazí se zpráva nebo po smlouvu zpracovává zprávy a přesměrovává koncový bod služby Bus.

Zobrazit **Tok scénáře/zprávy** v tomto tématu vzorek toku zprávy.

## <a name="agreements"></a>Smlouvami

Pokud máte zkušenosti s BizTalk serveru 2010 Trading partnera dohod použité pro zpracování upravit, podobný BizTalk služby obchodování partnera smlouvami povědomý. Většina nastavení smlouva jsou stejné a používat stejný terminologie. V některých případech se mnohem jednodušší nastavení smlouva ve srovnání s stejné nastavení BizTalk serveru. Microsoft Azure BizTalk Services podporuje X12 EDIFACT a AS2 doprava.

Microsoft Azure BizTalk Services také poskytuje nástroj **TPM dat migrace** migrovat obchodní partnery a smlouvami z modulu BizTalk serveru obchodování partnera na portál služeb BizTalk. Nástroje pro migraci dat TPM je k dispozici jako součást sady nástrojů, které si můžete stáhnout z [MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057). Balíček obsahuje taky readme, který obsahuje pokyny, jak používat nástroj a základní informace o řešení problémů pro nástroj.

## <a name="schemas"></a>Schémata

Služba BizTalk poskytuje schémata upravit, které lze použít v řešení BizTalk služby.  Kromě toho BizTalk serveru upravit schémata lze také služby BizTalk protože kořenový uzel schématu upravit je stejná napříč Admin.systému, jakož i BizTalk služeb. Proto bude moct přímo prohlédněte BizTalk serveru upravit schémata a jejich použití v upravit řešení, které vyvinete službách BizTalk. Schémata si taky můžete stáhnout z [MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057).

## <a name="maps-transforms"></a>Mapy (transformací)

Mapy v Admin.systému nazývají transformace služby BizTalk. Migrace map z Admin.systému ke službám BizTalk může jít o některý z složitější úlohy dosáhnout (podle toho, mapy složitost). Mapování nástroj používaný služby BizTalk se liší od BizTalk mapování. I když mapování vypadá převážně, základní formátu mapy se liší. Functoids (označované jako služby BizTalk **Mapy operace** ) dostupné pro uživatele se liší stejně.  Ve skutečnosti nelze použít přímo na mapu BizTalk služby BizTalk. Také všechny dostupné na serveru BizTalk functoids jsou k dispozici jako operace mapování služby BizTalk.

### <a name="new-transform-operations"></a>Nové transformace operace

Seznam mapování mezi operacemi transformace dostupné může zdát velmi liší od Admin.systému mapování, BizTalk služby transformací mít nové způsoby provádění úloh. Například BizTalk služby transformací mít **Seznam operace** dostupné. Toto není k dispozici v BizTalk mapování.  **Seznam operace** umožňují vytvářet a pracovat na "Seznam", kde je seznam několika položek (označovaná taky jako "řádky") a všechny požadované položky může obsahovat více členy (označovaná taky jako "sloupce").  Můžete seznam seřadit, vyberte položky podle stavu, atd.

Jiný příklad použití nových funkcí v BizTalk služby transformací jsou **Operace opakovat**.  Je v mapování Admin.systému vytvořit vnořené smyčky.  Tedy operace mapy smyčka přidají pro transformací BizTalk služby.

Jiný příklad ještě je **Pokud-pak-jinak** mapy operace výraz.  Zahájením Pokud pak jinak operace bylo možné v mapování BizTalk, ale povinné více functoids provádění zdánlivě jednoduché úkolu.

### <a name="migrating-biztalk-server-maps"></a>Migrace Admin.systému mapy

Microsoft Azure BizTalk Services poskytuje nástroj pro migraci Admin.systému mapuje transformace BizTalk služby. **BTMMigrationTool** je k dispozici v rámci sady **nástrojů** součástí [BizTalk Services SDK stáhnout](http://go.microsoft.com/fwlink/p/?LinkId=235057). Další informace o nástroji najdete v článku [Převedení BizTalk mapování a transformace služby BizTalk](https://msdn.microsoft.com/library/windowsazure/hh949812.aspx).

Můžete taky prohlédnout ukázku Sandro Pereira BizTalk MVP o tom, jak [migrovat Admin.systému map transformace BizTalk služby](http://social.technet.microsoft.com/wiki/contents/articles/23220.migrating-biztalk-server-maps-to-windows-azure-biztalk-services-wabs-maps.aspx).

## <a name="orchestrations"></a>Orchestrations

Pokud potřebujete přejít Admin.systému průběhu zpracování na Microsoft Azure, orchestrations bude nutné protože Microsoft Azure nepodporuje pracovního orchestrations Admin.systému.  Může přepsat funkci průběhu ve službě Windows Workflow Foundation 4.0 (WF4).  To je aktuálně žádné migrace z Admin.systému orchestrations na WF4 bude dokončení revize. Tady je několik zdrojů pro pracovního postupu pro Windows:

- [*Integrace služby WCF pracovního postupu s fronty Bus služby a témata*](https://msdn.microsoft.com/library/azure/hh709041.aspx) tak, že Paolo Salvatori. 

- [ *Vytváření aplikací s Windows Workflow Foundation a Azure* relaci](http://go.microsoft.com/fwlink/p/?LinkId=237314) z konference sestavení 2011.

- [*Windows Workflow Foundation Developer Center*](http://go.microsoft.com/fwlink/p/?LinkId=237315) na webu MSDN.

- Si přečtěte následující [*dokumentaci Windows Workflow Foundation 4 (WF4)*](https://msdn.microsoft.com/library/dd489441.aspx) na webu MSDN.

## <a name="other-considerations"></a>Další informace

Následuje několik důležité informace, které musíte při používání služeb BizTalk.

### <a name="fallback-agreements"></a>Náhradní smlouvami

Upravit serveru BizTalk zpracování má koncept "Nouzového smlouvami".  Znamená BizTalk služeb **není** zatím máte nouzového smlouva koncept.  Tématech BizTalk si přečtěte následující dokumentaci [Z dohod rolí v tématu upravit zpracování](http://go.microsoft.com/fwlink/p/?LinkId=237317) a [Konfigurace globálního nebo vlastnosti smlouva nouzového](https://msdn.microsoft.com/library/bb245981.aspx) informace při použití nouzového dohod BizTalk serveru.

### <a name="routing-to-multiple-destinations"></a>Směrování na více míst

Služby BizTalk mosty v jeho aktuálním stavu nepodporuje směrování zpráv na více míst pomocí publikovat-přihlášení k odběru modelu. Je místo toho směrovat zprávy od konferenčního mostu služby BizTalk k tématu Bus služby, který potom může mít víc předplatných zpráva na víc než jeden koncový bod.

## <a name="conclusion"></a>Uzavření

Microsoft Azure BizTalk Services se aktualizuje na běžná milníky přidat další funkce a funkce. Při každé aktualizaci podíváme vpřed na referenční rozšířené funkce usnadnit vytváření pomocí služby BizTalk a dalších Azure technologiích řešení začátku do konce.

## <a name="see-also"></a>Viz taky

[Vývoj podnikových aplikací s Azure](https://msdn.microsoft.com/library/azure/hh674490.aspx)

[EDImessageflow]: ./media/biztalk-migrating-to-edi-guide/IC719455.png
