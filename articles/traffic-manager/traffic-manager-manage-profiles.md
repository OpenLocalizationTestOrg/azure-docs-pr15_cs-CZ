<properties
    pageTitle="Správa profilů Azure přenosy správce | Microsoft Azure"
    description="Tento článek usnadňuje vytváření, zakázat, povolit, odstranění a zobrazení historie profil Azure přenosy správce."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="manage-an-azure-traffic-manager-profile"></a>Správa profil správce přenosy Azure

Přenosy správce profily způsobem směrování přenosu řízení distribuce umožnění datových přenosů do cloudu služeb nebo koncové body webu. Tento článek vysvětluje, jak vytvářet a spravovat tyto profily.

## <a name="create-a-traffic-manager-profile-using-quick-create"></a>Vytvoření profilu přenosy správce pomocí vytvořit

Profil správce přenosy rychle vytvoříte pomocí vytvořit v Azure klasické portálu. Rychlé vytvoření umožňuje vytvářet profily s základní nastavení. Však nelze použít vytvořit nastavení, jako například sadu koncové body (cloudovými službami a weby), převzetí pořadí metodu směrování přenosu převzetí nebo nastavení sledování. Po vytvoření profilu, můžete nakonfigurovat nastavení Azure klasické portálu. Přenosy správce podporuje maximálně 200 koncové body za profilu. Ale většina použití nevyžadují jenom některých koncové body.

### <a name="to-create-a-traffic-manager-profile"></a>Vytvoření profilu přenosy správce

