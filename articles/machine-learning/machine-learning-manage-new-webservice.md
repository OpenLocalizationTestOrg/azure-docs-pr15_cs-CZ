<properties
    pageTitle="Správa webové služby na portálu Azure počítače výukové Web Serivces | Microsoft Azure"
    description="Správa přístupu Azure počítače výukové pracovního prostoru a nasazením a správou ML rozhraní API webových služeb"
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="v-donglo"/>


# <a name="manage-a-web-service-using-the-azure-machine-learning-web-services-portal"></a>Správa webové služby na portálu Azure počítače výukové webové služby

Můžete spravovat počítače výukové nových a klasické webové služby na portálu Microsoft Azure počítače výukové webové služby. Protože klasické webovým službám a nové webové služby založené na různých základní technologie, máte možnosti mírně odlišnou správy pro každý z nich.

Na portálu počítače výukové webovým službám máte tyto možnosti:

- Sledujte, jak webová služba používá.
- Konfigurace popis, aktualizovat klíče pro web služby (pouze nové), aktualizujte svůj úložiště účtu klíčové (pouze nový) povolit protokolování a povolit nebo zakázat ukázková data.
- Odstranění webové služby.
- Vytvoření, odstranění nebo aktualizace fakturace plány jednotného zasílání zpráv (pouze nový).
- Přidávání a odstraňování koncové body (pouze klasické)

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="manage-new-web-services"></a>Správa nové webové služby

Ke správě nové webové služby:

