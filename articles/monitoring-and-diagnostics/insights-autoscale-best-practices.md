<properties
    pageTitle="Doporučené postupy pro sledování Azure neobsahovaly text. | Microsoft Azure"
    description="Přečtěte si Principy efektivní použití neobsahovaly text v Azure Monitor."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="ashwink"/>

# <a name="best-practices-for-azure-monitor-autoscaling"></a>Doporučené postupy pro sledování Azure neobsahovaly text

V následujících částech v tomto dokumentu pomohou porozumět osvědčené postupy pro automatické měřítko Azure. Po zkontrolování tyto informace, byste měli lépe efektivně použít automatické měřítko v Azure infrastruktury.

## <a name="autoscale-concepts"></a>Automatické měřítko konceptů

- Zdroj může mít jenom *jeden* nastavení automatické měřítko
- Nastavení automatické měřítko může mít jeden nebo více profily a každého profilu můžete mít jedno nebo více pravidel automatické měřítko.
- Nastavení automatické měřítko měřítko instance vodorovně, tedy *se* zvýšením *případy a* zmenšením počet instancí.
 Nastavení automatické měřítko má maximum, minimum a výchozí hodnota instancí.
- Automatické měřítko úlohy vždy bude číst přidružené míru zobrazit jako, ověřování, zda má překročených nakonfigurované mezní škálování nebo měřítko. Zobrazení seznamu metrik této automatické měřítko můžete zobrazit jako na [běžné metriky Azure Monitor neobsahovaly text](insights-autoscale-common-metrics.md).
- Všechny mezní hodnoty se vypočítávají úrovni instance. Například "měřítko oddálit 1 instancí při průměrná využití procesoru > 80 % po počet instancí 2", znamená škálování průměr CPU ve všech instancích je větší než 80 %.
- Vždy dostávat oznámení o selhání prostřednictvím e-mailu. Konkrétně vlastníka, Přispěvatel a čtenáři prostředků cílové dostávat e-maily. Také vždy dostanete e-mail s *obnovení* automatické měřítko obnoví z selhání kdy a kdy začíná fungují normálně.
- Je můžete určovat, jestli se změnami upozornění úspěšné měřítko akce prostřednictvím e-mailu a webhooks.

## <a name="autoscale-best-practices"></a>Automatické měřítko doporučené postupy

Použijte následující doporučené postupy při použití automatické měřítko.

### <a name="ensure-the-maximum-and-minimum-values-are-different-and-have-an-adequate-margin-between-them"></a>Ujistěte se, maximální a minimální hodnotu se liší a mají odpovídající okraje mezi nimi
Pokud máte nastavení, které obsahuje minimálně = 2, maximální = 2 a aktuální instance počet je 2, může dojít nic neudělat měřítko. Zachovat odpovídající okraje mezi počty maximální a minimální instance, které jsou včetně. Automatické měřítko vždy mezi tyto limity měřítko.

### <a name="manual-scaling-is-reset-by-autoscale-min-and-max"></a>Ruční změna velikosti je obnovit po automatické měřítko min a max
Ruční aktualizaci počet instancí hodnoty nad nebo pod maximální velikost zpátky na minimum (Pokud se pod) a maximum (Pokud se nad) automaticky upraví modul automatické měřítko. Například vytvořit rozsahu 3 až 6. Pokud jste ještě jednou instancí pracovního, modul automatické měřítko měřítko na 3 instance při příštím spuštění. Podobně se by měřítko se změnami zpět 8 instance 6 při příštím spuštění.  Ruční změna velikosti je velmi dočasná, pokud nastavíte tato automatické měřítko pravidla.

### <a name="always-use-a-scale-out-and-scale-in-rule-combination-that-performs-an-increase-and-decrease"></a>Vždy používejte kombinaci škálování a měřítko pravidla, která provádí zvýšení a snížení
Pokud používáte jenom jedna část kombinace, automatické měřítko měřítko se změnami, jednoduché, nebo v, dokud maximum nebo minimum, dosažení.

### <a name="do-not-switch-between-the-azure-portal-and-the-azure-classic-portal-when-managing-autoscale"></a>Nepřepínat Azure portálem a portálu Azure klasické při správě automatické měřítko
Cloud Services a aplikace služeb (Web Apps) použijte portál Azure (portal.azure.com) můžete vytvořit a spravovat nastavení automatické měřítko. Virtuální počítač měřítko sady dosáhnete PoSH rozhraní příkazového řádku a rozhraní REST API můžete vytvořit a spravovat nastavení automatické měřítko. Nepřepínat portálu Azure klasické (manage.windowsazure.com) a portálu Azure (portal.azure.com) při správě konfigurace automatické měřítko. Azure klasické portálem a jeho základní back-end má omezení. Přejděte na portál Azure spravovat automatické měřítko pomocí grafického uživatelského rozhraní. Možnosti jsou použít automatické měřítko Powershellu, rozhraní příkazového řádku nebo rozhraní REST API (pomocí Průzkumníka zdroje Azure).