1. **Nasazení cloudovými službami a weby v provozním prostředí.** Další informace o cloudové služby najdete v článku [Cloudovým službám](http://go.microsoft.com/fwlink/p/?LinkId=314074). Další informace o webech najdete v tématu [weby](http://go.microsoft.com/fwlink/p/?LinkId=393327).

2. **Přihlaste se k portálu Azure klasické.** Klikněte na tlačítko **Nový** v levém dolním rohu na portálu, klikněte na **síťové služby > Správce přenosy**a potom klikněte na **Vytvořit** spustíte konfiguraci vašeho profilu.
3. **Konfigurace předponou DNS.** Zadejte svůj profil správce přenosy jedinečné DNS předpony. Můžete zadat jenom předpony pro název domény přenosy správce.
4. **Vyberte předplatné.** Vyberte vhodné Azure předplatné. Každý profil je přidružená k jedné předplatné. Pokud máte jenom jedno předplatné, tato možnost nezobrazí.
5. **Vyberte metodu směrování přenosu.** Vyberte metodu směrování přenosu v **přenosy směrování zásad**. Další informace o směrování metody přenosu najdete v článku [o správce přenosy přenosy směrování metody](traffic-manager-routing-methods.md).
6. **Klikněte na "Vytvořit" v profilu**. Po dokončení konfigurace profilu můžete vyhledat profilu v podokně přenosy správce na portálu Azure klasické.
7. **Na portálu Azure klasické konfigurace koncové body, sledování a další nastavení.** Pomocí rychlé vytvoření konfiguruje pouze základní nastavení. Je třeba konfigurovat další nastavení, například seznam koncové body a pořadí převzetí koncového bodu.


## <a name="disable-enable-or-delete-a-profile"></a>Zakázání, povolení nebo odstranění profilu

Existujícího profilu můžete zakázat tak tento přenosy správce neodkazuje na nakonfigurováno koncové body koncových uživatelů. Při vypnutí profil správce přenosy profilu informace obsažené v profilu zůstane nedotčený a může upravovat v rozhraní přenosy správce.  Odkazy k životopisu při zapnutí profilu. Při vytváření profil přenosy správce na portálu Azure klasické automaticky zapnutá. Pokud se rozhodnete, že profil je už není potřebné, můžete ho odstranit.

### <a name="to-disable-a-profile"></a>Jak zakázat profilu

1. Pokud používáte vlastní název domény, změňte záznam CNAME na Internet DNS server tak, aby ukazoval už do vlastního profilu přenosy správce.
2. Přenosy se zastaví byli přesměrováni koncové body prostřednictvím nastavení profilu přenosy správce.
3. Vyberte profil, který chcete zakázat. Na stránce Správce přenosy zvýraznění kliknutím do sloupce vedle název profilu na profil. Poznámka: Klepnutím na název profilu nebo na šipku vedle názvu otevře na stránce nastavení profilu.
4. Po výběru profilu, klikněte na **Zakázat** v dolní části stránky.

### <a name="to-enable-a-profile"></a>Chcete-li povolit profilu

1. Vyberte profil, který chcete zakázat. Na stránce Správce přenosy zvýraznění kliknutím do sloupce vedle název profilu na profil. Poznámka: Klepnutím na název profilu nebo na šipku vedle názvu otevře na stránce nastavení profilu.
2. Po výběru profilu, zaškrtněte políčko **Povolit** v dolní části stránky.
3. Pokud používáte vlastní název domény, vytvořte záznam CNAME zdroje na Internet DNS server tak, aby ukazovaly na název domény pro svůj profil správce dopravy.
4. Přenosy přesměrován koncové body znovu.

### <a name="to-delete-a-profile"></a>Chcete-li odstranit profilu

1. Zajistit, aby záznam o prostředku DNS na Internetu DNS server už používala zdroje záznam CNAME, který odkazuje na název domény pro svůj profil správce dopravy.
2. Vyberte profil, který chcete zakázat. Na stránce Správce přenosy zvýraznění kliknutím do sloupce vedle název profilu na profil. Poznámka: Klepnutím na název profilu nebo na šipku vedle názvu otevře na stránce nastavení profilu.
3. Po výběru profilu, klikněte na **Odstranit** v dolní části stránky.

## <a name="view-traffic-manager-profile-change-history"></a>Historie změn zobrazit přenosy správce profilu

Zobrazit historii změn pro svůj profil správce přenosy na Azure klasické portálu Správa přístupových.

### <a name="to-view-your-traffic-manager-change-history"></a>Chcete-li zobrazit historii změn přenosy správce

1. V levém podokně portálu Azure klasické klikněte na **Správa služby**.
2. Na stránce Správa přístupových klikněte na **Protokoly operací**.
3. Na stránce protokoly operací můžete filtrovat zobrazíte historii změn pro svůj profil správce dopravy. Po výběru možnosti filtrování, klikněte na políčko pro zobrazení výsledků.

   - Chcete-li zobrazit změny pro všechny vaše profily, vyberte svoje předplatné a časový rozsah a zvolte **Přenosy správce** z místní nabídky **typu** .
   - Pokud chcete filtrovat podle název profilu, zadejte název profilu v poli **Název služby** nebo vyberte z místní nabídky.
   - Zobrazit podrobnosti každé jednotlivé změny, vyberte řádek se změny, které si přejete zobrazit a klepněte na tlačítko **podrobností** v dolní části stránky. V okně **Podrobnosti operace** můžete zobrazit XML reprezentaci rozhraní API objekt, který byl vytvořila nebo aktualizovala jako součást operace.

## <a name="next-steps"></a>Další kroky

- [Přidání koncový bod](traffic-manager-endpoints.md)
- [Konfigurace metodu směrování překlopení](traffic-manager-configure-failover-routing-method.md)
- [Konfigurace metodu směrování kruhového](traffic-manager-configure-round-robin-routing-method.md)
- [Konfigurace metodu směrování výkonu](traffic-manager-configure-performance-routing-method.md)
- [Internetovou doménu společnosti přejděte na název domény přenosy správce](traffic-manager-point-internet-domain.md)
- [Poradce při potížích přenosy správce stavu sníženou kvalitu.](traffic-manager-troubleshooting-degraded.md)