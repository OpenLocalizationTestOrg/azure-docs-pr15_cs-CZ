<properties
    pageTitle="Povolit Enterprise stav roamingové služby Azure Active Directory | Microsoft Azure"
    description="Nejčastější dotazy k nastavení organizace stavu cestovní v zařízení s Windows. Předávání stavu Enterprise nabízí setkat i v případě jednotné uživatelé na svých zařízeních Windows a zkracuje dobu potřebnou pro konfiguraci nové zařízení."
    services="active-directory"
    keywords="pole organizace stavu cestovní, windows cloudu, jak povolit cestovní stavu enterprise"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>



# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>Povolit Enterprise stav roamingové služby Azure Active Directory

Cestovní Enterprise stavu je k dispozici jakékoli organizaci s předplatným Premium Azure Active Directory (Azure AD). Podrobné informace o tom, jak získat předplatné Azure AD, podívejte se na [stránku produkt Azure AD](https://azure.microsoft.com/services/active-directory).

Po povolení Enterprise stavu cestovní organizaci automaticky udělena licencí pro předplatné zdarma, omezené použití k Azure Rights Management. Tento bezplatného předplatného se omezí na šifrování a dešifrování nastavení organizace a data aplikací synchronizovali službou cestovní stavu organizace. musí mít na placené předplatné využití úplné funkcí Azure Rights Management.

Po získání předplatného Premium Azure AD, postupujte takto povolit cestovní stavu Enterprise:

1. Přihlaste se k portálu Azure klasické.
2. V levé části vyberte **Služby ACTIVE DIRECTORY**a vyberte v adresáři, pro kterou chcete povolit cestovní stavu organizace.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming.png)
3. Přejděte na kartu **KONFIGUROVAT** nahoře.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-configure.png)
4.  Posunujte stránku dolů a vyberte **Uživatelé mohou SYNCHRONIZOVAT nastavení a podnikových aplikací dat**a klikněte na **Uložit**.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-select-all-sync-settings.png)

Pro zařízení s Windows 10 chcete přecházet nastavení s Enterprise stavu Roamingová služba zařízení musí ověřit identitu Azure AD pomocí. Zařízení připojených k Azure AD primární přihlášení uživatele po identitě Azure AD tak nevyžaduje žádnou další konfiguraci. Zařízení, které používají tradiční místní Active Directory musí být správce IT [Připojte zařízení doméně Azure AD pro Windows 10 prostředí](active-directory-azureadjoin-devices-group-policy.md).

## <a name="sync-data-storage"></a>Ukládání dat synchronizace
Cestovní stavu organizace dat je hostovaný v jedné nebo více [Azure oblastí](https://azure.microsoft.com/regions/ ) , který nejlépe zarovná s hodnotou země/oblasti v instanci služby Azure Active Directory. Cestovní stavu organizace dat je oddíly založené na tři hlavní zeměpisné oblastí: Severní Americe, EMEA a APAC. Cestovní stavu Enterprise data pro klienta místně nachází se zeměpisnou oblast a není replikovat různých oblastí.  Příklad zákazníci, kteří mají své země/oblasti nastavenou na jeden z zemích EMEA, které jako "Francie" nebo "Zambie", budou mít data hostovaná v jednom nebo Azure oblasti v Evropě.  Zákazníci, kteří hodnotu jejich země/oblasti v Azure AD do jedné ze zemí Severní Amerika jako "USA" nebo "Kanada" bude mít data hostovaná kterýmkoliv z Azure oblastí v USA.  Zákazníci, kteří hodnotu jejich země/oblasti v Azure AD do jedné ze zemí APAC jako "Austrálie" nebo "Nový Zéland" bude mít data hostovaný do jednoho nebo více z Azure oblastí v rámci země Asie.  Států Jižní Ameriky a Antarktida dat bude použitý ve jednu nebo více Azure oblastí v USA.  Země/oblasti hodnota je nastavena jako součást procesu Azure AD adresáře vytváření a nelze změnit následně. 

Pokud budete potřebovat další informace o umístění ukládání dat, oznamte lístek s [podporou Azure](https://azure.microsoft.com/support/options/).

## <a name="manage-enterprise-state-roaming"></a>Správa organizace stavu cestovní
Globální správci služby Azure AD můžete povolit a zakažte Enterprise stavu cestovní Azure klasické portálu.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-manage.png)

Globální správci mohou omezit nastavení synchronizace skupinám zabezpečení.

Globální správci zobrazíte zprávy o stavu uživatele zařízení synchronizovat i tak, že vyberete určitým uživatelem v seznamu **Uživatelé** instanci služby Active Directory a po kliknutí na kartu **zařízení** a možnost zobrazení **synchronizace podnikových aplikací dat a nastavení zařízení**.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-device-sync-settings.png)

