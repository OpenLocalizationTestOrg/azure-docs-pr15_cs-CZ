<properties
    pageTitle="Řešení potíží řízení přístupu na základě rolí | Microsoft Azure"
    description="Pokud potřebujete pomoc s problémy nebo dotazy týkající se řízení přístupu na základě rolí zdroje."
    services="azure-portal"
    documentationCenter="na"
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="kgremban"/>

# <a name="role-based-access-control-troubleshooting"></a>Na základě rolí řízení přístupu k řešení potíží

## <a name="introduction"></a>Úvod

[Řízení přístupu na základě rolí](role-based-access-control-configure.md) je výkonné funkce, která umožňuje jemně odstupňovaná přístup ke zdrojům v Azure delegáta. To znamená, že máte pocit jistí udělit práva k užívání přesně to, co potřebují určité osoby a další. Však v některých případech může být složité model prostředků pro Azure zdroje a může být obtížné porozumět přesně co chcete udělit oprávnění.

Tento dokument umožňují vědět, co můžete očekávat při používání některých rolích Azure portálu. Tyto tři role zabývat těmito oblastmi všechny typy zdrojů:

- Vlastník  
- Skupiny přispěvatelů  
- Čtečky  

Vlastníci a přispěvatelům mají úplný přístup k možnostem správy, ale Přispěvatel nelze udělení přístupu k další uživatele nebo skupiny. Co potřebujete trochu zajímavější s roli Čtenář tak, aby se, kde bude věnovat některé času. Naleznete v [článku get začít řízení přístupu na základě rolí](role-based-access-control-configure.md) podrobné informace o tom, jak udělit přístup.

## <a name="app-service-workloads"></a>Aplikace služby úloh

### <a name="write-access-capabilities"></a>Možnosti přístupu k zápisu

Udělení přístupu uživatelů jen pro čtení na jeden web appu některé funkce jsou zakázány, nemusí očekávat. Následující možnosti správy vyžadují **psaní** přístup do webových aplikací (Přispěvatel nebo vlastník) a nebude k dispozici v jakémkoli scénáři jen pro čtení.

- Příkazy (například start zastavit, atd.)
- Změna nastavení, jako je obecné konfigurace, nastavení, nastavení zálohování a nastavení sledování
- Přístup k publikování přihlašovací údaje a jiných tajemství jako nastavení aplikace a připojovací řetězec
- Přenos protokoly
- Konfigurace protokoly pro diagnostiku
- Console (příkazového řádku)
- Aktivní a poslední nasazení (pro místní libovolná nepřetržitý nasazení)
- Předpokládané výdaje
- Testuje web
- Virtuální sítě (viditelné pouze pro čtení Pokud virtuální sítě nakonfigurované dříve uživatel přístup pro zápis).

Pokud budete mít přístup k některé z těchto dlaždice, musíte požádejte správce skupiny přispěvatelů přístup k web appu.

### <a name="dealing-with-related-resources"></a>Práce s související materiály

Jsou webové aplikace komplikovaně podle stavu několika různých zdrojů, které mezi. Tady je typické skupina se pár weby:

![Skupina zdroje web app](./media/role-based-access-control-troubleshooting/website-resource-model.png)

Jako výsledek, pokud někdo povolíte přístup k jenom web app a mnoho funkcí na zásuvné webu Azure portálu přestane být platná.

Tyto položky přistupují **psaní** **plán služeb aplikací** , které odpovídá k vašemu webu:  

- Zobrazení web appu je ceny osy (Free nebo standardní)  
- Konfigurace měřítko (počet instance, velikost virtuálního počítače, nastavení automatické měřítko)  
- Kvóty (úložiště, šířku pásma, procesoru)  

Tyto položky přistupují **psaní** celé **pole Skupina zdroje** , která obsahuje váš web:  

- Certifikáty SSL a vazby (totiž certifikáty SSL možné sdílet mezi weby v rámci stejné skupiny prostředků a geo umístění)  
- Upozornění pravidel  
- Nastavení automatické měřítko  
- Přehledy součásti aplikace  
- Testuje web  

## <a name="virtual-machine-workloads"></a>Virtuální počítač úloh

Podobně jako pomocí webových aplikací web apps, některé funkce na zásuvné virtuálního počítače vyžadovat přístup pro zápis virtuálního počítače, případně dalších zdrojů ve skupině zdroje.

Virtuálních počítačích souvisejí domény názvy virtuálních sítí, účty úložiště a upozornění pravidla.

Tyto položky přistupují **Zapisovat** do **virtuálního počítače**:

- Koncové body  
- IP adresy  
- Disků  
- Rozšíření  

Tyto přistupují **psát** do **virtuálního počítače**i na **pole Skupina zdroje** (spolu s název domény), která se nachází v:  

- Nastavení dostupnosti  
- Načtení rovnováha nastavení  
- Upozornění pravidel  

Pokud budete mít přístup k některé z těchto dlaždice, musíte požádejte svého správce skupiny přispěvatelů přístupu pro skupiny zdrojů.

## <a name="see-more"></a>Další informace naleznete v
- [Řízení přístupu na základě rolí](role-based-access-control-configure.md): Začínáme s RBAC Azure portálu.
- [Předdefinované role](role-based-access-built-in-roles.md): zobrazení podrobností o rolích dodaných standardní v RBAC.
- [Vlastní role v Azure RBAC](role-based-access-control-custom-roles.md): Naučte se vytvářet vlastní role podle vašich potřeb přístup.
- [Vytvoření aplikace access změnit sestava historie](role-based-access-control-access-change-history-report.md): Udržujte si přehled o změně přiřazování rolí v RBAC.
