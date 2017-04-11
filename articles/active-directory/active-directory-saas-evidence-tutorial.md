<properties
    pageTitle="Kurz: Azure Active Directory integrace s Evidence.com | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Evidence.com."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/23/2016"
    ms.author="asmalser"/>


# <a name="tutorial-azure-active-directory-integration-with-evidencecom"></a>Kurz: Azure Active Directory integrace s Evidence.com

Cílem tohoto kurzu je ukazují, jak nastavit jednotné přihlašování mezi Azure Active Directory (AAD) a Evidence.com. Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:
    
* Platné předplatné Microsoft Azure
* Povolené předplatné Evidence.com s jednotné přihlašování (e-mailu earlyaccess@evidence.com Pokud není povolené na základě SAML jednotné přihlašování)

Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace pomocí panelu přístup AAD AAD uživatelů, kterým jste přiřadili Evidence.com přístup.

## <a name="add-evidencecom-to-your-directory"></a>Přidání Evidence.com do adresáře

Tato část popisuje, jak přidat Evidence.com jako integrovanou aplikaci služby Azure Active Directory.

**Chcete-li povolit integraci aplikace důkaz:**

1.  V [Azure klasické portál](https://manage.windowsazure.com), v levém navigačním podokně klikněte na **Služby Active Directory**.

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Pokud chcete otevřít zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horním na **aplikace** .

4.  Otevřít galerii aplikace, klikněte na tlačítko **Přidat**a potom klikněte na **Přidat aplikaci z Galerie**.

5.  Do pole Hledat zadejte **Evidence.com**.

6.  V podokně výsledků vyberte **Evidence.com**a potom klikněte na **Dokončit** přidat aplikaci.


## <a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Tato část popisuje, jak povolit uživatelům ověřit Evidence.com s účtem v Azure Active Directory federation podle protokolu SAML pomocí.

**Konfigurace jednotného přihlašování, proveďte následující kroky:**

1.  Po přidání Evidence.com na portálu Azure klasické, klikněte na **Konfigurovat jednotného přihlašování**. 
 
2.  Na další obrazovce vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

3.  V okně Konfigurovat adresa URL App zadejte adresu URL místo, kam se uživatelé Přihlaste se k svoji adresu URL Evidence.com klienta (Příklad: https://yourtenant.evidence.com a potom na tlačítko **Další**. 

4.  Klikněte na odkaz na **Stažení certifikát** a uložit na místní jednotku pevného disku. Adresy URL metadata (Entity ID, znak jednotné přihlašování v adresy URL a Sign Out adresy URL) a tento certifikát se použijí k nastavení jednotného přihlašování k portálu Evidence.com. 

5.  V okně prohlížeče samostatnou webovou, přihlaste se k vaší Evidence.com klient jako správce a přejděte na kartu **Správce**
      
6.  Klikněte na **jednotné přihlašování k subjekt**
 
7.  Vyberte **SAML na základě jednotného přihlášení**
 
8.  Zkopírujte adresu **URL Vystavitel**hodnoty **Jednotného přihlašování** a **Jeden Odhlásit se** zobrazí na portálu Azure klasické a příslušná pole v Evidence.com.

9.  Otevřít certifikátu stáhli v kroku 4 v textovém editoru, jako je Notepad.exe a zkopírujte a vložte obsah do pole **Certifikátu zabezpečení** . 

10. Konfigurace kromě Evidence.com.
 
11. Na portálu Azure klasický zaškrtněte **potvrďte, že jste nakonfigurovali jednotného přihlašování uvedeným výše**. Kontrola to vám umožní aktuální certifikát, který chcete začít pracovat pro toto políčko aplikace.
 
12. Na stránce Potvrzení přihlášení klepněte na **Dokončit**.  


## <a name="creating-an-evidencecom-test-user"></a>Vytvoření uživatele test Evidence.com

Uživatelům Azure AD mohli přihlásit musí být zřízení pro přístup do aplikace Evidence.com. Tato část popisuje, jak vytvořit Azure AD uživatelských účtů uvnitř Evidence.com

**Chcete-li vytvořit uživatelský účet v Evidence.com:**

1.  V okně webového prohlížeče Přihlaste se k webu společnosti Evidence.com jako správce.

2.  Přejděte na kartu **Správce** .

3.  Klikněte na **Přidat uživatele**.

4.  Klikněte na tlačítko **Přidat** .

5.  **E-mailovou adresu** přidaný uživatel musí odpovídat uživatelské jméno uživatele v Azure AD, který chcete dát přístup. Pokud uživatelské jméno a e-mailovou adresu nejsou stejné hodnoty ve vaší organizaci, můžete **Evidence.com > atributy > jednotného přihlašování** část Azure klasické portálu změnit nameidenitifer posílat Evidence.com za účelem být e-mailovou adresu.


## <a name="assigning-users-to-evidencecom"></a>Přiřazení uživatelů k Evidence.com

Zřizování AAD uživatelům neuvidíte Evidence.com na jejich Panel přístup musí být přiřazena přístup do portálu Azure klasické.

**Přiřazení uživatelů k Evidence.com:**

1.  Na úvodní stránce Evidence.com na portálu Azure klasické klikněte na **přiřadit uživatelům Evidence.com**.
 
2.  V nabídce **Zobrazit** vyberte, zda chcete přiřadit Evidence.com uživatele nebo skupiny a klikněte na tlačítko se zaškrtnutím.
 
3.  V seznamu **Uživatelé** vyberte uživatele do skupiny, ke kterému chcete přiřadit Evidence.com.
 
4.  Zápatí stránky klikněte na tlačítko **přiřadit** .

