<properties 
   pageTitle="Nastavení proxy serveru webové pro zařízení StorSimple | Microsoft Azure"
   description="Naučte se používat Windows PowerShell pro StorSimple ke konfiguraci nastavení serveru proxy web pro zařízení s StorSimple."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-web-proxy-for-your-storsimple-device"></a>Konfigurace proxy serveru webové StorSimple zařízení

## <a name="overview"></a>Základní informace

Tento kurz popisuje, jak používat Windows PowerShell pro StorSimple ke konfiguraci a zobrazit nastavení proxy serveru webové pro zařízení s StorSimple. Nastavení proxy serveru webové používají zařízení StorSimple při komunikaci s cloudu. Web proxy server slouží k přidání další úrovně zabezpečení, filtr obsahu mezipaměti požadavků na šířku pásma usnadnění nebo dokonce nápovědu analýzy.

Proxy serveru webové je volitelná konfigurace StorSimple zařízení. Konfigurace proxy serveru webové jenom přes Windows PowerShell pro StorSimple. Konfigurace vyskytuje dvou krocích:

1. Nejdřív konfigurovat nastavení serveru proxy web pomocí Průvodce nastavením nebo prostředí Windows PowerShell pro StorSimple rutiny.

2. Potom povolit nastavení proxy nakonfigurované webu pomocí Windows Powershellu pro StorSimple rutiny.

Po dokončení konfigurace serveru proxy web můžete zobrazit nastavení proxy server nakonfigurovaný web ve službě Microsoft Azure StorSimple správce a prostředí Windows PowerShell pro StorSimple. 

Po přečtení tohoto kurzu, budou moct:

- Konfigurace proxy serveru webové pomocí Průvodce nastavením a rutiny
- Povolení webového serveru proxy pomocí rutiny
- Zobrazení nastavení proxy serveru webové Azure klasické portálu
- Poradce při potížích během konfigurací proxy web


## <a name="configure-web-proxy-via-windows-powershell-for-storsimple"></a>Konfigurace proxy serveru webové prostřednictvím prostředí Windows PowerShell pro StorSimple

Použijte některý z následujících ke konfiguraci nastavení serveru proxy web:

- Průvodce nastavením, který vás provede kroky konfigurace.

- Rutin v prostředí Windows PowerShell pro StorSimple.

Každá z těchto metod pojednává v následujících částech.

## <a name="configure-web-proxy-via-the-setup-wizard"></a>Konfigurace proxy serveru webové pomocí Průvodce nastavením

Použití Průvodce nastavením vás provede kroky pro konfigurace proxy serveru webové. Proveďte následující kroky pro nastavení proxy serveru webové na vašem zařízení.

#### <a name="to-configure-web-proxy-via-the-setup-wizard"></a>Konfigurace proxy serveru webové pomocí Průvodce nastavením

1. V nabídce sériové konzola možnost 1, **Přihlaste se pomocí plný přístup** a zadejte **heslo správce zařízení**. Zadejte tento příkaz zahájit relaci Průvodce nastavením:

    `Invoke-HcsSetupWizard`

2. Pokud je poprvé, aby použili jste Průvodce nastavením pro registrace zařízení, budete muset nastavení všechny požadované síti, dokud nebude aktivován konfigurací proxy web. Pokud vaše zařízení již registrována, můžete přijmout všechna nastavení síťových, dokud nebude aktivován konfigurací proxy web. V Průvodci nastavením po zobrazení výzvy ke konfiguraci nastavení serveru proxy web zadejte **Ano**.

3. **Webová adresa URL Proxy**zadejte IP adresu nebo plně kvalifikovaný název domény (FQDN) serveru proxy web a číslo portu TCP, které se mají zařízení použít při komunikaci s cloudu. Použijte v tomto formátu:

    `http://<IP address or FQDN of the web proxy server>:<TCP port number>`

    Ve výchozím nastavení je zadané číslo portu TCP 8080.

