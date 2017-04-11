<properties
    pageTitle="Azure AD Connect: Podporovaných topologií | Microsoft Azure"
    description="Toto téma obsahuje podrobnosti podporované a nepodporované topologií Azure AD Connect"
    services="active-directory"
    documentationCenter=""
    authors="AndKjell"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="billmath"/>

# <a name="topologies-for-azure-ad-connect"></a>Topologie pro Azure AD připojení
Cílem toto téma je popisuje různé místním prostředím a Azure AD topologií se synchronizací Azure AD Connect jako klíčové integrace řešení. Popsány podporované a nepodporované konfigurace.

Pokud chcete legendu pro obrázky v dokumentu:

Popis | Ikona
-----|-----
Struktura místní služby Active Directory| ![AD](./media/active-directory-aadconnect-topologies/LegendAD1.png)
Služby Active Directory filtrované importu| ![AD](./media/active-directory-aadconnect-topologies/LegendAD2.png)
Synchronizace server Azure AD Connect| ![Synchronizace](./media/active-directory-aadconnect-topologies/LegendSync1.png)
Azure AD Connect synchronizace "pracovní režimu serveru"| ![Synchronizace](./media/active-directory-aadconnect-topologies/LegendSync2.png)
GALSync s FIM2010 nebo MIM2016| ![Synchronizace](./media/active-directory-aadconnect-topologies/LegendSync3.png)
Azure AD Connect synchronizace serveru, podrobné| ![Synchronizace](./media/active-directory-aadconnect-topologies/LegendSync4.png)
Azure AD adresáře |![AAD](./media/active-directory-aadconnect-topologies/LegendAAD.png)
Nepodporované scénáře | ![Nepodporované](./media/active-directory-aadconnect-topologies/LegendUnsupported.png)

## <a name="single-forest-single-azure-ad-tenant"></a>Jeden struktury, jedna Azure AD klienta
![Jednoduchá doménové jednoho klienta](./media/active-directory-aadconnect-topologies/SingleForestSingleDirectory.png)

Nejběžnější topologie je strukturu jednoho tenanta místním s jednu nebo víc domén a jeden Azure AD. Ověřování Azure AD synchronizace hesel používá. Expresní instalace Azure AD Connect podporuje pouze topologie.

### <a name="single-forest-multiple-sync-servers-to-one-azure-ad-tenant"></a>Jeden struktury, více synchronizace servery na jednoho klienta Azure AD
![Jednoduchá doménové filtrované nepodporované](./media/active-directory-aadconnect-topologies/SingleForestFilteredUnsupported.png)

