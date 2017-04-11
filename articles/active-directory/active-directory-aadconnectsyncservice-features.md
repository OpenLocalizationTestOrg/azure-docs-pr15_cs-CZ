<properties
    pageTitle="Funkce služby Azure AD Connect synchronizace a konfigurace | Microsoft Azure"
    description="Popisuje funkce straně služba služby Azure AD Connect synchronizace."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="andkjell;markvi"/>

# <a name="azure-ad-connect-sync-service-features"></a>Azure AD Connect synchronizační služba funkce.

Funkce synchronizace Azure AD Connect má dvě složky:

- Místní součásti s názvem **synchronizace Azure AD Connect**, neboli **sync engine**.
- Služba umístěných v Azure AD označovaná taky jako **synchronizace služby Azure AD Connect**

Toto téma vysvětluje, jak tyto funkce **synchronizace služby Azure AD Connect** a konfiguraci aplikace je pomocí Windows Powershellu.

Tato nastavení jsou nakonfigurované tak, že [Modul Azure Active Directory pro Windows PowerShell](http://aka.ms/aadposh). Stáhněte si a nainstalujte odděleně od Azure AD Connect. Rutiny pro uvedených v tomto tématu byla zavedená v [2016 březen uvolněte (build 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1). Pokud máte rutiny uvedených v tomto tématu nebo jejich nevytvoří stejný výsledek, ujistěte se, že spustíte nejnovější verzi.

Konfigurace v adresáři Azure AD zobrazíte spustit `Get-MsolDirSyncFeatures`.  
![Get-MsolDirSyncFeatures výsledek](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)

Většina nastavení mohou pouze měnit Azure AD Connect.

Tato nastavení můžete nakonfigurovat tak, že `Set-MsolDirSyncFeature`:

DirSyncFeature | Komentář
--- | ---
[DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) | Umožňuje atributu být uložená v karanténě po duplicitní jiného objektu spíše než celý objekt selhání při exportu.
[EnableSoftMatchOnUpn](#userprincipalname-soft-match) | Umožňuje objekty spojení se nepoužijí userPrincipalName kromě primární SMTP adresu.
[SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) | Umožňuje modul Synchronizace aktualizovat atributu userPrincipalName spravovat licence uživatelů (nefederované).

Po povolení funkce se nedá zakázat znovu.

>[AZURE.NOTE] Z 24 srpen 2016 funkci *Duplikovat atribut odolnost proti chybám* je standardně pro nové Azure AD adresáře. Tato funkce se taky chránění a zapnuta adresáře vytvořené před tímto datem. Obdržíte e-mailové oznámení při adresáři se brzo získat tuto funkci povolit.

Následující nastavení nakonfigurované tak, že Azure AD Connect a nelze změnit tak, že `Set-MsolDirSyncFeature`:

DirSyncFeature | Komentář
--- | ---
DeviceWriteback | [Azure AD Connect: Povolení zpětného zápisu zařízení](active-directory-aadconnect-feature-device-writeback.md)
DirectoryExtensions | [Azure AD Connect synchronizace: rozšíření Directory](active-directory-aadconnectsync-feature-directory-extensions.md)
PasswordSync | [Provádění synchronizace hesel se synchronizací Azure AD Connect](active-directory-aadconnectsync-implement-password-synchronization.md)
UnifiedGroupWriteback | [Náhled: Skupina zpětného zápisu](active-directory-aadconnect-feature-preview.md#group-writeback)
UserWriteback | Aktuálně podporované.

## <a name="duplicate-attribute-resiliency"></a>Duplikování odolnost proti chybám atribut
Místo selhání zřízení objekty s duplicitními UPN / proxyAddresses, duplicitní atribut je "v karanténě" a dočasné hodnota je přiřazena. Po vyřešení konfliktů dočasné Přípony se automaticky změní na správnou hodnotu. Toto chování může být užitečné pro UPN a proxyAddress samostatně. Další podrobnosti najdete v tématu [synchronizace identit a duplicitní atribut odolnost](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).

## <a name="userprincipalname-soft-match"></a>Měkké POZVYHLEDAT UserPrincipalName
Pokud tato funkce je povoleno, POZVYHLEDAT měkkých aktivované řešení UPN kromě [primární SMTP adresu](https://support.microsoft.com/kb/2641663), která je vždy povolen. Měkkých shody se používá vyhovovat stávajícím uživatelům cloudu v Azure AD místních uživatelů.

V případě potřeby POZVYHLEDAT místní účty AD s existujícími účty vytvořené v cloudu a nepoužíváte Exchange Online, bude tato funkce je užitečná. V tomto scénáři obecně nemáte důvod, proč se nastavit atribut SMTP v cloudu.

Tato funkce je na ve výchozím nastavení pro nově vytvořený Azure AD adresáře. Si můžete prohlédnout, pokud je tato funkce zapnutá za vás spuštěním:  
```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

Pokud tato funkce není aktivní Azure AD adresáře, můžete ji povolit spuštěním:  
```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a>Synchronizovat aktualizace userPrincipalName
Z historických důvodů aktualizace atributu UserPrincipalName pomocí služby synchronizace z místního zablokovaný, pokud jsou splněné obě následující podmínky:

- Uživatel se spravuje (nefederované).
- Uživatel nemá přiřazenou licenci.

Další informace najdete v tématu [uživatelská jména v Office 365, Azure nebo Intune se neshodují místních hlavních názvů uživatelů nebo alternativní přihlašovací ID](https://support.microsoft.com/kb/2523192).

Povolení tato funkce umožňuje modul synchronizace k aktualizaci userPrincipalName, když se změněné místně a používáte synchronizaci hesel. Pokud používáte federace, tato funkce není podporována.

Tato funkce je na ve výchozím nastavení pro nově vytvořený Azure AD adresáře. Si můžete prohlédnout, pokud je tato funkce zapnutá za vás spuštěním:  
```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

Pokud tato funkce není aktivní Azure AD adresáře, můžete ji povolit spuštěním:  
```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

Po povolení této funkce existujících hodnot userPrincipalName zůstane jako-je. Na další změnu userPrincipalName atribut v místním nasazení synchronizace normální delta na uživatele se aktualizuje s UPN.  

## <a name="see-also"></a>Viz taky

- [Synchronizace Azure AD Connect](active-directory-aadconnectsync-whatis.md)
- [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
