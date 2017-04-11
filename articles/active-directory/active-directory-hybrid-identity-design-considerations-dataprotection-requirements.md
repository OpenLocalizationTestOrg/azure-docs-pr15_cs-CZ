<properties
    pageTitle="Azure Active Directory hybridní identity navrhování - určit požadavky na ochranu dat | Microsoft Azure"
    description="Při plánování řešení totožnost hybridní popsána požadavky na ochranu dat pro firmy a jaké možnosti jsou k dispozici nejlépe splňovat tyto požadavky."
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

#<a name="plan-for-enhancing-data-security-through-strong-identity-solution"></a>Plán pro lepší zabezpečení dat pomocí řešení silných identity

Cílem prvního kroku k ochraně dat je určit, kdo má přístup k tato data a v rámci tohoto procesu, musíte mít identitu řešení, které můžete lze integrovat s systému a poskytuje funkce a tak mohli ověřovat. A mohli ověřovat často zaměňovány s překrývajícími a jejich role nesprávně pochopeny. Ve skutečnosti jsou velmi liší, jak je znázorněno na následujícím obrázku:

![](./media/hybrid-id-design-considerations/mobile-devicemgt-lifecycle.png)
 
**Fází životního cyklu správy mobilních zařízení**

Při plánování řešení totožnost hybridní je třeba porozumět požadavky na ochranu dat pro firmy a jaké možnosti jsou k dispozici nejlépe splňovat tyto požadavky.
 
>[AZURE.NOTE]
Po dokončení plánování zabezpečení dat, prohlédněte si [určit vícefaktorové ověřování požadavky](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md) zajistit, že zvolíte týkající se požadavků na vícefaktorové ověřování nebyly ovlivňují rozhodnutí, které jste udělali v této části.

## <a name="determine-data-protection-requirements"></a>Zjistit, požadavky ochrany dat
V době mobilita většina společností má společné cíle: umožnit uživatelům mohli zvýšení produktivity být produktivní na mobilních zařízeních během místní nebo vzdálené odkudkoli. Když to může být společné cíle, společnosti, která má tyto požadavky by vás týkají ohledně množství hrozeb, které musí být zmírnit k zabezpečení dat společnosti a zachování soukromí uživatele. Každou společnost může být různé požadavky v tomto ohledu; různé dodržování předpisů pravidla, která se budou lišit podle toho, jaký odvětví společnosti funguje vede k jiné navrhování. 

Jsou tu ale i některé zabezpečení aspekty, které mají být prozkoumat a ověřit, bez ohledu na odvětví, které jsou vysvětleny v další části.

## <a name="data-protection-paths"></a>Cesty ochrana dat

![](./media/hybrid-id-design-considerations/data-protection-paths.png)
 
**Cesty ochrana dat**

V diagramu budou komponentu identity první z nich k ověření dat se před přístupem k. Ale tato data může být v různých státech po dobu, kdy byl přístupný. Každé číslo na tohoto diagramu představuje cestu, ve kterém může být data uložena nastane okamžik v čase. Níže jsou vysvětleny tyto hodnoty:

1. Ochrana dat na úrovni zařízení.
2. Ochrana dat na cestě.
3. Ochrana dat u ostatních místní.
4. Ochrana dat v klidu v cloudu.

Ačkoli technické řídí se bude povolit IT chránit samotná data o každém z těchto fází nejsou nabízeny přímo řešení totožnost hybridní je nezbytné, aby hybridní identity řešení může využívání místním prostředím a cloudu identity správy prostředky k identifikaci uživatele před udělit přístup k datům. Při plánování řešení totožnost hybridní zajistit, že jsou podle požadavky organizace odpovědět na následující otázky:

## <a name="data-protection-at-rest"></a>Ochrana dat na ostatní
Bez ohledu na to, kde se data v klidu (zařízení, cloudu a místní) je důležité provádět vyhodnocení v tomto ohledu pochopit potřeb organizace. Tato oblast zajistit, že se zobrazí dotaz na následující otázky:

- Vaše společnost musím chránit data na ostatních?
 - Pokud ano, hybridní identity řešením je integrovat s aktuální místní infrastrukturu?
 - Pokud ano, hybridní identity řešením je integrovat se vaše úlohami umístěné v cloudu?
- Je Správa identit cloudu nemůže zajistit ochranu přihlašovací údaje uživatele a dalších dat uložených v cloudu?

## <a name="data-protection-in-transit"></a>Ochrana dat při přenosu šifrovaná
Musí být chráněny data při přenosu šifrovaná mezi zařízení a datacentra nebo zařízení a cloudu. Ale právě na cestě neznamená procesu komunikaci s komponentu mimo vaše cloudové služby, Přesune interně taky, například mezi dvěma virtuální sítě. Tato oblast zajistit, že se zobrazí dotaz na následující otázky:

- Vaše společnost musím chránit data při přenosu šifrovaná?
 - Pokud ano, hybridní identity řešením je integrovat s zabezpečené ovládacími prvky, například SSL/TLS?
- Správa identit cloudu zachovat přenosy a v úložišti adresáře (v rámci a mezi datacentrech) přihlášení?


## <a name="compliance"></a>Dodržování předpisů
Právní, právních a regulačních předpisů se liší podle odvětví, do kterého patří vaší společnosti. Společnosti v vysoké regulovaném odvětví musí adresu Správa identit pochybnosti týkající se dodržování předpisů problémy. Právní jako je Sarbanes Oxleyův (SOX), přenosnost stavu pojištění zodpovědnost zákona HIPAA (), Act Gramm Leach Bliley (GLBA) a platební kartou oborovému dat zabezpečení Standardní (PCI rozhodování) jsou velmi striktně týkající se identity a přístup. Řešení identit hybridní, které se převezme vaší společnosti může mít základní funkce splňující požadavky jednoho nebo víc z těchto pravidel. Tato oblast zajistit, že se zobrazí dotaz na následující otázky:

- Hybridní identity řešením je kompatibilní s zákonných požadavků pro svoji firmu?
- Řešení identit hybridní má součástí funkcí, které vám umožní vaší společnosti, aby byl kompatibilní se standardem zákonných požadavků? 
 
>[AZURE.NOTE]
Ujistěte se, pořizovat poznámky každá odpověď a interpretaci důvodech odpověď. [Definování strategie ochranu dat](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) budou přesměrovány na tlačítko Možnosti a výhody a nevýhody jednotlivých možností.  Tak, že máte zodpovězené otázek, kterou vybereme kterou nejlepší možnost se hodí vyhovovaly potřebuje.

## <a name="next-steps"></a>Další kroky
 [Určit požadavky na správu obsahu](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)


## <a name="see-also"></a>Viz taky
[Přehled aspektech návrhu](active-directory-hybrid-identity-design-considerations-overview.md)
