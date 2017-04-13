<properties
    pageTitle="Konfigurace metody směrování přenosu správce | Microsoft Azure"
    description="Tento článek vysvětluje, jak Konfiguruje směrování způsoby ve Správci přenosy"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="configure-traffic-manager-routing-methods"></a>Konfigurace metody směrování přenosu správce

Azure přenosy správce nabízí tři směrování způsoby, kterými se řídí směrování přenosu pro koncové body dostupnými službami. Metoda směrování přenosu bude použit pro všechny dotazy DNS dostali rozhodnout, jaký koncový bod má být vrácena v odpovědi na DNS.

Třemi způsoby přenosy směrování k dispozici ve Správci přenosy:

- **Priority (priorita):** Když budete chtít použít koncový bod primární služby a zadejte zálohy v případě primární nebude dostupná, vyberte "Prioritu".
- **Váha:** Vyberte "váha" když budete chtít distribuce přenosy přes sadu koncové body, buď rovnoměrně nebo podle gramáží, které můžete definovat.
- **Výkonu:** "Výkonu" vyberte, pokud se koncové body v různých zeměpisné lokality a chcete koncoví uživatelé používat "nejbližší" koncový bod z hlediska nejnižší latence sítě.

## <a name="configure-priority-routing-method"></a>Konfigurace metodu směrování priority (priorita)

Bez ohledu na režim webu Azure weby už poskytují překlopení funkcí pro weby v rámci datacentra (neboli oblast). Přenosy správce poskytuje převzetí weby v jiném datacentrech.

Běžné vzor pro službu převzetí je směrování přenosů na primární služby a poskytují sadu identickými záložní služby pro překlopení. Následující kroky popisují, jak nakonfigurovat tento uspořádaný převzetí s Azure cloudovými službami a weby:

