<properties
    pageTitle="Provádění synchronizace hesel se synchronizací Azure AD Connect | Microsoft Azure"
    description="Informace o fungování synchronizace hesel a jak ji povolit."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="markusvi;andkjell"/>


# <a name="implementing-password-synchronization-with-azure-ad-connect-sync"></a>Provádění synchronizace hesel se synchronizací Azure AD Connect
Toto téma obsahuje informace, abyste mohli synchronizovat vaše aby platnost uživatelského hesla z místního Active Directory (AD) cloudové Azure Active Directory (Azure AD).

## <a name="what-is-password-synchronization"></a>Co je synchronizace hesel
Pravděpodobnost, že jsou blokovány práce s aplikací z důvodu zapomenuté heslo souvisí s počet různých hesel, je třeba pamatovat. Další hesla, musíte mít na paměti, vyšší pravděpodobnost v žádném případě nezapomeňte jednu. Dotazy a volání o resetování hesla a další heslo záležitostech týkajících se požadovat mnoho prostředků technickou podporu.

Synchronizace hesel je funkce synchronizovat aby platnost uživatelského hesla z místní služby Active Directory cloudové Azure Active Directory (Azure AD).
Tato funkce umožňuje přihlášení ke službám Azure Active Directory (například Office 365, Microsoft Intune, CRM Online a Azure AD Domain Services) použití stejné heslo používáte pro přihlášení ke službě Active Directory vaší místní.

![Co je Azure AD Connect](./media/active-directory-aadconnectsync-implement-password-synchronization/arch1.png)

Omezením počtu hesla, které uživatelé potřebují udržovat jenom s jedním synchronizace hesel vám pomůže:

- Zlepšete produktivitu uživatelů
- Snížit náklady na technickou podporu  

Také pokud vyberete možnost používat [**federaci s AD FS**](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect), můžete volitelně povolit synchronizace hesel jako zálohu v případě, že se nezdaří infrastruktury služby AD FS.

Synchronizace hesel je rozšíření funkci synchronizace adresářů prováděným Azure AD Connect synchronizace. Použití synchronizace hesel v prostředí, potřebujete:

- Instalace Azure AD připojit  
- Konfigurace synchronizace adresářů mezi vaší místní AD a Azure Active Directory
- Povolení synchronizace hesel

Další podrobnosti najdete v tématu [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)

