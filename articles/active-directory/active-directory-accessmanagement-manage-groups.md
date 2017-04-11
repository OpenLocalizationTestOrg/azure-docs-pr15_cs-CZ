<properties
    pageTitle="Správa skupin v Azure Active Directory | Microsoft Azure"
    description="Informace o vytváření a Správa skupin pro správu uživatelů Azure pomocí služby Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/29/2016"
    ms.author="curtand"/>


# <a name="managing-groups-in-azure-active-directory"></a>Správa skupin v Azure Active Directory

> [AZURE.SELECTOR]
- [Azure portálu](active-directory-groups-create-azure-portal.md)
- [Azure klasické portálu](active-directory-accessmanagement-manage-groups.md)
- [Prostředí PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)


Jednou z funkcí Správa uživatelů služby Azure Active Directory (Azure AD) je možnost vytvářet skupiny uživatelů. Skupina umožňuje provádět úlohy správy třeba přiřazování licencí nebo oprávnění pro několik uživatelů najednou. Můžete také skupiny přiřadit oprávnění pro přístup k

- Zdroje, jako jsou objekty v adresáři
- Zdroje externích k adresáři například SaaS aplikací, Azure služby, webů služby SharePoint nebo místní zdroje

Kromě toho vlastníka zdroje můžete taky přiřazení přístup ke zdroji do skupiny Azure AD vlastní někdo jiný. Přiřazení uděluje členové této skupiny přístup k tomuto prostředku. Potom vlastník skupiny spravuje členství ve skupině. Efektivní vlastníka zdroje deleguje vlastník skupiny oprávnění pro přiřazení uživatelů k jejich zdroje.

## <a name="how-do-i-create-a-group"></a>Jak vytvořím skupinu?

V závislosti na služeb, které organizace odebírá můžete vytvořit skupinu pomocí jedné z následujících akcí:
- klasický portálu Azure
- portál účtu Office 365
- na portálu účet Windows Intune

Jak provést v portálu Azure klasické jsme budete popsat úkoly. Další informace o používání jiných Azure portály ke správě adresáři Azure AD najdete v tématu [Správa Azure AD adresář](active-directory-administer.md).

1. [Azure klasické portál](https://manage.windowsazure.com)vyberte **Služby Active Directory**a vyberte název adresáře vaší organizace.

2. Vyberte kartu **skupiny** .

3. Vyberte možnost **Přidat skupinu**.

4. V okně **Přidat skupinu** zadejte název a popis skupiny.


## <a name="how-do-i-add-or-remove-individual-users-in-a-security-group"></a>Jak přidat nebo odebrat jednotlivé uživatele do skupiny zabezpečení?

**Přidání jednotlivých uživatelů do skupiny**

1. [Azure klasické portál](https://manage.windowsazure.com)vyberte **Služby Active Directory**a vyberte název adresáře vaší organizace.

2. Vyberte kartu **skupiny** .

3. Otevřete skupinu, do kterého chcete přidat členy. Otevřete kartu **členy** vybrané skupiny, pokud ji už není zobrazení.

4. Vyberte možnost **Přidat členy**.

5. Na stránce **Přidání členů** vyberte jméno uživatele nebo skupiny, kterou chcete přidat jako členové této skupiny. Ujistěte se, že tento název se přidá do podokna **vybrané** .


**Chcete-li odebrat jednotlivé uživatele ze skupiny**

1. [Azure klasické portál](https://manage.windowsazure.com)vyberte **Služby Active Directory**a vyberte název adresáře vaší organizace.

2. Vyberte kartu **skupiny** .

3. Otevřete skupinu, ze kterého chcete odebrat členy.

4. Vyberte kartu **Členové** , vyberte jméno člena, kterou chcete odebrat z této skupiny a potom klikněte na **Odebrat**.

6. Potvrďte příkazového řádku, že chcete odebrat tohoto člena ze skupiny.


## <a name="how-can-i-manage-the-membership-of-a-group-dynamically"></a>Jak můžu dynamicky spravovat členství ve skupině?

V Azure AD můžete velmi snadno nastavíte jednoduché pravidlo a zjistit, které uživatelé můžou být členy skupiny. Jednoduché pravidlo je taková, která provádí pouze jeden porovnání. Například pokud se aplikace SaaS přiřadí skupiny, můžete nastavit pravidla přidání uživatelů, kteří mají funkce "Prodejce". Pokud chcete toto pravidlo pak uděluje přístup k této aplikaci SaaS všem uživatelům s tímto názvem projektu v adresáři.

Při jakékoli atributy změny uživatele systému všechna pravidla dynamické skupina v adresáři zobrazíte-li změnit atribut uživatele by aktivace všechny skupiny přidá nebo odstraní. Pokud uživatel splňuje pravidla ve skupině, přidají se jako člen do příslušné skupiny. Pokud už splňují pravidla, v jakém jsou členem skupiny, odeberou se jako členů z této skupiny.

> [AZURE.NOTE] Můžete nastavit pravidla pro dynamické členství na skupiny zabezpečení nebo Office 365. Členství ve skupinách vnořené aktuálně nepodporuje přiřazení Skupina založená na aplikace.
>
> Dynamické členství ve skupinách vyžadují licenci Azure AD Premium přiřadit
>
> - Správce, kdo vám spravuje pravidlo ve skupině
> - Všichni členové skupiny

**Chcete-li povolit dynamické členství pro skupinu**

1. [Azure klasické portál](https://manage.windowsazure.com)vyberte **Služby Active Directory**a vyberte název adresáře vaší organizace.

2. Vyberte kartu **skupiny** a otevřete skupinu, kterou chcete upravit.

3. Vyberte kartu **Konfigurovat** a pak nastavte **Povolení dynamického členství** na hodnotu **Ano**.

4. Nastavení jednoduchého jedno pravidlo pro skupinu, které chcete určit, jak dynamické členství pro tento skupinové funkce. Přesvědčte se, zda **přidávat uživatele kde** možnost zaškrtnuté a potom vyberte vlastnosti uživatele ze seznamu (například oddělení, funkce, atd.)

5. Potom vyberte podmínku (nerovná, je rovno, ne začíná, začíná, neobsahuje, obsahuje, neodpovídá, POZVYHLEDAT).

6. Zadejte hodnotu porovnání vlastností vybraného uživatele.

Další informace o tom, jak vytvořit *složitější* pravidla (pravidla, která může obsahovat více porovnání) pro dynamické členství ve skupinách najdete v tématu [použití atributů do vytvářet složitější pravidla](active-directory-accessmanagement-groups-with-advanced-rules.md).

## <a name="additional-information"></a>Další informace

Tyto články poskytují další informace o Azure Active Directory.

* [Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory](active-directory-manage-groups.md)

* [Azure Active Directory rutiny pro konfiguraci nastavení skupiny](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)

* [Co je Azure Active Directory?](active-directory-whatis.md)

* [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)
