<properties
    pageTitle="Poradce při potížích: Vytvoření a připojte k pracovního prostoru aplikace Groove výukové počítači | Microsoft Azure"
    description="Řešení běžných problémů při vytváření a připojení do pracovního prostoru výukové počítače Azure"
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
    ms.date="09/09/2016"
    ms.author="garye"/>


# <a name="troubleshooting-guide-create-and-connect-to-an-machine-learning-workspace"></a>Příručka pro řešení potíží: vytvoření a připojit k počítači výukové prostoru

Tato příručka poskytuje řešení pro některé často výzvy při nastavování pracovní prostory výukové počítače Azure.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a>Vlastník pracovního prostoru

Při vytváření nového pracovního prostoru počítače výukové ID zadejte v poli Vlastník pracovního prostoru musí být platný účet Microsoft (dřív to bylo Windows Live ID), například john-contoso@live.com nebo john-contoso@hotmail.com. Nesmí být jiných výrobců účtem, třeba firemní e-mailovému účtu. Pokud chcete vytvořit bezplatný účet Microsoft, přejděte na [www.live.com](http://www.live.com).

Všimněte si, že účet, který jste použili k přihlášení k portálu Azure k vytvoření pracovního prostoru nemá automaticky oprávnění k *otevření* pracovního prostoru, pokud nezadáte jako vlastníka tohoto účtu. Otevřete pracovního prostoru aplikace Groove ve počítače výukové studiu, musíte být přihlášení ke Account Microsoft, která byla definována jako vlastník pracovního prostoru nebo budete muset přijmout pozvání od vlastníka ke pracovního prostoru. Z portálu Microsoft Azure klasické můžete však *Spravovat* pracovní prostor, který obsahuje možnost změnit vlastníka a konfigurace aplikace access.

Další informace o správě pracovního prostoru aplikace Groove v tématu [Správa pracovního prostoru výukové počítače Azure].

[Správa pracovního prostoru výukové počítače Azure]: machine-learning-manage-workspace.md

## <a name="allowed-regions"></a>Povolené oblastí

Výukové počítače momentálně neexistuje v omezený počet oblastí. Pokud vaše předplatné neobsahuje některé z těchto oblastí, může zobrazit chybová zpráva, "Máte žádné v oblasti povolené."

Požádat o přidaná do oblasti vašeho předplatného, vyberte **Kontaktovat podporu společnosti Microsoft** z portálu Microsoft Azure klasické, vyberte jako typ problému **Fakturace** a postupujte podle pokynů k odeslání žádosti o.

![Kontaktujte podporu od Microsoftu][screen1]

## <a name="storage-account"></a>Úložiště účtu

Služba strojového výukové vyžaduje účet úložiště pro ukládání data. Můžete použít existující účet úložiště nebo můžete vytvořit nový účet úložiště při vytváření nového pracovního prostoru výukové počítače (Pokud máte kvóty pro vytvoření nového účtu úložiště).

<!-- These instructions no longer work, but I'm not sure what to replace them with
To see if you can create a new storage account, in the Classic Portal, go to **Settings** and then click **Usage**.
-->

![Vytvoření pracovního prostoru][screen2]

Po vytvoření nového pracovního prostoru výukové počítače se můžete přihlásit Studio výukové počítači pomocí účtu Microsoft, které jste určili jako vlastník pracovního prostoru. Pokud se zobrazí chybová zpráva "Pracovního prostoru nebyl nalezen" (podobný následující snímek), použijte následující postup odstranit soubory cookie prohlížeče.

![Pracovní prostor nenašlo se.][screen3]

**Chcete-li odstranit soubory cookie prohlížeče**

Pokud používáte Internet Explorer, klikněte na tlačítko **Nástroje** v pravém horním rohu a vyberte **Možnosti Internetu**.  

![Možnosti Internetu][screen4]

Na kartě **Obecné** klikněte na **Odstranit...**

![Na kartě Obecné][screen5]

V dialogovém okně **Odstranit historii procházení** zkontrolujte, že je vybraná možnost **soubory cookie a data webu** a klikněte na **Odstranit**.

![Odstranit soubory cookie][screen6]

Po odstranění souborů cookie, restartujte prohlížeč a přejděte na stránku, na [Webu Microsoft Azure počítače Learning](https://studio.azureml.net) . Až se zobrazí výzva k zadání uživatelského jména a hesla, zadejte jednoho účtu Microsoft, které jste zadali jako vlastník pracovního prostoru.

Naším cílem je počítači s počítačem bezproblémové co nejvíce podporovat. Na [Fórum komunity Azure počítače výukové](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) jak pomoct vám nabídnout lepší publikovat všechny komentáře a problémy.

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png
