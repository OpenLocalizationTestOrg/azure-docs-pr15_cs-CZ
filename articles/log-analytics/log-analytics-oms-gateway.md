<properties
    pageTitle="Připojení k OMS pomocí brány OMS počítačích a zařízeních | Microsoft Azure"
    description="Připojte OMS Správa přístupových práv zařízení a sledovat Operations Manager počítače pomocí brány OMS odeslání dat ve službě OMS při nemají přístup k Internetu."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>
<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="banders"/>

# <a name="connect-computers-and-devices-to-oms-using-the-oms-gateway"></a>Připojení k OMS pomocí brány OMS počítačích a zařízeních

Tento dokument popisuje, jak OMS Správa přístupových práv zařízení a sledovat System Center operace správce SCOM počítače můžete odeslat data ve službě OMS při nemají přístup k Internetu. Brána OMS můžete shromáždit data a odešlete ji ke službě OMS nim jejich jménem.

Brána je přeposílání proxy pro HTTP, který podporuje tunelování HTTP pomocí příkazu HTTP připojení. Brány zpracovat až 2 000 OMS souběžně připojení zařízení při spuštění aplikace v 4-core CPU 16 GB serveru s Windows.

Jako příklad vaší organizace nebo velké organizace může mít servery s připojení k síti, ale pravděpodobně připojení k Internetu. Jiný příklad bude pravděpodobně nutné mnoho místa zařízení obchod (POS) s prostředků sledování přímo. A jiný příklad Operations Manager brány OMS jako použít proxy server. V těchto příkladech brány OMS přenášet data z agentů, které jsou instalovány na těchto serverech nebo POS zařízení OMS.

Místo každý jednotlivé agent odesílání dat přímo do OMS, které vyžadují přímé připojení k Internetu všechna data agent místo toho prochází jeden počítač, který má připojení k Internetu. Tento počítač slouží k instalaci a používat bránu. V tomto scénáři můžete nainstalovat agentů ve všech počítačích, ve které chcete shromáždit data. Brány klikněte převádí data z agentů OMS přímo – brány není analyzovat data, která se převede.

Sledovat OMS brány a analýza výkonu a data události pro server, kde je nainstalovaný, musíte nainstalovat agenta OMS na počítači brány také nainstalovaným.

Brány musí mít přístup k Internetu a odeslání dat do OMS. Každý agent také může mít připojení k síti na svou bránu tak, aby agentů automaticky přenášet data mezi libovolnými brány. Nejlepších výsledků dosáhnete neinstalovat brány na počítač, na kterém je také řadiče domény.

Zde je diagram zobrazující tok dat z přímé agentů k OMS.

![diagram přímé agent](./media/log-analytics-oms-gateway/direct-agent-diagram.png)

Zde je diagram zobrazující toku dat z Operations Manager do OMS.

![Operations Manager diagram](./media/log-analytics-oms-gateway/scom-mgt-server.png)

## <a name="install-the-oms-gateway"></a>Instalace OMS brány

Instalaci této brány nahradí předchozí verze brány, že máte nainstalovanou (protokolu analýzy předávání).

Požadavky: .net Framework 4.5, Windows Server 2012 R2 SP1 a vyšší

