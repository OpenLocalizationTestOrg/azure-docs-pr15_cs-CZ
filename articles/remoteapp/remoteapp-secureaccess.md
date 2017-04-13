
<properties 
    pageTitle="Zabezpečení aplikace access Azure RemoteApp a za | Microsoft Azure"
    description="Zjistěte, jak zabezpečený přístup k Azure RemoteApp pomocí podmíněného přístupu v Azure Active Directory"
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="securing-access-to-azure-remoteapp-and-beyond"></a>Zabezpečení aplikace access Azure RemoteApp a za

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

V tomto článku jsme získáte základní informace o správce můžete nastavení kanálu zabezpečený přístup počínaje koncového uživatele prostřednictvím Azure RemoteApp a končící znakem zabezpečené zdroje ATP jiné aplikace back-end databázi SQL. Cílem je Ujistěte se, jestli pouze uživatelé schůzky požadované podmínky mají přístup k vzdálené aplikace a, zabezpečené back-end přístup pouze z kontrolované Azure RemoteApp prostředí a ne z jiných umístění.

Existují 3 hlavních oblastí, které se podívat, potřebuje správce:

![Azure RemoteApp podmíněného přístupu co byste měli zvážit](./media/remoteapp-secureaccess/ra-conditionalenvironment.png)

Pro čtení na informace a odpovědi na Tyhle otázky.

## <a name="who-can-access-the-collection"></a>Kdo má přístup k kolekci?
Správce vybere uživatelé s přístupem ke vzdálené aplikace v kolekci. Můžete použít Azure Active Directory (Azure AD) pracovní nebo školní účty (dříve nazývanou, "účty organizace") nebo Microsoft účty (například @outlook.com). Většina podnikové scénáře použití Azure AD účtů; které vám umožní použít podmíněné access funkce popisované později a jsou také pouze volba Collections doméně. Zbytek v článku se předpokládá, že účty Azure AD se službou Azure RemoteApp.

**Jak můžeme mít provést:**

Pomocí účtů Azure AD pomocí řízení přístupu k Azure RemoteApp dostaneme dvě věci:

1.  Pořád věděli kdo má přístup k aplikace jsme publikovali a přístup ke všem zpět konce tyto aplikace připojení k.
2.  Máme kontrolovat podkladového Azure AD, ať můžeme vytvořit a odstranit uživatelských účtů, nastavit zásady pro hesla, použijte vícefaktorové ověřování, atd. 

## <a name="how-is-the-collection-accessed-from-where"></a>Jak se přistupuje kolekci? Odkud?
Běžně správci chtít definování zásad pro přístup k veřejné internetového prostředí, například Azure RemoteApp. Například zájmu zajištění uživatelům přístup k prostředí z mimo podnikovou síť používat vícefaktorové ověřování (MFA) k získání přístupu; nebo možná mají být blokovány úplně odebrat.

Azure RemoteApp mohou správci funkce dostupné prostřednictvím Azure AD Premium nastavit zásady podmíněného přístupu pro jejich Azure RemoteApp prostředí. Vytváření sestav a výstrahy funkce lze také použít ke sledování, jak se přistupuje prostředí.

### <a name="how-to-set-up-conditional-access-for-azure-remoteapp"></a>Jak nastavit podmíněného přístupu pro Azure RemoteApp
Budeme projděte si příklad scénáře – RemoteApp Azure správce chce blokovat přístup k prostředí, když uživatelé jsou mimo podnikovou síť.

>[AZURE.NOTE] Předpokladu, že jste aktualizovali Azure AD osy Premium a že jste vytvořili aspoň jedna kolekce Azure RemoteApp.

1.  Azure portálu klikněte na kartu **Služby Active Directory** . Klikněte na adresář, který chcete konfigurovat.

    Mějte na paměti: Podmíněné přístup je vlastnost adresáře a ne Azure RemoteApp tak všechny konfigurace probíhá na úrovni adresáře. Také znamená to, že musíte být správce adresáře tyto změny provedete.

2.  Klikněte na **aplikace**a potom klikněte na **Microsoft Azure RemoteApp** nastavit podmíněného přístupu. Všimněte si, že můžete nastavit podmíněného přístupu pro každou aplikaci "software jako služba" v adresáři vašeho samostatně.
![Nastavení podmíněného přístupu pro Azure RemoteApp](./media/remoteapp-secureaccess/ra-conditionalaccessscreen.png)
 

3.  Na kartě **Konfigurovat** nastavení **Povolit pravidla přístupu** na zapnuto.
![Povolte pravidla přístupu pro Azure RemoteApp](./media/remoteapp-secureaccess/ra-enableaccessrules.png)
 

4.  Nyní můžete konfigurovat různými pravidla a zvolte, který chcete použít, aby:

    1. Klikněte na **Blokovat přístup, když není v práci** zcela zabránit uživatelům v přístupu k Azure RemoteApp mimo sítě prostředí, které zadáte.
    2. Vyberte možnost dole definovat rozsahy IP adres, které představují "důvěryhodné síti". Všechno mimo můžou být bude odmítnuto.

5.  Konfiguraci otestujte spuštěním klienta Azure RemoteApp z IP adresu mimo oblast, kterou jste zadali. Po přihlášení pomocí svých přihlašovacích údajů Azure AD byste měli vidět nějaká takováto zpráva:

![Odepření přístupu k Azure RemoteApp](./media/remoteapp-secureaccess/ra-accessdenied.png)
 

