<properties
    pageTitle="Typy rizika události zjištěné ochranou Azure Active Directory Identity | Microsoft Azure"
    description="V tomto tématu najdete podrobné základní informace o dostupných typů rizikové událostí v Azure Active Directory Identity Protection"
    services="active-directory"
    keywords="Ochrana identity služby Azure active directory, zjišťování aplikace cloudu, Správa aplikací, zabezpečení, rizika, úroveň rizika, chyba, zásady zabezpečení"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="markvi"/>

#<a name="types-of-risk-events-detected-by-azure-active-directory-identity-protection"></a>Typy rizika události zjištěné ochranou Azure Active Directory Identity 

V Azure Active Directory Identity ochrany události rizika, které jsou události, která:

- byly označena za podezřelou

- Označuje, že identitu může mít byla ohroženo. 

Toto téma obsahuje podrobný přehled dostupných typů rizikové události.


## <a name="leaked-credentials"></a>Prozrazený přihlašovací údaje

Prozrazený přihlašovací údaje jsou k dispozici vystavil veřejně v tmavě webu výzkumných zabezpečení Microsoft. Tyto údaje jsou obvykle umístěny ve formátu prostého textu. Porovnáván Azure AD pověření a pokud existuje shoda, jsou hlášené jako "Leaked přihlašovací údaje" v ochrana Identity.

Prozrazený přihlašovací údaje, které rizikové události jsou klasifikované jako "Vysoká" závažnosti riziková událost, protože poskytují viditelně uživatelské jméno a heslo jsou k dispozici pro se zlými úmysly.

## <a name="impossible-travel-to-atypical-locations"></a>Možné cestovní atypické umístění

Tento typ události rizika identifikuje dva sign in pocházející z geograficky vzdálených míst, kde alespoň jedno z umístění možná by vás atypické pro uživatele, vzhledem dřívější chování. Kromě toho je časový interval mezi dva sign in kratší než času, které byste měli mít pořídili uživateli přecházíte z první umístění sekundu označující, že jiný uživatel používá stejné přihlašovací údaje. 

Tento počítač výukové algoritmus ignoruje zjevných "*falešně pozitivní*" usnadnit možné cestovní podmínky, například virtuální privátní sítě a umístění pravidelně používá jinými uživateli v organizaci.  Systém má počáteční výukové dobu 14 dní během nichž učí nového uživatele přihlašovací chování.

Možné cestovní bývá dobré ukazatele, který hacker bylo možno úspěšně přihlášení. NEPRAVDA pozitivní však může dojít při uživatele je cestování pomocí nového zařízení nebo prostřednictvím sítě VPN, které obvykle nepoužívá ostatním uživatelům v organizaci. Jiný zdroj NEPRAVDA pozitivní je aplikace, které nesprávně předejte serveru IP adresy jako klienta IP adresy, která může obsahovat vzhled sign in je hostovaný probíhají v datovém centru, které tuto aplikaci je back-end (často jsou to Microsoft datacentrech, které může poskytnout vzhledu přihlášení přijímat umístěte od Microsoftu vlastní IP adresy). Úroveň rizika pro tento riziková událost důsledku tyto pozitivní NEPRAVDA, je "**Střední**".

##<a name="sign-ins-from-infected-devices"></a>Přihlášení z infikovaných zařízení

Tento typ události rizika identifikuje přihlášení z zařízení infekce s malware, která je známo, komunikovat aktivně bot serveru. To je určen korelace IP adresy zařízení uživatele před IP adresy, které byly kontaktu s bot serveru. 

Riziková událost identifikuje IP adres není zařízeních. Když několik zařízení za jeden IP adresa a jenom některé jsou řídil síti bot přihlášení na jiných zařízeních Moje aktivační událost Tato událost zbytečně, tedy důvod, proč pro klasifikaci tento riziková událost jako "**nízké**".  

Doporučujeme, abyste Kontaktujte uživatele a prohledat všechny uživatele zařízení. Je také možné, že infekce uživatele osobním zařízením, nebo jak jsme zmínili dříve, tento někdo používal infikovaných zařízení ze stejného IP adresu jako uživatel. Infikovaných zařízení jsou často infekce tak, že malware, které nebyly dosud označena antivirový software a může také nastavit jako zvyky chybná uživatele, které může být příčinou zařízení nasazení.