1. Stáhněte si nejnovější verzi brány OMS z [Webu služby Stažení softwaru](http://download.microsoft.com/download/2/5/C/25CF992A-0347-4765-BD7D-D45D5B27F92C/OMS%20Gateway.msi).
2. Spusťte instalaci, poklikejte na **OMS Gateway.msi**.
3. Na úvodní stránce **Další**.  
    ![Průvodce nastavením brány](./media/log-analytics-oms-gateway/gateway-wizard01.png)
4. Na stránce podmínky licenční smlouvy vyberte, pokud **přijměte podmínky licenční smlouvy** souhlasíte s EULA a potom **Další**.
5. Na stránce portů a proxy adresy:
    1. Zadejte číslo portu TCP se nemusí používat bránu pro. Nastavení otevře toto číslo portu brána Windows firewall. Výchozí hodnota je 8080.
    Platný rozsah číslo portu je 1 65535. Pokud vstupní nepatří do tato oblast, zobrazí se chybová zpráva.
    2. Pokud chcete Pokud serveru s nainstalovanou bránou potřebujete-li použít na proxy server, zadejte adresu proxy kde brány musí k připojení. Například `http://myorgname.corp.contoso.com:80` Pokud prázdné, brány se bude snažit k přímé připojení k Internetu. V opačném brány se připojí k proxy server. Pokud proxy server vyžaduje ověření, zadejte svoje uživatelské jméno a heslo.
        ![Brána pro Průvodce konfigurací proxy](./media/log-analytics-oms-gateway/gateway-wizard02.png)  
    3. Klikněte na tlačítko **Další**
6. Pokud nemáte Microsoft Updates povoleno, zobrazí se stránka služby Microsoft Update, kde si můžete zvolit povolit Microsoft Updates. Výběr z nabídky a klikněte na tlačítko **Další**. Jinak pokračujte dalším krokem.
7. Na stránce cílovou složku ponechejte výchozí složky **%ProgramFiles%\OMS brány** nebo zadejte místo, kam chcete nainstalovat brány a klikněte na tlačítko **Další**.
8. V dialogu jste připravení na stránce pro instalaci klikněte na **instalovat**. Řízení uživatelských účtů se může zobrazit požadavku na oprávnění k instalaci. Pokud ano, klikněte na **Ano**.
9. Po dokončení instalace klikněte na **Dokončit**. Můžete ověřit, že služba běží otevřením modulu snap-in services.msc a ověřte, že **OMS brány** se zobrazí v seznamu služeb.  
    ![Služby – OMS brány](./media/log-analytics-oms-gateway/gateway-service.png)

## <a name="install-an-agent-on-devices"></a>Instalace agent na zařízení

V případě potřeby najdete v tématu [připojení Windows počítačů protokolu analýzy](log-analytics-windows-agents.md) informace o tom, jak nainstalovat přímo připojené agentů. Článek popisuje, jak si můžete nainstalovat agenta pomocí Průvodce nastavením nebo pomocí příkazového řádku.

## <a name="configure-oms-agents"></a>Konfigurace OMS agentů

V tématu [Konfigurace proxy serveru a nastavení brány firewall s Microsoft Agent sledování](log-analytics-proxy-firewall.md) informace o konfiguraci agent používat proxy server, což je tento případ je brány.

Operace správce agenti zasílat některá data například Operations Manager upozornění konfigurace hodnocení, instance místa a kapacita dat pomocí serveru pro správu. Další velkých objemů dat, například služby IIS, výkon, zabezpečení a protokoly jsou odeslány přímo OMS brány. Úplný seznam dat, které prochází každý kanál najdete v článku [řešení přidat analýzy protokolu z Galerie řešení](log-analytics-add-solutions.md) .

>[AZURE.NOTE]
Pokud máte v plánu brány pomocí služby Vyrovnávání zatížení, přečtěte si článek [Volitelně konfigurace služby Vyrovnávání zatížení sítě](#optionally-configure-network-load-balancing).

## <a name="configure-a-scom-proxy-server"></a>Konfigurace SCOM proxy server

Konfigurace Operations Manager přidáte brány má fungovat jako proxy server. Aktualizace konfigurací proxy konfigurací proxy automaticky platí pro všechny agentů operace nařízeného.

Použití brány na podporu Operations Manager, potřebujete:

- Sledování programu Microsoft Agent (agent verze – **8.0.10900.0** a novější) na serveru brány nainstalovali a nakonfigurovali OMS pracovních prostorů, se kterými chcete komunikovat.
- Brány musí být připojení k Internetu nebo připojení k serveru proxy, který nemá.

### <a name="to-configure-scom-for-the-gateway"></a>Konfigurace SCOM brány

1. Spusťte konzolu Operations Manager a v části **Operace správy sadu**, klikněte na **připojení** a klikněte na **Nastavení Proxy serveru**:  
    ![Operations Manager – konfigurace Proxy serveru](./media/log-analytics-oms-gateway/scom01.png)
2. Vyberte **použít proxy server pro přístup k sadě operace správy** a zadejte IP adresu serveru OMS brány. Ujistěte se, že začínáte `http://` předponu:  
    ![Operations Manager – adresy proxy serveru](./media/log-analytics-oms-gateway/scom02.png)
3. Klikněte na **Dokončit**. Váš server Operations Manager je připojen k OMS pracovního prostoru.

## <a name="configure-network-load-balancing"></a>Konfigurace služby Vyrovnávání zatížení sítě

Můžete nakonfigurovat brány vysoké dostupnosti vytvořením clusteru vyrovnávání zatížení sítě. Cluster spravuje přenosy od vašeho agentů přesměrováním požadované připojení z Microsoft sledování agentů přes jeho uzlů. Pokud jeden server brány havaruje, přenos přesměrován do jiných uzlů.

1. Otevřete Správce vyrovnávání zatížení sítě a vytvoření clusteru.
2. Klikněte pravým tlačítkem myši obrázku před přidáním brány a vyberte **Vlastnosti clusteru.** Konfigurace clusteru mít adresy IP:  
    ![Správce vyrovnávání zatížení sítě – clusteru IP adres](./media/log-analytics-oms-gateway/nlb01.png)
3. Spojení server OMS brány s Microsoft Agent sledování nainstalovali, klikněte pravým tlačítkem myši obrázku IP adresa a klikněte na **Přidat hostitele obrázku**.  
    ![Správci – to Vyrovnávání zatížení sítě přidat hostitele do obrázku](./media/log-analytics-oms-gateway/nlb02.png)
4. Zadejte IP adresu serveru brány, který chcete připojit:  
    ![Sítě Správce vyrovnávání zatížení – přidání hostitele clusteru: připojení](./media/log-analytics-oms-gateway/nlb03.png)
5. V počítačích, které nemají připojení k Internetu je potřeba použít IP adresu clusteru při konfiguraci **Vlastností agenta sledování Microsoft**:  
    ![Microsoft sledování vlastností agenta – nastavení proxy serveru.](./media/log-analytics-oms-gateway/nlb04.png)

## <a name="configure-for-automation-hybrid-workers"></a>Nastavení pro automatizaci hybridní zaměstnanců

Pokud máte automatizaci hybridní pracovníky ve vašem prostředí, zadejte podle těchto kroků ručně, dočasné řešení, jak konfigurovat brána je podporují.

V následujících krocích je potřeba vědět Azure oblasti, kde jsou uložená automatizaci účtu. Vyhledejte umístění:

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. Vyberte službu Azure automatizaci.
3. Vyberte příslušný účet Azure automatizaci.
4. Zobrazení svých regionu v části **umístění**.  
    ![Azure portál – umístění automatizaci účtu](./media/log-analytics-oms-gateway/location.png)

Pomocí následujících tabulkách můžete zjistit adresu URL pro každé umístění:

**Adresy URL služby úlohy runtime dat**

| **umístění** | **ADRESA URL** |
| --- | --- |
| Severní centrální USA | ncus-jobruntimedata výrobní su1.azure-automation.net |
| Západní Evropě | Společnost Microsoft jobruntimedata výrobní su1.azure-automation.net |
| Jižní centrální USA | scus-jobruntimedata výrobní su1.azure-automation.net |
| Východní USA | eus-jobruntimedata výrobní su1.azure-automation.net |
| Centrální Kanada | kopie jobruntimedata výrobní su1.azure-automation.net |
| Severní Evropě | Ne-jobruntimedata výrobní su1.azure-automation.net |
| Jihovýchodní Asie | moři – jobruntimedata výrobní su1.azure-automation.net |
| Centrální Indie | CID-jobruntimedata výrobní su1.azure-automation.net |
| Japonsko | JPE-jobruntimedata výrobní su1.azure-automation.net |
| Austrálie | pomocného mechanismu řízení jobruntimedata výrobní su1.azure-automation.net |

**Adresy URL služby Agent**

| **umístění** | **ADRESA URL** |
| --- | --- |
| Severní centrální USA | ncus-agentservice výrobní 1.azure-automation.net |
| Západní Evropě | Společnost Microsoft agentservice výrobní 1.azure-automation.net |
| Jižní centrální USA | scus-agentservice výrobní 1.azure-automation.net |
| Východní USA | eus2-agentservice výrobní 1.azure-automation.net |
| Centrální Kanada | kopie agentservice výrobní 1.azure-automation.net |
| Severní Evropě | Ne-agentservice výrobní 1.azure-automation.net |
| Jihovýchodní Asie | moři – agentservice výrobní 1.azure-automation.net |
| Centrální Indie | CID-agentservice výrobní 1.azure-automation.net |
| Japonsko | JPE-agentservice výrobní 1.azure-automation.net |
| Austrálie | pomocného mechanismu řízení agentservice výrobní 1.azure-automation.net |

Pokud váš počítač je registrovaná jako pracovník hybridní automaticky pro opravy pomocí řešení pro správu aktualizace, pomocí těchto kroků:

1. Přidejte do seznamu povolených hostitelů na brány OMS adresy URL služby dat Runtime úlohy. Příklad: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
2. Restartujte službu brány OMS pomocí následující rutiny prostředí PowerShell:`Restart-Service OMSGatewayService`

Pokud je na nacházejí automatizace Azure pomocí rutiny hybridní pracovníka registrace, pomocí těchto kroků:

1. Přidání adresy URL služby registrace agenta do seznamu povolených hostitele na brány OMS. Příklad:`Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
2. Přidejte do seznamu povolených hostitelů na brány OMS adresy URL služby dat Runtime úlohy. Příklad: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
3. Restartujte OMS brány.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Užitečné rutiny prostředí PowerShell

Rutiny pro můžete dokončení úkolů, které jsou potřebné k aktualizaci brány OMS nastavení. Předtím, než je používat, nezapomeňte:

1. Nainstalujte OMS brány (MSI).
2. Otevřete okno Powershellu.
3. K importu modulu, zadejte tento příkaz:`Import-Module OMSGateway`
4. Pokud žádná k chybě v předchozím kroku, modulu byl úspěšně importován a lze použít. Typ`Get-Module OMSGateway`
5. Po provedení změn pomocí rutin zajistit, aby restartovat službu brány.

Pokud dojde k chybě v kroku 3 modulu nebyl importovány. K chybě může dojít při prostředí PowerShell nedokáže najít modul. Můžete najít v brána Instalační cesta: C:\Program File\Microsoft OMS Gateway\PowerShell.

| **Rutina** | **Parametry** | **Popis** | **Příklady** |
| --- | --- | --- | --- |
| `Set-OMSGatewayConfig` | Klíč (povinné) <br> Hodnota | Změní konfiguraci služby | `Set-OMSGatewayConfig -Name ListenPort -Value 8080` |
| `Get-OMSGatewayConfig` | Klíč | Získá konfiguraci služby | `Get-OMSGatewayConfig` <br> <br> `Get-OMSGatewayConfig -Name ListenPort` |
| `Set-OMSGatewayRelayProxy` | Adresa <br> Uživatelské jméno <br> Heslo | Nastaví adresu (a přihlašovací údaje) relay (nadřazeného) proxy | 1. nastavení proxy serveru odpověď a pověření:`Set-OMSGatewayRelayProxy -Address http://www.myproxy.com:8080 -Username user1 -Password 123` <br> <br> 2. nastavení proxy odpověď, který nevyžaduje ověření:`Set-OMSGatewayRelayProxy -Address http://www.myproxy.com:8080` <br> <br> 3. zrušte zaškrtnutí políčka nastavení, proxy odpověď, nemusí proxy odpověď:`Set-OMSGatewayRelayProxy -Address ""` |
| `Get-OMSGatewayRelayProxy` |   | Získá adresy proxy relay (odesílání dat) | `Get-OMSGatewayRelayProxy` |
| `Add-OMSGatewayAllowedHost` | Host (povinné) | Přidá hostiteli do seznamu povolených | `Add-OMSGatewayAllowedHost -Host www.test.com` |
| `Remove-OMSGatewayAllowedHos`t | Host (povinné) | Odebere hostiteli ze seznamu Povolené | `Remove-OMSGatewayAllowedHost -Host www.test.com` |
| `Get-OMSGatewayAllowedHost` |   | Získá aktuálně povolené hostitele (místně nakonfigurované povoleny Host (hostitel), pouze nezahrnujte automaticky stažené povolené hosts) | `Get-OMSGatewayAllowedHost` |
| `Add-OMSGatewayAllowedClientCertificate` | Předmět (povinné) | Přidá certifikát klienta vyměřené poplatky za jeho seznamu povolených | `Add-OMSGatewayAllowedClientCertificate -Subject mycert` |
| `Remove-OMSGatewayAllowedClientCertificate` | Předmět (povinné) | Odebere ze seznamu Povolené předmět certifikátu klienta | `Remove- OMSGatewayAllowedClientCertificate -Subject mycert` |
| `Get-OMSGatewayAllowedClientCertificat`přirozeného logaritmu e |   | Získá aktuálně povolené klienta certifikát předmětů (pouze místně konfigurované povolené než se předměty, nezahrnujte automaticky stažených povolené předměty) | `Get-OMSGatewayAllowedClientCertificate` |

## <a name="troubleshoot"></a>Poradce při potížích s

Doporučujeme nainstalovat agenta OMS počítačů, které máte nainstalované brány. Pak můžete agent získat informace o události, které jsou zaznamenány brána.

![Prohlížeč událostí – OMS brány protokolu](./media/log-analytics-oms-gateway/event-viewer.png)

**ID událostí OMS brány a popisy**

Následující tabulka zobrazuje ID událostí a popisy OMS brány protokolu událostí.

| **ID** | **Popis** |
| --- | --- |
| 400 | Chyba všechny aplikace, ve kterém není konkrétní ID |
| 401 | Chybné konfigurace. Příklad: listenPort = "text" místo celé číslo |
| 402 | Výjimky při analýze TLS handshake zprávy |
| 403 | Chyba sítě. Příklad: Nelze se připojit ke cílový server |
| 100 | Obecné informace |
| 101 | Má spuštěná služba |
| 102 | Zastavení služby |
| 103 | Příkaz připojit HTTP dostali z klienta |
| 104 | Není připojení HTTP příkazu ovládacího prvku |
| 105 | Určení serveru není v seznamu povolených nebo s cílovým portem není zabezpečený port (443) <br> <br> Zkontrolujte, že agent MMA na serveru brány a agentů komunikaci s brány připojeni do stejného pracovního prostoru protokolu analýzy.|
| 105 | Chyba TcpConnection – certifikát neplatné klienta: CN = brány <br><br> Zajistit, aby: <br> <br> & #149; Používáte bránu s číslem verze 1.0.395.0 nebo vyšší. <br> & #149; Agent MMA na serveru brány a agentů komunikaci s brány připojeni do stejného pracovního prostoru protokolu analýzy. |
| 106 | Z nějakého důvodu, kterou je relace TLS podezřelých a zamítnuté |
| 107 | Ověřil TLS relace |

**Výkonnosti shromažďovat**

Následující tabulka zobrazuje výkonnosti k dispozici pro OMS brány. Můžete přidat čítače sledování výkonu.

| **Jméno** | **Popis** |
| --- | --- |
| Připojení klienta brány/aktivní OMS | Počet aktivních klientů připojení (TCP) |
| Počet OMS brány/chyb | Počet chyb |
| OMS brány/připojení klientů | Počet připojení klientů |
| Počet OMS brány/odmítnutí | Počet odmítnutí kvůli každé chyby ověření TLS |

![Výkonnosti OMS brány](./media/log-analytics-oms-gateway/counters.png)


## <a name="get-assistance"></a>Získání pomoci

Pokud je máte přihlášený k portálu Azure, můžete vytvořit žádost o pomoc s brány OMS nebo jiné služby Azure nebo funkce služby.
Požádat o pomoc, klikněte na požadovaný symbol otazník v pravém horním rohu na portálu a potom klikněte na tlačítko **Nová žádost o podporu**. Vyplňte formulář Nová žádost o podporu.

![Nová žádost o podporu](./media/log-analytics-oms-gateway/support.png)

Můžete taky sdělit svůj názor o OMS nebo protokolu analýzy na [fórum pro názory Microsoft Azure](https://feedback.azure.com/forums/267889).

## <a name="next-steps"></a>Další kroky

- [Zdroje dat přidat](log-analytics-data-sources.md) shromáždit data od zdrojů připojení v pracovním prostoru OMS a uložte ho do úložiště OMS.
