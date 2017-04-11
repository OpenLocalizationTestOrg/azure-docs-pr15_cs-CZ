<properties
    pageTitle="Nastavení zásad a MDM skupiny | Microsoft Azure"
    description="Informace o zásad skupiny a mobilní zařízení nastavení správy (MDM), použité na podnikové vlastní zařízeních. Tyto zásady zaevidují do celého zařízení uživatele."
    services="active-directory"
    keywords="Co jsou zásady skupiny a nastavení MDM pro cestovní stavu Enterprise, cestovní stavu Enterprise cloudu windows"
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

# <a name="group-policy-and-mdm-settings"></a>Nastavení zásad skupiny a MDM

Použijte tyto zásady skupiny a nastavení mobilního zařízení (MDM) správy jenom na zařízeních podnikové vlastnictví, protože tyto zásady platí pro celou zařízení uživatele. Použití zásad správy MDM k zakázání nastavení synchronizace pro osobní, budou uživatelský zařízení negativně ovlivnit použít toto zařízení. Kromě toho jiných uživatelských účtů v zařízení budou také ovlivněny zásadu.

Podniky, které chcete spravovat cestovní pro osobní zařízení (nespravované) můžete používat portál Azure povolit nebo zakázat cestovní, spíše než pomocí zásad skupiny nebo MDM.
Následující tabulka popisuje nastavení zásad k dispozici.

## <a name="mdm-settings"></a>Nastavení MDM
Nastavení zásad MDM platí pro Windows 10 a Windows 10 Mobile.  Windows 10 Mobile neexistuje podpora pouze účet Microsoft na základě cestovní přes účet Onedrivu.  Získáte v části "Zařízení a koncové body" podrobné informace o jaká zařízení podporují pro synchronizaci Azure AD založena.

| Jméno                               | Popis                                                          |
|------------------------------------|----------------------------------------------------------------------|
| Povolit připojení k účtu Microsoft | Umožňuje uživatelům ověření pomocí účtu Microsoft ze zařízení |
| Povolit nastavení synchronizace             | Umožňuje přecházet nastavení systému Windows a aplikace data. Zakázání této zásady zakáže synchronizace, jakož i zálohování na mobilních zařízeních                  |

## <a name="group-policy-settings"></a>Nastavení zásad skupiny
Nastavení zásad skupiny platí pro Windows 10 zařízení připojených k doméně služby Active Directory. Tabulka obsahuje také starší verze nastavení, která by zdá se spravuje nastavení synchronizace, ale nefungují s Enterprise stavu cestovní pro Windows 10, které jsou označeny s "Nepoužívejte" v popisu.

| Jméno                                | Popis |
|-------------------------------------|-------------|
| Účty: Účty Microsoft blok  |Toto nastavení zásad uživatelé přidávání nových účtů Microsoft na tomto počítači|
| Můžu synchronizovat                         |Zabráníte uživatelům přecházet nastavení systému Windows a aplikace dat|
| Můžu synchronizovat přizpůsobení             |Zakáže synchronizaci rohu skupiny motivy|
| Můžu synchronizovat nastavení prohlížeče        |Zakáže synchronizaci skupiny Internet Explorer|
| Můžu synchronizovat hesla               |Zakáže synchronizaci hesel skupiny|
| Můžu synchronizovat další nastavení systému Windows  |Zakáže synchronizaci ostatní okna nastavení skupiny|
| Můžu synchronizovat ploše pro individuální nastavení |Nepoužívejte; nemá žádný vliv|
| Nelze synchronizovat na připojení účtované podle objemu dat  |Zakáže roamingu na připojení, například mobilní 3 G podle objemu dat|
| Můžu synchronizovat aplikace                    |Nepoužívejte; nemá žádný vliv|
|Můžu synchronizovat nastavení aplikace             |Zakáže cestovní dat aplikace|
|Můžu synchronizovat nastavení start           |Nepoužívejte; nemá žádný vliv|


## <a name="related-topics"></a>Příbuzná témata
- [Cestovní stavu Enterprise – přehled](active-directory-windows-enterprise-state-roaming-overview.md)
- [Povolit enterprise stav roamingové služby Azure Active Directory](active-directory-windows-enterprise-state-roaming-enable.md)
- [Nastavení a data cestovní časté otázky](active-directory-windows-enterprise-state-roaming-faqs.md)
- [Odkaz na cestovní nastavení Windows 10](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
