<properties 
    pageTitle="Běžné úlohy správy služby cloudu | Microsoft Azure" 
    description="Naučte se spravovat cloudové služby na portálu Azure. Tyto příklady používají portálu Azure." 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/02/2016"
    ms.author="adegeo"/>


# <a name="how-to-manage-cloud-services"></a>Jak spravovat Cloudovým službám

> [AZURE.SELECTOR]
- [Azure portálu](cloud-services-how-to-manage-portal.md)
- [Azure klasické portálu](cloud-services-how-to-manage.md)

Cloudové služby je spravovaný v oblasti **Cloudovým službám (klasické)** portálu Azure. Tento článek popisuje některé běžné akce, které byste měli vzít při správě cloudovým službám. Zahrnuje aktualizace, odstranění, změna velikosti a podporu fázované nasazení výroby.

Další informace o tom, jak změnit velikost cloudové služby je k dispozici [v tomto poli](cloud-services-how-to-scale-portal.md).

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Postup: Aktualizovat do cloudové služby role nebo nasazení

Pokud potřebujete aktualizovat kód aplikace pro vaše cloudové služby, použijte na zásuvné služby cloudu **Aktualizovat** . Můžete jedné role nebo aktualizovat všechny role. Pokud chcete aktualizovat, můžete nahrát nový balíček služby nebo soubor konfigurace služby.

1. [Azure portál][]vyberte cloudové služby, kterou chcete aktualizovat. Tento krok otevře zásuvné instanci služby cloudu.

2. V zásuvné klikněte na tlačítko **Aktualizovat** .

    ![Tlačítko Aktualizovat](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Aktualizujte nasazení nový soubor balíčku služby (.cspkg) a soubor konfigurace služby (.cscfg).

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. **Pokud chcete** aktualizovat nápisem nasazení a účtu úložiště. 

5. Pokud žádné role jenom jedna instance role, vyberte **nasazení i v případě jedné nebo více rolí obsahují jedna instance** umožnit upgrade na pokračovat. 

    Azure pouze zaručit dostupnost služby 99.95 procent během aktualizací služby cloudu Pokud každou roli obsahuje aspoň dvě instance role (virtuálních počítačích). Dvě instance role v jednom počítači virtuální bude zpracovávat žádosti klienta o během automaticky aktualizován druhý.

6. Zaškrtněte políčko **Spustit nasazení** být použita po dokončení nahrávání balíček aktualizace.

7. Klepnutím na **OK** spusťte aktualizace služby.



## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Postup: nasazení podporovat fázované nasazení výroby zaměnit

Pokud se rozhodnete nasaďte novou verzi do cloudové služby, fáze a otestujte nové verzi v testovacím prostředí vaše cloudové služby. Umožňuje **Zaměnit** přepnout adresy URL, kterými dvou nasazení adresami a podporu nového vydání výroby. 

Můžete zaměnit nasazení na stránce **Služby cloudu** nebo na řídicím panelu.

1. [Azure portál][]vyberte cloudové služby, kterou chcete aktualizovat. Tento krok otevře zásuvné instanci služby cloudu.

2. V zásuvné tlačítko **Zaměnit** .

    ![Odkládací cloud Services](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Zobrazí se následující potvrzení.

    ![Odkládací cloud Services](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Po ověření údajů o nasazení kliknutím na **OK** zaměnit nasazení.

    Odkládací nasazení proběhne rychle protože jediné, které se mění je virtuální adresy IP (adres VIP) pro nasazení.

    Uložit výpočetním náklady, můžete odstranit pracovní nasazení po ověření, že provozní nasazení funguje očekávaným způsobem.

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Postup: propojení zdroje do cloudové služby

Portál Azure není propojovat zdroje jako aktuální Azure klasické portálu. Místo toho nasaďte další zdroje informací do stejné skupiny prostředků používá cloudové služby.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Postup: odstranění nasazení a do cloudové služby

Chcete-li odstranit do cloudové služby, je potřeba odstranit jednotlivé existující nasazení.

Uložit výpočetním náklady, můžete odstranit pracovní nasazení po ověření, že provozní nasazení funguje očekávaným způsobem. Je faktura výpočetním nákladů instance nasazeném role, které jsou zastaveno.

Pomocí následujícího postupu odstranění nasazení nebo cloudové služby. 

1. [Azure portál][]vyberte cloudové služby, kterou chcete odstranit. Tento krok otevře zásuvné instanci služby cloudu.

2. V zásuvné kliknutím na tlačítko **Odstranit** .

    ![Odkládací cloud Services](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. Můžete odstranit celý cloudovou službu kontrolou **Cloudová služba a jeho nasazení** nebo **výrobní nasazení** nebo **pracovní nasazení**.

    ![Odkládací cloud Services](./media/cloud-services-how-to-manage-portal/delete-blade.png) 

4. Kliknutím na tlačítko **Odstranit** dole.

5. Pokud chcete odstranit cloudové služby, klikněte na **Odstranit cloudové služby**. Pak v okně pro potvrzení klikněte na **Ano**.

> [AZURE.NOTE]
> Když do cloudové služby odstraněna a podrobné sledování nakonfigurovaný, musíte odstranit data ručně ze svého účtu úložiště. Informace o tom, kde najdete metriky tabulek najdete v [tomto](cloud-services-how-to-monitor.md) článku.

[Azure portálu]: https://portal.azure.com

## <a name="next-steps"></a>Další kroky

* [Obecné konfigurace cloudové služby](cloud-services-how-to-configure-portal.md).
* Zjistěte, jak [nasadit do cloudové služby](cloud-services-how-to-create-deploy-portal.md).
* Konfigurace [vlastní název domény](cloud-services-custom-domain-name-portal.md).
* Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate-portal.md).