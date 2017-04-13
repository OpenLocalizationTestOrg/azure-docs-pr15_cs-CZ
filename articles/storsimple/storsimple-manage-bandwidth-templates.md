<properties
   pageTitle="Správa šablon šířky pásma StorSimple | Microsoft Azure"
   description="Popisuje, jak spravovat StorSimple šířky pásma šablon, které umožňují řídit šířku pásma."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-storsimple-bandwidth-templates"></a>Správa šablon šířky pásma StorSimple pomocí služby StorSimple správce

## <a name="overview"></a>Základní informace

Šablony šířky pásma umožňují konfigurovat využití šířky pásma sítě mezi více času den plánů úroveň dat ze zařízení StorSimple do cloudu.

Pomocí omezení plány šířky pásma máte tyto možnosti:

- Zadejte vlastní šířku pásma plány v závislosti na použití sítě zátěží na projektu.

- Soustředit správy a opětovné použití kalendáře na několika zařízeních snadno a bezproblémové způsobem.

> [AZURE.NOTE] Tato funkce je dostupná jenom pro zařízení fyzicky StorSimple a ne pro virtuální zařízení.

Všechny šablony šířky pásma pro službu jsou zobrazeny ve formátu tabulky a obsahují následující informace:

- **Název** – jedinečný název přiřazené k šabloně šířky pásma, když byl vytvořen.

- **Plán** – počet plány obsažené v šabloně dané šířky pásma.

- **Používá** – počet svazky pomocí šablon šířky pásma.

Stránka **Konfigurovat** StorSimple Správce služby Azure klasické portálu umožňuje spravovat šířky pásma šablony.

Taky můžete najít další informace, které vám pomohou při konfiguraci šířky pásma šablony:

- Otázky a odpovědi šablony šířky pásma
- Doporučené postupy pro šířku pásma šablony

## <a name="add-a-bandwidth-template"></a>Přidání šablony šířky pásma

Proveďte následující kroky k vytvoření nové šablony šířky pásma.

#### <a name="to-add-a-bandwidth-template"></a>Přidání šablony šířky pásma

1. Na stránce **Konfigurovat** StorSimple Správce služby klikněte na **Přidat/upravit šířky pásma šablonu**.

2. V dialogovém okně **Přidat/upravit šířky pásma šablony** :

   1. Z rozevíracího seznamu **Šablona** vyberte **vytvořit nový** přidáte novou šablonu šířky pásma.
   2. Zadejte jedinečný název pro šablonu šířky pásma.

3. Definování **šířky pásma plánu**. Chcete vytvořit plán:

   1. V rozevíracím seznamu vyberte dny v týdnu plánu nakonfigurované. Zaškrtnutím políček nachází před příslušných dnů v seznamu můžete vybrat více dní.
   2. Možnost **Celý den** , pokud plánu nevynucují pro celý den. Pokud toto políčko zaškrtnuté, už zadaný **Čas zahájení** a **Čas ukončení**. Plán se spustí od 12:00 11:59 odp.
   3. V rozevíracím seznamu vyberte **Čas zahájení**. Při je to začne se na plán.
   4. V rozevíracím seznamu vyberte **Čas ukončení**. To je, když se zastaví na plán.

         > [AZURE.NOTE] Překrývající se plány nejsou povolené. Pokud počátečního a koncového času budou výsledkem překrývající se plán, zobrazí se chybová zpráva o tom.

   5. Zadejte **šířku pásma sazba**. To je v MB sekundu (MB /) používané zařízení StorSimple v operace týkající se cloudu (nahrávání a stahování). Zadejte číslo od 1 do 1 000 pro toto pole.

   6. Klikněte na ikonu kontrola ![ikona kontroly](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). Šablonu, kterou jste vytvořili se přidají do seznamu šířka pásma šablon na stránce **Konfigurace** služby.

    ![Vytvoření nové šablony šířky pásma](./media/storsimple-manage-bandwidth-templates/HCS_CreateNewBT1.png)

