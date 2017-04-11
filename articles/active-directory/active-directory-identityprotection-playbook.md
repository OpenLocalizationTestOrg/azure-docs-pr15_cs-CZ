<properties
    pageTitle="Azure Active Directory Identity Protection playbook | Microsoft Azure"
    description="Zjistěte, jak Ochrana Azure AD Identity umožňují omezit možnost mohl využívat hostují identity nebo zařízení a k zajištění identitu nebo zařízení, která byla dříve podezřelé nebo známo, že být."
    services="active-directory"
    keywords="Ochrana identity služby Azure active directory, zjišťování aplikace cloudu, Správa aplikací, zabezpečení, rizika, úroveň rizika, chyba, zásady zabezpečení"
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
    ms.date="08/22/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection-playbook"></a>Azure Active Directory identit ochrany playbook 

Tento playbook umožňuje:

- Vyplnění dat v prostředí ochrana Identity simulace rizikové události a chyby
- Nastavení zásad podmíněné přístupu na základě rizika a otestovat vliv tyto zásady


## <a name="simulating-risk-events"></a>Napodobení rizikové události

Tato část uvádí pomocí kroků pro simulace následující typy rizika událostí:

- Přihlášení z anonymní IP adresy (snadno)
- Přihlášení z cizí umístění (střední)
- Možné cestovní atypické umístění (těžko)

Další rizika události nelze simulovaného bezpečně.


### <a name="sign-ins-from-anonymous-ip-addresses"></a>Přihlášení z anonymní IP adres

Tento typ události rizika identifikuje uživatele, kteří mají úspěšně přihlášení z IP adresu, který byl zjištěn jako anonymní proxy IP adresu. Tyto proxy servery jsou používány osoby, které chcete skrýt IP adresu svého zařízení a mohou být použity pro lidi se zlými úmysly.

**Tak, aby napodobily přihlášení z anonymní IP, proveďte následující kroky**:

