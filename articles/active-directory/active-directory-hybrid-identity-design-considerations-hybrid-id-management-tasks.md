<properties
    pageTitle="Azure Active Directory hybridní identity návrh aspektech - zjistěte úlohy správy identit hybridní | Microsoft Azure"
    description="Řízení přístupu podmíněné zkontroluje Azure Active Directory určité podmínky, které jste vybrali při ověřování uživatele a před umožněním přístup k aplikaci. Když jsou splněné tyto podmínky, uživatel ověření a povolený přístup k aplikaci."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="plan-for-hybrid-identity-lifecycle"></a>Plánování hybridního Identity cyklus 

Identita je jedním ze základů strategie enterprise mobilita a aplikace access. Zda jsou přihlášení ke mobilní zařízení nebo aplikací SaaS, vaší identity je klíčová k získání přístupu k všechno. Na nejvyšší úrovni řešení správy identit zahrnuje sjednocení pocházejících a synchronizaci mezi vaší identity úložištích zahrnující automatizace a centralizace proces vytváření zdroje. Řešení identit by měl být centralizované identity mezi místním prostředím a cloudu a také použít některý z federaci identity udržovat centralizované ověřování a bezpečné sdílení a spolupráce s externími uživateli a firmy. Zdroje v rozsahu operačních systémů a aplikací lidem z nebo přidružen organizace. Organizační struktura můžete změnit tak, aby zahrnoval zřizovací zásady a postupy.

Také je důležité, abyste měli řešení pro identitu zaměřené na posílí uživatelů tak, že jim poskytnete samoobslužné prostředí ponechat stále produktivní. Řešení identity je robustnější případě, že ji povolí jednotného přihlašování pro uživatele přes všechny zdroje, které potřebují přístup správcům vůbec úrovně můžete použít standardní postupy pro správu přihlašovací údaje uživatele. Některé úrovně správy můžete snížit nebo vyloučit, podle toho, šířka zřizovací řešení pro správu. Kromě toho můžete bezpečně distribuovat funkce správy, ať už ručně nebo automaticky mezi různými organizace. Správce domény, například si můžete vytisknout pouze osoby a zdroje v této domény. Tento uživatel pro správu a zřizovací úkoly můžete udělat, ale nemá oprávnění k provádění úkolů konfigurace, jako jsou vytváření pracovních postupů.


## <a name="determine-hybrid-identity-management-tasks"></a>Určení úlohy správy identit hybridní
Distribuce úkoly správy ve vaší organizaci zvýšení přesnosti a účinnosti správy a zlepšuje zůstatek pracovní zátěž organizace. Tady jsou kontingenční tabulky, které definování systému správy identit robustní.

 ![](./media/hybrid-id-design-considerations/Identity_management_considerations.png)


Definování úlohy správy identit hybridní, je třeba porozumět některé základní vlastnosti organizace, která bude přijímání hybridní identity. Je důležité pochopit aktuální úložištích použitý pro identitu zdroje. Musíte znát tyto základní prvky, bude mít základní požadavky a na základě, že byste potřebovali pokládat odstupňovaný otázky, které se vás lepší rozhodnutí návrh řešení Identity pro.  

Při definování tyto požadavky splnit, aby se aspoň odpovědět na následující otázky

- Zřizování možnosti: 
 - Podporuje řešení identit hybridní správu přístupu robustní účtu a zřizovací systém?
 - Jak se uživatelé, skupiny a hesla přejdete na spravován?
 - Je správa životního cyklu identit neodpovídá? 
      - Jak dlouho heslo aktualizace účtu pozastavení trvat?
      
- Správa licencí: 
 - Správa licencí hybridní identity řešení úchyty dělá
     - Pokud ano, jaké funkce jsou dostupné?
- Řešení řešit řízení skupinových licence? 
      – Pokud ano, je možné přiřadit skupinu zabezpečení? 
       – Pokud ano, v adresáři cloudu automaticky přiřadí licence všem členům skupiny? 
        – Co se stane, pokud je uživatel později přidat nebo odebrat ze skupiny, bude licence automaticky přiřazení nebo odebrání podle potřeby? 

- Integrace se službou jiných poskytovatelů identit třetích stran:
- Můžete toto hybridní řešení integrovaný s poskytovatelů identit třetích stran implementovat jednotné přihlašování?
- Je možné sjednocení všechny různých Zprostředkovatelé identit jiní do systému kompaktní identity?
- Pokud ano, jak, a které budou a jaké funkce jsou dostupné?

## <a name="synchronization-management"></a>Správa synchronizace
Jedním z cílů správce identity moci přenést všechna Zprostředkovatelé identit jiní a ponechat stále synchronizovat. Zachovat synchronizovat data podle zprostředkovatelem identit autoritativní předlohy. Ve scénáři identity hybridní s modelu synchronizované správy správy identit všechny uživatele a zařízení v místního serveru a synchronizaci účtů a, případně hesla do cloudu. Uživatel zadá stejné heslo místního jako mu nebo bude v cloudu a při přihlášení, heslo ověření identity řešení. Tento model používá nástroj pro synchronizaci adresářů.
 
![](./media/hybrid-id-design-considerations/Directory_synchronization.png)VELKÁ2 návrh synchronizace řešení totožnost hybridní zajistit, že jsou odpovědi na následující otázky: • co jsou k dispozici pro hybridní identity řešení synchronizace řešení?
• Jaké jednotné přihlašování k dispozici funkce?
• Jaké možnosti pro federaci identity mezi B2B a B2C?

## <a name="next-steps"></a>Další kroky
[Určení strategie řízení hybridní identity](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)


## <a name="see-also"></a>Viz taky
[Přehled aspektech návrhu](active-directory-hybrid-identity-design-considerations-overview.md)