### <a name="choose-the-appropriate-statistic-for-your-diagnostics-metric"></a>Vyberte vhodné statistický metriky diagnostických nástrojů
Pro diagnostiku metriky můžete mezi *průměr*, *Minimum*, *maximální* a *celkové* jako metriky zobrazit jako. Nejběžnější statistický je *průměr*.

### <a name="choose-the-thresholds-carefully-for-all-metric-types"></a>Zvolte mezní hodnoty pečlivě pro všechny typy metrických
Doporučujeme pečlivě různých prahové hodnoty pro škálování a měřítko se změnami podle praktických situacích.

Jsme *nedoporučujeme* nastavení automatické měřítko jako v příkladech pod s stejné nebo velmi podobné mezní hodnoty v podmínky a:

- Zvětšení instance 1 počítat při počtu podprocesů < = 600
- Zmenšení instance o 1 počítat při počtu podprocesů > = 600

Podívejme se na příklad co může vést k činnost, která může zdát přehlednější. Cosider následující kroky.

1. Předpokládejme některých 2 na začátku a potom průměrný počet podprocesů na instanci zvětší tak, aby 625.
2. Automatické měřítko měřítko, přidání instanci 3.
3. Potom se předpokládá, že počet průměr podprocesů přes instance přejde k 575.
4. Před zmenšení, pokusí automatické měřítko odhadu jaké konečný stav bude, pokud zachován v. Například 575 × 3 (aktuální instance počet) = 1,725 / 2 (výsledný počet případy, kdy diagramů s měřítky) = 862.5 vlákna. To znamená, že automatické měřítko musel okamžitě škálování znovu i po měřítko, případně počet průměr podprocesů zůstane stejná i spadá jenom malou. Ale pokud zvětšování znovu celého procesu by opakovat, vést k nekonečné smyčce.
5. Chcete-li předejít tato situace (dále jen "flapping"), automatické měřítko není Neomezovat vůbec. Místo toho přeskočí a reevaluates podmínce při příštím, který spustí úlohy služby. Protože Automatické měřítko neobjeví pracovat, i když počet průměr vlákna byl 575 to může splést více lidem.

Odhad během měřítko se změnami se má vyhnout "flappy" situací. Toto chování vezměte v úvahu při volbě stejné prahové hodnoty pro škálování a v.

Doporučujeme odpovídající okraje mezi škálování a prahové hodnoty. Jako příklad zvažte následující lepší kombinace pravidlo.

- Zvětšení instance 1 počítat při využití procesoru % > = 80
- Zmenšení instance 1 počítat při využití procesoru % < = 60

V tomto případě  

1. Předpokládejme, že v některých 2 začínat.
2. Pokud průměrná využití procesoru % všech instancí půjde 80, automatické měřítko měřítko, přidání třetí instance.
3. Teď se předpokládá, že v čase využití procesoru % patří 60.
4. Automatické měřítko na měřítku v pravidlo vypočte konečný stav kdyby se měřítko se změnami. Například 60 × 3 (aktuální instance počet) = 180 / 2 (výsledný počet případy, kdy diagramů s měřítky) = 90. Tak, aby automatické měřítko není měřítko se změnami vzhledem k tomu, že by mělo škálování zase spustí. Místo toho přeskočí zmenšení.
5. Zkontroluje, další automatické časové měřítko procesoru dál tak, aby 50. Odhadne znovu - 50 × 3 instance = 150 / 2 instance = 75, což je pod škálování mezní 80, takže se změní v úspěšně na 2 instance.

### <a name="considerations-for-scaling-threshold-values-for-special-metrics"></a>Co byste měli zvážit pro změnu velikosti prahové hodnoty pro zvláštní metriky
 Speciální metriky například míru délka úložiště nebo Bus frontě je mezní hodnota je průměrný počet zpráv, které za číslo aktuální instance. Pečlivě vyberte zvolit mezní hodnota této metriky.

Pojďme ho ilustrují příklad zajistit srozumitelný lepší chování.

- Instance zvýšit 1 počet při úložiště fronty zpráva count > = 50
- Zmenšení instance o 1 počet při úložiště fronty zpráva počet < = 10

Zvažte následující kroky:

1. V některých 2 úložiště fronty.
2. Zachovat zprávy chodit a při kontrole fronty úložiště celkový počet různých hodnot přečte 50. Může předpokládá, že tento automatické měřítko by měl akce spustit pracovní postup škálování. Ale může se pořád 50/2 = 25 zpráv v rámci instance. Ano škálování nevyskytuje. První škálování stát třeba celkový počet zpráv ve frontě úložiště 100.
3. Potom se předpokládá, že celkový počet zpráv dosáhne 100.
4. 3 instance fronty úložiště je přidán kvůli škálování akce.  Další akce škálování nebude stát, dokud celkový počet zpráv ve frontě dosáhne 150, protože 150/3 = 50.
5. Teď zmenšovat počet zpráv. S 3 instance první měřítko akce se stane, když celkový počet zpráv ve všech frontách přidat do 30, protože 30/3 = 10 zpráv v rámci instance, což je prahové měřítko se změnami.

