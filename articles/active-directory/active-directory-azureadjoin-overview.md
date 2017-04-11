<properties
    pageTitle="Rozšíření možností cloudu na zařízeních s Windows 10 až Azure Active Directory připojit se ke | Microsoft Azure"
    description="Poskytuje podrobné základní informace o tom, jak zařízeních s Windows 10 můžete využít Azure AD připojit k získání registrována na Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="extending-cloud-capabilities-to-windows-10-devices-through-azure-active-directory-join"></a>Rozšíření možností cloudu na zařízeních s Windows 10 až Azure Active Directory připojit se ke

## <a name="what-is-azure-active-directory-join"></a>Co je Azure Active Directory připojit se ke?
Azure Active Directory připojení (Azure AD spojení) je funkce, které zaregistruje zařízení vlastnictví společnosti v Azure Active Directory povolit Centrální správa zařízení. To umožňuje uživatelům například zaměstnanců a studenty připojení ke cloudu enterprise prostřednictvím služby Azure Active Directory. Díky zjednodušené nasazení systému Windows a přístup k organizační aplikace a zdroje z libovolného zařízení s Windows, podnikové vlastní i osobně vlastnictví (BYOD).

Azure AD spojení je určená pro podniky, které jsou cloudové první/cloudu jen – obvykle malé a střední firmy, které nemají infrastruktury služby Active Directory pro Windows Server místního. Aby uvedené, Azure AD připojit se ke můžete a rovněž použít ve velkých organizacích na zařízeních, které jsou nemůže udělat tradiční domény spojení (mobilní zařízení, třeba), nebo pro uživatele, kteří hlavně potřebují pro přístup k Office 365 nebo jiných aplikací Azure AD SaaS.

I když připojení k tradiční doméně nabízí stále že nejlepší místního prostředí na zařízeních, které umožňují připojení domény, Azure AD spojení je vhodná pro zařízení, která se nemůže připojit se ke domény. Azure AD spojení je také vhodný pro správu uživatelů v cloudu. Stejně tak pomocí možnosti správy mobilních zařízení místo pomocí nástroje pro správu tradiční domény jako zásad skupiny a konfigurace správce (nástroj SCCM System Center).

![Základní informace o společnosti a osobní zařízení s místní službou Active Directory a Azure AD](./media/active-directory-azureadjoin/active-directory-azureadjoin-overview.png)


## <a name="why-should-enterprises-adopt-azure-ad-join"></a>Proč podniky přijmout Azure AD připojení?

* **Firmy, které jsou převážně v cloudu**: Pokud jste přesunuli nebo přesunutí do modelu, ve kterém jsou snížení svoji místní stopu a chcete používat více v cloudu, Azure AD připojení může hodit. Možná jste vytvořili Azure AD účty ať už ručně nebo prostřednictvím synchronizace služby Active Directory místní. V obou případech máte účet v Azure AD a můžete ji použijete k přihlášení k Windows 10. Uživatele můžete připojit počítačích Azure AD pomocí některého z mimo pole zkušenosti (počáteční nastavení počítače) nebo v nabídce nastavení. Po připojení, uživatelé budou moct užít dobrý pocit jednoho přihlašování (SSO) aplikace access do cloudu zdrojů, jako jsou Office 365 v prohlížeči nebo v aplikacích Office.

* **Vzdělávací instituce**: scénáře častých o reprodukujte vzdělávací instituce mít dva typy uživatele: pedagogický sbor a studenty. Členech jsou považovány za dlouhodobé členů organizace, tak, aby vytváření místních účtů pro ně žádoucí. Ale studentů jsou shorter-term členů organizace a tím dá se ovládat v Azure AD. To znamená, že měřítko adresáře můžete posune do cloudu místo uložené místního. Také znamená to, studenty můžete přihlášení k Windows pomocí svých účtů Azure AD a získat přístup k Office 365 zdrojů v prohlížečích nebo v aplikacích Office.

* **Maloobchodě**: jiný scénář se často o od zákazníků je jejich přání ke správě sezónní pracovníci snadněji.  Znovu účty pro zaměstnance dlouhodobé, plný úvazek obvykle vytvářejí jako místní účty v doméně počítačích. Ale sezónní pracovníci patří shorter-term organizační, takže je vhodné pro správu je místo, kam uživatelské licence snadněji přesouvat kolem. Vytvoření těchto uživatelských účtů v cloudu s licencí Office 365 umožňuje uživatelům získat výhodách přihlášení k Windows a v aplikacích Office pomocí účtu Azure AD. Mezitím udržujete větší flexibilitu s jejich licence po odejdou.
* **Jiné podniky**: Ačkoli Udržovat uživatele v Active Directory vaší místní můžete pořád využít s uživateli Azure AD připojen. Je proto, že Azure AD nabízí zjednodušené připojující experience, Správa efektivně zařízení, zápis správy automatické mobilního zařízení a možnosti přihlášení pro Azure AD a místních zdrojů.  

## <a name="what-capabilities-does-azure-ad-join-offer"></a>Jaké funkce Azure AD připojení nabízí?
Azure AD připojit můžete mít takto:

* **Automatické vytváření podnikové vlastní zařízení**: S Windows 10 uživatelů konfigurovat ještě nepracovali, zatavené zařízení můžete v části krabice experience bez IT zapojení.


* **Podpora pro moderní velikostech**: Azure AD spojení označené jako pracuje na zařízení, která nemají tradiční domény připojení možnosti.  


