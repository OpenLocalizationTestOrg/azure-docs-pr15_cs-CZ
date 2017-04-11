<properties
    pageTitle="Správa pracovního prostoru aplikace Groove počítače výukové | Microsoft Azure"
    description="Správa přístupu Azure počítače výukové pracovního prostoru a nasazením a správou ML rozhraní API webových služeb"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="garye"/>


# <a name="manage-an-azure-machine-learning-workspace"></a>Správa pracovního prostoru výukové počítače Azure

>[AZURE.NOTE] Postupy uvedené v tomto článku jsou důležité pro Azure počítače výukové klasické webové služby. Informace o správě webové služby na portálu počítače výukové webovým službám najdete v tématu [Správa webové služby na portálu Azure počítače výukové webové služby](machine-learning-manage-new-webservice.md).

Na portálu Azure klasické, můžete spravovat pracovní prostory výukové počítače do:

- Sledování využití pracovního prostoru
- Konfigurace pracovní prostor a povolit nebo odepřít přístup
- Správa webových služeb vytvořené v pracovním prostoru
- Odstranění pracovního prostoru

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Karta řídicí panel představujte navíc základní informace o použití pracovní prostor a stručný přehled informacím pracovního prostoru.  

> [AZURE.TIP] V Azure Studio výukové počítače, na kartě **Webovým službám** můžete přidat, aktualizovat nebo odstranění počítače výukové webové služby.

Spravovat pracovní prostor:

