<properties 
    pageTitle="Běžné cloudové služby úlohy správy (klasický) | Microsoft Azure" 
    description="Naučte se spravovat cloudové služby na portálu Azure klasické." 
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
    ms.date="08/10/2016"
    ms.author="adegeo"/>





# <a name="how-to-manage-cloud-services"></a>Jak spravovat Cloudovým službám

> [AZURE.SELECTOR]
- [Azure portálu](cloud-services-how-to-manage-portal.md)
- [Azure klasické portálu](cloud-services-how-to-manage.md)

V oblasti **Cloudové služby** Azure klasické portál aktualizovat roli služby nebo nasazení, zvýšení úrovně fázované nasazení výroby, můžete propojit materiály ke cloudové službě tak, aby viděli závislosti prostředků a měřítko zdroje společně a odstranění cloudové služby, nebo na nasazení.


## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Postup: Aktualizovat do cloudové služby role nebo nasazení

Pokud potřebujete aktualizovat kód aplikace pro vaše cloudové služby, použijte na řídicí panel, **Cloudovým službám** stránku nebo stránky **instance** **Aktualizovat** . Můžete jedné role nebo aktualizovat všechny role. Budete muset nahrát nový balíček služby a služby konfigurační soubor.

1. V [Azure klasické portál](https://manage.windowsazure.com/), na řídicí panel, **Cloudovým službám** stránky nebo **instance** stránky klikněte na **Aktualizovat**.

    ![UpdateDeployment](./media/cloud-services-how-to-manage/CloudServices_UpdateDeployment.png)

2. **Nasazení popisek**zadejte název pro identifikaci nasazení (například mycloudservice4). Nasazení popisku v části **úvodní** najdete na řídicím panelu.

3. Do **balíčku**umožňuje **Procházet** nahrát soubor balíčku služby (.cspkg).

4. **Konfigurace**umožňuje **Procházet** nahrát soubor konfigurace služby (.cscfg).

5. V **roli**vyberte položku **vše** Pokud chcete upgradovat všechny role ve službě cloud. Abyste mohli provést aktualizaci jedné role, vyberte roli, kterou chcete aktualizovat. I když zvolíte konkrétní rolí aktualizovat, aktualizace v souboru konfigurace služby platí pro všechny role.

6. Pokud aktualizace změní počet role nebo velikost jakoukoli roli, zaškrtněte políčko **Povolit aktualizace Pokud se změní role rozměry a počet role** povolit aktualizace pokračovat. 

    Mějte na paměti, že při změně velikosti role (to znamená velikost virtuální počítače hostujícího instanci rolí) nebo počet role, pokaždé role (virtuální počítač) musí být znovu s nainstalovanou bitovou kopií a místní data budou ztracena.

7. Pokud žádné služby role jenom jedna instance role, vyberte **aktualizovat i v případě, že jeden nebo více role obsahují jedna instance zaškrtněte políčko** povolit upgrade pokračovat. 

    Azure pouze zaručit dostupnost služby 99.95 procent během aktualizací služby cloudu Pokud každou roli obsahuje aspoň dvě instance role (virtuálních počítačích). V jednom počítači virtuální zpracovat žádosti klienta o době, kdy je aktualizován druhý, který umožňuje.

8. Kliknutím na tlačítko **OK** (zaškrtnutí) začněte aktualizace služby.



## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Postup: nasazení podporovat fázované nasazení výroby zaměnit

Umožňuje **Zaměnit** podporovat pracovní nasazení do cloudové služby výroby. Pokud se rozhodnete nasaďte novou verzi do cloudové služby, můžete fáze a otestujte nové verzi v testovacím prostředí vaše cloudové služby během vaši zákazníci používají ální ve výrobním. Až budete připravení na podporu nového vydání výroby, můžete **Zaměnit** přejdete adresy URL, ve kterých jsou určeny dvě nasazení. 

Můžete zaměnit nasazení na stránce **Služby cloudu** nebo na řídicím panelu.

1. V [Azure klasické portál](https://manage.windowsazure.com/)klikněte na **Cloudovým službám**.

2. V seznamu cloudové služby klikněte na cloudovou službu ho vyberte.

3. Klikněte na **Zaměnit**.

    Zobrazí se následující potvrzení.

    ![Odkládací cloud Services](./media/cloud-services-how-to-manage/CloudServices_Swap.png)

4. Po ověření údajů o nasazení kliknutím na tlačítko **Ano** zaměnit nasazení.

    Odkládací nasazení proběhne rychle protože jediné, které se mění je virtuální adresy IP (adres VIP) pro nasazení.

    Uložit výpočetní náklady, můžete odstranit zavedení v testovacím prostředí když vy víte, že nové nasazení výrobních provádíte podle očekávání.

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Postup: propojení zdroje do cloudové služby

Zobrazíte vaše cloudové služby v závislosti na dalších zdrojů, můžete propojit instance databáze SQL Azure nebo účtu úložiště cloudové služby. Můžete propojení a zrušení propojení zdroje na stránce **Propojené prostředky** a sledovat jejich použití na řídicím panelu cloudové služby. Pokud účet propojené úložiště monitorování zapnutá, můžete sledovat celkový počet požadavků na řídicím panelu cloudové služby.

Použijte **Připojit** k propojení nové nebo existující databáze SQL instance úložiště účet nebo ke cloudové službě. Pak můžete změnit velikost databázi spolu s role cloudové služby, která se s ním pracovat na stránce **Měřítko** . (Účet úložiště změní automaticky při použití.) Další informace najdete v tématu [jak měřítko Cloudová služba a propojené prostředky](cloud-services-how-to-scale.md). 

Můžete taky můžete sledovat, Správa a změnit velikost databáze v uzel **databáze** Azure klasické portálu. 

"Propojením" zdroje v tomto smyslu není aplikace připojení ke zdroji. Pokud vytvoříte novou databázi pomocí **odkazu**, musíte přidat řetězce připojení kód aplikace a potom upgrade cloudovou službu. Budete taky musíte přidat připojovací řetězec Pokud aplikace používá zdrojů v propojených úložiště účtu.

Následující postup popisuje propojení nové instance databáze SQL, nasazený na nové databáze SQL serveru, do cloudové služby.

### <a name="to-link-a-sql-database-instance-to-a-cloud-service"></a>Pokud chcete vytvořit odkaz instance SQL databáze do cloudové služby

1. V [Azure klasické portál](http://manage.windowsazure.com/)klikněte na **Cloudovým službám**. Klikněte na název cloudovou službu otevřete řídicího panelu.

2. Klikněte na **propojené zdroje**.

    Otevře se stránka **Propojené prostředky** .

    ![LinkedResourcesPage](./media/cloud-services-how-to-manage/CloudServices_LinkedResourcesPage.png)

3. Klikněte na **odkaz zdroj** nebo **odkaz**.

    Spustí se průvodce **Odkaz zdroje** .

    ![Odkaz Page1](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkPage1.png)

4. Klikněte na **vytvořit nový zdroj** nebo **propojit existující zdroj**.

5. Zvolte typ zdroje, pokud chcete vytvořit odkaz. V [Azure klasické portál](http://manage.windowsazure.com/)klikněte na **Databáze SQL**. (Klasické portálu náhled Azure nepodporuje propojení účtu úložiště ke službě cloudového.)

6. Dokončete konfiguraci databáze, postupujte podle pokynů uvedených v článku Nápověda k oblasti **Databáze SQL** Azure klasické portálu.

    Můžete sledovat průběh operaci propojení do oblasti zprávy.

    ![Postup propojení](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkProgress.png)

    Po dokončení propojení stav propojených zdrojů na řídicím panelu cloudové služby můžete sledovat. Informace o měřítka propojené databáze SQL zjistěte, [jak zobrazit Cloudová služba a propojené prostředky](cloud-services-how-to-scale.md).

### <a name="to-unlink-a-linked-resource"></a>Pokud chcete odpojit propojených zdrojů

1. V [Azure klasické portál](http://manage.windowsazure.com/)klikněte na **Cloudovým službám**. Klikněte na název cloudovou službu otevřete řídicího panelu.

2. Klikněte na **Propojené prostředky**a potom vyberte zdroj.

3. Klikněte na tlačítko **Zrušit propojení**. Klikněte na **Ano** v okně pro potvrzení.

    Odpojení databázi SQL nemá žádný vliv na databáze nebo aplikace připojení k databázi. Pořád můžete spravovat databáze v oblasti **Databáze SQL** Azure klasické portálu.



## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Postup: odstranění nasazení a do cloudové služby

Chcete-li odstranit do cloudové služby, je potřeba odstranit jednotlivé existující nasazení.

Snížit náklady výpočetním, můžete odstranit pracovní nasazení po ověření, že provozní nasazení funguje očekávaným způsobem. I když není spuštěná do cloudové služby jste fakturované výpočetním nákladů na role instance.

Pomocí následujícího postupu odstranění nasazení nebo cloudové služby. 

1. V [Azure klasické portál](http://manage.windowsazure.com/)klikněte na **Cloudovým službám**.

2. Vyberte cloudové služby a klikněte na **Odstranit**. (Bez otevření řídicího panelu vyberte do cloudové služby, klikněte na libovolné místo kromě názvu v okně položka služby cloudu.)

    Pokud máte nasazení v pracovní nebo produkční, zobrazí se nabídka možností podobá následující v dolní části okna. Chcete-li odstranit cloudové služby, je potřeba odstranit existující nasazení.

    ![Odstranění nabídky](./media/cloud-services-how-to-manage/CloudServices_DeleteMenu.png)


3. Odstranit nasazení, klikněte na **Odstranit provozní nasazení** nebo **pracovní nasazení**. Pak v okně pro potvrzení klikněte na **Ano**. 

4. Pokud chcete odstranit cloudové služby, opakujte krok 3, v případě potřeby odstranit vaše nasazení.

5. Pokud chcete odstranit cloudové služby, klikněte na **Odstranit cloudové služby**. Pak v okně pro potvrzení klikněte na **Ano**.

> [AZURE.NOTE]
> Pokud podrobného sledování nakonfigurovaný pro vaše cloudové služby, Azure neodstraní monitorování dat ze svého účtu úložiště při odstranění cloudovou službu. Bude potřeba odstranit data ručně. Informace o tom, kde najdete metriky tabulky najdete v tématu "jak: Access podrobného monitorování dat mimo portálu Azure klasické" v [článku jak sledování Cloudovým službám](cloud-services-how-to-monitor.md).

## <a name="next-steps"></a>Další kroky

 * [Obecné konfigurace cloudové služby](cloud-services-how-to-configure.md).
* Zjistěte, jak [nasadit do cloudové služby](cloud-services-how-to-create-deploy.md).
* Konfigurace [vlastní název domény](cloud-services-custom-domain-name.md).
* Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate.md).
