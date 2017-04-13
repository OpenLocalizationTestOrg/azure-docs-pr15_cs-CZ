<properties
   pageTitle="Nastavení s názvem přihlašovacími údaji | Microsoft Azure"
   description="Zjistěte, jak chcete pověření Visual Studio můžete pomocí ověření žádosti o Azure publikovat aplikace Azure z aplikace Visual Studio nebo ke sledování existující cloudové služby. "
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

# <a name="setting-up-named-authentication-credentials"></a>Nastavení pojmenované přihlašovacími údaji

Publikování aplikace Azure z aplikace Visual Studio nebo ke sledování existující cloudové služby, je nutné zadat přihlašovací údaje, které aplikace Visual Studio můžete použít k ověření požadavky na Azure. Existuje několik míst ve Visual Studiu, kde můžete přihlásit k poskytování těchto přihlašovacích údajů. Například z Průzkumníka serveru můžete otevřít místní nabídku pro uzel **Azure** a vyberte **připojit k Azure**. Při přihlašování, informace o předplatném přidruženého k vašemu účtu Azure je k dispozici ve Visual Studiu a nemusíte už nic další, že musíte udělat.

Azure nástroje taky podporuje starší způsobu poskytování přihlašovací údaje pomocí souboru předplatného (.publishsettings soubor). Toto téma popisuje tento metodu, která je pořád v Azure SDK 2.2 podporovaná.

Následující položky jsou potřeba pro ověřování Azure.

- ID předplatného

- Platné certifikátu X.509 v3

>[AZURE.NOTE] Délka certifikátu X.509 v3 klíče musí být alespoň 2048 bitů. Azure odmítne všechny certifikát, který nesplňuje tohoto požadavku nebo, která není platný.

Visual Studio používá ID předplatného společně s údaji certifikát jako přihlašovací údaje. Příslušná oprávnění odkazuje předplatné soubor (.publishsettings), který obsahuje veřejný klíč certifikátu. Předplatné soubor může obsahovat přihlašovací údaje pro více než jedno předplatné.

Informace o předplatném v dialogovém okně **Nový a úpravy předplatného** můžete upravit, jak je vysvětleno dále v tomto tématu.

Pokud chcete vytvořit certifikát, můžete postupujte podle pokynů v [vytvořte a uložte certifikát správy Azure](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) a ručně uložit certifikát [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885).

>[AZURE.NOTE] Tyto přihlašovací údaje vyžadované Visual Studio ke správě cloudovým službám nejsou stejné přihlašovací údaje, které jsou potřebné k ověření žádost o proti služby Azure úložiště.

## <a name="modify-or-export-authentication-credentials-in-visual-studio"></a>Úprava nebo exportovat přihlašovacími údaji ve Visual Studiu

Můžete také nastavit, úprava nebo exportovat vaší přihlašovacími údaji v dialogovém okně **Nové předplatné** , které se zobrazí, když provedete některou z následujících akcí:

- V **Průzkumníku serveru**otevřete místní nabídky pro uzel **Azure** , zvolte **Správa předplatného**, zvolte kartu **certifikáty** a klikněte na tlačítko **Nový** nebo **Upravit** .

- Při publikování Azure cloudové služby v průvodci **Publikovat Azure aplikace** vyberte **Spravovat** v seznamu **Zvolte předplatné** a pak zvolte kartu certifikáty a klikněte na tlačítko **Nový** nebo **Upravit** .

Následující postup předpokládá, že je otevřeno dialogové okno **Nové předplatné** .

### <a name="to-set-up-authentication-credentials-in-visual-studio"></a>Nastavení přihlašovacích ve Visual Studiu

1. V **Vyberte stávající certifikát** pro seznam ověřování vyberte certifikát.

1. Klikněte na tlačítko **Kopírovat úplnou cestu** . Cesta k certifikát (soubor .cer) je zkopírovali do schránky.

    >[AZURE.IMPORTANT] Publikovat Azure aplikace Visual Studio, musíte nahrajete certifikát [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885).

1. Nahrání certifikátu do [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885):

    1. Zvolte odkaz Azure portálu.

         Otevře se [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885) .

    1. Přihlaste se k [Azure klasické portál](http://go.microsoft.com/fwlink/?LinkID=213885)a potom klikněte na tlačítko **Cloudovým službám** .

    1. Zvolte cloudové služby, které vás zajímá.

        Otevře se stránka pro službu.

    1. Na kartě **certifikátů** klikněte na tlačítko **Odeslat** .

    1. Vložte úplnou cestu k souboru .cer, který jste právě vytvořili a potom zadejte heslo, které jste zadali.
