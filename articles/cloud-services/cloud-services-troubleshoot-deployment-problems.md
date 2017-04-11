<properties
 pageTitle="Poradce při potížích nasazení služby cloudu | Microsoft Azure"
 description="Existuje několik běžných problémů, které můžou nastat při nasazení do cloudové služby Azure. Tento článek obsahuje některé z nich řešení."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="troubleshoot-cloud-service-deployment-problems"></a>Poradce při potížích nasazení služby cloudu

Při nasazení balíčku aplikace cloudové služby Azure, můžete získat informace o nasazení z podokna **Vlastnosti** na portálu Azure. Můžete podrobnosti v tomto podokně umožňují Poradce při potížích s cloudové služby a zadejte tyto informace na podporu Azure při otevírání novou žádost o podporu.

V podokně **Vlastnosti** najdete takto:

* Na portálu Azure klikněte na nasazení cloudové služby, klikněte na **všechna nastavení**a potom klikněte na **Vlastnosti**.
* Na portálu Azure klasické klikněte na nasazení cloudové služby, klikněte na **řídicí panel**umístěný v pravém dolním rohu stránky (pod **první pohled dával**). Mějte na paměti, že se na toto podokno bez popisku "Vlastnosti".

> [AZURE.NOTE] Kliknutím na ikonu v pravém horním rohu podokna můžete obsah podokna **Vlastnosti** kopírovat do schránky.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="problem-i-cannot-access-my-website-but-my-deployment-is-started-and-all-role-instances-are-ready"></a>Problém: nemůžete získat přístup k webu, ale Moje nasazení je spuštěn a všechny instance role připraveni

Na webu adresu URL v portálu nezahrnuje číslo portu. Výchozí port pro weby je 80. Pokud chcete spustit v různých port nakonfigurovaný aplikace, musíte přidat číslo portu správné na adresu URL při přístupu k webu.

1. Na portálu Azure klikněte na nasazení cloudové služby.
2. V podokně **Vlastnosti** portálu Azure zaškrtněte porty instancí role (ve skupinovém rámečku **Vstup koncové body**).
3. Nejsou-li číslo portu 80, přidejte hodnotu správný port na adresu URL přistupujete k aplikaci. Chcete-li jiné než výchozí port, zadejte adresu URL a za ním dvojtečku (:) a za ním uveďte číslo portu bez mezer.

## <a name="problem-my-role-instances-recycled-without-me-doing-anything"></a>Problém: Moje role instance recykloval bez mě museli něco dělat

Služba retušování dojde automaticky Azure rozpozná problém uzly a tedy přesune role instance nových uzlů. Když k tomu dojde, může se zobrazit vaše role instance recyklace automaticky. Pokud chcete zjistit, pokud služby retušování došlo k chybě:

1. Na portálu Azure klikněte na nasazení cloudové služby.
2. V podokně **Vlastnosti** portálu Azure zkontrolujte informace a zjistit, zda služby retušování došlo v době, pozorovanými role recyklace.

Role bude taky odpadkového zhruba jednou za měsíc během hostitelského operačního systému a hosta OS aktualizace.  
Další informace najdete v blogovém příspěvku [Role Instance restartuje termín inovací OS](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx)

## <a name="problem-i-cannot-do-a-vip-swap-and-receive-an-error"></a>Problém: můžu nemůžete dělat VIP vyměňovat a dojít k chybě

Odkládací VIP není povoleno, pokud probíhá nasazení aktualizace. Nasazení aktualizace může probíhají automaticky, když:

* Nové hostovaný operační systém je k dispozici a není nakonfigurován pro automatické aktualizace.
* Service retušování probíhá.

Pokud chcete zjistit, pokud je automatické aktualizace brání způsobem vyměňovat VIP:

1. Na portálu Azure klikněte na nasazení cloudové služby.
2. V podokně **vlastností** portálu Azure podívejte se na hodnotu **stavu**. Pokud je **připravená**, zaškrtněte **poslední operace** zobrazíte-li jeden naposledy se stalo, který může bránit vyměňovat VIP.
3. Opakujte kroky 1 a 2 pro nasazení výroby.
4. Je-li automatické aktualizace v procesu, čekat na Dokončit před pokusem o proveďte vyměňovat VIP.

## <a name="problem-a-role-instance-is-looping-between-started-initializing-busy-and-stopped"></a>Problém: Instanci rolí je mezi Začínáme, Probíhá inicializace, zaneprázdněn(a) a zastaveno opakování.

Tato podmínka znamenat problém se souborem kód balíčku a konfigurace aplikace. V takovém případě by měl být k dispozici stav změna každých několik minut a portálu Azure může Říkejte něco jako **recyklace**a **zaneprázdněn(a)**, **Probíhá inicializace**. Tento údaj označuje, že je něco správné snažit se aplikace, která je uchovávání instanci rolí spuštění.

Další informace o tom, jak řešení tohoto problému najdete v blogovém publikovat [Azure PaaS výpočet diagnostiky dat](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx) a [běžné problémy, které způsobují role do koše](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

## <a name="problem-my-application-stopped-working"></a>Problém: Aplikace Nepřesouvat

1. Na portálu Azure klikněte na instanci rolí.
2. V podokně **Vlastnosti** portálu Azure vezměte v úvahu následující podmínky, které problém vyřešit:
   * Pokud instanci rolí naposledy přestal (zaškrtnete políčko přínosu **Přerušit počítání**), může být aktualizace nasazení. Počkejte, pokud instanci rolí obnoví funguje na vlastní.
   * Pokud instanci rolí je **zaneprázdněn(a)**, zkontrolujte kódu aplikace zobrazíte, pokud je zpracována [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck) událost. Možná budete muset přidat nebo odstranit některé kód, který zpracovává Tato událost.
   * Absolvovat dat diagnostiky a Poradce při potížích scénáře v příspěvku na blogu [Azure PaaS výpočet diagnostiky Data](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

>[AZURE.WARNING] Pokud odpadkového cloudové služby, je obnovit vlastností pro nasazení, efektivně mazání informace u původní problém.

## <a name="next-steps"></a>Další kroky

Zobrazte další [Poradce při potížích s články](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) při cloudovým službám.

Další řešení problémů s cloudové služby rolí pomocí Azure PaaS počítačových diagnostiky dat najdete v tématu [Jan Williamson řada](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