Další informace o tom, jak adresu malware infekce naleznete v článku [Centrum ochrany proti malwaru](http://go.microsoft.com/fwlink/?linkid=335773&clcid=0x409).


## <a name="sign-ins-from-anonymous-ip-addresses"></a>Přihlášení z anonymní IP adres

Tento typ události rizika identifikuje uživatele, kteří mají úspěšně přihlášení z IP adresu, který byl zjištěn jako anonymní proxy IP adresu. Tyto proxy servery jsou používány osoby, které chcete skrýt IP adresu svého zařízení a mohou být použity pro lidi se zlými úmysly.

Doporučujeme okamžitě kontaktujte uživateli, abyste ověřili, pokud ho používali anonymní IP adres. Úroveň rizika pro tento typ události rizika totiž "**Medium**" sám o sobě anonymní IP není silných označení narušení zabezpečení účtu.

## <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Přihlášení z IP adres pomocí podezřelé aktivity

Tento typ události rizika identifikuje IP adresy, ze kterých vysoký počet neúspěšných pokusů o přihlášení se zobrazily, přes víc uživatelských účtů přes krátký časový úsek. Odpovídá přenosy vzorků IP adresy používané útočníků a je silných indikátor, že účty jsou už nebo mají být. Toto je počítač výukové algoritmus, který ignoruje zjevných "*false pozitivní*", například IP adresy, které se používají pravidelně jinými uživateli v organizaci.  Systém má počáteční výukové dobu 14 dní kde učí přihlašovací chování nového uživatele a nového klienta.

Doporučujeme obrátit se uživateli, abyste ověřili, pokud jsou ve skutečnosti přihlášení z IP adresy, která je označená jako podezřelé. Úroveň rizika pro tento typ události je "**Medium**", protože několik zařízení může být za stejnou adresou, zatímco pouze můžou být některé zodpovědný za podezřelou aktivity. 


## <a name="sign-in-from-unfamiliar-locations"></a>Přihlášení z cizí umístění

Tento typ události rizik je mechanismus v reálném čase přihlašovací hodnocení, které považuje za posledních přihlašovací umístění (IP, šířky a délky a ASN) určit umístění nové / cizí. Systém informacemi o předchozí umístění používané uživatele a byly použity tyto "známé" umístění. Riziko i se při spuštění aktivují dojde přihlášení z oblasti, která není v seznamu známé umístění. Systém má počáteční výukové dobu 14 dní, po které ho neoznačí nové umístění jako cizí umístění. Systém také ignoruje přihlášení z známé zařízení a umístění, které jsou zeměpisně zavřít známé umístění. <br>
Cizí umístění můžete poskytnout silných údaj, který může se zlými úmysly pokusíte použít krádež identity. NEPRAVDA pozitivní může dojít při uživatele je cestování vyzkoušet nové zařízení nebo použití nového VPN. Úroveň rizika pro tento typ události důsledku tyto falešně pozitivní, je "**Střední**".


## <a name="azure-ad-anomalous-activity-reports"></a>Sestavy Azure AD neobvyklých aktivit

Některé z těchto událostí rizika byly dostupné prostřednictvím sestavy Azure AD neobvyklých aktivit Azure portálu. Následující tabulka uvádí různé typy událostí rizika a odpovídající sestava **Azure AD neobvyklých činností** . Společnost Microsoft pokračuje investovat do tohoto místa a plány nepřetržitě zlepšit přesnost rozpoznávání existující zvláštní události rizika a přidat nové události typy rizika průběžné. 



| Typ události rizika ochranu identity | Odpovídající Azure AD sestava neobvyklých činností: |
| :-- | :-- |
| Prozrazený přihlašovací údaje    | Uživatelé, kteří mají prozrazený přihlašovací údaje |
| Možné cestovní atypické umístění | Aktivity nepravidelné přihlášení |
| Přihlášení z infikovaných zařízení    | Přihlášení z možného infikovaných zařízení |
| Přihlášení z anonymní IP adres  | Přihlášení z neznámých zdrojů |
| Přihlášení z IP adres pomocí podezřelé aktivity | Přihlášení z IP adres pomocí podezřelé aktivity |
| Znaky v cizí umístění    | - |
| Uzamčení události    | - |

Následující sestavy Azure AD neobvyklých aktivity nejsou zahrnuty jako rizikové událostí v ochrana Azure AD Identity a tedy nebudou dostupné prostřednictvím ochrana Identity. Tyto sestavy jsou pořád dostupné v portálu Azure, ale budou se být zastaralý někdy v budoucnosti jako jsou právě nahrazeny rizika událostí v ochrana Identity.

- Přihlášení za k více chybám
- Přihlášení z různých zeměpisných


## <a name="see-also"></a>Viz taky

- [Ochrana Identity Azure Active Directory](active-directory-identityprotection.md)


