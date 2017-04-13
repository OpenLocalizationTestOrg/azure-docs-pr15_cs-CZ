<properties
   pageTitle="Rozdíly mezi cloudovými službami a struktury služby | Microsoft Azure"
   description="Přehled pro migraci aplikací z Cloudovým službám struktury služby."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="learn-about-the-differences-between-cloud-services-and-service-fabric-before-migrating-applications"></a>Další informace o rozdílech mezi cloudovými službami a struktury služby před migrací aplikací.
Microsoft Azure služby struktury je platform aplikace další generování cloudu pro vysoce scalable vysoce spolehlivé distribuované aplikace. Uvádí mnoho nových funkcí pro sbalení nasazení, upgradu a Správa aplikací distribuované cloudu. 

Toto je úvodní příručku migrace žádosti z cloudové služby struktury služby. Se zaměřuje na architektonické a rozdíly v návrhu mezi cloudovými službami a struktury služby.
 
## <a name="applications-and-infrastructure"></a>Aplikace a infrastrukturu

Základní rozdíl mezi cloudovými službami a struktury služby je vztah mezi VMs, pracovního vytížení a aplikací. Úlohu tady je definována jako kód, který napíšete provádět s konkrétním úkolem a poskytování služby.
 
 - **Cloudové služby je o nasazení aplikace jako VMs.** Kód, který napíšete je pevně svázáno instanci OM, jako je webu nebo pracovního Role. Nasazení úlohu do cloudové služby je nasazení jednu nebo více instancí OM, spuštěné pracovní zátěž. Existuje bez oddělování aplikací a VMs a tak neexistuje formální definice aplikace. Aplikaci můžete považovat jako sady instancí webu nebo pracovního Role v rámci Cloudovým službám nasazení nebo celé nasazení Cloudovým službám. V tomto příkladu se zobrazuje aplikace jako sady instancí role.
 
![Cloudové služby aplikace a topologie][1]

 - **Služba struktury je o nasazení aplikace na existující VMs nebo počítačích se systémem struktury služby systému Windows nebo Linux.** Služby, kterou jste napsali jsou úplně oddělené ze zdrojových infrastruktury, které jsou pryč vyjádřeny tak, že platformu aplikace služby struktury tak, aby aplikace může být nasazené na více prostředí. Pracovní zátěž v služby struktury se nazývá "službu" a jeden nebo víc služeb jsou seskupená aplikace dříve definované uživatelem, která poběží na platformu služby struktury aplikace. Více aplikací může být nasazené na jednoho obrázku struktury služby.
 
![Aplikace služby struktury a topologie][2]
 
Služby struktury samotné je aplikační vrstva platformy, která poběží na Windows nebo Linux, zatímco Cloudovým službám systému pro nasazení Azure Správa přístupových práv VMs se úlohami připojené.
Model služby struktury aplikace má několik výhod:

 - Časy rychlého nasazení. Vytváření OM instancí může být časově náročný. V struktury služby jsou VMs nasazeny pouze po vytvoření clusteru, který hostuje platformu služby struktury aplikace. Od této chvíle může být nasazené balíčků aplikací na clusteru velmi rychle.
 - HD hostingu. Ve službě Cloud Services hostuje OM Role pracovníka jeden pracovní zátěž. Služba struktury aplikace způsoby nezávislý VMs spuštěné, což znamená, že nasadíte velkého počtu aplikací malým počtem poštovních VMs, které můžete snížit celkové náklady větší nasazení.
 - Struktury služby, které platform mohlo by umožnit spuštění kdekoliv, který má systému Windows Server a Linux počítačích, ať už jde Azure nebo místní. Platformu poskytuje vrstvu odběru prostřednictvím podkladových infrastruktury v různých prostředích mohlo by umožnit spuštění aplikace. 
 - Správa distribuované aplikace. Služba struktury je platformy, nejen hosts distribuované aplikace, ale taky pomáhá je spravovat jejich životního cyklu nezávisle na hostingu OM nebo životního cyklu počítače.

## <a name="application-architecture"></a>Architektura aplikací

Architektura aplikace Cloudovým službám obvykle obsahuje mnoho závislostí externí služby, jako je například Bus služby Azure tabulky a úložiště objektů Blob, SQL, Redis a ostatní ke správě dat aplikace a komunikace mezi Web a pracovní role v nasazení Cloudovým službám a vrátil. Příklad kompletní aplikaci Cloudovým službám může vypadat takto:  

![Cloud Services architektura][9]

