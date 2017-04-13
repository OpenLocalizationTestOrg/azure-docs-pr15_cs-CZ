<properties
   pageTitle="Konfigurace projekt služby Azure cloudu pomocí aplikace Visual Studio | Microsoft Azure"
   description="Zjistěte, jak nakonfigurovat projekt služby Azure mraků ve Visual Studiu, v závislosti na vašim požadavkům pro tento projekt."
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

# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a>Konfigurace projekt služby Azure cloudu pomocí aplikace Visual Studio

Konfigurace projekt služby Azure cloudu v závislosti na vašim požadavkům pro tento projekt. Můžete nastavit vlastnosti projektu pro následující kategorie:

- **Publikovat do cloudové služby Azure**

  Můžete nastavit vlastnost, abyste měli jistotu, že není nechtěného odstranění existující cloudové služby používaný Azure.

- **Spuštění nebo ladění do cloudové služby v místním počítači**

  Můžete vybrat konfiguraci služby, kterou chcete použít a označuje, jestli chcete začít emulátoru Azure úložiště.

- **Ověření balíčku služby cloudu při vytvoření**

  Můžete se rozhodnout, jako chybné zacházet s všechna upozornění, takže můžete zajistit, že bude bez jakýchkoli problémů nasazení balíčku služby cloudu. Pokud nasazení a potom zjistíte, že došlo k chybě sníží čekání času.

Následující obrázek ukazuje, jak k výběru konfigurace budete používat při spuštění a ladění místně cloudové služby. Vlastnosti projektu, které vyžadují v tomto okně můžete nastavit, jak je vidět na obrázku.

![Konfigurace projektu Microsoft Azure](./media/vs-azure-tools-configuring-an-azure-project/IC713462.png)

## <a name="to-configure-an-azure-cloud-service-project"></a>Konfigurace projekt služby Azure cloudu

1. Abyste mohli nakonfigurovat projekt služby cloudu z **Průzkumníka řešení**, otevřete místní nabídky pro projekt cloudové služby a klikněte na **Vlastnosti**.

  V editoru Visual Studio zobrazí se stránka s názvem projektu služby cloudu.

1. Zvolte kartu **vývoj** .

1. Abyste měli jistotu, že není odstraníte omylem stávajícího nasazení v Azure, v okně s dotazem před odstraněním existujícího seznamu nasazení, vyberte **hodnotu True**.

1. Chcete-li vybrat zvolte konfiguraci služby, který chcete použít při spuštění nebo ladění místně, vaše cloudové služby v seznamu **Konfigurace služby** konfiguraci služby.

  >[AZURE.NOTE] Pokud chcete vytvořit konfiguraci služby, kterou chcete použít, najdete v článku jak: Správa konfigurace služeb a profily. Pokud chcete změnit konfiguraci služby pro roli, najdete v článku [jak nakonfigurovat role pro službu Azure cloudu pomocí aplikace Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

1. Jestli Pokud chcete začít emulátoru Azure úložiště při spuštění nebo ladění místně, vaše cloudové služby ve **emulátoru úložiště Start Azure**zvolte **True**.

1. Abyste měli jistotu, že nelze publikovat Pokud došlo k chybám ověření balíčku v **upozornění považovat za chyby**, vyberte **hodnotu True**.

1. Abyste měli jistotu, že vaše role web používá stejný port každém spuštění místně v IIS Express v **použít web projektu porty**, vyberte **hodnotu True**. Pokud chcete použít určitý port pro konkrétní web projektu, otevření místní nabídky pro web project klikněte na kartu **Vlastnosti** , klikněte na kartu **Web** a změnit číslo portu v nastavení **Adresa Url Project** v části **Služby IIS Express** . Zadejte například `http://localhost:14020` jako adresa URL project.

1. Uložte změny, které jste provedli vlastnosti projektu cloudové služby, klikněte na tlačítko **Uložit** na panelu nástrojů.

## <a name="next-steps"></a>Další kroky

Další informace o tom, jak konfigurovat Azure cloudové služby projekty ve Visual Studiu najdete v tématu [Konfigurace Azure projektu pomocí několika konfigurace služeb](vs-azure-tools-multiple-services-project-configurations.md).
