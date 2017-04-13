<properties 
   pageTitle="Přístup k soukromé Azure mračnech s Visual Studio | Microsoft Azure"
   description="Zjistěte, jak získat přístup k prostředkům soukromé cloudu pomocí aplikace Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="accessing-private-azure-clouds-with-visual-studio"></a>Přístup k soukromé Azure mračnech s Visual Studio

##<a name="overview"></a>Základní informace

Ve výchozím nastavení Visual Studio podporuje koncové body ZBÝVAJÍCÍ veřejné Azure cloudu. To je problém, ale při používání aplikace Visual Studio s soukromé Azure cloudu. Certifikáty slouží ke konfiguraci aplikace Visual Studio pro přístup k koncové body ZBÝVAJÍCÍ soukromé Azure cloudu. Můžete získat tyto certifikáty prostřednictvím svého Azure publikování souboru nastavení.

## <a name="to-access-a-private-azure-cloud-in-visual-studio"></a>Přístup k soukromé Azure cloudu ve Visual Studiu

1. [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885) pro soukromé cloudu stáhněte si soubor s nastavením publikovat nebo kontaktujte svého správce pro nastavení publikování. Na veřejné verzi Azure je odkaz pro stažení to [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/). (Soubor, který si stáhnete by mít příponu .publishsettings.)

1. V **Průzkumníku serveru** ve Visual Studiu zvolte uzel **Azure** a v místní nabídce vyberte příkaz **Spravovat předplatná** .

    ![Správa předplatného příkaz](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. V dialogovém okně **Spravovat Microsoft Azure předplatných** vyberte kartu **certifikáty** a klikněte na tlačítko **importovat** .

    ![Import Azure certifikátů](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. V dialogovém okně **Importovat Microsoft Azure předplatná** přejděte do složky, kde můžete uloženého souboru nastavení publikování a vyberte soubor a pak klikněte na tlačítko **importovat** . Certifikáty v nastavení publikování importují do aplikace Visual Studio. Teď byste měli být moct komunikovat s soukromé cloudové zdroje.

    ![Import nastavení publikování](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

## <a name="next-steps"></a>Další kroky

[Publikování na služby Azure cloudu z aplikace Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)

[Postup: soubor ke stažení a Import publikovat nastavení a informace o předplatném](https://msdn.microsoft.com/library/dn385850(v=nav.70).aspx)