1. Azure klasické portálu v levém podokně klikněte na ikonu **Přenosy správce** otevřete podokno přenosy správce.
2. V podokně přenosy správce na portálu Azure klasické vyhledejte správce přenosy profil, který obsahuje nastavení, které chcete změnit. Otevřete stránku nastavení profilu, klikněte na šipku vpravo od názvu profilu.
3. Na stránce vašeho profilu klikněte na **koncové body** v horní části stránky. Ověřte, zda cloudovými službami a weby, které chcete zahrnout do konfiguraci.
4. Klikněte na **Konfigurovat** nahoře a otevře se stránka konfigurace.
5. **Způsob nastavení směrování přenosu**zkontrolujte, že metoda směrování přenosu **překlopení**. Pokud není, klikněte v rozevíracím seznamu **překlopení** .
6. **Seznamu priorita převzetí**upravte pořadí převzetí koncových bodů. Po výběru metodu směrování přenosu **převzetí** záleží na pořadí vybraných koncové body. Primární koncový bod je zcela nahoře. Nahoru nebo přesunout dolů změňte pořadí podle potřeby. Další informace o tom, jak nastavte prioritu převzetí pomocí Windows Powershellu najdete v článku [Set-AzureTrafficManagerProfile](http://go.microsoft.com/fwlink/p/?LinkId=400880).
7. Ověřte, že je **Sledování nastavení** nakonfigurované řádně podporovat. Sledování zaručuje, že koncové body, které jsou v režimu offline neodesílají přenosy. Sledování koncové body, zadejte cestu a název souboru. Lomítko "/" je platnými položkami pro relativní cestu a znamená, že soubor databáze v kořenovém adresáři (výchozí).
8. Po dokončení změny konfigurace, klikněte na **Uložit** v dolní části stránky.
9. Otestujte změny v konfiguraci.
10. Až svůj profil správce přenosy pracuje, upravte záznam DNS na autoritativní server DNS tak, aby ukazovaly na název domény přenosy správce názvu domény společnosti.

## <a name="configure-weighted-routing-method"></a>Konfigurace váženého metodu směrování

Běžné přenosy směrování metoda vzorek je poskytují sadu identickými koncové body, které zahrnují cloudovými službami a weby, a směrování přenosů na jednotlivá pole v kruhového způsobem. Následující kroky popisují, jak nakonfigurovat tento typ metodu směrování přenosu.

>[AZURE.NOTE] Azure weby už poskytují funkcí pro weby v rámci datacentra (neboli oblast) Vyrovnávání zatížení kruhového. Přenosy správce vám umožní určit metodu směrování přenosu kruhového weby v jiném datacentrech.

1. Azure klasické portálu v levém podokně klikněte na ikonu **Přenosy správce** otevřete podokno přenosy správce.
2. V podokně přenosy správce vyhledejte správce přenosy profil, který obsahuje nastavení, které chcete změnit. Otevřete stránku nastavení profilu, klikněte na šipku vpravo od názvu profilu.
3. Na stránce profilu klikněte na **koncové body** v horní části stránky a ověřit, zda koncové body služby, které chcete zahrnout do konfiguraci.
4. Na stránce vašeho profilu klikněte na **Konfigurovat** nahoře a otevře se stránka konfigurace.
5. **Metoda směrování přenosu nastavení**zkontrolujte, že metoda směrování přenosu **kruhového**. Pokud není, klikněte v rozevíracím seznamu **kruhového** .
6. Ověřte, že je **Sledování nastavení** nakonfigurované řádně podporovat. Sledování zaručuje, že koncové body, které jsou v režimu offline neodesílají přenosy. Sledování koncové body, zadejte cestu a název souboru. Lomítko "/" je platnými položkami pro relativní cestu a znamená, že soubor databáze v kořenovém adresáři (výchozí).
7. Po dokončení změny konfigurace, klikněte na **Uložit** v dolní části stránky.
8. Otestujte změny v konfiguraci.
9. Až svůj profil správce přenosy pracuje, upravte záznam DNS na autoritativní server DNS tak, aby ukazovaly na název domény přenosy správce názvu domény společnosti.

## <a name="configure-performance-traffic-routing-method"></a>Konfigurace metodu směrování přenosu výkonu

Metoda směrování přenosu výkonu umožňuje směrovaly do koncového bodu s nejnižší latence z klienta sítě. Obvykle datacentra s nejnižší latence probíhá nejbližší zeměpisné vzdálenost. Tento způsob směrování přenosu nelze účet pro změny v reálném čase v konfiguraci sítě nebo načíst.

1. Azure klasické portálu v levém podokně klikněte na ikonu **Přenosy správce** otevřete podokno přenosy správce.
2. V podokně přenosy správce vyhledejte správce přenosy profil, který obsahuje nastavení, které chcete změnit. Otevřete stránku nastavení profilu, klikněte na šipku vpravo od názvu profilu.
3. Na stránce profilu klikněte na **koncové body** v horní části stránky a ověřit, zda koncové body služby, které chcete zahrnout do konfiguraci.
4. Na stránce profilu klikněte na **Konfigurovat** nahoře a otevře se stránka konfigurace.
5. **Způsob nastavení směrování přenosu**zkontrolujte, že metoda směrování přenosu * *výkonu*. Pokud není, klikněte na * *výkonu** v rozevíracím seznamu.
6. Ověřte, že je **Sledování nastavení** nakonfigurované řádně podporovat. Sledování zaručuje, že koncové body, které jsou v režimu offline neodesílají přenosy. Sledování koncové body, zadejte cestu a název souboru. Lomítko "/" je platnými položkami pro relativní cestu a znamená, že soubor databáze v kořenovém adresáři (výchozí).
7. Po dokončení změny konfigurace, klikněte na **Uložit** v dolní části stránky.
8. Otestujte změny v konfiguraci.
9. Až svůj profil správce přenosy pracuje, upravte záznam DNS na autoritativní server DNS tak, aby ukazovaly na název domény přenosy správce názvu domény společnosti.

## <a name="next-steps"></a>Další kroky

* [Správa profilů přenosy správce](traffic-manager-manage-profiles.md)
* [Metody směrování přenosu správce](traffic-manager-routing-methods.md)
* [Testování nastavení přenosy správce](traffic-manager-testing-settings.md)
* [Internetovou doménu společnosti přejděte na správce přenos domény](traffic-manager-point-internet-domain.md)
* [Správa koncové body přenosy správce](traffic-manager-manage-endpoints.md)
* [Poradce při potížích přenosy správce stavu sníženou kvalitu.](traffic-manager-troubleshooting-degraded.md)
