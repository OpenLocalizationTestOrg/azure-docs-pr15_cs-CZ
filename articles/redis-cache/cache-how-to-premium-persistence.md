<properties 
    pageTitle="Postup při konfiguraci trvání dat pro mezipaměť Redis Premium Azure" 
    description="Zjistěte, jak konfigurovat a spravovat trvání dat instance Azure Redis mezipaměti vašeho Premium osy" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-data-persistence-for-a-premium-azure-redis-cache"></a>Postup při konfiguraci trvání dat pro mezipaměť Redis Premium Azure

Azure Redis mezipaměti má nabídky různých mezipaměti, které poskytují flexibilitu při volbě mezipaměti, velikosti a funkcí, včetně nové osy Premium.

Vrstvy premium Azure Redis mezipaměti obsahuje funkce, jako jsou clusterů, trvalé a podpora virtuální sítě. Tento článek popisuje postup při konfiguraci trvalé v instanci premium Azure Redis mezipaměti.

Informace o dalších prémiových funkcí mezipaměti najdete v článku [Úvod Azure Redis mezipaměti Premium osy](cache-premium-tier-intro.md).

## <a name="what-is-data-persistence"></a>Co je trvání dat?
Trvalé redis umožňuje uchovávat data uložená v Redis. Můžete taky pořizovat snímky a obecnějším údajům data, která můžete načíst když se nepovede hardwaru. Toto je velmi velké využít přes Basic nebo standardní osy, kde jsou data uložena v paměti a může být možnou ztrátu dat, když se nepovede, kde jsou mezipaměti uzlů dolů. 