### <a name="future-conditional-access-features"></a>Budoucí podmíněné přístup k funkcím 
Tým Azure Active Directory pracuje na nové funkce v aplikaci Access podmíněné. Správci budou moct vytvářet nové typy pravidel mimo síť umístění na základě pravidel. Veřejné náhled nových funkcí by měl být brzy k dispozici.

### <a name="how-to-monitor-access-to-azure-remoteapp"></a>Jak sledovat přístup k Azure RemoteApp
Skvělé funkce používat vedle podmíněného přístupu je funkci vykazování Azure Active Directory Premium. Můžete použít sestavy na monitor, který používá prostředí a zjistit podezřelých aktivity.

Třeba může zobrazit jména uživatelů, kteří k nim získat přístup Azure RemoteApp, kolikrát se začaly a kdy.

1.  Azure portálu klikněte na **Služby Active Directory**a potom klikněte na adresář.

2.  Přejděte na kartu **sestavy** .

3.  V seznamu zpráv vyberte **využití aplikace** v části **integrované aplikace**.

    Zobrazí se některé agregované statistiky Azure RemoteApp. 
![Souhrnné Statistika přístup Azure RemoteApp](./media/remoteapp-secureaccess/ra-accessstats.png)
 
5.  Klepněte na aplikaci zobrazíte informace o uživatelích přístup k Azure RemoteApp.
![Statistika přístup uživatelů pro Azure RemoteApp](./media/remoteapp-secureaccess/ra-userstats.png)
 
### <a name="summary"></a>Souhrn
S Azure Active Directory Premium můžete nastavit pravidla přístupu Azure RemoteApp (a jiného softwaru jako aplikací služby prostřednictvím Azure AD). Pravidla se aktuálně omezuje zásady umístění podle sítě, ale bude v budoucnu prodlužuje na další aspekty Správa podnikového.

Azure AD Premium nabízí oznámení a sledování funkcí, které rozšířit ovládací prvek správce má nad jejich Azure RemoteApp prostředí.

## <a name="how-do-i-make-sure-my-secure-resource-is-accessible-only-from-my-azure-remoteapp-environment"></a>Jak mohu přesvědčit, že můj zabezpečené zdroje je dostupná jenom z Moje Azure RemoteApp prostředí?
V předchozích částech tohoto článku jsme věnovaných zabezpečení přístupu k prostředí Azure RemoteApp. Jsme mají provést, vyberte uživatele, kteří mají povolený přístup a nastavením pravidel přístup na další ovládací prvek, jak můžete pomocí služby.

Běžné situace Azure RemoteApp nasazení je, že vzdálené aplikace potřebují komunikovat s back-end zdroj, například databázi SQL. Zdroj je hostovaný buď místně (například v podnikové síti) nebo v cloudu (například v Azure IaaS). Správci často chcete mít jistotu, že zdroje back-end přístupný pouze nasazené přes Azure RemoteApp tak není třeba aplikace práci přímo na počítači uživatele a přístup k Internetu, veřejné. Azure RemoteApp je často uváděný jako centrální správa přístupových práv a zabezpečené prostředí a tedy pouze cesty, přes který mají uživatelé pracují s tímto zdrojem back-end.

Řešení je vložit Azure RemoteApp prostředí a zabezpečené zdroje v stejné Azure virtuální síti (VNET). Pokud je zdroj v na jiný web, můžete vytvořit připojení VPN k webu, například vytvořit VNet trvající Azure datového centra a místního prostředí zákazníků.

Azure RemoteApp podporuje dva typy kolekce nasazení kterého můžete přidat vlastní VNET:

-   Non doméně: aplikace bude mít "řádku pohledu" z jiných zdrojů v VNET. Například to mohou sloužit k připojení aplikace k SQL databázi, která používá ověřování serveru SQL (aplikací ověření uživatele přímo v databázi)

-   Doméně: virtuálních počítačích používaný Azure RemoteApp připojeni k řadiče domény v VNET. To je užitečná, když aplikace muset ověřování Windows řadiče domény za účelem získání přístupu ke zdroji back-end.
![Kolekce doméně v Azure RemoteApp](./media/remoteapp-secureaccess/ra-domainjoined.png)
 
### <a name="how-to-create-a-secure-connection-between-azure-and-my-on-premises-environment"></a>Jak vytvořit zabezpečené připojení mezi místním prostředím a Azure
Existuje několik možností konfigurace připojení Azure a místních prostředí. Užitečný přehled možností je k dispozici.

S Azure RemoteApp musíte nejdřív konfigurace vaší VNet a použijte ji během vytváření kolekce webů. 

## <a name="the-complete-solution"></a>Úplné řešení
Následující obrázek ukazuje úplné řešení, které jsme jsou součástí kanálu zabezpečený přístup z koncového uživatele prostřednictvím RemoteApp Azure (ARA) zdroje back-end.
![Zabezpečené Azure RemoteApp](./media/remoteapp-secureaccess/ra-secureoverview.png) ve fázi 1 vybraným uživatelům jsme vytvořili pravidla přístupu, které určují způsob, jak lze získat přístup ARA. V následujícím příkladu jsme pouze povolit přístup uživatelé pracují mimo podnikovou síť. Vyhovujících zápisu než uživatelé nebudou moct přistupovat k prostředí ARA vůbec.
V "Fázi 2" jsme vystavili zdroje back-end pouze prostřednictvím konfigurace VNet/VPN, které máme kontrolovat. Azure RemoteApp je uložená ve stejném VNet. V konečném důsledku je, že zdroje můžete jenom k nim získat přístup prostřednictvím prostředí ARA.