##<a name="data-retention"></a>Uchovávání informací
Synchronizované s Azure prostřednictvím cestovní stavu organizace se zachovají data donekonečna udržovat pokud je provedena operace ručně odstranit nebo dat v záleží na zastaralá. 

**Explicitní odstranění:** Data je Azure správce odstraní uživatele nebo v adresáři nebo odstranění žádosti správce explicitně, že data se odstraní.

- **Odstranění uživatelů**: když je uživatel v Azure AD, uživatelský účet cestovní dat bude označen pro odstranění a budou odstraněny mezi 180 do 90 dnů. 
- **Odstranění adresáře**: odstranění celý adresář v Azure AD je operaci okamžitě. Všechna data nastavení přiřazené k, které adresáře bude označen pro odstranění a budou odstraněny mezi 180 do 90 dnů. 
- **V žádosti o odstranění**: Pokud správce Azure AD požaduje odstranit ručně dat nebo nastavení dat určitým uživatelem, správce můžete poslat lístek s [Azure podpory](https://azure.microsoft.com/support/). 

**Odstranění zastaralá data**: Data, která není bylo k nim získat přístup pro jeden rok ("uchovávání informací období") se použije jako zastaralé a budou odstraněny z Azure. Doba uchovávání informací se mohou změnit však nesmí být menší než 90 dní. Zastaralá data může být určité skupiny nastavení systému Windows a aplikace nebo všechna nastavení pro uživatele. Příklad:
 
- Pokud žádná zařízení přístup ke kolekci určité nastavení (například aplikace je odebráno ze zařízení, nebo skupinu nastavení, například "Leze" není k dispozici pro všechny uživatele zařízení), pak ní stane zastaralé za období uchování a budou odstraněny. 
- Uživatel vypnout nastavení synchronizace na všech jeho zařízeních, pak žádná data nastavení se k nim získat přístup a všechna data nastavení pro tohoto uživatele se změní na zastaralé a budou odstraněny po doba uchovávání informací. 
- Pokud správce služby directory Azure AD vypne cestovní stavu Enterprise pro celý adresář, pak všem uživatelům v tomto adresáře bude přestat synchronizovat nastavení a všechna data nastavení pro všechny uživatele se změní na zastaralé a budou odstraněny za období uchování. 

**Obnovení odstraněných dat**: zásady uchovávání dat není, která dokáže nahradit. Jakmile se trvale odstraní data nebude obnovitelné. Je ale důležité mít na paměti, že data nastavení pouze se odstraní z Azure, není zařízení s koncovým uživatelem. Pokud libovolného zařízení znovu připojí později Enterprise stavu Roamingová služba, bude nastavení znova synchronizovali a uložené v Azure.


## <a name="related-topics"></a>Příbuzná témata
- [Cestovní stavu Enterprise – přehled](active-directory-windows-enterprise-state-roaming-overview.md)
- [Nastavení a data cestovní časté otázky](active-directory-windows-enterprise-state-roaming-faqs.md)
- [Skupina nastavení zásad a MDM pro nastavení synchronizace](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
- [Odkaz na cestovní nastavení Windows 10](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