1.  Přihlaste se k portálu [Microsoft Azure počítače výukové webové služby](https://services.azureml.net/quickstart) pomocí účtu Microsoft Azure – pomocí účtu, který máte přidružený k předplatnému Azure.
2.  V nabídce klikněte na **Web služby**.

Zobrazí se seznam nasazeném webové služby pro vaše předplatné. 

Pokud chcete spravovat webové služby, klikněte na webové služby. Na stránce webové služby můžete provést tyto akce:

- Klikněte na webová služba ke správě ho.
- Klikněte na fakturace plán na webové služby ji aktualizovat.
- Odstranění webové služby.
- Zkopírujte webové služby a ho nasadit do jiné oblasti.

Když kliknete na webové služby, otevře se webová stránka rychlý úvod služby. Stránka webové služby rychlý úvod má dvě možnosti nabídky, které umožňují řídit webové služby:

- **Řídicí panel** – umožňuje zobrazit využití webové služby.
- **Konfigurace** – umožňuje přidat popisný text, aktualizujte klíč účtu úložiště přidružený k webové službě a povolit nebo zakázat ukázková data.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Sledování jak webová služba používá ###

Klikněte na kartu **řídicího PANELU** .

Na řídicím panelu můžete zobrazit celkové využití webová služba průběhu určitého časového období. Můžete vybrat období zobrazíte období rozevírací nabídky v pravém horním rohu použití graf s dílčími pruhy. Řídicí panel zobrazí následující informace:

- **Požadavky v čase** zobrazuje krok graf počtu žádosti o vybrané časové období. Můžou pomoct zjistit, zda jste zaznamenali vrcholy pole v části použití.
- **Požadavky na odpovědi na žádosti o** zobrazuje celkový počet hovory odpovědi na žádosti o službu obdržel přes vybrané časové období a kolik je z ní se nezdařila.
- **Čas výpočet průměru odpovědi na žádosti o** slouží k zobrazení průměrně dobu potřebnou k provedení přijaté žádosti.
- **Žádosti o dávku** zobrazuje celkový počet dávku žádosti o službu obdržel přes vybrané časové období a kolik je z ní se nezdařila.
- **Průměrnou latenci úlohy** slouží k zobrazení průměrně dobu potřebnou k provedení přijaté žádosti.
- **Chyby** zobrazuje souhrnný počet chyb, které se uskutečnily na volání webové služby.
- **Služby náklady** zobrazuje náklady pro fakturaci plán přidružený ke službě.

### <a name="configuring-the-web-service"></a>Konfigurace webové služby ###

Vyberte možnost **KONFIGUROVAT** nabídky.

Můžete nastavit následující vlastnosti:

* **Popis** umožňuje zadat popis webové služby.
* **Název** umožňuje zadat název webové služby
* **Klávesy** umožňuje otočit rozhraní API klíče primárního a sekundárního.
* **Klíč účtu úložiště** umožňuje aktualizovat klíč účtu úložiště přidružené ke změnám webové služby. 
* **Povolení ukázkových dat** umožňuje poskytovat ukázkových dat, které můžete použít k testování odpovědi na žádosti o služby. Pokud jste vytvořili webová služba ve počítače výukové Studio ukázkových dat se považuje z dat vaší sloužící k školení modelu. Pokud jste vytvořili službu programově data odebranými ukázkovými daty, zadaný jako součást balíčku JSON.

### <a name="managing-billing-plans"></a>Správa fakturace plány ###

Klikněte na možnost nabídky **plány jednotného zasílání zpráv** z webové služby rychlý úvod stránky. Můžete taky kliknout na plánu přidružený k určité webové služby Správa plán.

* **Nový** umožňuje vytvořit nový plán.
* **Přidat nebo odebrat plán instance** umožňuje "rozšiřování" existující plán přidáte kapacity.
* **Upgrade/přechodu** umožňuje "rozšíření" existující plán přidáte kapacity.
* **Odstranění** umožňuje odstranění plánu.

Klikněte na možnost plán zobrazíte jeho řídicího panelu. Řídicí panel vám snímku nebo plán použití za vybrané časové období. Chcete-li vybrat časové období chcete-li zobrazit, klikněte na rozevírací seznam **období** v pravé horní části řídicího panelu. 

Řídicí panel plán poskytuje následující údaje:

* **Popis plánu** zobrazuje informace o nákladech a kapacita spojený s plánem.
* **Plánování použití** zobrazí počet transakce a výpočetním hodiny, které byly započítávány plánu.
* **Webové služby** zobrazí číslo webové služby, které používají tento plán.
* **Horní volání tak, že připojení webové služby** zobrazí horní čtyři webové služby, které využívají hovory, které bude účtovaná proti plánu.
* **Horní webovým službám tak, že výpočet nulovou** zobrazí horní čtyři webové služby, které používají výpočetním prostředky, které bude účtovaná proti plánu.

## <a name="manage-classic-web-services"></a>Správa klasické webové služby

> [AZURE.NOTE] Postupy uvedené v této části jsou věnované správě klasické webové služby v portálu Azure počítače výukové webové služby. Informace o správě klasické webové služby pomocí Studio výukové počítače a portálu Azure klasického naleznete v tématu [Správa pracovního prostoru výukové počítače Azure](machine-learning-manage-workspace.md).

Ke správě klasické webové služby:

1.  Přihlaste se k portálu [Microsoft Azure počítače výukové webové služby](https://services.azureml.net/quickstart) pomocí účtu Microsoft Azure – pomocí účtu, který máte přidružený k předplatnému Azure.
2.  V nabídce klikněte na **Klasické webové služby**.

Ke správě klasické webové služby, klikněte na **Klasické webové služby**. Na stránce klasické webovým službám máte tyto možnosti:

- Klikněte na webová služba zobrazíte přidružené koncové body.
- Odstranění webové služby.

Při správě klasické webové služby spravujete všech koncové body samostatně. Když kliknete na webové služby na stránce webové služby, otevře se seznam koncové body přidružený ke službě. 

Na stránce koncový bod klasické webová služba můžete přidat a odstranit koncové body v rámci služby. Další informace o přidávání koncové body najdete v článku [Vytvoření koncové body](machine-learning-create-endpoint.md).

Klikněte na jednu z koncové body a otevře se stránka rychlý úvod webové služby. Na stránce Rychlý úvod dvěma způsoby nabídek, které umožňují řídit webové služby:

- **Řídicí panel** – umožňuje zobrazit využití webové služby.
- **Konfigurace** – umožňuje přidat popisný text, zapnutí protokolování chyb a vypnutí aktualizace klíč pro ukládání účet přidružený k webové službě a povolit nebo zakázat ukázková data.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Sledování jak webová služba používá ###

Klikněte na kartu **řídicího PANELU** .

Na řídicím panelu můžete zobrazit celkové využití webová služba průběhu určitého časového období. Můžete vybrat období zobrazíte období rozevírací nabídky v pravém horním rohu použití graf s dílčími pruhy. Řídicí panel zobrazí následující informace:

- **Požadavky v čase** zobrazuje krok graf počtu žádosti o vybrané časové období. Můžou pomoct zjistit, zda jste zaznamenali vrcholy pole v části použití.
- **Požadavky na odpovědi na žádosti o** zobrazuje celkový počet hovory odpovědi na žádosti o službu obdržel přes vybrané časové období a kolik je z ní se nezdařila.
- **Čas výpočet průměru odpovědi na žádosti o** slouží k zobrazení průměrně dobu potřebnou k provedení přijaté žádosti.
- **Žádosti o dávku** zobrazuje celkový počet dávku žádosti o službu obdržel přes vybrané časové období a kolik je z ní se nezdařila.
- **Průměrnou latenci úlohy** slouží k zobrazení průměrně dobu potřebnou k provedení přijaté žádosti.
- **Chyby** zobrazuje souhrnný počet chyb, které se uskutečnily na volání webové služby.
- **Služby náklady** zobrazuje náklady pro fakturaci plán přidružený ke službě.

### <a name="configuring-the-web-service"></a>Konfigurace webové služby ###

Vyberte možnost **KONFIGUROVAT** nabídky.

Můžete nastavit následující vlastnosti:

* **Popis** umožňuje zadat popis webové služby. Popis pole je povinné.
* **Protokolování** umožňuje povolit nebo zakázat chyb přihlášení koncový bod. Další informace o přihlášení najdete v článku povolit [protokolování pro počítač výukové webové služby](machine-learning-web-services-logging.md).
* **Povolení ukázkových dat** umožňuje poskytovat ukázkových dat, které můžete použít k testování odpovědi na žádosti o služby. Pokud jste vytvořili webová služba ve počítače výukové Studio ukázkových dat se považuje z dat vaší sloužící k školení modelu. Pokud jste vytvořili službu programově data odebranými ukázkovými daty, zadaný jako součást balíčku JSON.

## <a name="grant-or-suspend-access-to-web-services-for-users-in-the-portal"></a>Udělit nebo pozastavit přístup k webovým službám pro uživatele na portálu

Na portálu Azure klasické, můžete povolit nebo odepřít přístup k určitým uživatelům.

### <a name="access-for-users-of-new-web-services"></a>Přístupu pro uživatele nové webové služby

Chcete-li povolit uživatelům pracovat s webové služby na portálu Azure počítače výukové webové služby, musíte přidat je jako co správci předplatného Azure.

Přihlaste se k [Azure klasické portál](https://manage.windowsazure.com/) pomocí účtu Microsoft Azure – pomocí účtu, který máte přidružený k předplatnému Azure.

1. V navigačním podokně klikněte na **Nastavení**a potom klikněte na **Správce**.
2. V dolní části okna klikněte na **Přidat**. 
3. V dialogovém okně Správce spolu přidat A zadejte e-mailovou adresu člověka, kterému chcete přidat jako spolu správce a potom vyberte předplatné, které chcete spolu správce pro přístup k.
4. Klikněte na **Uložit**.

### <a name="access-for-users-of-classic-web-services"></a>Přístupu pro uživatele klasické webové služby

Spravovat pracovní prostor:

Přihlaste se k [Azure klasické portál](https://manage.windowsazure.com/) pomocí účtu Microsoft Azure – pomocí účtu, který máte přidružený k předplatnému Azure.

1. V panelu služby Microsoft Azure klikněte na **VÝUKOVÉ počítače**.
1. Klikněte na pracovní prostor, který chcete spravovat.
1. Klikněte na kartu **KONFIGUROVAT** .

Na kartě konfigurace můžete pozastavit přístup do pracovního prostoru počítače výukové po kliknutí na **ODMÍTNOUT**. Uživatelé už budou moct pracovní prostor otevřít ve počítače výukové studiu. Obnovení přístupu, klikněte na **Povolit**.

Vybraným uživatelům:

Ke správě dalších účtů, kteří mají přístup do pracovního prostoru ve počítače výukové studiu, **Přihlaste se k ML Studio** na kartě klikněte na **řídicí panel** . Pracovní prostor se otevře v počítači výukové Studio. Tady klikněte na kartu **Nastavení** a potom **uživatelů**. Klikněte na **POZVAT další uživatele** uživatelům přístup do pracovního prostoru, nebo vyberte uživatele a klikněte na **Odebrat**.

> [AZURE.NOTE] **Přihlaste se k ML Studio** odkaz otevře Studio výukové počítače pomocí Microsoft Account právě jste se přihlásili k. Account Microsoft, který jste použili pro přihlášení k portálu Azure klasické pracovní prostor vytvoříte tak automaticky nemá oprávnění k otevření pracovního prostoru. Pokud chcete otevřít pracovní prostor, musíte být přihlášení ke Account Microsoft, která byla definována jako vlastník pracovního prostoru nebo budete muset přijmout pozvání od vlastníka ke pracovního prostoru.
