<properties
    pageTitle="Vytvořte účet Azure dávku | Microsoft Azure"
    description="Naučte se vytvořit účet Azure dávku Azure portálu spustit ve velkém měřítku paralelní úloh v cloudu"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/21/2016"
    ms.author="marsma"/>

# <a name="create-an-azure-batch-account-using-the-azure-portal"></a>Azure dávku účet vytvořte pomocí portálu Azure

> [AZURE.SELECTOR]
- [Azure portálu](batch-account-create-portal.md)
- [Správa dávku .NET](batch-management-dotnet.md)

Naučte se vytvořit účet Azure dávku [Azure portál][azure_portal]a kde najdete důležité vlastnosti účtu, jako je přístupové klávesy a účet adresy URL. Také probereme tato dávku ceny a propojení účet Azure úložiště ke svému účtu dávku, aby mohli používat [balíčků aplikací](batch-application-packages.md) a [zachovat výstupu projektu a úkolu](batch-task-output.md).

## <a name="create-a-batch-account"></a>Vytvoření dávky účtu

1. Přihlaste se k [portálu Azure][azure_portal].

2. Klikněte na **Nový** > **Výpočet** > **dávku služby**.

    ![List v Marketplace][marketplace_portal]

3. Zobrazí se **Nový účet dávku** zásuvné. Zobrazit položky ** až *e* pod pro popis jednotlivých prvků zásuvné.

    ![Vytvoření dávky účtu][account_portal]

    na. **Název účtu**: jedinečný název pro váš účet dávku. Tento název musí být jedinečný v rámci Azure oblast, kterou je vytvořen (viz *umístění* níže). Může obsahovat jenom písmena, číslice a musí být 3 24 znaků.

    b. **Předplatné**: předplatné, ve kterém chcete vytvořit účet dávku. Pokud máte jenom jedno předplatné, je vybrané ve výchozím nastavení.

    c. **Pole Skupina zdroje**: existující zdroj seskupení k vašemu novému účtu dávku nebo pokud chcete vytvořit novou.

    d. **Umístění**: Azure oblast ve kterém chcete vytvořit účet dávku. Pouze oblasti podporované svoje předplatné a pole Skupina zdroje se zobrazí jako možnost.

    e. **Úložiště účtu** (volitelné): účtu úložiště **univerzální** přidružení (propojení) k vašemu novému účtu dávku. Další informace najdete v tématu [propojené úložišti Azure účet](#linked-azure-storage-account) pod.

4. Klikněte na **vytvořit** vytvořte účet.

  Na portálu označuje, že je **zavedení** účet a po dokončení, zobrazí se upozornění **bylo úspěšné zavedení** v *oznámení*.

## <a name="view-batch-account-properties"></a>Zobrazení vlastností dávku účtu

Po vytvoření účtu můžete otevřít **zásuvné dávku účtu** pro přístup k původní nastavení a vlastnosti. Získat přístup k nastavení účtu a vlastnosti pomocí levé nabídce zásuvné dávku účtu.

![Dávkové zásuvné účet Azure portálu][account_blade]

* **Adresa URL dávku účtu**: aplikace vytvoříte s [dávku vývoj rozhraní API](batch-technical-overview.md#batch-development-apis) potřebovat účtu URL přidávání a používání zdrojů a průběh úloh účet. Adresa URL účtu dávku má v tomto formátu:

    `https://<account_name>.<region>.batch.azure.com`

![Adresa URL účtu dávku portálu][account_url]

* **Přístupových kláves z verze**: aplikace potřebovat přístupové klávesy při práci s zdrojů ve vašem účtu dávku. Pokud chcete zobrazit nebo obnovit účtu dávku přístupové klávesy, zadejte `keys` do pole **Hledat** nabídce nalevo na zásuvné dávku účtu vyberte **klíče**.

    ![Klávesy dávku účet Azure portálu][account_keys]

## <a name="pricing"></a>Ceny

Účty dávku nabídnuta pouze v "Bezplatné vrstva," což znamená, že nejsou platit pro přímo účet dávku. Bude účtovaná pro podkladového zdroje Azure výpočetním, které používání dávku řešení a pro zdroje využívané další služby při spuštění svého pracovního vytížení. Například vám bude účtovaná za uzlech výpočetním ve vaší fondů a pro data máte uložené v úložišti Azure jako vstupní a výstupní úkolů. Podobně pokud používáte funkci [balíčků aplikací](batch-application-packages.md) listu, vám bude účtovaná za prostředků Azure úložiště slouží k uložení balíčků aplikací. V tématu [dávku ceny] [ batch_pricing] Další informace.

## <a name="linked-azure-storage-account"></a>Propojený klient úložišti Azure

Jak jsme zmínili dříve, můžete (volitelně) propojíte **univerzální** úložiště účtu k vašemu účtu dávku. Funkce [balíčků aplikací](batch-application-packages.md) dávku používá úložiště objektů blob v propojených hlavní účel účtu úložiště, stejně jako knihovnu [Dávku soubor konvence .NET](batch-task-output.md) . Tyto volitelné funkce vám pomohou při nasazení aplikace, které spustit dávku úkolů a uchování data, která vytvářejí.

Dávkové aktuálně podporuje *pouze* typ účtu úložiště **univerzální** podle pokynů v kroku 5, [vytvořit účet úložiště](../storage/storage-create-storage-account.md#create-a-storage-account), [účty adresářové služby Azure o úložiště](../storage/storage-create-storage-account.md). Když propojíte účet Azure úložiště ke svému účtu dávku, být se odkaz *pouze* účtem **univerzální** úložiště.

![Vytvoření účtu úložiště "Univerzální"][storage_account]

Doporučujeme vytvořit účet úložiště využívat váš účet dávku.

>[AZURE.WARNING] Dávejte při obnovení přístupových kláves propojený klient úložiště. Obnovit pouze jeden klíč účtu úložiště a klikněte na **Synchronizovat klíče** propojené zásuvné účtu úložiště. Počkejte, než pět minut umožňuje kláves rozšíří do výpočetního uzlů v fondy, a pak obnovit a synchronizovat na klávesu v případě potřeby. Pokud obnovit oba klíče ve stejnou dobu uzly výpočetních nebudou moct synchronizovat buď klíče a bude ztratíte přístup k účtu úložiště.

  ![Obnovení účtu klíče úložiště][4]

## <a name="batch-service-quotas-and-limits"></a>Kvóty služby dávku a omezení

Upozorňujeme, že stejně jako u předplatného Azure a další služby Azure některých [kvót a omezení](batch-quota-limit.md) použít u dávku účtů. Aktuální kvóty dávku účtu se zobrazí v portálu účtu **Vlastnosti**.

![Dávku účtu kvóty Azure portálu][quotas]

Mějte kvót jsou navrhování a škálování dávku úloh. Například pokud váš fond není dosažení cílové počtu uzlů výpočetním, které jste zadali, může nedosáhnete limit kvóty core účtu dávku.

Navíc nezapomeňte, že nejsou omezené na účet dávku předplatného Azure. Spuštění pracovního vytížení více dávku jednoho účtu dávku nebo distribuce vaší vytížení dávku účty v rámci stejného předplatného, ale v různých oblastech Azure.

V mnoha kvót můžete zvýšit jednoduše s žádostí o bezplatně podpora odeslaný Azure portálu. Informace o žádosti o zvýšení kvót naleznete v tématu [kvót a limity pro službu Azure dávku](batch-quota-limit.md) .

## <a name="other-batch-account-management-options"></a>Další způsoby správy účtu dávku

Kromě použití portálu Azure, můžete vytvořit a spravovat účty dávka s tímto:

* [Rutiny prostředí PowerShell dávku](batch-powershell-cmdlets-get-started.md)
* [Azure rozhraní příkazového řádku](../xplat-cli-install.md)
* [Správa dávku .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Další kroky

* V tématu [Přehled funkcí Azure dávku](batch-api-basics.md) zobrazíte další informace o konceptů dávku služeb a funkcí. V článku popisuje hlavní dávku zdroje, jako jsou skupiny, výpočetních uzly, projektů a úkolů a obsahuje přehled funkcí služby podporující nástroj rozsáhlé výpočetních pracovní zátěž spuštění aplikace.

* Naučte se základy vývoje dávkách aplikace pomocí [dávku .NET klienta knihovny](batch-dotnet-get-started.md). [Úvodní článek](batch-dotnet-get-started.md) vás provede funkční aplikaci, která používá službu dávku provést úlohu ve více uzlech výpočetním a obsahuje používat úložišti Azure pracovní soubor pracovního vytížení a načítání.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Obnovení účtu klíče úložiště"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png
