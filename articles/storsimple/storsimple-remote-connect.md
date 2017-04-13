<properties 
   pageTitle="Vzdáleně připojovat k zařízení StorSimple | Microsoft Azure"
   description="Vysvětluje, jak nakonfigurovat zařízení Vzdálená správa a jak se připojit k prostředí Windows PowerShell pro StorSimple prostřednictvím protokolu HTTP nebo HTTPS."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/21/2016"
   ms.author="alkohli" />

# <a name="connect-remotely-to-your-storsimple-device"></a>Připojení vzdálených StorSimple zařízení

## <a name="overview"></a>Základní informace

Vzdálená prostředí Windows PowerShell můžete připojit k StorSimple zařízení. Když se připojíte tímto způsobem, neuvidíte nabídku. (Zobrazí nabídku jenom v případě, že použijete konzole sériové na zařízení k připojení.) Pomocí prostředí Windows PowerShell Vzdálená připojit k určité prostředí runspace. Můžete taky určit jazyk zobrazení. 

Další informace o používání prostředí Windows PowerShell Vzdálená správa zařízení, přejděte na [Pomocí Windows Powershellu pro StorSimple ke správě StorSimple zařízení](storsimple-windows-powershell-administration.md).

Tento kurz vysvětleno, jak nakonfigurovat zařízení Vzdálená správa a jak se připojit k prostředí Windows PowerShell pro StorSimple. Připojení přes Windows PowerShell Vzdálená můžete pomocí protokolu HTTP nebo HTTPS. Při výběru způsobu připojení k prostředí Windows PowerShell pro StorSimple, ale, zvažte následující skutečnosti: 

- Přímo na konzole sériové zařízení se můžete připojit zabezpečené, ale připojení ke konzole sériové přes síťové přepínače není. Buďte obezřetní rizika zabezpečení při připojení konzoly sériové zařízení přes síťové přepínače. 

- Připojení přes HTTP relaci může nabízejí větší zabezpečení než připojení prostřednictvím sériové konzoly v síti. Není to však nejbezpečnější způsob, je přípustné v důvěryhodném sítích. 

- Připojení přes protokol HTTPS v relaci pomocí certifikátu podepsaného svým držitelem je nejbezpečnější a doporučené možnost.

Můžete se připojit vzdálené rozhraní prostředí Windows PowerShell. Vzdálený přístup k zařízení StorSimple prostřednictvím rozhraní prostředí Windows PowerShell, ale není povolený ve výchozím nastavení. Je třeba nejprve povolit vzdálená správa zařízení a potom na straně klienta, který se používá pro přístup zařízení.

Podle kroků popsaných v tomto článku byly provedeny na hostitele systému Windows Server 2012 R2.

## <a name="connect-through-http"></a>Připojení pomocí protokolu HTTP

Připojení k prostředí Windows PowerShell pro StorSimple prostřednictvím protokolu HTTP relace nabízí větší zabezpečení než připojení prostřednictvím konzole sériové StorSimple zařízení. Není to však nejbezpečnější způsob, je přípustné v důvěryhodném sítích.

Portál Azure klasické nebo sériové konzoly můžete používá ke konfiguraci Vzdálená správa. Vyberte jednu z následujících postupů:

