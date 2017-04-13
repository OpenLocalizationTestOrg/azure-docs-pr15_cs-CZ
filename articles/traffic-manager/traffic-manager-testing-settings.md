<properties
    pageTitle="Testování nastavení správce přenosy | Microsoft Azure"
    description="Tento článek vám pomůže otestovat nastavení přenosy správce"
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
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="test-your-traffic-manager-settings"></a>Otestujte svoje nastavení správce dopravy

K otestování nastavení přenosy správce, musíte mít více klientů na různých místech, ze kterých můžete spustit testů. Přepněte koncové body ve vašem profilu přenosy správce dolů postupně po jednom.

* Nastavte hodnotu DNS TTL zhoršeným tak, aby změny šířit rychle (například 30 sekund).
* Znáte IP adresy Azure cloudovými službami a weby v profil, který testujete.
* Pomocí nástrojů, které vám umožní překladu názvu DNS na IP adresu a zobrazení, které budou řešit.

Při kontrole zobrazíte, že názvy DNS překládal IP adresy koncové body ve svém profilu. Jména by měla vyřešit v souladu s metodu směrování přenosu v profilu přenosy správce definované. Pomocí nástroje **nslookup** nebo **dostanete** jmen DNS.

Následující příklady umožňují testování profilu přenosy správce.

### <a name="check-traffic-manager-profile-using-nslookup-and-ipconfig-in-windows"></a>Kontrola profil správce přenosy pomocí nástroje nslookup a ipconfig v systému Windows

1. Jako správce otevřete příkazu nebo příkazovém řádku prostředí Windows PowerShell.
2. Typ `ipconfig /flushdns` vyprázdnění mezipaměti překládání DNS.
3. Typ `nslookup <your Traffic Manager domain name>`. Zkontroluje, například následující příkaz název domény u předponu *myapp.contoso*

        nslookup myapp.contoso.trafficmanager.net

    Typické výsledek zobrazí následující informace:

    * Název DNS a IP adresu serveru DNS přistupuje Pokud chcete vyřešit tento název domény přenosy správce.
    * Název domény přenosy správce, jste zadali do příkazového řádku po "nslookup" a IP adresu, na který řeší přenosy správce domény. Druhá IP adresu je důležité ke kontrole. Měla by se shodovat veřejné virtuální adresu IP (VIP) pro jeden z cloudovým službám nebo weby v profilu přenosy správce, který testujete.

## <a name="how-to-test-the-failover-traffic-routing-method"></a>Jak otestovat metodu směrování přenosu překlopení

1. Nechte všechny koncové body nahoru.
2. Použití jednoho klienta, požádat o služeb překládá názvy DNS pro název domény společnosti pomocí nástroje nslookup nebo podobného nástroje.
3. Zajistěte, aby odpovídal přeložit IP adresu primární koncového bodu.
4. Snížilo primární koncový bod nebo odebrat sledování soubor tak, aby tento přenosy správce myslí, že je aplikace dolů.
5. Čekat DNS čas to-Live (TTL) profil správce přenosy plus další dvě minuty. Třeba při DNS TTL 300 sekund (5 minut), je nutné počkat sedm minut.
6. Vyprázdnění DNS klienta mezipaměti a žádost o DNS rozlišení pomocí nástroje nslookup. Ve Windows můžete vyprázdněte mezipaměť DNS pomocí příkazu ipconfig/flushdns.
7. Zajistěte, aby odpovídal přeložit IP adresu sekundární koncového bodu.
8. Opakujte postup jejich přenesením dolů každý koncový bod zase. Ověřte, zda DNS vrací IP adresu další koncového bodu v seznamu. Jakmile nastavíte všechny koncové body dolů, měli znova získá IP adresu primární koncového bodu.

## <a name="how-to-test-the-weighted-traffic-routing-method"></a>Jak otestovat metodu směrování váženého přenosy

1. Nechte všechny koncové body nahoru.
2. Použití jednoho klienta, požádat o služeb překládá názvy DNS pro název domény společnosti pomocí nástroje nslookup nebo podobného nástroje.
3. Ujistěte se, že přeložit IP adresu, který odpovídá některému z vaší koncové body.
4. Vyprázdněte mezipaměť DNS klienta a opakujte kroky 2 a 3 pro každý koncový bod. Měli byste vidět různé IP adres pro každou z vaší koncové body vrací.

## <a name="how-to-test-the-performance-traffic-routing-method"></a>Jak otestovat metodu směrování přenosu výkonu

Efektivní otestovat metodu směrování přenosu výkonu, musíte mít klienty umístěné v různých částech na světě. Klienti můžete vytvořit v různých Azure oblastech, které lze použít k testování svých služeb. Pokud máte globální síť, můžete vzdáleně Přihlaste se k klienty v dalších částech světa a spustit testy odtud.

Můžete taky uvolnit webová vyhledání DNS a se dostanete službách. Některé z těchto nástrojů umožnit můžete zkontrolovat překládá názvy DNS název z různých míst po celém světě. Vyhledávat na "Vyhledávání DNS" příklady. Služeb třetí strany jako Gomez nebo hlavní mohou sloužit k potvrzení, že jsou profily distribuce přenosy podle očekávání.

## <a name="next-steps"></a>Další kroky

* [O směrování metod přenosy přenosy správce](traffic-manager-routing-methods.md)
* [Důležité informace o výkonu přenosy správce](traffic-manager-performance-considerations.md)
* [Poradce při potížích přenosy správce stavu sníženou kvalitu.](traffic-manager-troubleshooting-degraded.md)




