<properties
    pageTitle="Zabezpečit cloudové zdroje s Azure MFA a služby AD FS"
    description="Tohle je stránka Azure Multi-Factor ověřování, který popisuje, jak začít s Azure MFA a AD FS v cloudu."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a>Zabezpečení cloudové zdroje s Azure Vícefaktorové ověřování a služby AD FS

Pokud vaše organizace je federované se službou Azure Active Directory, použijte k zabezpečení prostředky, které jsou používána v Azure AD Azure Vícefaktorové ověřování nebo Active Directory Federation Services. Použijte následující postupy pro zabezpečení prostředků Azure Active Directory s Azure Vícefaktorové ověřování nebo Active Directory Federation Services.

## <a name="secure-azure-ad-resources-using-ad-fs"></a>Zabezpečení prostředků Azure AD pomocí služby AD FS

Zabezpečení cloudové zdroje, nejprve povolit účet pro uživatele a pak nastavení deklarace pravidla. Tento postup provede jednotlivými kroky:

1. Pomocí kroků uvedených v [Turn-on vícefaktorové ověřování pro uživatele](active-directory/multi-factor-authentication-get-started-cloud.md#turn-on-multi-factor-authentication-for-users) povolit účet.
2. Spusťte konzolu Správa AD FS.
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/adfs1.png)
3. Přejděte na **Může důvěryhodnosti stran** a klikněte pravým tlačítkem myši na může stran důvěryhodné. Vyberte **Upravit deklarace pravidla...**
4. Klikněte na **Přidat pravidlo...**
5. Z rozevírací nabídky vyberte **Odeslat deklarací pomocí pravidla vlastní** a klikněte na tlačítko **Další**.
6. Zadejte název pravidla deklarace.
7. V části vlastní pravidlo: přidejte následující text:

    ```
    => issue(Type = "http://schemas.microsoft.com/claims/authnmethodsreferences", Value = "http://schemas.microsoft.com/claims/multipleauthn");
    ```

    Odpovídající tvrzení:

    ```
    <saml:Attribute AttributeName="authnmethodsreferences" AttributeNamespace="http://schemas.microsoft.com/claims">
    <saml:AttributeValue>http://schemas.microsoft.com/claims/multipleauthn</saml:AttributeValue>
    </saml:Attribute>
    ```

8. Klikněte na **OK** a **Dokončit**. Zavřete konzolu Správa AD FS.

Uživatelé můžou proveďte přihlášení pomocí metody místní (například čipové karty).

## <a name="trusted-ips-for-federated-users"></a>Důvěryhodné IP adresy pro federovaní uživatelé
Důvěryhodné IP adresy správcům umožňují obtokem dvoustupňového ověření pro konkrétní IP adresy nebo federované uživatelů, kteří mají žádosti o pocházející z ve své vlastní intranetu. Následující části popisují, jak nakonfigurovat federovaní uživatelé a obtokem dvoustupňového ověření Azure Multi-Factor Authentication důvěryhodných adres IP při žádost pochází z v síti intranet federovaní uživatelé. Je to dosáhnout konfiguraci služby AD FS-li použít předávací nebo filtrovat příchozí šabloně nároku s typem deklaraci identity v podnikové síti.

Tento příklad používá Office 365 pro naše může stran považuje za důvěryhodnou.

### <a name="configure-the-ad-fs-claims-rules"></a>Konfigurace pravidel deklarací služby AD FS

První věc, kterou jsme potřeba udělat je třeba nakonfigurovat deklarace služby AD FS. Vytvoříme dva deklarací pravidla, jeden typ deklaraci identity v podnikové síti a další jeden pro uchovávání uživatelích přihlášení.

1. Spusťte nástroj AD FS Správa.
2. V levé části vyberte **Může stran považuje za důvěryhodnou**.
3. Klikněte pravým tlačítkem myši na **Identity platformu Microsoft Office 365** a vyberte **Upravit deklarace pravidla...** 
 ![Cloudu](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)
4. Na vydávání transformace pravidla, klikněte na **Přidat pravidlo.** 
 ![Cloudu](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)
5. Na transformace deklarace pravidlo Průvodce přidáním vyberte **předat prostřednictvím filtru příchozí deklarace** z rozevíracího seznamu a klikněte na tlačítko **Další**.
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)
6. V dialogovém okně vedle názvu deklarace pravidlo pojmenujte pravidla. Příklad: InsideCorpNet.
7. Z rozevíracího seznamu vedle příchozí typ deklarace, vyberte **V podnikové síti**.
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)
8. Klikněte na **Dokončit**.
9. Na vydávání transformace pravidla klikněte na **Přidat pravidlo**.
10. Přidání transformace deklarace pravidlo průvodce vyberte **Odeslat deklarací pomocí pravidla pro vlastní** z rozevíracího seznamu a klikněte na tlačítko **Další**.
11. Do pole v části název pravidla deklarace: Zadejte *zachovat uživatelé přihlášení*.
12. V poli vlastní pravidla zadejte:

        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip5.png)
13. Klikněte na **Dokončit**.
14. Klikněte na tlačítko **použít**.
15. Klikněte na **Ok**.
16. Zavřete okno Správa AD FS.



### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a>Konfigurace Azure Multi-Factor Authentication důvěryhodné IP adresy s federovaní uživatelé
Teď, když nároky na místě, jsme konfigurace důvěryhodných IP adresy.

1. Přihlaste se k [Azure klasické portálu](https://manage.windowsazure.com).
2. Na levé straně klikněte na **Služby Active Directory**.
3. V části adresář vyberte v adresáři, ve které chcete nastavit důvěryhodných IP adresy.
4. Na adresář, který jste vybrali klikněte na **Konfigurovat**.
5. V části vícefaktorové ověřování klikněte na **Nastavení služby Správa**.
6. Na stránce nastavení služby v části důvěryhodných IP adresy, vyberte **Přeskočit služba Multi-Factor-authentication pro požadavky z federovaní uživatelé na můj intranet.** 
 ![Cloudu](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip6.png)
7. Klikněte na **Uložit**.
8. Po použití aktualizací klepnutím na tlačítko **Zavřít**.


Je to! V tomto okamžiku federovaní uživatelé Office 365 jenom si měli používat MFA při deklaraci pochází z mimo podnikové síti.