> [AZURE.NOTE] Podrobné informace o Active Directory Domain Services nakonfigurované pro synchronizaci FIPS a heslo najdete v článku [Synchronizace hesel a FIPS](#password-synchronization-and-fips).

## <a name="how-password-synchronization-works"></a>Jak funguje synchronizace hesel
Služba Active Directory domain ukládá hesla v podobě formátovanými jako hash hodnotu skutečné uživatelského hesla. Hodnotu hash je výsledkem jednosměrné matematické funkce ("*algoritmus hash*"). Neexistuje žádná metoda vrátit výsledek jednosměrné funkce ve formátu prostého textu verzi hesla. Algoritmus hash hesla nelze použít k přihlášení k místní síti.

Synchronizovat svoje heslo, Azure AD Connect synchronizace extrahuje algoritmus hash hesla z místní služby Active Directory. Zpracování navíc zabezpečení se použije pro algoritmus hash hesla před synchronizací ověřování služby Azure Active Directory. Hesla se synchronizují na jednotlivé uživatele a chronologicky.

Skutečnými daty tok procesu synchronizace heslo je podobný synchronizaci uživatelských dat například DisplayName nebo e-mailové adresy. Hesla se však synchronizují častěji než okno synchronizace adresářů standardní pro další atributy. Synchronizace heslo probíhá každé dvě minuty. Nemůžete změnit frekvenci tento proces. Při synchronizaci heslo databáze přepíše stávající heslo cloudu.

Při prvním povolit funkci Synchronizace hesel, provede počáteční synchronizaci hesel všem uživatelům v rozsahu. Není možné definovat explicitně podmnožinu uživatelská hesla, který chcete synchronizovat.

Pokud změníte heslo místního, aktualizované heslo je synchronizovaný, nejčastěji během několika minut.
Funkci synchronizace heslo ztracená pokusech o synchronizaci selhalo. Pokud dojde k chybě při pokusu o synchronizaci hesla, k chybě přihlášení svůj prohlížeč událostí.

Synchronizace hesla nemá žádný vliv na aktuálně přihlášeného uživatele.
Aktuální relaci služby cloudu nebyl ovlivněn změnu synchronizované hesla, ke kterým dochází při přihlášení ke službě cloudového okamžitě. Však cloudovou službu vyžaduje ověření znovu, musíte k zadání nového hesla.

> [AZURE.NOTE] Synchronizace hesel je podporována pouze pro uživatele typ objektu ve službě Active Directory. Není podporovaná pro typ objektu iNetOrgPerson.

### <a name="how-password-synchronization-works-with-azure-ad-domain-services"></a>Jak funguje synchronizace hesel u Azure AD Domain Services
Můžete taky použít funkci synchronizace heslo k synchronizaci místních hesla k [Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-overview.md). Tento scénář umožňuje Azure AD Domain Services ověření uživatelé v cloudu pomocí metody k dispozici ve vaší místní AD. Možnosti v tomto scénáři je podobný pomocí migraci nástroj ADMT (Active Directory) v místním prostředí.

### <a name="security-considerations"></a>Otázky bezpečnosti pro
Při synchronizaci hesel, svoje heslo ve formátu prostého textu, nebude vystaven funkci synchronizace heslo Azure AD, nebo na jakékoli přidružené služby.

Je také žádný požadavek na na adresářová služba Active Directory pro ukládání ve formátu obousměrně šifrované heslo. Výtah algoritmus hash hesla služby Active Directory se používá k přenosu mezi místní AD a Azure Active Directory. Přehledu algoritmus hash hesla se nedají použít k přístupu k prostředkům ve vašem místním prostředí.

### <a name="password-policy-considerations"></a>Informace o heslech zásad
Existují dva typy zásady pro hesla, které jsou ovlivněny povolení synchronizace hesel:

1. Zásady složitost hesla
2. Zásad vypršení platnosti hesla

**Zásady složitost hesla**  
Pokud zapnete synchronizaci hesel, zásady složitost hesla v na adresářová služba Active Directory přepsat složitost zásady v cloudu pro synchronizovaní uživatelé. Můžete všechny platné hesla na adresářová služba Active Directory pro přístup ke službám Azure AD.

> [AZURE.NOTE] Hesla pro uživatele, kteří vytvářejí přímo v cloudu podléhají pořád zásady pro hesla definovaný v cloudu.

**Zásad vypršení platnosti hesla**  
Pokud je uživatel v rozsahu synchronizace hesel, heslo účtu cloudu nastavenou*Neomezené platnosti"*.
Přihlaste se ke službám cloudu pomocí synchronizované heslo, které vypršela v místním prostředí můžete dál. Heslo cloudu se aktualizuje při příštím změnit heslo do místního prostředí.

### <a name="overwriting-synchronized-passwords"></a>Přepsání synchronizaci hesel
Správce může ručně resetování hesla pomocí Windows Powershellu.

V tomto případě nové heslo přepíše synchronizované heslo a všechny zásady pro hesla definovaný v cloudu platí pro nové heslo.

Pokud změníte své heslo místního znovu nové heslo synchronizované do cloudu a přepíše ručně aktualizované heslo.

## <a name="enabling-password-synchronization"></a>Povolení synchronizace hesel
Synchronizace hesel automaticky zapnutá, když nainstalujete Azure AD Connect pomocí **Expresní nastavení**. Další informace najdete v tématu [Začínáme s Azure AD Connect pomocí Expresní nastavení](./connect/active-directory-aadconnect-get-started-express.md).

Pokud používáte vlastní nastavení při instalaci Azure AD Connect, povolení synchronizace heslo na přihlašovací stránce uživatele. Další informace najdete v tématu [Vlastní instalace Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md).

![Povolení synchronizace hesel](./media/active-directory-aadconnectsync-implement-password-synchronization/usersignin.png)

### <a name="password-synchronization-and-fips"></a>Synchronizace hesel a FIPS
Pokud váš server uzamčená dolů podle informace standardní FIPS (Federal Processing), potom MD5 byl zakázán.

**Chcete-li povolit MD5 synchronizaci hesel, proveďte následující kroky:**

1. Přejděte na **%programfiles%\Azure AD Sync\Bin**.
2. Otevřete **miiserver.exe.config**.
3. Přejděte na uzel **Konfigurace/runtime** (na konci souboru).
4. Přidání uzel takto:`<enforceFIPSPolicy enabled="false"/>`
5. Uložte provedené změny.

Pro informaci tohoto výstřižku se, jak by měl vypadat:

```
    <configuration>
        <runtime>
            <enforceFIPSPolicy enabled="false"/>
        </runtime>
    </configuration>
```

Informace o zabezpečení a FIPS najdete v článku [AAD Sync hesla a šifrování FIPS dodržování předpisů](https://blogs.technet.microsoft.com/enterprisemobility/2014/06/28/aad-password-sync-encryption-and-fips-compliance/)

## <a name="troubleshooting-password-synchronization"></a>Poradce při potížích s synchronizace hesel
Pokud hesla nesynchronizujete podle očekávání, může to být podmnožinu uživatele nebo pro všechny uživatele.

- Pokud máte problém s jednotlivé objekty, pak v tématu [Poradce při potížích s jeden objekt, který nelze provést synchronizaci hesel](#troubleshoot-one-object-that-is-not-synchronizing-passwords).
- Pokud máte problém, kde jsou synchronizovány žádná hesla, přečtěte si článek [Poradce při potížích, kde jsou synchronizovány žádná hesla](#troubleshoot-issues-where-no-passwords-are-synchronized).

### <a name="troubleshoot-one-object-that-is-not-synchronizing-passwords"></a>Poradce při potížích s jeden objekt, který nelze provést synchronizaci hesel
Je můžete snadno heslo synchronizace Poradce při potížích s kontrolou stav objektu.

Spuštění ve **službě Active Directory uživatele a počítačů**. Najděte uživatele a zkontrolujte, že **uživatel musí změnit heslo při příštím přihlášení** nevybrané.
![Active Directory produktivní hesla](./media/active-directory-aadconnectsync-implement-password-synchronization/adprodpassword.png)  
Pokud je vybrána, požádejte uživatele k přihlášení a změnit heslo. Dočasná hesla nejsou synchronizovány Azure AD.

Pokud to vypadá v pořádku ve službě Active Directory, dalším krokem je sledování uživatele v modulu synchronizace. Provedením uživatel Azure AD z místní služby Active Directory, uvidíte, pokud je na požadovaném objektu chybu popisný.

1. Spusťte **[Správce služby synchronizace](active-directory-aadconnectsync-service-manager-ui.md)**.
2. Klikněte na **spojovací čáry**.
3. Vyberte **Konektor služby Active Directory** uživatele se nachází v.
4. Vyberte **místo vyhledávací spojnici**.
5. Najděte uživatele, kterého hledáte.
6. Vyberte kartu **souvisejících zdrojích** a ujistěte se, že alespoň jedno pravidlo synchronizace uvedený **Synchronizaci hesel** jako **PRAVDA**. Ve výchozím nastavení je v poli Název pravidla synchronizace **v z AD - uživatele AccountEnabled**.  
    ![Informace o souvisejících zdrojích o uživateli](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync.png)  
7. Pak [postupujte podle uživatele](active-directory-aadconnectsync-service-manager-ui-connectors.md#follow-an-object-and-its-data-through-the-system) až metaverse mezery Azure AD spojnice. Objekt místo spojnice měli odchozího pravidla se **Synchronizaci hesel** nastavena na **hodnotu True**. Ve výchozím nastavení je v poli Název pravidla synchronizace **na AAD - uživatel připojit**.  
    ![Spojnice místo vlastnostech uživatele](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync2.png)  
8. Zobrazit podrobnosti synchronizace heslo objektu pro uplynulém týdnu, klikněte na **protokolu..**.  
    ![Podrobnosti o protokolu objektu](./media/active-directory-aadconnectsync-implement-password-synchronization/csobjectlog.png)  

Ve sloupci stav může mít tyto hodnoty:

Stav | Popis
---- | -----
Úspěch | Heslo úspěšně synchronizování.
FilteredByTarget | **Uživatel musí**změnit heslo při příštím přihlášení je nastavit heslo. Nesynchronizovala heslo.
NoTargetConnection | Žádný objekt v metaverse nebo do pole Azure AD spojnice.
SourceConnectorNotPresent | Žádný objekt v oboru místní služby Active Directory spojnice.
TargetNotExportedToDirectory | Objektu v prostoru spojnice Azure AD ještě ještě exportovány.
MigratedCheckDetailsForMoreInfo | Záznam byla vytvořená před sestavení 1.0.9125.0 a zobrazí se ve stavu starší verze.

### <a name="troubleshoot-issues-where-no-passwords-are-synchronized"></a>Poradce při potížích, kde jsou synchronizovány bez hesla
Začněte tím, že spuštění skriptu v části [Stav nastavení synchronizace heslo](#get-the-status-of-password-sync-settings). Poskytuje přehled konfigurace synchronizace heslo.  
![Výstup skript Powershellu nastavení synchronizace hesel](./media/active-directory-aadconnectsync-implement-password-synchronization/psverifyconfig.png)  
Pokud není zapnuta v Azure AD nebo pokud není povolený kanálu stav synchronizace, spusťte Průvodce instalací připojit. Klikněte na **Vlastní nastavení synchronizace možnosti** a zrušte zaškrtnutí políčka synchronizaci hesel. Tato změna dočasně zakáže funkci. Potom spusťte průvodce a znovu povolit synchronizaci hesel. Spusťte skript znovu a zkontrolujte správnost konfiguraci.

Když se při skript, který je žádné prezenční signál, spusťte skript v [aktivační událost úplné synchronizaci všech hesel](#trigger-a-full-sync-of-all-passwords). Tento skript lze také ostatních případech, kdy konfigurace je správně, ale nejsou synchronizovány hesla.

#### <a name="get-the-status-of-password-sync-settings"></a>Stav nastavení synchronizace hesel

```
Import-Module ADSync
$connectors = Get-ADSyncConnector
$aadConnectors = $connectors | Where-Object {$_.SubType -eq "Windows Azure Active Directory (Microsoft)"}
$adConnectors = $connectors | Where-Object {$_.ConnectorTypeName -eq "AD"}
if ($aadConnectors -ne $null -and $adConnectors -ne $null)
{
    if ($aadConnectors.Count -eq 1)
    {
        $features = Get-ADSyncAADCompanyFeature -ConnectorName $aadConnectors[0].Name
        Write-Host
        Write-Host "Password sync feature enabled in your Azure AD directory: "  $features.PasswordHashSync
        foreach ($adConnector in $adConnectors)
        {
            Write-Host
            Write-Host "Password sync channel status BEGIN ------------------------------------------------------- "
            Write-Host
            Get-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector.Name
            Write-Host
            $pingEvents =
                Get-EventLog -LogName "Application" -Source "Directory Synchronization" -InstanceId 654  -After (Get-Date).AddHours(-3) |
                    Where-Object { $_.Message.ToUpperInvariant().Contains($adConnector.Identifier.ToString("D").ToUpperInvariant()) } |
                    Sort-Object { $_.Time } -Descending
            if ($pingEvents -ne $null)
            {
                Write-Host "Latest heart beat event (within last 3 hours). Time " $pingEvents[0].TimeWritten
            }
            else
            {
                Write-Warning "No ping event found within last 3 hours."
            }
            Write-Host
            Write-Host "Password sync channel status END ------------------------------------------------------- "
            Write-Host
        }
    }
    else
    {
        Write-Warning "More than one Azure AD Connectors found. Please update the script to use the appropriate Connector."
    }
}
Write-Host
if ($aadConnectors -eq $null)
{
    Write-Warning "No Azure AD Connector was found."
}
if ($adConnectors -eq $null)
{
    Write-Warning "No AD DS Connector was found."
}
Write-Host
```

#### <a name="trigger-a-full-sync-of-all-passwords"></a>Aktivace úplné synchronizaci všech hesel
Můžete aktivovat úplné synchronizaci všech hesla pomocí následující skript:

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
$aadConnector = "<CASE SENSITIVE AAD CONNECTOR NAME>"
Import-Module adsync
$c = Get-ADSyncConnector -Name $adConnector
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1
$c.GlobalParameters.Remove($p.Name)
$c.GlobalParameters.Add($p)
$c = Add-ADSyncConnector -Connector $c
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $false
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $true
```

## <a name="next-steps"></a>Další kroky

* [Azure AD připojení synchronizace: Přizpůsobení možností synchronizace](active-directory-aadconnectsync-whatis.md)
* [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)
