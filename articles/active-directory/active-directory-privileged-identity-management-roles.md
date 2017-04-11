<properties
   pageTitle="Role v osobních | Microsoft Azure"
   description="Zjistěte, jaké role se používají pro privilegovaných identit s příponou privilegovaných Správa identit Azure."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="roles-in-azure-ad-privileged-identity-management"></a>Role ve službě Správa identit Azure AD oprávněními

<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

Ve vaší organizaci různých správní role v Azure AD můžou uživatelům přiřadit. Přiřazení rolí určit, které úkoly, jako je přidání nebo odebrání uživatelů nebo změna nastavení služby uživatelé můžou provádět Azure AD, Office 365 a jiné Microsoft Online Services a propojených aplikací.  

Globální správce můžete aktualizovat, které uživatelé budou **trvale** přiřazená k rolím v Azure AD pomocí rutin prostředí PowerShell jako `Add-MsolRoleMember` a `Remove-MsolRoleMember`, nebo prostřednictvím portálu klasické jako pokynů v tématu [přiřazení rolí správce v Azure Active Directory](active-directory-assign-admin-roles.md).

Správa identit Azure AD oprávněními (osobních) slouží ke správě zásad pro přístup privilegovaných uživatelů k v Azure AD. Správce osobních informací uživatelům přiřadí jednu nebo více rolí v Azure AD a můžete uživatel trvale v roli, nebo použít roli přiřadit. Když uživatel trvale přiřazenou roli nebo aktivuje měl role přiřazení, a pak můžete spravují Azure Active Directory, Office 365 a jiných aplikací s oprávnění přiřazená k jejich role.

Není žádný rozdíl v dialogu přístup věnovat uživatel, který trvale versus přiřazení měl role. Jediný rozdíl je, že někteří uživatelé nepotřebujete tento přístup vždy. Jsou určené nárok na roli a můžete ho zapnout a vypnutí vždy, když budou muset.

## <a name="roles-managed-in-pim"></a>Role spravovaných v správce osobních informací

Privilegovaných Správa identit umožňuje přiřazení uživatelů do běžné role správců, včetně:


- **Globální správce** (označovaná taky jako Company Administrators) má přístup ke všem funkcím pro správu. Máte víc než jeden globální správce ve vaší organizaci. Osoba, která zaregistruje k zakoupení Office 365 automaticky změní globální správce.
- **Privilegovaných role správce** spravuje Azure AD správce osobních informací a aktualizuje přiřazování rolí pro ostatní uživatele.  
- **Správce fakturace** nakupuje, spravuje předplatné, spravuje požadavky podpory a sleduje stav služby.
- **Správce hesel** obnoví hesla, spravuje žádosti o služby a sleduje stav služby. Správci hesel se omezuje resetování hesel pro uživatele.
- **Správce služby** spravuje žádosti o služby a sleduje stav služby.

  > [AZURE.NOTE] Pokud používáte Office 365, pak před přiřazení role Správce služby pro uživatele, nejdřív přiřadíte uživatel oprávnění správce služby, jako je Exchange Online.

- **Správce správy uživatelů** obnoví hesla, sleduje stav služby a spravuje uživatelských účtů, skupin uživatelů a žádosti o služby. Správce správy uživatelů nelze odstranit globální správce, vytvořit další roli správce nebo resetování hesel pro fakturace, globální správce a Správce služeb.
- **Správce systému Exchange** má přístup pro správu k Exchange Online přes Centrum pro správu Exchange (EAC) a můžete dělat skoro všechny úkoly v Exchange Online.
- **Správce služby SharePoint** má přístup pro správu Sharepointu online přes Centrum pro správu služby SharePoint Online a může v Sharepointu Online dělat skoro všechny úkoly.
- **Správce Skypu pro firmy** má přístup pro správu ke Skypu pro firmy přes Skype pro firmy Centrum pro správu a můžete ve Skypu pro firmy Online dělat skoro všechny úkoly.

Přečtěte si tyto články podrobné informace o [přiřazování rolí správce v Azure AD](active-directory-assign-admin-roles.md) a [přiřazení rolí správců v Office 365](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504).

<!--**PLACEHOLDER: The above article may not be the one we want since PIM gets roles from places other that Office 365**-->


Od správce osobních informací můžete [přiřadit tyto role pro uživatele](active-directory-privileged-identity-management-how-to-add-role-to-user.md) tak, aby uživatel může [aktivovat roli v případě potřeby](active-directory-privileged-identity-management-how-to-activate-role.md).

Pokud chcete dát jiného uživatele přístup ke správě v osobních samotné, role, které správce osobních informací vyžaduje uživatel měl jsou popsány dále v [tom, jak udělit přístup správce osobních informací](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).


<!-- ## The PIM Security Administrator Role **PLACEHOLDER: Need description of the Security Administrator role.**-->

## <a name="roles-not-managed-in-pim"></a>Role není spravovaný v správce osobních informací

Rolí v Exchange Online nebo SharePoint Online, s výjimkou jsou uvedeny výše, nejsou představující v Azure AD a proto nejsou zobrazené na osobních. Další informace o změně přiřazení jemně odstupňovaná rolí v těchto službách Office 365 najdete v článku [oprávnění v Office 365](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d).

V Azure AD nejsou taky reprezentované Azure předplatná a skupiny zdrojů. Správa Azure předplatná, přečtěte si, [jak přidat nebo změnit Azure správcovských rolí](../billing-add-change-azure-subscription-administrator.md) a další informace o Azure RBAC najdete v článku [Řízení přístupu Azure Role-Based](role-based-access-control-configure.md).

<!--**The above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>Role uživatelů a přihlášení
U některých služeb společnosti Microsoft a aplikací, nemusí být uživateli přiřadit roli umožňující tohoto uživatele jako správce.

Přístup k portálu Azure klasické vyžaduje že uživatel být správce služeb nebo správce spolu na předplatném Azure i v případě, že uživatel není nutné spravovat Azure předplatná.  Například spravovat nastavení pro Azure AD na portálu klasické, uživatel musí být globálním správcem Azure AD a dalších správce předplatné Azure předplatného.  Zjistěte, jak přidávat uživatele k Azure předplatným, najdete v článku [jak přidat nebo změnit Azure správcovských rolí](../billing-add-change-azure-subscription-administrator.md).

Přístup ke službám Microsoft Online Services může vyžadovat uživatel taky přiřadit licenci před otevřením služby na portálu a provádění úkolů správy.

## <a name="assign-a-license-to-a-user-in-azure-ad"></a>Přiřazení licence uživateli v Azure AD

1. Přihlaste se k [Azure klasické portálu] (http://manage.windowsazure.com) pomocí účtu globálního správce nebo správce spolu účtu.
2. Výběr **Všech položek** v hlavní nabídce.
3. Vyberte adresář, který chcete pracovat s nainstalovanou licence s ním spojené.
4. Vyberte **licence**. Zobrazí se seznam dostupných licencí.
5. Vyberte licence plán, který obsahuje licencí, které chcete rozdělit.
6. Vyberte **přiřadit uživatelům**.
7. Vyberte uživatele, kterému chcete přiřadit licenci.
8. Klikněte na tlačítko **přiřadit** .  Uživatele můžete teď se přihlaste k Azure.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Další kroky
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