1.  Stažení [Prohlížeče mandátu](https://www.torproject.org/projects/torbrowser.html.en).
2.  V prohlížeči mandát, přejděte na [https://myapps.microsoft.com](https://myapps.microsoft.com).   
3.  Zadejte přihlašovací údaje účtu, který se má zobrazit v sestavě **přihlášení z anonymní IP adres** .

Přihlášení se zobrazí na řídicím panelu ochrana Identity do 5 minut. 


###<a name="sign-ins-from-unfamiliar-locations"></a>Přihlášení z cizí umístění

Riziko cizí umístění je mechanismus v reálném čase hodnocení přihlašovací, které považuje za posledních přihlašovací umístění (IP, šířky a délky a ASN) určit umístění nové / cizí. Systém ukládá předchozí IP adresy, šířky a délky a avíza expedice zboží uživatele a byly použity tyto být známé umístění. Místo přihlášení se považuje za cizí, pokud umístění přihlašovací neodpovídá žádné z existující známé umístění.

Ochrana Identity Azure Active Directory:  

 - má počáteční výukové dobu 14 dní, po které ho neoznačí nové umístění jako cizí umístění.
 - ignoruje přihlášení z známé zařízení a umístění, které jsou zeměpisně zavřít existující známé umístění.

Tak, aby napodobily cizí umístění, budete muset přihlásit pomocí umístění a zařízení, které tento účet nemá přihlášení z před. 


**Tak, aby napodobily přihlášení z cizí umístění, proveďte následující kroky**:

1.  Vyberte účet, který obsahuje aspoň 14 dní přihlašovací historie. 

2.  Udělejte jednu:
    
    na. Při používání sítě VPN, přejděte na [https://myapps.microsoft.com](https://myapps.microsoft.com) a zadejte přihlašovací údaje účtu, který chcete, aby napodobily riziková událost pro.

    b. Požádejte spolupracovníkovi jinde se přihlásit pomocí účtu přihlašovacích údajů (nedoporučuje se).

Přihlášení se zobrazí na řídicím panelu ochrana Identity do 5 minut.
 
### <a name="impossible-travel-to-atypical-location"></a>Možné cestovní atypické umístění
Napodobení podmínce možné cestovní totiž složité algoritmus používá počítače se naučíte odstraňování plevele, NEPRAVDA pozitivní například není možné cestovní z známé zařízení nebo přihlášení z VPN, které využívají ostatních uživatelů v adresáři. Kromě toho algoritmu vyžaduje přihlašovací historii 3 až 14 dní pro uživatele před zahájením generování rizikové události.

**Tak, aby napodobily možné cestovní atypické umístění, proveďte následující kroky**:

1.  Pomocí standardní prohlížeče, přejděte na [https://myapps.microsoft.com](https://myapps.microsoft.com).  

2.  Zadejte přihlašovací údaje účtu, který chcete vytvořit událost možné cestovní rizika pro.

3.  Změna uživatelského agenta. Změna uživatelského agenta v Internet Exploreru z nástrojů pro vývojáře nebo změna uživatelského agenta ve Firefoxu / Chrome pomocí doplňku přepínač uživatelského agenta.

4.  Změňte svoji IP adresu. Svoji IP adresu můžete změnit pomocí sítě VPN mandát doplněk nebo otáčející se si nový počítač v Azure v různých datacentra.

5.  Přihlášení k [https://myapps.microsoft.com](https://myapps.microsoft.com) jako dříve pomocí stejné přihlašovací údaje a objevit během pár minut po předchozí přihlášení.

Přihlášení se zobrazí na řídicím panelu ochrana Identity hodin 2-4 při instalaci.<br>
Z důvodu složité počítače studijní zapojené modely je pravděpodobné, který nebude získat vybraných kontakty nahoru.<br> Můžete chtít replikovat tento postup pro více účtů Azure AD.


## <a name="simulating-vulnerabilities"></a>Napodobení chyby 
Chyby jsou slabých v Azure AD prostředí, které můžete využívat chybná actor. Právě se v Azure AD Identity ochrany využít další funkce Azure AD vytažená 3 typy chyb. Těchto chyb se zobrazí na řídicím panelu ochrana Identity automaticky po tyto funkce jsou nastavené.

-   Azure AD [Vícefaktorové ověřování?](../multi-factor-authentication/multi-factor-authentication.md)
-   Azure AD [Zjišťování aplikace cloudu](active-directory-cloudappdiscovery-whatis.md).
-   Azure AD [Správa privilegovaných identit](active-directory-privileged-identity-management-configure.md). 



##<a name="user-compromise-risk"></a>Riziko narušení zabezpečení uživatele

**Chcete-li otestovat riziko narušení zabezpečení pro uživatele, proveďte následující kroky**:

1.  Přihlaste se k [https://portal.azure.com](https://portal.azure.com) pomocí přihlašovacích údajů globálního správce pro vašeho klienta.

2.  Přejděte na **ochranu Identity**. 

3.  Na hlavní **Ochrana Azure AD Identity** zásuvné klikněte na **Nastavení**. 

4.  Na zásuvné **Nastavení portálu** klikněte v části **pravidla zabezpečení** **uživatele napadnout rizika**. 

5.  Na zásuvné **přihlášení rizika** vypnout **povolte pravidlo pro** a potom klikněte na **Uložit** nastavení.

6.  Pro dané uživatelský účet simulovat cizí umístění nebo anonymní IP riziková událost. **Střední**to bude zvýšit úroveň rizika uživatelů pro daného uživatele.

7.  Počkejte několik minut a potom zkontrolujte, že úroveň uživatelů pro vaše uživatele **médiu**.

8.  Přejděte na zásuvné **Nastavení portálu** .

9.  **Na zásuvné **Riziko narušení zabezpečení uživatele** pod zaškrtávacím políčkem, **povolte pravidlo**vyberte.** 

10. Vyberte jednu z následujících možností:

    na. Pokud chcete blokovat, vyberte položku **Střední** ve skupinovém rámečku **blok přihlásit**.

    b. K jejímu vynucení změna zabezpečené hesla, vyberte položku **Střední** v části **požadovat vícefaktorové ověřování**.

13. Klikněte na **Uložit**.

14. Nyní můžete otestovat podmíněné přístupu na základě rizika tak, že se při používání uživatele k úrovni zvýšeným rizika. Pokud je uživatel riziko střední, v závislosti na konfiguraci vašeho zásad vaše přihlašovací je buď blokovat nebo budete muset změnit heslo. 
<br><br>
![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
<br>

 
##<a name="sign-in-risk"></a>Přihlašovací rizika

 
**Při testování přihlášení rizika, postupujte podle těchto kroků:**

1.  Přihlaste se k [https://portal.azure.com](https://portal.azure.com) pomocí přihlašovacích údajů globálního správce pro vašeho klienta.

2.  Přejděte na **ochranu Identity**.

3.  Na hlavní **Ochrana Azure AD Identity** zásuvné klikněte na **Nastavení**. 

4.  Na zásuvné **Nastavení portálu** klikněte v části **pravidla zabezpečení**, **Přihlaste se rizika**.

5.  Na zásuvné **přihlášení rizika **klikněte **na** v části **Povolit pravidla**. 

7.  Vyberte jednu z následujících možností:

    na. Pokud chcete blokovat, vyberte položku **Střední** ve skupinovém rámečku **blok přihlášení**

    b. K jejímu vynucení změnu zabezpečené hesla, vyberte položku **Střední** v části **požadovat vícefaktorové ověřování**.

8.  Pokud chcete blokovat, vyberte v části Block přihlášení médiu.

9.  Pokud chcete vynutit vícefaktorové ověřování, vyberte položku **Střední** v části **požadovat vícefaktorové ověřování**.

10. Klikněte na **Uložit**.

11. Nyní můžete otestovat podmíněné přístupu na základě rizika pomocí simulace cizí umístění nebo anonymní události rizika IP, protože jsou oba **Střední** rizikové události.

<br>
![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")
<br>


## <a name="see-also"></a>Viz taky

 - [Ochrana Identity Azure Active Directory](active-directory-identityprotection.md)
