<properties
    pageTitle="Azure AD Connect stavu časté otázky"
    description="V tomto článku najdete odpovědi na otázky týkající se stavu připojení Azure AD. V tomto článku popisuje dotazy týkající se používání služby včetně fakturační modelu, funkce, omezení a podpora."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>


# <a name="azure-ad-connect-health-frequently-asked-questions-faq"></a>Azure AD Connect časté otázky ke stavu

V tomto článku najdete odpovědi na otázky týkající se stavu připojení Azure AD. V tomto článku popisuje dotazy týkající se používání služby včetně fakturační modelu, funkce, omezení a podpora.

## <a name="general-questions"></a>Obecné otázky



**Otázka: můžu spravovat více Azure AD adresářů. Jak se dá přepnout s Azure Active Directory Premium?**

Můžete přepínat mezi různých klientů Azure AD výběrem aktuálně podepsané do pole uživatelské jméno v pravém horním rohu a vyberte odpovídající účet. Pokud tady není uvedené účet, vyberte odhlásit a potom pomocí přihlašovacích údajů globální správce adresáře, který má Azure Active Directory Premium povolenou, a přihlaste se.

## <a name="installation-questions"></a>Instalace otázky



**Otázka: Jaký je dopad instalaci Agent stavu připojení Azure AD na jednotlivé servery?**

Dopad instalace Microsoft Azure AD připojení stavu agentů služby AD FS, webové aplikace Proxy servery, Azure AD Connect (sycn) servery, řadiče domény je minimální z hlediska procesoru, šířku pásma sítě spotřebu paměti a úložiště.

Níže uvedených čísel jsou aproximaci.

- Procesor spotřebu: zvýšení ~ 1 %
- Spotřebu paměti: až 10 % celkového pamětí

>[AZURE.NOTE] V případě agent moci komunikace s Azure agent ukládá se v něm data místně, až definovaný maximální limit. Agent přepíše "režim cached" dat na základě "nejméně naposledy vyřízení".

- Místní rezervy úložiště pro Azure AD připojení agenti stavu: ~ 20 MB
- Servery služby AD FS se doporučuje vytvořit místo na disku (minimálně 1 GB) 1024 MB auditování kanálu AD FS pro Azure AD agenti stavu připojení zpracuje všechna data auditování, než se přepíše.

**Otázka: můžu muset restartujte servery v průběhu instalace Azure AD připojení agenti stavu?**

Ne. Instalace aplikace agentů nevyžadují restartujte server. Instalace některé základní kroky však může vyžadovat restartování serveru.

Například v systému Windows Server 2008 R2 instalace .net 4.5 Framework vyžaduje restartování serveru.


**Otázka: znamená Azure AD připojení stavu služby přípravou předávací nastavit informace http proxy?**

Ano.  Pro na přejdete operací, můžete nakonfigurovat Agent stavu pro předávání žádostí o odchozí http pomocí HTTP Proxy. Další informace najdete v tématu [Konfigurace Azure AD připojení stavu agentů Proxy pro HTTP.](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy)
Pokud potřebujete ke konfiguraci proxy při registraci Agent, budete muset změnit nastavení Internet Exploreru Proxy předem.
1. Otevřete Internet Explorer -> Nastavení -> Internet možnosti -> připojení -> Nastavení místní sítě.
2. Vyberte použít Proxy Server pro síť LAN.
3. Pokud máte porty různých proxy pro HTTP a HTTPS/zabezpečené, vyberte možnost Upřesnit.

**Otázka: Azure AD připojení stavu služeb podporuje základní ověřování při připojování k Http proxy?**

Ne. Mechanismus při zadávání libovolného uživatelského jména a hesla pro základní ověřování není aktuálně podporován.


**Otázka: Jaká verze služby AD DS podporují podle stavu připojení Azure AD pro službu AD DS?**

Sledování služby AD DS je podporované při instalaci na následující operační systém verze:

- Windows Server 2008 R2
- Windows Server 2012
- Windows serveru 2012 R2

## <a name="operations-questions"></a>Operace otázky



**Otázka: je potřeba povolit auditování na Proxy servery AD FS aplikace nebo na svůj Web aplikace servery Proxy?**

Ne, auditování nemusí být povolený na servery proxy serveru webové aplikace (WAP). Povolte na servery služby AD FS.


**Otázka: Jak získat vyřešit Azure AD připojení upozornění stavu?**

Upozornění na připojení stavu Azure AD získat přeložit na podmínce úspěch. Azure AD připojení agenti stavu zjišťování a informujte podmínky úspěch služby v pravidelných intervalech. Několik upozornění na potlačení je založená na čase. Jinými slovy Pokud není 72 hodin z generování výstrah stejným chyby, upozornění automaticky vyřeší.




**Otázka: jaké portů brány firewall je potřeba otevřít Azure AD připojení stavu Agent práce?**

Je třeba porty TCP/UDP 443 a 5671 otevřít Azure AD připojení stavu Agent moct komunikovat s koncové body služby Azure AD stavu.


**Otázka: Proč se zobrazuje dva servery se stejným názvem v portálu stavu připojení Azure AD?**

Po agent odebrat ze serveru, serveru není automaticky odebrány z Azure AD připojení portálu Microsoft.  Pokud můžete ručně odstranil agent ze serveru nebo samotném serveru, musíte ručně odstranit položku serveru z portálu Microsoft Azure AD stavu připojení. Další informace najdete v tématu [odstranit server nebo službu instanci.](active-directory-aadconnect-health-operations.md#delete-a-server-or-service-instance)

Pokud reimaged serveru nebo vytvořili novou serveru se v detailech stejné (například název počítače) a z portálu Microsoft Azure AD stavu připojení neodebírat už registrovaných serveru nainstalovány agent na novém serveru může zobrazit dvě položky se stejným názvem.  
V tomto případě byste měli odstranit položku patřící do starší serveru ručně. Data pro tento server by měl být zastaralý.

## <a name="related-links"></a>Související odkazy

* [Azure AD Connect stavu](active-directory-aadconnect-health.md)
* [Azure AD Connect instalaci agenta stavu](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect stavu operace](active-directory-aadconnect-health-operations.md)
* [Použití Azure AD stavu připojení se službou AD FS](active-directory-aadconnect-health-adfs.md)
* [Použití stavu připojení Azure AD pro synchronizaci](active-directory-aadconnect-health-sync.md)
* [Pomocí Azure AD stavu připojení se službou AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect historie verzí stavu](active-directory-aadconnect-health-version-history.md)