Azure Redis mezipaměti nabízí Redis trvalé pomocí [RDB modelu](http://redis.io/topics/persistence), kde jsou data uložena v účet Azure úložiště. Při konfiguraci trvalé Azure Redis mezipaměti potrvají snímek mezipaměti Redis v binárním formátu Redis podle frekvenci konfigurovatelné záložní disk. Pokud dojde k kritické události, která zakáže primární a otevřené mezipaměti, mezipaměti nepřijímal pomocí poslední snímek.

Trvalé je možné konfigurovat z zásuvné **Novou mezipaměť Redis** při vytváření mezipaměti a na zásuvné **Nastavení** pro stávající mezipaměti premium.

## <a name="create-a-premium-cache"></a>Vytvořit mezipaměť premium

Můžete vytvořit mezipaměť a nakonfigurovat trvalé, přihlaste se k [portálu Azure](https://portal.azure.com) a klikněte na **Nový**->**dat + úložiště**>**Redis mezipaměti**.

![Vytvoření Redis mezipaměti][redis-cache-new-cache-menu]

Abyste mohli nakonfigurovat trvalé, nejdřív vyberte některou z mezipaměti **Premium** v zásuvné **Zvolte ceny osy** .

![Zvolte ceny osy][redis-cache-premium-pricing-tier]

Po výběru premium ceny vrstva klikněte na **Redis trvalé**.

![Redis trvalé][redis-cache-persistence]

Kroků uvedených v následující části popisují, jak nakonfigurovat Redis trvalé na novou mezipaměť premium. Po konfiguraci Redis trvalé klikněte na **vytvořit** vytvoříte novou mezipaměť premium s Redis trvalé.

## <a name="configure-redis-persistence"></a>Konfigurace Redis trvalé

Redis trvalé je nakonfigurovaný na zásuvné **Redis trvání dat** . Pro nové mezipaměti je tento zásuvné přístup při proces vytváření mezipaměti podle popisu v předchozí části. Pro existující mezipaměti zásuvné **Redis trvání dat** je zobrazená po kliknutí na zásuvné **Nastavení** pro mezipaměť.

![Redis nastavení][redis-cache-settings]

K povolení Redis doby trvání, klikněte na možnost **Povolit** povolit zálohování RDB (Redis databáze). Jak zakázat Redis trvalé mezipaměti dříve povolené premium, klikněte na **Zakázané**.

Pokud chcete konfigurovat interval zálohování, vyberte z rozevíracího seznamu **Záložní četnosti** . Možnosti zahrnují **15 minut**, **30 minut**, **60 minut**, **6 hodin**, **12 hodin**a **24 hodin**. Tento interval počítá po předchozím zálohování úspěšném dokončení a když ji uplyne zálohu zahájení.

Klikněte na **Úložiště účtu** vyberte účet úložiště, který chcete použít a vyberte buď **primárního klíče** nebo **vedlejší klíč** z rozevíracího seznamu **Klíč úložiště** . Účet úložiště musí zvolte ve stejné oblasti jako mezipaměti a účet **Úložiště Premium** se nedoporučuje, protože úložiště premium má vyšší výkon. 

>[AZURE.IMPORTANT] Pokud obnovených klávesu úložiště pro váš účet trvalé musí rechoose požadované klíč z rozevíracího seznamu **Klíč úložiště** .

![Redis trvalé][redis-cache-persistence-selected]

Kliknutím na **OK** uložte Konfigurace trvalé.

Další zálohování (nebo první zálohování pro nové mezipaměti) je, které iniciuje po uplyne záložní četnosti.



## <a name="persistence-faq"></a>Trvalé časté otázky

V tomto seznamu najdete odpovědi na nejčastější dotazy týkající se trvalé Azure Redis mezipaměti.

-   [Můžete povolit trvalé dříve vytvořené mezipaměti?](#can-i-enable-persistence-on-a-previously-created-cache)
-   [Můžete změnit po vytvoření mezipaměti záložní počet_plateb?](#can-i-change-the-backup-frequency-after-i-create-the-cache)
-   [Proč mám záložní četnost 60 minut dojde více než 60 minut mezi zálohy?](#why-if-i-have-a-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups)
-   [Co se stane staré zálohy, když je nastavená jako zálohu?](#what-happens-to-the-old-backups-when-a-new-backup-is-made)
-   [Co se stane, když mám nastavit na jinou velikost a zálohy se obnoví, který byl před měřítka operací?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)

### <a name="can-i-enable-persistence-on-a-previously-created-cache"></a>Můžete povolit trvalé dříve vytvořené mezipaměti?

Ano, Redis trvalé je možné konfigurovat při vytvoření mezipaměti i na stávající mezipaměti premium.

### <a name="can-i-change-the-backup-frequency-after-i-create-the-cache"></a>Můžete změnit po vytvoření mezipaměti záložní počet_plateb?

Zálohování četnost na zásuvné **Redis trvání dat** Ano, můžete změnit. Pokyny najdete v tématu [Konfigurace Redis trvalé](#configure-redis-persistence).

### <a name="why-if-i-have-a-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups"></a>Proč mám záložní četnost 60 minut dojde více než 60 minut mezi zálohy?

Interval záložní počet_plateb nespustí, dokud proces předchozím zálohování byla úspěšně dokončena. Pokud je záložní počet_plateb 60 minut a bude trvat proces zálohování 15 minut úspěšně, následující zálohování nezačne 75 minut po počáteční čas dané předchozí zálohy.

### <a name="what-happens-to-the-old-backups-when-a-new-backup-is-made"></a>Co se stane staré zálohy, když je nastavená jako zálohu?

Všechny zálohy kromě nejnovějšího se automaticky odstraní. Toto odstranění nemusí okamžitě stát, ale nejsou starší zálohy donekonečna udržovat zachován.

### <a name="what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation"></a>Co se stane, když mám nastavit na jinou velikost a zálohy se obnoví, který byl před měřítka operací?

-   Pokud budete mít diagramů s měřítky větší velikost, podívejte se žádný vliv.
-   Pokud je měřítko menší velikost a máte vlastní [databází](cache-configure.md#databases) nastavení, která je větší než [omezit databáze](cache-configure.md#databases) pro váš nový velikost dat v těchto databáze není možné obnovit. Další informace najdete v tématu [je svůj vlastní nastavení problémového během měřítka databáze?](cache-how-to-scale.md#is-my-custom-databases-setting-affected-during-scaling)
-   Pokud je měřítko menší velikost a není dost místa v menší velikost pro uložení všech dat z posledního zálohování, bude v průběhu obnovení obvykle použití zásad vyřazení [allkeys lru](http://redis.io/topics/lru-cache) vyloučení klíče.

## <a name="next-steps"></a>Další kroky
Naučte se používat více prémiových funkcí mezipaměti.

-   [Úvod k osy Azure Redis mezipaměti Premium](cache-premium-tier-intro.md)
  
<!-- IMAGES -->

[redis-cache-new-cache-menu]: ./media/cache-how-to-premium-persistence/redis-cache-new-cache-menu.png

[redis-cache-premium-pricing-tier]: ./media/cache-how-to-premium-persistence/redis-cache-premium-pricing-tier.png

[redis-cache-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-persistence.png

[redis-cache-persistence-selected]: ./media/cache-how-to-premium-persistence/redis-cache-persistence-selected.png

[redis-cache-settings]: ./media/cache-how-to-premium-persistence/redis-cache-settings.png