### <a name="considerations-for-scaling-when-multiple-profiles-are-configured-in-an-autoscale-setting"></a>Co byste měli zvážit pro změnu velikosti, když je v nastavení automatické měřítko nakonfigurováno více profilů

V nastavení automatické měřítko, můžete zvolit výchozí profil, který bude obecně použit bez všechny závislosti na plánu nebo čas, nebo zvolte profil opakované nebo profilu dobu určitou s rozsahem datum a čas.

Při automatické měřítko služby zpracuje, vždy kontroluje v následujícím pořadí:

1. Pevné datum profilu
2. Opakované profilu
3. Výchozí profil ("vždy")

Pokud splnění podmínky profilu automatické měřítko Neprovádět kontrolu další podmínky profilu pod ním. Automatické měřítko zpracovává pouze jeden profil najednou. To znamená, že pokud chcete zahrnout také podmínku zpracování z profilu nižší úrovně, musí obsahovat pravidla taky aktuální profil.

Pojďme si rozebrat to pomocí příkladu:

Na následujícím obrázku vidíte nastavení automatické měřítko pomocí výchozího profilu minimální instancí = 2 a maximální instance = 10. V tomto příkladu pravidla není nakonfigurován škálování při počtu zpráv ve frontě je větší než 10 a měřítko se změnami při počtu zpráv ve frontě je menší než 3. Nyní můžete změnit zdroj měřítko mezi instancemi 2 až 10.

Kromě toho je opakované profil pondělí. Nastavte minimální instancí = 2 a maximální instance = 12. To znamená v pondělí, první automatické časové měřítko hledá tuto podmínku Pokud počet instancí 2, nastaví nové nejméně 3. Dokud automatické měřítko nadále najít tato podmínka profilu obsažena (pondělí), pouze zpracuje tato procesoru měřítko mimo/se změnami pravidla nakonfigurován pro tento profil. V současné době Neprovádět kontrolu dobu fronty. Ale podle potřeby můžete taky podmínka délky fronty kontrolovaná byste měli zahrnout pravidla z výchozího profilu i ve vašem profilu pondělí.

Podobně při automatické měřítko přepne zpět do výchozího profilu, nejdřív zkontroluje, pokud jsou splněny minimální a maximální podmínky. Pokud je číslo instancí v době 12, změní se v až 10 maximální povolenou pro výchozí profil.

![nastavení automatické měřítko](./media/insights-autoscale-best-practices/insights-autoscale-best-practices.png)

### <a name="considerations-for-scaling-when-multiple-rules-are-configured-in-a-profile"></a>Co byste měli zvážit pro změnu velikosti při nastavení více pravidel v profilu
Jsou případy, kdy budete muset nastavit více pravidel v profilu. Následující sady pravidel automatické měřítko se vyvolají pomocí služby nastavená pro používání více pravidel.

Automatické měřítko *měřítko,*spuštěn pokud došlo ke splnění žádné pravidlo.
*Měřítko se změnami*je nutné automatické měřítko všechna pravidla splnění.

Ke znázornění, se předpokládá, že máte následující pravidla 4 automatické měřítko:

- Pokud procesoru < 30 % měřítko-1
- Pokud paměti < 50 %, měřítko-1
- Pokud využití procesoru > 75 % škálování 1
- Pokud paměti > 75 % škálování 1

Klikněte sledovat důsledky:

- Pokud se sloupcem procesor 76 % a Memory is 50 %, jsme škálování.
- Pokud je procesoru 50 % a Memory is 76 % jsme škálování.

Na druhé straně Pokud procesoru je 25 % a paměti automatické 51 % měřítko znamená **není** měřítko se změnami. Za účelem měřítko se změnami, procesoru musí být 29 % a paměti 49 %.

### <a name="always-select-a-safe-default-instance-count"></a>Vždy vyberte bezpečných výchozí instanci počet
Výchozí instanci počet je důležité, automatické měřítko přizpůsobí služby, které jsou započteny metriky nebudou k dispozici. Proto vyberte výchozí instanci je tento počet bezpečné pro váš úloh.

### <a name="configure-autoscale-notifications"></a>Konfigurace oznámení na automatické měřítko
Automatické měřítko s upozorněním správci a přispěvatelům zdroje e-mailem Pokud dojde k některé z následujících podmínek:

- Automatické měřítko služby se nezdaří, čeká zpracování.
- Metriky nejsou k dispozici pro automatické měřítko služby rozhodování měřítko.
- Metriky jsou k dispozici (obnovení) znovu rozhodnout měřítko.
Kromě toho výše uvedených podmínek můžete nakonfigurovat e-mailu nebo webhook oznámení pro oznámení pro úspěšné měřítko akce.
