<properties
   pageTitle="Historie verzí vydání spojnice | Microsoft Azure"
   description="Toto téma obsahuje seznam všech verzích spojnice Forefront Identity správce FIM () a Microsoft Identity správce (MIM)"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/17/2016"
   ms.author="billmath"/>

# <a name="connector-version-release-history"></a>Historie verzí vydání spojnice
Spojnice Forefront Identity správce FIM () a Microsoft Identity správce (MIM) se automaticky aktualizují často.

>[AZURE.NOTE]
Toto téma je FIM a MIM pouze. V Azure AD Connect nejsou podporovány tyto spojnice.

Toto téma seznam všech verzích vydané spojnice.

Související odkazy:

- [Stáhněte si nejnovější spojnic](http://go.microsoft.com/fwlink/?LinkId=717495)
- [Obecný spojnice LDAP](active-directory-aadconnectsync-connector-genericldap.md) dokumentaci
- Si přečtěte následující dokumentaci odkaz [Obecný spojnice SQL](active-directory-aadconnectsync-connector-genericsql.md)
- [Konektor služby Web](http://go.microsoft.com/fwlink/?LinkID=226245) si přečtěte následující dokumentaci odkaz
- Si přečtěte následující dokumentaci odkaz [Prostředí PowerShell spojnice](active-directory-aadconnectsync-connector-powershell.md)
- Si přečtěte následující dokumentaci odkaz [Lotus Domino spojnice](active-directory-aadconnectsync-connector-domino.md)

## <a name="111170"></a>1.1.117.0
Vydání: 2016 březen

**Nové spojnice**  
Původní verze [Obecný SQL spojnice](active-directory-aadconnectsync-connector-genericsql.md).

**Nové funkce:**

- Konektor obecný LDAP:
    - Přidaná podpora pro import delta s Isode.
- Konektor služby Web:
    - Aktualizace csEntryChangeResult aktivity a aktivity setImportErrorCode povolit objekt vyrovnat chyby vrátit zpět do modul synchronizace.
    - Aktualizace SAP6 a SAP6User šablony pro použití nových funkcí úrovně chyby objektu.
- Konektor Domino Lotus:
    - Ve exportovat třeba jeden certifier za adresář. Teď můžete stejné heslo pro všechny udělení licence certifikátorům usnadnit řízení.

**Pevné problémy:**

- Konektor obecný LDAP:
    - Některé atributy odkaz Pošta můžete IBM nebyly zjištěné správně.
    - Pro otevřít LDAP při importu delta byly zkráceny prázdné znaky na začátku a konce řetězce.
    - Pro Novell a NetIQ přejmenovat exportovat, které přesunout objekt mezi organizačních jednotek/kontejnery a současně objekt se nezdařila.
- Konektor služby Web:
    - Pokud webová služba měli více koncové pro stejný vazbu, klikněte na spojnici není Seznamte se s správně tyto koncové.
- Konektor Domino Lotus:
    - Export atribut fullName k databázi pošty nefungují.
    - Export, které přidat i odebrat člena ze skupiny exportovány pouze přidané členy.
    - Pokud je dokument poznámky neplatný (isValid atribut nastavena na hodnotu false) a potom spojnici dojde k chybě.

## <a name="older-releases"></a>Starší verze
Před 2016 březnu fungovat vydané spojnice jako témata podpory pro.

**Obecný LDAP**

- [KB3078617](https://support.microsoft.com/kb/3078617) - 1.0.0597 září 2015
- [KB3044896](https://support.microsoft.com/kb/3044896) - 1.0.0549, březen 2015
- [KB3031009](https://support.microsoft.com/kb/3031009) - 1.0.0534 ledna 2015
- [KB3008177](https://support.microsoft.com/kb/3008177) - 1.0.0419 září 2014
- [KB2936070](https://support.microsoft.com/kb/2936070) - 4.3.1082, březen 2014

**WebServices**

- [KB3008178](https://support.microsoft.com/kb/3008178) - 1.0.0419 září 2014

**Prostředí PowerShell**

- [KB3008179](https://support.microsoft.com/kb/3008179) - 1.0.0419 září 2014

**Lotus Domino**

- [KB3096533](https://support.microsoft.com/kb/3096533) - 1.0.0597 září 2015
- [KB3044895](https://support.microsoft.com/kb/3044895) - 1.0.0549, březen 2015
- [KB2977286](https://support.microsoft.com/kb/2977286) - 5.3.0712, srpen 2014
- [KB2932635](https://support.microsoft.com/kb/2932635) - 5.3.1003, únor 2014  
- [KB2899874](https://support.microsoft.com/kb/2899874) - 5.3.0721, říjen 2013
- [KB2875551](https://support.microsoft.com/kb/2875551) - 5.3.0534, srpen 2013

## <a name="next-steps"></a>Další kroky
Další informace o konfiguraci [Azure AD Connect synchronizovat](active-directory-aadconnectsync-whatis.md) .

Další informace o [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
