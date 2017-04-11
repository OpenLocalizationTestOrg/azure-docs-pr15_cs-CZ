<properties
    pageTitle="Azure AD Connect: Porty | Microsoft Azure"
    description="Tato stránka je stránka technické informace pro porty, které musejí být otevřenou pro Azure AD Connect"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="billmath"/>

# <a name="hybrid-identity-required-ports-and-protocols"></a>Hybridní Identity povinné portů a protokolů
Následující dokument je technické odkaz na požadované porty a protokoly, které jsou zapotřebí pro implementaci řešení totožnost hybridní zadejte informace. Pomocí následující obrázek a podívejte se do příslušné tabulky.

![Co je Azure AD Connect](./media/active-directory-aadconnect-ports/required1.png)

## <a name="table-1---azure-ad-connect-and-on-premises-ad"></a>Tabulka 1 - Azure AD připojení a místním AD
Tato tabulka popisuje portů a protokolů, které jsou potřeba pro komunikaci mezi server Azure AD Connect a využití místního AD.

Protocol (protokol) | Porty | Popis
--------- | --------- |---------
DNS|53 (TCP/UDP)| Vyhledávání DNS na cílové struktury.
Protokol Kerberos|88 (TCP/UDP)| Ověřování protokolem Kerberos struktuře AD.
MS-RPC |135 (TCP/UDP)| Vyvolají během počáteční konfigurace Průvodce Azure AD Connect váže k struktuře AD.
LDAP|389 (TCP/UDP)| Použít pro import dat z AD. Data zašifrován Kerberos podepsat a razítko.
LDAP/SSL|636 (TCP/UDP)| Použít pro import dat z AD. Přenos dat byla podepsaná a šifrovaná. Použít jen pokud používáte SSL.
VZDÁLENÉ VOLÁNÍ PROCEDUR |49152 – 65535 (náhodná vysoké RPC Port)(TCP/UDP)| Vyvolají během počáteční konfigurace Azure AD Connect váže k strukturami AD. Další informace najdete v článku [KB929851](https://support.microsoft.com/kb/929851) [KB832017](https://support.microsoft.com/kb/832017)a [KB224196](https://support.microsoft.com/kb/224196) .

## <a name="table-2---azure-ad-connect-and-azure-ad"></a>Tabulka 2 - Azure AD připojení a Azure AD
Tato tabulka popisuje portů a protokolů, které jsou potřeba pro komunikaci mezi server Azure AD Connect a Azure AD.

Protocol (protokol) |Porty |Popis
--------- | --------- |---------
NASTAVIT INFORMACE HTTP|80 (TCP/UDP)| Použít ke stahování seznamy odvolaných certifikátů (seznamy odvolaných certifikátů) k ověření certifikátů SSL.
PROTOKOL HTTPS|443(TCP/UDP)| Použít synchronizovat síť se službou Azure AD.

Seznam adres URL a IP adresy, musíte otevřít v bráně firewall, najdete v článku [Office 365 URL a rozsahy IP adres](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="table-3---azure-ad-connect-and-federation-serverswap"></a>Tabulka 3 - Azure AD připojení a federační servery nebo WAP
Tato tabulka popisuje portů a protokolů, které jsou potřeba pro komunikaci mezi server Azure AD Connect a federace/WAP servery.  

Protocol (protokol) |Porty |Popis
--------- | --------- |---------
NASTAVIT INFORMACE HTTP|80 (TCP/UDP)| Použít ke stahování seznamy odvolaných certifikátů (seznamy odvolaných certifikátů) k ověření certifikátů SSL.
PROTOKOL HTTPS|443(TCP/UDP)| Použít synchronizovat síť se službou Azure AD.
WinRM|5985| WinRM posluchače

## <a name="table-4---wap-and-federation-servers"></a>Tabulka 4 - WAP a federační servery
Tato tabulka popisuje portů a protokolů, které jsou potřeba pro komunikace mezi federačními servery a WAP servery.

Protocol (protokol) |Porty |Popis
--------- | --------- |---------
PROTOKOL HTTPS|443(TCP/UDP)| Použít pro ověřování.

## <a name="table-5---wap-and-users"></a>Tabulka 5 – WAP a uživatelů
Tato tabulka popisuje portů a protokolů, které jsou potřeba pro komunikaci mezi uživateli a servery WAP.

Protocol (protokol) |Porty |Popis
--------- | --------- |--------- |
PROTOKOL HTTPS|443(TCP/UDP)| Použít pro ověřování zařízení.
TCP|49443 (TCP)| Používá k ověření certifikátů.

## <a name="table-6a--6b---azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tabulka 6a & 6b - agent stavu připojení Azure AD pro (AD FS/synchronizace) a Azure AD
Následující tabulka popisuje koncové body, portů a protokolů, které jsou potřeba pro komunikaci mezi agenti stavu připojení Azure AD a Azure AD

### <a name="table-6a---ports-and-protocols-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tabulka 6a - portů a protokolů pro agent stavu připojení Azure AD pro (AD FS/synchronizace) Azure AD
Tato tabulka popisuje následující odchozí portů a protokolů, které jsou potřeba pro komunikaci mezi agenti stavu připojení Azure AD a Azure AD.  

Protocol (protokol) |Porty  |Popis
--------- | --------- |--------- |
PROTOKOL HTTPS|443(TCP/UDP)| Odchozí
Bus služby Azure|5671 (TCP/UDP)| Odchozí

### <a name="6b---endpoints-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>6b - koncové body agent stavu připojení Azure AD pro (AD FS/synchronizace) a Azure AD
Seznam koncové body naleznete v [části požadavky agent stavu připojení Azure AD](active-directory-aadconnect-health-agent-install.md#requirements).