Aplikace služby struktury můžete taky rozhodnete sdělit nám stejné externí služby do dokončení aplikací. V tomto příkladu Cloudovým službám architektura nejjednodušší cestu migrace z Cloudovým službám do služby struktury je nahrazení pouze Cloudovým službám nasazení aplikace služby struktury zachování celkové struktury stejné. Na webu a pracovní role můžete přenést služby struktury příslušnosti služeb s minimálními kód změny.

![Architektura služby struktury po jednoduché migraci][10]

V této fázi systému by měla fungovat stejně jako před. Díky funkcí služby struktury stavové, ukládá externí stavu můžete internalized jako stavová služba případně. Toto je složitější než jednoduchý migrace Web a pracovní rolí služby struktury příslušnosti služeb, protože vyžaduje psaní vlastní služby, které poskytují odpovídající funkce aplikace jako externí služby dřív. Výhody tím patří: 

 - Odebrání externích závislostí 
 - Sjednocení pocházejících nasazení, Správa a upgrade modely. 
 
Příklad výsledné architektura internalizing těchto služeb může vypadat takto:

![Architektura služby struktury po celou migraci][11]

## <a name="communication-and-workflow"></a>Komunikace a pracovního postupu

Většina cloudové služby aplikace se skládají z více než jednu úroveň. Aplikace služby struktury Podobně se skládá z více služeb (obvykle mnoho služby). Jsou dva běžné modely komunikace přímé komunikaci a prostřednictvím externího trvalé úložiště.

### <a name="direct-communication"></a>Přímé komunikace

Pomocí přímé komunikace úrovní komunikovat přímo prostřednictvím koncový bod zveřejněné pro každou úroveň. V příslušnosti prostředí například cloudové služby, tento prostředky výběr instanci roli OM buď náhodně nebo kruhového zůstatek zatížení a připojení k jeho koncového bodu přímo.

![Cloud Services přímé komunikace][5]

 Přímé komunikace je běžné komunikace modelu služby struktury. Klíčové rozdíl mezi služby struktury a Cloudovým službám je tento v Cloudovým službám připojíte k OM, že služba struktury umožňuje připojit se ke službě. Toto je důležité rozlišení z několika důvodů:

 - Služby v služby struktury nejsou vázaný na VMs hostujících služby může pohyb clusteru a ve skutečnosti očekává pohyb různých důvodů: zdrojů vyrovnávání, převzetí, aplikace a infrastruktury upgrady a umístění nebo k načtení omezení. To znamená, že můžete kdykoli změnit adresu instanci služby. 
 - OM v služby struktury můžete hostovat více služeb, oba objekty mají jedinečné koncové body.

Služba struktury poskytuje mechanismus zjišťování služby s názvem služba WINS, které lze použít k řešení adresy koncového bodu služby. 

![Přímé komunikace struktury služby][6]

### <a name="queues"></a>Fronty

Běžné mechanismus komunikace mezi úrovní v příslušnosti prostředí například Cloudovým službám je používat externí úložiště fronty k trvale ukládání pracovních úkolů z jedné úrovně do jiného. Běžné situace je web osy, které odesílá úlohy Azure fronty nebo Bus služby, kde můžete instancí pracovního Role dequeue a zpracování úloh.

![Cloud Services fronty komunikace][7]

Stejný model komunikace můžete použít v struktury služby. To může být užitečné při migraci stávající aplikace Cloud Services na struktury služby. 

![Přímé komunikace struktury služby][8]
 
## <a name="next-steps"></a>Další kroky

Nejjednodušší cestu migrace z Cloudovým službám do struktury služba je k nahrazení pouze Cloudovým službám nasazení aplikace služby struktury zachování celkové struktury aplikace přibližně stejné. Následující článek obsahuje Průvodce pomůže převést webu nebo pracovního rolí služby struktury příslušnosti služby.

 - [Jednoduchý migrace: převedení webu nebo pracovního rolí služby struktury příslušnosti služby](./service-fabric-cloud-services-migration-worker-role-stateless-service.md)

<!--Image references-->
[1]: ./media/service-fabric-cloud-services-migration-differences/topology-cloud-services.png
[2]: ./media/service-fabric-cloud-services-migration-differences/topology-service-fabric.png
[5]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-direct.png
[6]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-direct.png
[7]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-queues.png
[8]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-queues.png
[9]: ./media/service-fabric-cloud-services-migration-differences/cloud-services-architecture.png
[10]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-simple.png
[11]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-full.png