* **Podpora pro existující účty organizace**: uživatelé už není potřeba vytvářejí a spravují osobní účet Microsoft pro přístup nejlepší možnosti na zařízeních vydané společnosti stejným způsobem jako ve Windows 8. Budou moct používat jejich stávajících účtů práce v Azure AD místo. Pro mnoho organizací to v podstatě znamená, že uživatelé můžete nastavit a přihlášení k Windows s stejné přihlašovací údaje, které používají pro přístup k Office 365.


* **Zápis správy automatické mobilní zařízení**: zařízení můžete automaticky zaregistrované v případě připojení k Azure AD Správa mobilních zařízení. Tento proces spolupracuje Microsoft Intune a partnerských řešení pro správu mobilních zařízení. Po dokončení správy zařízení pomocí Intune správce IT může monitor/zařízení můžete spravovat Azure AD připojen vedle doméně zařízení v konzole Správa SCCM.


* **Jednotné přihlašování k prostředkům společnosti**: uživatelé mohli využívat jednotného přihlašování z plochy na aplikace a zdroje v cloudu, například Office 365 a tisíce podnikových aplikací, které jsou závislé na Azure AD pro ověřování prostřednictvím [Azure AD Connect](active-directory-azureadjoin-deployment-aadjoindirect.md). Podniková vlastnictví zařízení připojených k Azure AD taky moct užít dobrý pocit jednotného přihlašování k místním zdrojům při zařízení při podnikové síti se systémem a odkudkoli, kde jsou k dispozici tyto materiály přes [Azure AD aplikace Proxy](https://msdn.microsoft.com/library/azure/Dn768219.aspx).


* **Cestovní stavu s operačním systémem**: nastavení usnadnění, weby, Wi-Fi hesla a další nastavení jsou synchronizovány přes podnikové vlastní zařízení bez nutnosti osobním účtem Microsoft.


* **Řešení připravené na Enterprise pro Windows Store**: pro Windows Store podporuje licencování s účty Azure AD a získání aplikací. Organizace můžete-multilicence aplikace a zpřístupníte je uživateli z jejich organizace.

## <a name="how-do-different-devices-work-with-azure-ad-join"></a>Jak fungují různých zařízeních s Azure AD připojení?

| Podnikové zařízení (připojen k místní domény)                                                                                                                                                                                         | Podnikové zařízení (spojeny do cloudu)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Osobní zařízení                                                                                                         |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| Uživatelé můžou Přihlaste se k Windows pomocí přihlašovacích údajů práce (stejně jako dnes).                                                                                                                                                                        | Mohli uživatelé přihlašovat k Windows pomocí přihlašovacích údajů práce, které jsou spravovány v Azure AD. Platí to pro podnikové zařízení ve třech případech: 1) organizace nemá služby Active Directory místně (například small business). 2) organizace nevytvoří všechny uživatelské účty ve službě Active Directory (například účty studentů, konzultanty nebo sezónní pracovníci nevytváří, ve službě Active Directory). 3) v organizaci podnikové zařízení, které nelze připojit k (místní) doménu, třeba telefony nebo tablety se systémem Mobile SKU (například sekundární zařízení věnovat prostorového uspořádání factory/maloobchod). Azure AD spojení podporuje připojení podnikové zařízení pro spravovaná a federovaný organizace. | Přihlášení k uživatelé Windows s jejich osobní přihlašovací údaje účtu Microsoft (stejně jako).                                                |
| Uživatelé mají přístup k cestovní nastavení a enterprise pro Windows Store. Tyto služby pracovat s účty práce a nevyžadovat při tom osobním účtem Microsoft. Při této akci musí organizace se připojit k Azure AD jejich na adresářová služba Active Directory.                                        | Uživatele můžete udělat samoobslužných funkcí nastavení. Můžete půjde prostřednictvím prvním spuštění (FRX) přes svůj pracovní účet jako alternativu k s IT zřízení zařízení, i když jsou podporované obě metody.                                                                                                                                                                                                                                                                                                                                                                             | Uživatelé můžou snadno přidat pracovní účet, který je spravovaný v služby Active Directory a Azure AD.                                                      |
| Uživatelé mají jednotné přihlašování umožňuje ze stolního počítače práci aplikace, weby a zdrojů – včetně místních zdrojů a cloudu aplikace, které používají Azure AD pro ověřování.                                                                                                            | Zařízení jsou automaticky registrace v adresáři organizace (Azure AD) a automaticky zaregistrované v Správa mobilních zařízení. (Azure AD Premium funkce).                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Uživatelé mají možnost jednotné přihlašování mezi aplikacemi a podívejte se na weby nebo materiály s tímto účtem práce.                                              |
| Uživatele můžete přidat své osobní účty Microsoft pro přístup k jejich osobní obrázky a soubory bez vlivu na data organizace. (Cestovní nastavení pokračovat v práci s účty své práce.) Účet Microsoft umožňuje přihlašování a už jednotky cestovní nastavení.  | Mohou uživatelé samoobslužného vytvoření nového hesla (SSPR) na přihlašování, což znamená, že můžete obnovit zapomenuté heslo. (Azure AD Premium funkce).                                                                                                                                                                                                                                                                                                                                                                                                                                   | Uživatelé mají přístup k enterprise pro Windows Store tak, aby mohou získat a pomocí řádku obchodních aplikací na jejich osobních zařízení. |                                                               |


## <a name="additional-information"></a>Další informace
* [Windows 10 Enterprise: způsoby použití zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozšíření možností cloudu na zařízeních s Windows 10 až Azure Active Directory připojit se ke](active-directory-azureadjoin-user-upgrade.md)
* [Ověřování identitami bez hesla prostřednictvím Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Další informace o použití scénáře pro Azure AD připojení](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojte zařízení doméně Azure AD pro Windows 10 prostředí](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení připojení Azure AD](active-directory-azureadjoin-setup.md)