4. Klikněte na tlačítko **Uložit** v dolní části stránky a potom na tlačítko **Ano** po zobrazení výzvy k potvrzení. To bude uložte změny konfigurace, které jste udělali.

## <a name="edit-a-bandwidth-template"></a>Úprava šablony šířky pásma

Provedení následujících kroků můžete upravit šablonu šířky pásma.

### <a name="to-edit-a-bandwidth-template"></a>Chcete-li upravit šablonu šířky pásma

1. Klikněte na **Přidat/upravit šířky pásma šablonu**.

2. V dialogovém okně **Přidat/upravit šířky pásma šablony** :

   1. Z rozevíracího seznamu **Šablona** zvolte existující šablony šířky pásma, kterou chcete upravit.
   2. Dokončete změny. (Můžete změnit existující nastavení.)
   3. Klikněte na ikonu zaškrtnutí ![Ikona kontroly](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). Zobrazí se změněné šablony v seznamu šířka pásma šablon na stránce konfigurace služby.

3. Uložte provedené změny, klikněte na **Uložit** v dolní části stránky. Klikněte na **Ano** po zobrazení výzvy k potvrzení.

> [AZURE.NOTE] Nebude moct ukládat změny, pokud upravený plánu překrývá s existující plán v šabloně šířky pásma, kterou měníte.

## <a name="delete-a-bandwidth-template"></a>Odstranění šablony šířky pásma

Proveďte následující kroky pro odstranění šablony šířky pásma.

#### <a name="to-delete-a-bandwidth-template"></a>Odstranění šablony šířky pásma

1. V tabulkových seznam šablon šířky pásma pro službu vyberte šablonu, kterou chcete odstranit. Krajní vpravo od vybrané šablony se zobrazí ikonu Odstranit (**x**). Kliknutím na ikonu **x** odstranit šablonu.

2. Zobrazí se výzva k potvrzení. Klepněte na tlačítko **OK** .

Pokud je šablona nepoužívá všechny svazky, nebude moct pořizovat odstraňte ji. Zobrazí se chybová zpráva, že šablony se používá. Dialog s chybou zpráva se zobrazí avizující, že by měly být odstraněny všechny odkazy na šabloně.

Přístup ke stránce **Hlasitost kontejnery** a úpravou kontejnery svazku, které tuto šablonu použít tak, aby použít jinou šablonu nebo použití nastavení šířky pásma vlastní nebo neomezený můžete odstranit všechny odkazy na šabloně. Odebrání všechny odkazy můžete odstranit šablonu.

## <a name="use-a-default-bandwidth-template"></a>Použít výchozí šablonu šířky pásma

Výchozí šablonu šířky pásma je k dispozici a používá hlasitost kontejnery ve výchozím nastavení k jejímu vynucení ovládací prvky šířky pásma při přístupu ke cloudu. Výchozí šablona slouží také jako odkaz na připraveno pro uživatele, kteří vytvořit svoje vlastní šablony. Podrobnosti o tomto výchozí šablonu jsou:

- **Název** – neomezený noci a víkendy

- **Plán** – jeden plánovat od pondělí až pátek, která se vztahuje sazbu šířky pásma MB 1/s mezi 8: 00 a 17: 00 zařízení. Neomezeno možnost šířky pásma pro zbytek týdne.

Výchozí šablona je možné upravit. Použití této šablony (včetně upravenými verzemi) je sledováno.

## <a name="create-an-all-day-bandwidth-template-that-starts-at-a-specified-time"></a>Vytvoření celodenní zvláštní šablony šířky pásma, začínajícím v určeném časovém

Pomocí následujícího postupu vytvořte plán, který začíná zadaný čas a spustí celý den. V příkladu plánu začíná 9: 00 dopoledne a spustí až 9: 00 další během dopoledních hodin. Je důležité mít na paměti, počátečního a koncového času u dané plánu musí obě obsahovat stejný plánu 24 hodin a nemůže zahrnovat více dní. Pokud potřebujete nastavit šířku pásma šablony, které zahrnují více dní, musíte používat více plány (jak je uvedeno v příkladu).