1.  Přihlaste se k [Azure klasické portál](https://manage.windowsazure.com/) pomocí účtu Microsoft Azure – pomocí účtu, který máte přidružený k předplatnému Azure.
2.  V panelu služby Microsoft Azure klikněte na **VÝUKOVÉ počítače**.
3.  Klikněte na pracovní prostor, který chcete spravovat.

Na stránce pracovní prostor má tři karty:

- **Řídicí panel** – umožňuje zobrazení pracovního prostoru používání a informací
- **Konfigurace** – umožňuje spravovat přístup do pracovního prostoru
- **Webové služby** – umožňuje spravovat webové služby, které jste publikovali z tohoto pracovního prostoru

## <a name="to-monitor-how-the-workspace-is-being-used"></a>Chcete-li sledovat využití pracovního prostoru

Klikněte na kartu **řídicího PANELU** .

Na řídicím panelu můžete zobrazit celkové využití pracovního prostoru a získejte rychlý přehled informací o pracovním prostoru.

- **Výpočet** graf zobrazuje výpočetním zdrojů používaných v pracovním prostoru. Můžete změnit zobrazení tak, aby relativní a absolutní hodnoty a časový rámec zobrazených v grafu můžete změnit.
- **Přehled použití** zobrazí Azure úložiště používán v pracovním prostoru.
- **Stručný přehled** obsahuje souhrn informací o pracovním prostoru a užitečné odkazy.

> [AZURE.NOTE] **Přihlaste se k ML Studio** odkaz otevře Studio výukové počítače pomocí Microsoft Account právě jste se přihlásili k. Account Microsoft, který jste použili pro přihlášení k portálu Azure klasické pracovní prostor vytvoříte tak automaticky nemá oprávnění k otevření pracovního prostoru. Pokud chcete otevřít pracovní prostor, musíte být přihlášení ke Account Microsoft, která byla definována jako vlastník pracovního prostoru nebo budete muset přijmout pozvání od vlastníka ke pracovního prostoru.


## <a name="to-grant-or-suspend-access-for-users"></a>Lze udělit nebo pozastavení přístupu pro uživatele ##

Klikněte na kartu **KONFIGUROVAT** .

Na kartě konfigurace můžete provést tyto akce:

- Pozastavit přístup do pracovního prostoru počítače výukové po kliknutí na ODMÍTNOUT. Uživatelé už budou moct pracovní prostor otevřít ve počítače výukové studiu. Obnovení přístupu, klikněte na povolit.

Ke správě dalších účtů, kteří mají přístup do pracovního prostoru ve počítače výukové studiu, klikněte na **Přihlásit se ML Studio** na kartě **řídicího PANELU** (viz poznámka před týkající se **Přihlaste se k ML Studio**). Pracovní prostor se otevře v počítači výukové Studio. Tady klikněte na kartu **Nastavení** a potom **uživatelů**. Klikněte na **POZVAT další uživatele** uživatelům přístup do pracovního prostoru, nebo vyberte uživatele a klikněte na **Odebrat**.


## <a name="to-manage-web-services-in-this-workspace"></a>Správa webových služeb v tomto pracovním prostoru

Klikněte na kartu **Webové služby** .

Zobrazí se seznam webovým službám publikovaných pomocí tohoto pracovního prostoru.
Pokud chcete spravovat webové služby, klikněte na název v seznamu a otevře se stránka webové služby.

Webová služba může být jeden nebo více koncové body definované.

- Můžete definovat další koncové body kromě "Výchozí" koncový bod. Přidat koncový bod, klikněte na **Spravovat koncové body** v dolní části na řídicím panelu otevřete portálu Azure počítače výukové webové služby.

- Odstranění koncového bodu (nelze odstranit "Výchozí" koncový bod), zaškrtněte políčko na začátku řádku koncového bodu a klikněte na **Odstranit**. Koncový bod to odebere z webové služby.

    > [AZURE.NOTE] Pokud aplikace používá koncový bod služby web při odstranění koncového bodu, aplikace dojde k chybě při příštím pokusí o přístup ke službě.

Klikněte na název koncového bodu služby Web, aby se otevřela. 

Na řídicím panelu můžete zobrazit celkové využití webová služba průběhu určitého časového období. Můžete vybrat období zobrazíte období rozevírací nabídky v pravém horním rohu použití graf s dílčími pruhy. Řídicí panel zobrazí následující informace:

- **Požadavky v čase** zobrazuje krok graf počtu žádosti o vybrané časové období. Můžou pomoct zjistit, zda jste zaznamenali vrcholy pole v části použití.
- **Požadavky na odpovědi na žádosti o** zobrazuje celkový počet hovory odpovědi na žádosti o službu obdržel přes vybrané časové období a kolik je z ní se nezdařila.
- **Čas výpočet průměru odpovědi na žádosti o** slouží k zobrazení průměrně dobu potřebnou k provedení přijaté žádosti.
- **Žádosti o dávku** zobrazuje celkový počet dávku žádosti o službu obdržel přes vybrané časové období a kolik je z ní se nezdařila.
- **Průměrnou latenci úlohy** slouží k zobrazení průměrně dobu potřebnou k provedení přijaté žádosti.
- **Chyby** zobrazuje souhrnný počet chyb, které se uskutečnily na volání webové služby.
- **Služby náklady** zobrazuje náklady pro fakturaci plán přidružený ke službě.

Na stránce Konfigurovat můžete nastavit následující vlastnosti:

* **Popis** umožňuje zadat popis webové služby. Popis pole je povinné.
* **Protokolování** umožňuje povolit nebo zakázat chyb přihlášení koncový bod. Další informace o přihlášení najdete v článku povolit [protokolování pro počítač výukové webové služby](machine-learning-web-services-logging.md).
* **Povolení ukázkových dat** umožňuje poskytovat ukázkových dat, které můžete použít k testování odpovědi na žádosti o služby. Pokud jste vytvořili webová služba ve počítače výukové Studio ukázkových dat se považuje z dat vaší sloužící k školení modelu. Pokud jste vytvořili službu programově data odebranými ukázkovými daty, zadaný jako součást balíčku JSON.

[consume]: machine-learning-consume-web-services.md
[marketplace]: machine-learning-publish-web-service-to-azure-marketplace.md