Není podporována pro připojení k stejném klientovi Azure AD, s výjimkou [přípravu serveru](#staging-server)na více Azure AD Connect synchronizace serverech. Nepodporované je i v případě, že tyto jsou nakonfigurované pro synchronizaci vzájemně se vylučujících sady objektů. Se může mít za to pokud nebudete mít přístup všech domén ve struktuře z jednoho serveru nebo k distribuci zatížení napříč několika serveru.

## <a name="multiple-forests-single-azure-ad-tenant"></a>Více strukturami, jeden Azure AD klienta
![Více doménové jednoho klienta](./media/active-directory-aadconnect-topologies/MultiForestSingleDirectory.png)

Mnoho organizací má prostředí s více struktur místní služby Active Directory. Máte víc než jeden doménové místní služby Active Directory různých důvodů. Typické příklady vzorů s strukturami účtu zdroje a jako výsledek po fúze nebo WIA.

Pokud máte víc strukturami, všechny strukturami musí být dostupný jednoho Azure AD Connect synchronizace serveru. Nemáte připojení serveru pro doménu. V případě potřeby kontaktovat všechny doménové mohou být umístěny serveru v síti DMZ.

Průvodce instalací Azure AD Connect nabízí několik možností pro sloučení uživatelů ve více strukturami. Cílem je, že uživatel reprezentován pouze jednou v Azure AD. Existují některé běžné topologií, které můžete nakonfigurovat ve vlastní cesty instalace v Průvodci instalací. Vyberte odpovídající možnost představující topologii na stránce **jedinečně identifikující uživatelů**. Sloučení nakonfigurovaný jenom pro uživatele. Duplicitní skupiny nejsou konsolidované s nastavením výchozích.

V následující části jsou popsány běžné topologií: [samostatné topologií](#multiple-forests-separate-topologies), [úplné](#multiple-forests-full-mesh-with-optional-galsync)a [Účet zdroje](#multiple-forests-account-resource-forest).

Výchozí konfigurace v Azure AD Connect synchronizovat předpokládá:

1. Uživatelé musí mít jenom jeden účet povolené a struktury, kde je umístěn tento účet se používá k ověření uživatele. Tento předpoklad je pro obě synchronizaci hesel a pro federaci. UserPrincipalName a sourceAnchor/immutableID pocházet z této struktuře.
2. Uživatelé mají jenom jedna poštovní schránka.
3. Struktura, který je hostitelem poštovní schránky uživatele má nejlepší kvalitu dat atributy viditelné v na Exchange globální seznam adres. Pokud tam je žádné poštovní schránky na uživatele, pak všechny doménové mohou sloužit k přispívat tyto hodnoty atributu.
4. Pokud máte propojené poštovní schránky, pak se rovněž jiného účtu v jiné struktuře použít k přihlášení.

Pokud vaše prostředí neodpovídá tyto předpoklady, následující akce:

- Pokud máte víc účtů aktivní nebo více než jedna poštovní schránka, modul Synchronizace zvolí jednu a druhý ignorovat.
- Propojené poštovní schránka se žádný aktivní účet není exportován Azure AD. Uživatelský účet není vyjádřených jako člen ve všech skupinách. Propojené poštovní schránky v DirSync byste vždy vyjádřených jako normální poštovní schránky. Tato změna úmyslně chování je odlišné pro lepší podporu více doménové scénáře.

Další informace najdete v [Principy konfigurační výchozí](active-directory-aadconnectsync-understanding-default-configuration.md).

### <a name="multiple-forests-multiple-sync-servers-to-one-azure-ad-tenant"></a>Více strukturami, více synchronizace servery na jednoho klienta Azure AD
![Synchronizace s víc doménové více nepodporuje](./media/active-directory-aadconnect-topologies/MultiForestMultiSyncUnsupported.png)

Není podporovaná mít víc než jednu server Azure AD Sync připojení připojen k typu single Azure AD klienta. Výjimky je používání [přípravu serveru](#staging-server).

### <a name="multiple-forests--separate-topologies"></a>Více strukturami – samostatné topologií
**Uživatelé představují pouze jednou přes všechny adresáře**

![Jednou doménové s víc uživatelů](./media/active-directory-aadconnect-topologies/MultiForestUsersOnce.png)

![Doménové s víc samostatných discích topologií](./media/active-directory-aadconnect-topologies/MultiForestSeperateTopologies.png)

V tomto prostředí všechny strukturami místní jsou zpracovány jako samostatný entity a žádní uživatelé by měly tvořit v jiných doménové.
Jednotlivé domény obsahuje vlastní organizaci využívající Exchange a to není žádný GALSync mezi strukturami. Tato topologie může být situace po fúze/pořízení nebo v organizaci, kde každý organizační jednotka pracuje samostatný od sebe. Tyto strukturami jsou ve stejné organizaci využívající v Azure AD a zobrazí se jednotné GAL.
Na tomto obrázku všech objektů v každé doménové reprezentovat jednou v metaverse a v cílovém Azure AD klientovi agregované.

### <a name="multiple-forests--match-users"></a>Více strukturami – POZVYHLEDAT uživatelů
**Identity uživatele, které napříč několika adresáře**

Pro všechny podobnému sledu platí, že rozdělení a skupiny zabezpečení může obsahovat kombinaci uživatele, kontakty a FSP (cizí objekty zabezpečení)

FSP se používají v přidá představující členové z dalších doménových ve skupině zabezpečení. Všechny FSP jsou na skutečný objekt v Azure AD.

### <a name="multiple-forests--full-mesh-with-optional-galsync"></a>Více strukturami – úplné oka s volitelné GALSync
**Identity uživatele, které napříč několika adresáře. Odpovídají pomocí: atribut pošty**

![Pošty doménové s víc uživatelů](./media/active-directory-aadconnect-topologies/MultiForestUsersMail.png)

![Více doménové úplné mřížky](./media/active-directory-aadconnect-topologies/MultiForestFullMesh.png)

Topologie úplné mřížky umožňuje uživatelům a zdroje nacházet v libovolné doménové a běžně bude mezi dvěma stranami vztahy mezi strukturami.

Pokud je k dispozici v víc doménové Exchange, může volitelně být řešení GALSync místní. Každý uživatel by vyjádřených jako kontakt v dalších doménových. GALSync je běžně implementovaná pomocí účtu správce identit Forefront 2010 nebo Microsoft správce identit 2016. Azure AD Connect není možné použít pro místní GALSync.

V tomto scénáři objekty identity, jsou spojeny ke pomocí atribut pošta. Uživatel s poštovní schránkou v jedné doménové je připojen s kontakty v dalších doménových.

### <a name="multiple-forests--account-resource-forest"></a>Více strukturami – doménové účtu prostředku
**Identity uživatele, které napříč několika adresáře. Odpovídají pomocí: ObjectSID a msExchMasterAccountSID atributy**

![ObjectSID doménové s víc uživatelů](./media/active-directory-aadconnect-topologies/MultiForestUsersObjectSID.png)

![Více doménové AccountResource](./media/active-directory-aadconnect-topologies/MultiForestAccountResource.png)

V topologie doménové účtu zdroje můžete vybrat jeden nebo více strukturami účet s aktivní uživatelské účty. Máte taky jeden nebo více strukturami zdroje s zakázané účty.

V tomto scénáři jeden nebo více zástupců **doménové poskytujícího** vztahy důvěryhodnosti všechny **strukturami účtu**. Struktura zdroje má obvykle schématem rozšířené AD s Exchange a Lync. Všechny služby Exchange a Lync, jakož i ostatní sdílených služeb jsou umístěny v této struktuře. Uživatelé mají zakázaný uživatelský účet v této struktuře a poštovní schránky je propojená s struktuře účtu.

## <a name="office-365-and-topology-considerations"></a>Office 365 a co byste měli zvážit topologie
Některé úloh Office 365 potřeba podporovaných topologií určitá omezení. Pokud chcete použít jeden z těchto kroků, pak si projděte témata tématu podporovaných topologií pro pracovní zátěž.

Pracovní zátěž |  
--------- | ---------
Exchange Online | Pokud existuje více než organizace místním systémem Exchange (to znamená, Exchange má byly nasazeny do více než jeden doménové), pak je nutné použít Exchange 2013 SP1 nebo novější. Podrobnosti najdete tady: [hybridní nasazení s více struktur služby Active Directory](https://technet.microsoft.com/library/jj873754.aspx)
Skype pro firmy | Pokud používáte více strukturami místního, je podporována pouze topologie doménové účtu zdroje. Podrobnosti pro podporovaných topologií najdete tady: [klimatizace požadavky pro Server Skypu pro firmy 2015](https://technet.microsoft.com/library/dn933910.aspx)

## <a name="staging-server"></a>Pracovní serveru
![Pracovní serveru](./media/active-directory-aadconnect-topologies/MultiForestStaging.png)

Azure AD Connect podporuje nainstalovat na druhý server v **režimu pracovní**. Server v tomto režimu načte data ze všech propojených adresářích, ale není nic k zápisu připojeného adresáře. Používá cyklu normální synchronizace a proto má aktualizovanou verzi identity data. V selhání kde primární server nezdaří, je můžete převzít pracovnímu serveru. Můžete to udělat v Průvodci Azure AD Connect. Tento druhý server můžete nejlépe nachází v různých datacentra od žádná infrastruktura Server jsou sdíleny s primární server. Musíte zkopírovat ruční konfigurace změny na primárním serveru druhý server.

Přípravná serveru lze také otestovat nový vlastní konfigurace a efekt, který má na vaše data. Můžete zobrazit náhled změn a upravte konfiguraci. Až budete spokojení s novou konfiguraci, můžete vytvořit pracovní server active server a přesunutí starý active server přípravu režimu.

Tento způsob lze rovněž nahradit synchronizace s adresářem active server. Příprava nový server a jeho nastavení v pracovní režimu. Přesvědčte se, zda je v pořádku, zakáže pracovní režim (Aktivace ho) a vypnout aktivní server.

Je možné mít víc než jeden pracovní server, když budete chtít mít víc zálohy v různých datacentrech.

## <a name="multiple-azure-ad-tenants"></a>Víc klientů Azure AD
Microsoft doporučuje s jednoho klienta v Azure AD pro organizaci.
Než budete používat víc klientů Azure AD, tato témata vysvětlovat obvyklé scénáře umožňuje pomocí jednoho klienta.

Téma |  
--------- | ---------
Delegování pomocí správy jednotky | [Správa pro správu jednotky v Azure AD](active-directory-administrative-units-management.md)

![Více doménové více klienta](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectory.png)

Existuje relace 1:1 mezi server Azure AD Connect synchronizace a Azure AD klienta. U každého klienta Azure AD potřebujete jednu instalaci server Azure AD Connect synchronizace. Instance klienta Azure AD jsou záměrné samostatný a v jednom nelze uvidí uživatelé na jiném klientovi. Pokud je určená oddělení, a pak to konfigurace je podporována, ale jinak byste měli použít jednoho Azure AD klienta modelu.

### <a name="each-object-only-once-in-an-azure-ad-tenant"></a>Každý objekt jenom v klientovi Azure AD
![Jednoduchá doménové filtrované](./media/active-directory-aadconnect-topologies/SingleForestFiltered.png)

V této topologii jeden synchronizace server Azure AD Connect připojení k každého klienta Azure AD. Servery synchronizace Azure AD Connect musí být nakonfigurované pro filtrování tak, aby každý mít vzájemně se vylučujících sady objektů k ovládání na. Můžete třeba omezit každého serveru pro konkrétní doménu nebo organizační jednotku. DNS domény může být jenom registrováno v jednoduchém Azure AD klienta. UPN uživatelů v místní AD používají samostatné obory názvů. Na obrázku výše tři samostatné UPN například přípony jsou registrované v místní AD: contoso.com, fabrikam.com a wingtiptoys.com. Uživatelé v každém místního AD doménu použít jiný obor názvů.

Mezi instancemi klienta Azure AD je žádné GALsync. Adresář v Exchangi Online a Skypu pro firmy pouze uživatelé zobrazuje ve stejném klientovi.

Tato topologie má následující omezení jinak Podporované scénáře:

- Pouze jeden z Azure AD klientů můžete povolit hybridní nasazení Exchange místní služby Active Directory.
- Zařízení Windows 10 může být pouze přidružené k jednoho klienta Azure AD.

Požadavek sady vzájemně se vylučujících objektů se týká rovněž zpětného zápisu. Některé funkce zpětným nejsou podporované v této topologii vzhledem k tomu, že tyto funkce předpokládá jediná konfigurace místního:

-   Skupina zpětným s výchozí konfigurace
-   Zpětného zápisu zařízení

### <a name="each-object-multiple-times-in-an-azure-ad-tenant"></a>Jednotlivé objekty tisknutím v klientovi Azure AD
![Nepodporované jednoduchým doménové více klienta](./media/active-directory-aadconnect-topologies/SingleForestMultiDirectoryUnsupported.png) ![Nepodporované jednoduchým doménové více spojnic](./media/active-directory-aadconnect-topologies/SingleForestMultiConnectorsUnsupported.png)

- Není podporována synchronizace stejné uživateli víc klientů Azure AD.
- Není podporovaná provádět změny Pokud chcete, aby uživatelé v jedné Azure AD zobrazují jako kontaktů v jiném klientovi Azure AD konfigurace.
- Není podporována pro úpravy Azure AD Connect synchronizace pro připojení k víc klientů Azure AD.

### <a name="galsync-by-using-writeback"></a>GALsync pomocí zpětného zápisu
![MultiForestMultiDirectoryGALSync1Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync1Unsupported.png) ![MultiForestMultiDirectoryGALSync2Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync2Unsupported.png)

Azure AD klienti jsou záměrné samostatný.

- Není podporována změny konfigurace synchronizace Azure AD Connect k načtení dat z jiného klienta Azure AD.
- Nepodporované Export uživatelů mezi kontakty do jiného místní AD Azure AD Connect synchronizaci.

### <a name="galsync-with-on-premises-sync-server"></a>GALsync s místní synchronizace server
![MultiForestMultiDirectoryGALSync](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync.png)

Tuto možnost podporuje používat FIM2010/MIM2016 místním uživatelům GALsync mezi dvěma organizace Exchange. Uživatelé v jedné organizaci se zobrazí jako cizí uživatelů a kontakty v rámci organizace. Tyto služby Active Directory různých místních můžou být synchronizované pak k tenantům vlastní Azure AD.

## <a name="next-steps"></a>Další kroky
Informace o instalaci Azure AD Connect pro podobnému sledu najdete v tématu [Vlastní instalace Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md).

Další informace o konfiguraci [Azure AD Connect synchronizovat](active-directory-aadconnectsync-whatis.md) .

Další informace o [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
