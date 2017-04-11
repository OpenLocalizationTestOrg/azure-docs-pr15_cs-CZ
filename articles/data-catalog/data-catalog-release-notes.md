<properties
   pageTitle="Poznámky k verzi Azure katalog dat | Microsoft Azure"
   description="Poznámky k verzi pro katalog dat Azure."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-release-notes"></a>Poznámky k verzi Azure katalogu dat

## <a name="notes-for-the-november-20-2015-release-of-azure-data-catalog"></a>Poznámky pro 20 listopad 2015 vydání katalog dat Azure

### <a name="opening-data-sources-in-power-bi-desktop"></a>Otevření zdroje dat v Power BI Desktop

Při použití možnosti "Otevřít v Power BI Desktop" z portálu Microsoft **Azure katalogu dat** , uživatelé můžou v sharepointu 2010 jedna dva problémů v aplikaci Power BI Desktop:

- Zobrazí se dialogové okno s názvem "Nelze do otevřeného dokumentu"
- Aplikace Power BI Desktop se otevře, ale tento soubor nebude prázdné

Pro každou situaci problému může vyřešit stáhněte a nainstalujte nejnovější verzi Power BI Desktop z [PowerBI.com](https://powerbi.com).

## <a name="notes-for-the-november-13-2015-release-of-azure-data-catalog"></a>Poznámky z 13 listopadu 2015 vydání katalog dat Azure

### <a name="registering-and-connecting-to-teradata"></a>Registrace a připojení k Teradata

Při připojení k Teradata zdrojům dat uživatele jste nainstalovali správný ovladač Teradata ODBC, které odpovídají počtu bitů (32bitová verze nebo 64bitová verze) software, který se používá.

K tomuto datu vydání ADC posledních [ovladač Teradata ODBC pro windows (verze 15.10)](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) je kompatibilní s Office 2013, ale ne s Office 2016.

## <a name="notes-for-the-july-13-2015-release-of-azure-data-catalog"></a>Poznámky k 13 červenec 2015 vydání katalog dat Azure

### <a name="registering-and-connecting-to-oracle-database"></a>Registrace a připojení k databázi Oracle

Při připojení ke zdrojům dat databáze Oracle uživatelé jste nainstalovali správné ovladače Oracle, které odpovídají počtu bitů (32bitová verze nebo 64bitová verze) software, který se používá.

-   Při registraci zdroje dat Oracle v počítači se systémem Windows 32bitová verze se použijí ovladače Oracle 32bitová verze
-   Při registraci zdroje dat Oracle na počítači s 64bitovou verzi Windows, bude použita ovladače Oracle 64bitová verze
-   Při připojení ke zdrojům dat Oracle kroky v Excelu v počítači spuštěná 32bitová verze systému Microsoft Office na 64bitovém systému Windows, včetně ovladače Oracle 32bitová verze se použijí
-   Při připojení ke zdrojům dat Oracle kroky v Excelu na počítači s 64bitovou verzi systému Microsoft Office, 64-bit Oracle ovladače se použijí

### <a name="registering-and-connecting-to-sql-server-reporting-services"></a>Registrace a připojení k SQL Server Reporting Services

Podpora pro zdroje dat SQL Server Reporting Services (SSRS) je aktuálně omezena pouze servery nativním režimu. Podpora pro SSRS v integrovaném režimu sharepointu pole bude přidán do nové verze.

### <a name="opening-data-assets-in-excel"></a>Otevření prostředky dat v Excelu

Při otevření prostředky dat v aplikaci Microsoft Excel z portálu Microsoft **Azure katalogu dat** , uživatelé mohou výzva dialogové okno **Upozornění zabezpečení aplikace Microsoft Excel** . Toto je standardní, očekávaná a uživatelé můžou zaškrtněte políčko **Povolit** pokračovat.

Další informace najdete v tématu [Povolení nebo zakázání výstrah zabezpečení týkajících se odkazů a souborů z podezřelých webů](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a>Konfigurace proxy serveru a zásady a datové zdroje registrace

Uživatelé můžou nastat situace, místo, kam se můžete přihlásit k portálu katalog dat Azure, ale při pokusu o přihlášení k nástroji registrace zdroje dat se kterými se setkávají chybovou zprávu, které brání přihlášení.

Existují dvě možné příčiny pro tento problém chování:

**Příčina 1: konfigurace Active Directory Federation Services** Registrační nástroj zdroj dat používá ověřování pomocí formulářů pro ověřování přihlášení proti služby Active Directory. Pro úspěšné přihlášení ověřování pomocí formulářů v musí být povolený globální zásadu ověření správcem služby Active Directory.

V některých případech může dojít toto chování chyby pouze v případě, že uživatel je v podnikové síti nebo pouze v případě, že se uživatel připojuje mimo podnikové sítě. Globální zásady ověřování umožňuje metody ověřování povolit nezávisle na sobě pro intranetové nebo extranetové připojení. Chyby při přihlášení k může dojít, pokud není povolený ověřování pomocí formulářů pro síť, ze kterého se uživatel připojuje.

Další informace najdete v tématu [Konfigurace zásad ověřování](https://technet.microsoft.com/library/dn486781.aspx).

**Příčina 2: konfigurace proxy sítě** Pokud k podnikové síti používá proxy server, nemusí být registrační nástroj připojit k Azure Active Directory pomocí serveru proxy. Uživatele můžete zajistit, aby registrační nástroj úpravou nástroje konfiguračního souboru přidání Tato část do souboru:


      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


Vyhledejte soubor RegistrationTool.exe.config, spuštění nástroje pro registraci a potom spusťte nástroj pro správce úloh ve Windows. Na kartě Podrobnosti v Správce úloh klikněte pravým tlačítkem myši na RegistrationTool.exe a zvolte Otevřít umístění souboru z místní nabídky.