4. Zvolte typ ověřování jako **NTLM**, **základní**nebo **žádný**. Základní je nejméně bezpečný ověřování pro konfiguraci proxy serveru. Správce sítě LAN NT (NTLM) je vysoce zabezpečeném a složité ověřování protokol, který používá třemi způsob zasílání zpráv systému, (někdy čtyři Pokud další integrity je potřeba) k ověření uživatele. Výchozí ověřování je NTLM. Další informace najdete v tématu [základní](http://hc.apache.org/httpclient-3.x/authentication.html) a [ověřování NTLM](http://hc.apache.org/httpclient-3.x/authentication.html). 

    > [AZURE.IMPORTANT] **Ve službě StorSimple správce sledování grafy zařízení nefunguje Basic nebo ověřování NTLM je povolen v konfiguraci proxy serveru pro zařízení. Pro sledování grafy pro práci se musíte zajistit, že ověření je nastavený na žádný.**

5. Pokud používáte ověřování, zadejte **Web Proxy uživatelské jméno** a **Heslo Proxy Web**. Bude taky muset potvrďte heslo.

    ![Konfigurace proxy serveru webové na StorSimple Device1](./media/storsimple-configure-web-proxy/IC751830.png)

Pokud se registrujete zařízení poprvé, pokračujte s registrací. Pokud vaše zařízení byla již registrována, dojde k ukončení průvodce. Nastavení nakonfigurované budou uloženy.

Proxy serveru webové nyní také bude povoleno. Můžete [Povolit proxy serveru webové](#enable-web-proxy) krok přeskočit a přejít přímo do [Nastavení proxy serveru webové zobrazení Azure klasické portálu](#view-web-proxy-settings-in-the-azure-classic-portal).


## <a name="configure-web-proxy-via-windows-powershell-for-storsimple-cmdlets"></a>Konfigurace proxy serveru webové prostřednictvím prostředí Windows PowerShell pro StorSimple rutiny

Alternativní způsob konfigurace nastavení proxy serveru webové spočívá ve využití prostředí Windows PowerShell pro StorSimple rutiny. Proveďte následující kroky pro nastavení proxy serveru webové.

#### <a name="to-configure-web-proxy-via-cmdlets"></a>Konfigurace proxy serveru webové pomocí rutiny

1. V nabídce sériové konzola možnost 1, **Přihlaste se pomocí plný přístup**. Po zobrazení výzvy zadejte **heslo správce zařízení**. Výchozí heslo je `Password1`.

2. Na příkazovém řádku zadejte:

    `Set-HcsWebProxy -Authentication NTLM -ConnectionURI "<http://<IP address or FQDN of web proxy server>:<TCP port number>" -Username "<Username for web proxy server>"`

    Zadejte a potvrďte heslo po zobrazení výzvy, jak je ukázáno v následujícím příkladu.

    ![Konfigurace proxy serveru webové na StorSimple Device3](./media/storsimple-configure-web-proxy/IC751831.png)

Proxy serveru webové je nyní nakonfigurován a je potřeba povolit.

## <a name="enable-web-proxy"></a>Povolení webového serveru proxy

Proxy serveru webové je ve výchozím nastavení vypnutá. Po konfiguraci nastavení serveru proxy web na zařízení s StorSimple, budete muset používat Windows PowerShell pro StorSimple povolit nastavení webového proxy serveru.

> [AZURE.NOTE] **Tento krok se nepožaduje, pokud jste použili Průvodce nastavením pro nastavení proxy serveru webové. Proxy serveru webové je automaticky zapnutá ve výchozím nastavení po relaci Průvodce nastavením.**

Proveďte následující kroky v prostředí Windows PowerShell pro StorSimple Povolit proxy serveru webové na vašem zařízení:

#### <a name="to-enable-web-proxy"></a>Chcete-li povolit proxy serveru webové

1. V nabídce sériové konzola možnost 1, **Přihlaste se pomocí plný přístup**. Po zobrazení výzvy zadejte **heslo správce zařízení**. Výchozí heslo je `Password1`.

2. Na příkazovém řádku zadejte:

    `Enable-HcsWebProxy`

    Teď jste povolili konfigurací proxy web na zařízení s StorSimple.

    ![Konfigurace proxy serveru webové na StorSimple Device4](./media/storsimple-configure-web-proxy/IC751832.png)

## <a name="view-web-proxy-settings-in-the-azure-classic-portal"></a>Zobrazení nastavení proxy serveru webové Azure klasické portálu

Nastavení proxy serveru webové nakonfigurované prostřednictvím rozhraní prostředí Windows PowerShell a určeny z portálu klasické. Nastavení nakonfigurované můžete, ale zobrazit klasické portálu. Takto zobrazíte proxy serveru webové.

#### <a name="to-view-web-proxy-settings"></a>Chcete-li zobrazit nastavení webového proxy serveru
1. Přejděte na **StorSimple Správce služby > zařízení**. Vyberte a klikněte na zařízení a pak přejděte na **Konfigurovat**.
1. Posuňte se dolů na stránce **Konfigurovat** **Nastavení proxy serveru webové** části. Zobrazení nastavení proxy server nakonfigurovaný web na vašem zařízení StorSimple jak je ukázáno v následujícím příkladu.

    ![Proxy serveru webové zobrazení v portálu pro správu](./media/storsimple-configure-web-proxy/ViewWebProxyPortal_M.png)
 
## <a name="errors-during-web-proxy-configuration"></a>Chyby při konfigurací proxy web

Pokud nastavení proxy serveru webové nakonfigurovány nesprávně, chybové zprávy zobrazí se uživatelům ve Windows Powershellu pro StorSimple. Následující tabulka uvádí některé tyto chybové zprávy, jejich příčin a doporučené akce.

|Pořadové číslo.|Kód chyby HRESULT|Možná příčina|Doporučené akce|
|:---|:---|:---|:---|
|1.|0x80070001|Příkaz spustit z pasivní řadiče a komunikovat s active řadiče nedokáže.|Spusťte příkaz řadiči aktivní. Ke spuštění příkazu z pasivní řadiče, bude nutné opravit připojení z pasivní aktivní řadiči. Je třeba zapojit Microsoft Support, pokud toto připojení se přeruší.|
|2.|0x800710dd - identifikátor operace není platný|Nastavení pro proxy server nepodporuje na StorSimple virtuální zařízení.|Nastavení pro proxy server nepodporuje na StorSimple virtuální zařízení. Tyto pouze je možné konfigurovat na zařízení fyzicky StorSimple.|
|3.|0x80070057 - neplatných parametrů|Parametrů přidělený nastavení proxy serveru není platný.|Identifikátor URI není k dispozici ve správném formátu. Použijte v tomto formátu:`http://<IP address or FQDN of the web proxy server>:<TCP port number>`|
|4.|0x800706ba - server RPC není k dispozici|Příčinou je jedním z následujících akcí:</br></br>Shluk není nahoru.</br></br>Není spuštěná služba DataPath.</br></br>Příkaz spustit z pasivní řadiče a komunikovat s active řadiče nedokáže.|Zkontrolujte zapojit Microsoft Support ověřit, že je zapnut cluster a se službou datapath.</br></br>Spusťte příkaz z active řadiče. Spusťte příkaz z pasivní řadiče, musíte se zajistit že pasivní řadiče mohli komunikovat s aktivní správce. Je třeba zapojit Microsoft Support, pokud toto připojení se přeruší.|
|5.|0X800706be - volání RPC se nezdařila.|Clusteru je vypnutý.|Zkontrolujte zapojit Microsoft Support a ujistěte se, že clusteru je nahoru.|
|6.|0x8007138f - prostředku clusteru nebyla nalezena|Prostředek clusteru služby platformy nebyl nalezen. K tomu může dojít při instalaci není správného.|Budete muset provést factory obnovit na vašem zařízení. Možná bude potřeba vytvořit platformy zdroje. Obraťte se prosím Microsoft Support další kroky.|
|7.|0x8007138c - nejsou online zdroje obrázku|Platformu nebo datapath prostředky clusteru nejsou online.|Obraťte se prosím Microsoft Support aby zajistil, že jsou zdroje služby datapath, tak i platformu online.|

> [AZURE.NOTE] 
> 
> -  V seznamu výše chybové zprávy není vyčerpávající. 
> - Chyby související s nastavením proxy serveru webové nezobrazí v portálu Azure klasické ve službě StorSimple správce. Pokud se problém s proxy serveru webové po dokončení konfigurace se stav zařízení se změní na **Offline** v portálu klasické. |

## <a name="next-steps"></a>Další kroky

- Pokud při nasazení zařízení nebo konfiguraci nastavení serveru proxy web dojít jakékoli problémy, podívejte se do [Poradce při potížích s nasazením StorSimple zařízení](storsimple-troubleshoot-deployment.md).

- Naučte se používat službu StorSimple správce, přejděte na [službu StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md).