- [Používat portál Azure klasické přes protokol HTTP povolit vzdálená správa](#use-the-azure-classic-portal-to-enable-remote-management-over-http)

- [Pomocí sériové konzoly přes protokol HTTP povolit vzdálená správa](#use-the-serial-console-to-enable-remote-management-over-http)

Po povolení Vzdálená správa pomocí následujícího postupu Příprava klienta pro připojení ke vzdálené.

- [Příprava klienta pro připojení ke vzdálené](#prepare-the-client-for-remote-connection)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-http"></a>Používat portál Azure klasické přes protokol HTTP povolit vzdálená správa 

Na portálu Azure klasické přes protokol HTTP povolit vzdálená správa proveďte následující kroky.

#### <a name="to-enable-remote-management-through-the-azure-classic-portal"></a>Chcete-li povolit vzdálená správa portálu Azure klasické

1. Přístup k **zařízení** > **Konfigurovat** pro svoje zařízení.

2. Přejděte dolů do části **Vzdálená správa** .

3. **Povolení vzdáleného řízení** nastavena na hodnotu **Ano**.

4. Teď můžete připojit pomocí protokolu HTTP. (Výchozí hodnota je připojení přes protokol HTTPS.) Ujistěte se, že je vybraná HTTP.

    >[AZURE.NOTE] Připojení přes HTTP je jen v důvěryhodných sítích.

6. Klepněte na tlačítko **Uložit** v dolní části stránky.

### <a name="use-the-serial-console-to-enable-remote-management-over-http"></a>Pomocí sériové konzoly přes protokol HTTP povolit vzdálená správa

Proveďte následující kroky v konzole sériové zařízení povolit vzdálená správa.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Chcete-li povolit vzdálená správa pomocí sériové konzoly zařízení

1. V nabídce sériové konzola vyberte možnost 1. Další informace o používání sériové konzoly na zařízení přejděte na [připojit k prostředí Windows PowerShell pro StorSimple pomocí sériové konzoly zařízení](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console).

2. Na příkazovém řádku zadejte:`Enable-HcsRemoteManagement –AllowHttp`

3. Zobrazí se oznámení o zabezpečením použití protokolu HTTP se připojit k zařízení. Po zobrazení výzvy, potvrďte zadáním **Y**.

4. Povolte HTTP zadáním:`Get-HcsSystem`

5. Ověřte, že pole **RemoteManagementMode** zobrazí **HttpsAndHttpEnabled**. Následující obrázek znázorňuje nastavení v nátěrové.

     ![Sériové HTTPS a HTTP povolena](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-the-client-for-remote-connection"></a>Příprava klienta pro připojení ke vzdálené

Proveďte následující kroky v klientském počítači k povolení vzdáleného řízení.

#### <a name="to-prepare-the-client-for-remote-connection"></a>Příprava klienta pro připojení ke vzdálené

1. Spuštění relace s prostředí Windows PowerShell jako správce.

2. Zadejte tento příkaz Přidat do seznamu důvěryhodných hostitelů klienta IP adresu StorSimple zařízení: 

     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`

     Nahraďte <*device_ip*> IP adresu zařízení; Příklad: 

     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`

3. Zadejte následující příkaz Uložit přihlašovací údaje zařízení v proměnnou: 

     *$cred = get-přihlašovacích údajů*

4. V dialogovém okně, které se zobrazí:

    1. Zadejte uživatelské jméno v tomto formátu: *device_ip\SSAdmin*.
    2. Zadejte heslo správce zařízení, která zařízení bylo při konfiguraci pomocí Průvodce instalací. Výchozí heslo je *hesla1*.

7. Spuštění relace s prostředí Windows PowerShell na zařízení tak, že zadáte tento příkaz:

     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`

     >[AZURE.NOTE] Chcete-li vytvořit relaci prostředí Windows PowerShell pro použití s virtuální zařízení StorSimple, přidejte `–Port` parametr a určete veřejné číslo portu, které jste nakonfigurovali ve Vzdálená pro StorSimple virtuální zařízení.

     V tomto okamžiku byste si měli aktivní vzdálené prostředí Windows PowerShell relaci zařízení.

    ![Vzdálený PowerShell pomocí protokolu HTTP](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a>Připojte se pomocí HTTPS

Připojení k prostředí Windows PowerShell pro StorSimple prostřednictvím relaci HTTPS je nejbezpečnější a doporučené metoda vzdáleně připojení k Microsoft Azure StorSimple zařízení. Následující postupy popisují, jak nastavit sériové konzoly a klientské počítače tak, že používáte HTTPS pro připojení k prostředí Windows PowerShell pro StorSimple.

Portál Azure klasické nebo sériové konzoly můžete používá ke konfiguraci Vzdálená správa. Vyberte jednu z následujících postupů:

- [Pomocí portálu Azure klasické umožníte Vzdálená správa přes protokol HTTPS](#use-the-azure-classic-portal-to-enable-remote-management-over-https)

- [Pomocí sériové konzoly umožníte Vzdálená správa přes protokol HTTPS](#use-the-serial-console-to-enable-remote-management-over-https)

Když povolíte vzdálená správa, použijte následující postupy a připravit hostiteli Vzdálená správa připojení k zařízení ze vzdáleného hostitele.

- [Příprava hostiteli Vzdálená správa](#prepare-the-host-for-remote-management)

- [Připojení k zařízení ze vzdáleného hostitele](#connect-to-the-device-from-the-remote-host)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-https"></a>Pomocí portálu Azure klasické umožníte Vzdálená správa přes protokol HTTPS

Na portálu Azure klasické přes HTTPS povolit vzdálená správa proveďte následující kroky.

#### <a name="to-enable-remote-management-over-https-from-the-azure-classic-portal"></a>Chcete-li povolit vzdálená správa přes protokol HTTPS z portálu Microsoft Azure klasické

1. Přístup k **zařízení** > **Konfigurovat** pro svoje zařízení.

2. Přejděte dolů do části **Vzdálená správa** .

3. **Povolení vzdáleného řízení** nastavena na hodnotu **Ano**.

4. Nyní můžete se připojit pomocí HTTPS. (Výchozí hodnota je připojení přes protokol HTTPS.) Ujistěte se, že je vybraná HTTPS. 

5. Klikněte na tlačítko **Stáhnout Vzdálená správa certifikát**. Zadejte umístění pro uložení souboru. Bude muset nainstalovat certifikát v počítači klienta nebo Host (hostitel), který použijete k připojení k zařízení.

6. Klepněte na tlačítko **Uložit** v dolní části stránky.

### <a name="use-the-serial-console-to-enable-remote-management-over-https"></a>Pomocí sériové konzoly umožníte Vzdálená správa přes protokol HTTPS

Proveďte následující kroky v konzole sériové zařízení povolit vzdálená správa.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Chcete-li povolit vzdálená správa pomocí sériové konzoly zařízení

1. V nabídce sériové konzola vyberte možnost 1. Další informace o používání sériové konzoly na zařízení přejděte na [připojit k prostředí Windows PowerShell pro StorSimple pomocí sériové konzoly zařízení](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console).

2. Na příkazovém řádku zadejte: 

     `Enable-HcsRemoteManagement`

    To umožňoval HTTPS na vašem zařízení.

3. Ověřte, zda byla povolena HTTPS zadáním: 

     `Get-HcsSystem`

    Ujistěte se, že pole **RemoteManagementMode** zobrazí **HttpsEnabled**. Následující obrázek znázorňuje nastavení v nátěrové.

     ![Sériové HTTPS povolena](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)

4. Z výstupu `Get-HcsSystem`, zkopírujte pořadové číslo zařízení a uložit pro pozdější použití.

    >[AZURE.NOTE] Pořadové číslo odpovídá názvu CN v certifikátu.

5. Získejte certifikát Vzdálená správa zadáním: 
 
     `Get-HcsRemoteManagementCert`

    Podobně jako tento certifikát se zobrazí.

    ![Získání Vzdálená správa certifikátů](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)

5. Zkopírujte údaje v certifikátu z **---počáteční certifikát---** ukončíte **---certifikát---** do textového editoru, jako je Poznámkový blok a uložte jako soubor .cer. (Zkopírujete tento soubor na vzdálené hostitele při přípravě hostiteli.)

    >[AZURE.NOTE] Vytvořit nový certifikát, můžete `Set-HcsRemoteManagementCert` rutiny.

### <a name="prepare-the-host-for-remote-management"></a>Příprava hostiteli Vzdálená správa

Příprava hostitelském počítači pro připojení ke vzdálené, který používá protokol HTTPS v relaci, postupujte takto:

- [Importovat soubor .cer do kořenové úložiště klienta nebo vzdáleného hostitele](#to-import-the-certificate-on-the-remote-host).

- [Přidání zařízení pořadová čísla do souboru hosts na vzdáleného hostitele](#to-add-device-serial-numbers-to-the-remote-host).

Každá z těchto postupů je popsána níže.

#### <a name="to-import-the-certificate-on-the-remote-host"></a>Import certifikátu přihlášeného

1. Klikněte pravým tlačítkem myši na soubor .cer a vyberte **nainstalovat certifikát**. Spustí Průvodce importem certifikátu.

    ![Průvodce importem certifikátu 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)

2. **Umístění úložiště**vyberte **Místního počítače**a klikněte na tlačítko **Další**.

3. Vyberte možnost **umístit certifikátů v úložišti následující**a pak klikněte na **Procházet**. Přejděte do kořenové úložiště hostitele vzdálené a klikněte na tlačítko **Další**.

    ![Průvodce importem certifikátu 2](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)

4. Klikněte na **Dokončit**. Zobrazí se zpráva informující o tom, že import byl úspěšný.

    ![Průvodce importem certifikátu 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="to-add-device-serial-numbers-to-the-remote-host"></a>Chcete-li přidat zařízení pořadová čísla k hostiteli remote

1. Spusťte Poznámkový blok jako správce a otevřete soubor hosts c:\ \Windows\System32\Drivers\etc.

2. Přidejte následující tři položky do souboru hosts: **DATA 0 IP adresu**, **řadiče 0 pevné IP adresa**a **řadiče 1 pevné IP adresu**.

3. Zadejte pořadové číslo zařízení, které jste si předtím uložili. Namapujte to k IP adrese, jak je znázorněno na následujícím obrázku. U řadiče 0 a 1 řadiče domény přidejte **Controller0** a **Controller1** na konci pořadové číslo (CN název).

    ![Přidání CN názvu souboru hostitelů](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)

4. Uložit soubor hosts.

### <a name="connect-to-the-device-from-the-remote-host"></a>Připojení k zařízení ze vzdáleného hostitele

Umožňuje prostředí Windows PowerShell a SSL zadejte relaci SSAdmin na vašem zařízení ze vzdáleného hostitele nebo klienta. Relace SSAdmin mapy na možnost 1 v nabídce [sériové konzola](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console) zařízení.

Pomocí následujícího postupu v počítači, ze které chcete udělat připojení ke vzdálené prostředí Windows PowerShell.

#### <a name="to-enter-an-ssadmin-session-on-the-device-by-using-windows-powershell-and-ssl"></a>Pokud chcete zadat relaci SSAdmin na zařízení pomocí Windows Powershellu a SSL

1. Spuštění relace s prostředí Windows PowerShell jako správce.

2. Přidáním zařízení IP adresa klienta důvěryhodných hostitelů psát:

     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`

    Kde <*device_ip*> je IP adresa zařízení; Příklad: 

     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`

3. Vytvořte nový přihlašovacích údajů: 

     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`

    Kde je <*IP cílové zařízení*> IP adresu DATA 0 pro zařízení s; například **10.126.173.90** jak ukazuje předchozí obrázek souboru hosts. Navíc, zadejte heslo správce pro svoje zařízení.

4. Vytvořte relaci:

     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`

    Pro parametr - ComputerName rutinu zadejte <*pořadové číslo cílové zařízení*>. Pořadové číslo byl namapované na IP adresu dat v souboru hosts na hostitele vzdálené; 0 například **SHX0991003G44MT** jak je znázorněno na následujícím obrázku.

5. Typ: 

     `Enter-PSSession $session`

6. Budete muset počkat několik minut a pak budete připojeni do zařízení prostřednictvím HTTPS přes připojení SSL. Zobrazí se zpráva, že jste připojeni k zařízení.

    ![Vzdálený PowerShell pomocí HTTPS a SSL](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a>Další kroky

- Další informace o [používání Windows Powershellu ke správě StorSimple zařízení](storsimple-windows-powershell-administration.md).

- Další informace o [použití služby StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md).