#### <a name="to-create-an-all-day-bandwidth-template"></a>Vytvoření celodenní zvláštní šablony šířky pásma

1. Vytvoření plánu začínající 9: 00 dopoledne a spustí do půlnoci.

2. Přidejte další kalendář. Konfigurace druhý plánu spuštění od půlnoci až 9: 00 v během dopoledních hodin.

3. Uložení šablony šířky pásma.

Složené plán bude potom spustit současně e a spusťte celodenní.

## <a name="questions-and-answers-about-bandwidth-templates"></a>Otázky a odpovědi šablony šířky pásma

**Q**. Co se stane k ovládacím prvkům šířky pásma, když jste mezi plány? (Plán skončil a nějaký jiný ona nezačne.)

**A**. V takovém případě se být použity žádné ovládací prvky šířky pásma. To znamená, že zařízení můžete provádějte neomezenou šířku pásma vrstvení dat do cloudu.

**Q**. Můžete změnit šířku pásma šablony na zařízení s offline?

**A**. Nebudete moct upravit šířky pásma šablony na svazky kontejnery Pokud odpovídající zařízení je offline.

**Q**. Můžete upravit šablonu šířky pásma přidružené kontejneru hlasitost při práci v offline režimu související svazky?

**A**. Můžete změnit šířku pásma šablonu přidružené kontejneru hlasitost jehož svazky práci v offline režimu. Všimněte si, že při offline svazky žádná data bude mít vrstveny ze zařízení do cloudu.

**Q**. Můžete odstranit výchozí šablonu?

**A**. Přestože můžete odstranit výchozí šablonu, je vhodné provést. Použití výchozí šablonu, včetně upravený verzí sledovat. Data sledování analýzy a v průběhu času slouží ke zlepšení výchozí šablonu.

**Q**. Jak můžete zjistit, že je potřeba upravit šířky pásma šablony?

**A**. Jednou z znaménka, že potřebujete změnit šířku pásma šablony je při spuštění zobrazují sítě zpomalovat nebo vyseknutí tisknutím v den. V takovém případě sledování úložiště a použití sítě pohledem grafy v/v výkon a výkon sítě.

Z dat výkon sítě označte dobu den a hlasitost kontejnerů, ve kterých dojde k síti kritické. Pokud to se stane, když je vrstveny dat do cloudu (získat tyto informace z výkon vstupu a výstupu kontejnerů všechny Hlasitost zařízení do cloudu), pak budete muset změnit šířku pásma šablony přidružené kontejnery hlasitost.

Po změněné šablony se používají, musíte sledovat v síti znovu významné čekacích dob. Pokud pořád existují, budete muset návštěvě šablon šířky pásma.

**Q**. Co se stane, pokud ještě několika kontejnery hlasitosti na svém zařízení plánuje, které překrývají, ale platí jiné omezení pro každý?

**A**. Předpokládejme, že máte zařízení s 3 kontejnery hlasitost. Plány přidružené tyto kontejnery úplně překrývat. Pro každou z těchto kontejnerů omezení šířky pásma používá jsou 5, 10 a 15 MB / v tomto pořadí. Při vstupně-výstupních dochází ve všech těchto kontejnerů ve stejnou dobu, může být použity minimální hodnota 3 omezení šířky pásma: v tomto případě MB 5 / jako odchozí požadavky v/v sdílet stejné fronty.

## <a name="best-practices-for-bandwidth-templates"></a>Doporučené postupy pro šířku pásma šablony

Použijte tyto doporučené postupy pro zařízení s StorSimple:

- Konfigurace šířky pásma šablon na svém zařízení na povolit proměnné omezení z výkon sítě tak, že zařízení v různou dne. Tyto šablony šířky pásma při použití s záložní plány můžete efektivně využít další šířka pásma pro operace cloudu dobu největšího.

- Výpočet skutečných šířky pásma potřebných pro konkrétní implementaci v závislosti na velikosti nasazení a cíl čas požadované obnovení (RTO).

## <a name="next-steps"></a>Další kroky

Další informace o [použití služby StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md).
