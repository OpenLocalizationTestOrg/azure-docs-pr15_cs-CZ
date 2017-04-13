<properties 
   pageTitle="Azure mobilní zapojení Průvodce - nabízených/Reach odstraňováním potíží" 
   description="Uživatel interakce a oznámení o řešení problémů s v Azure Mobile zapojení" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-push-and-reach-issues"></a>Poradce při potížích s Průvodce pro nabízená a dosáhnout problémy

Následují možné problémy, ke kterým může dojít s jak zapojení Mobile Azure, odešle informace uživatelů.
 
## <a name="push-failures"></a>Nabízená k chybám

### <a name="issue"></a>Problém
- Posune nefungují (v aplikaci mimo aplikaci nebo obojí).

### <a name="causes"></a>Příčiny
- Opakovaně pokoušeli selhání nabízených údaj, že není správně integrovaná Azure Mobile zaměstnání, Reach nebo další funkce přijetí Mobile Azure nebo že upgrade je nutné v SDK známý problém s novou platformu operačního systému nebo zařízení.
- Otestujte jenom nabízená v aplikaci a jenom mimo aplikaci nabízených lze zjistit, co je problém v aplikaci nebo mimo aplikaci.
- Uživatelské rozhraní a rozhraní API v řešení potíží a zjistěte, jaké další informace o chybě je k dispozici otestujte obou míst.
- Odhlášení z aplikace nebude fungovat posune Pokud Azure Mobile zapojení a Reach jsou integrované v SDK.
- Posune nebude fungovat, pokud nejsou platné certifikáty, nebo pokud používáte výrobní porovnání vývojáře správně (pouze v iOS). (**Poznámka:** "mimo aplikaci" nabízená oznámení nelze dodat iOS, pokud máte obě vývojového (odchylka) a (výrobní) zkušební verze aplikace na stejné zařízení nainstalovaný od tokenu zabezpečení přidružený k vaší certifikátu může ukončena jejich platnost tak, že Apple. Tento problém můžete vyřešit, odinstalujte vývojáře a výrobní verze aplikace a znovu nainstalujte jenom jedna verze na zařízení s.)
- Odhlášení z aplikace nabízených počty fungují jinak v různých platformách (iOS s informacemi menší než Android pokud nativní posune jsou zakázány na zařízení, a rozhraní API můžete poskytují další informace o než v uživatelském rozhraní na stat nabízená).
- Odhlášení z aplikace mohou být posune blokovány zákazníci úrovni OS (iOS a Android).
- Odhlášení z aplikace posune se zobrazí jako zakázané v uživatelském rozhraní Azure Mobile zapojení, pokud nejsou integrované správně, ale se nemusí podařit, tiše z rozhraní API.
- V aplikaci posune nebude fungovat, pokud Azure Mobile zapojení a Reach jsou integrované v SDK.
- Posune GCM a ADM nebude fungovat, pokud nejsou Azure Mobile zaměstnání a na konkrétní server jsou integrované v SDK (pouze Android).
- V aplikaci a mimo aplikaci posune by měl otestuje samostatně k ověření, zda je nabízených nebo Reach problém.
- V aplikaci, kterou posune požadovat, aby aplikace otevřít získaná.
- V aplikaci se posune často nastavení filtrovanou podle značky informace vyjádření výslovného se změnami nebo odhlášení aplikace.
- Pokud používáte vlastní kategorii v Reach zobrazení v aplikaci oznámení, nepotřebujete sledovat správné životního cyklu oznámení, jinak nemusí zrušeno oznámení při uživatel zavřete ho.
- Pokud začnete s kampaní bez data ukončení volání a přijímání zařízení v aplikaci upozornění, ale slouží nebude zobrazovat ho ještě pořád bude uživatel dostávat oznámení o při příštím přihlášení do aplikace, i když ukončíte ručně kampaně.
- Problémy s rozhraní API nabízená potvrďte, že Opravdu chcete použít rozhraní API nabízená místo rozhraní API dosáhla (protože rozhraní API dosáhla slouží dochvilnější spoje) a že nejsou matoucí parametry "data" a "oznamovatel".
- Otestujte kampaně nabízených pomocí obou zařízení připojeného prostřednictvím Wi-Fi a 3G eliminuje síťové připojení jako zdroj možné problémy.

## <a name="push-testing"></a>Nabízená testování

### <a name="issue"></a>Problém
- Posune odesílat k určitému zařízení podle ID zařízení.

### <a name="causes"></a>Příčiny

- Test zařízení jsou nastavení jinak pro každou platformu, ale příčinou události v aplikaci na test zařízení a hledáte ID zařízení na portálu by měly fungovat k vyhledání ID zařízení pro všechny platformy.
- Test zařízení fungovat jinak IDFA porovnání IDFV (pouze v iOS).


## <a name="push-customization"></a>Přizpůsobení připínáčku

### <a name="issue"></a>Problém
- Rozšířené nabízených obsahu položky nebude fungovat (označení, vyzvánění, vibrace, obrázek, atd.).
- Odkazy ze posune nefungují (mimo aplikaci aplikace na web, na místo v aplikaci).
- Nabízená statistiky ukazují, že neodeslal push tolik lidí podle očekávání (příliš mnoho nebo není dost).
- Nabízená duplikovat nebo přijmete dvakrát.
- Nelze zaregistrovat zařízení test Azure Mobile zapojení posune (s vlastní výrobní nebo vývoj aplikací).

### <a name="causes"></a>Příčiny

- Pokud chcete vytvořit odkaz na určité místo v aplikaci vyžaduje "kategorie" (pouze Android).
- Hloubkové propojení schémata přesměrovat uživatele do jiného umístění po kliknutí na nabízená oznámení muset vytvořené v a spravuje vaše aplikace a zařízení s operačním systémem není zapojení Mobile přímo. (**Poznámka:** mimo aplikaci oznámení nelze vytvořit propojení přímo v aplikaci umístění s operačním systémem iOS mohou s Androidem.)
- Externí obrázek servery muset nebudou moct používat HTTP "GET" a "SÍDLO" pro celkového posune pracovat (pouze Android).
- Ve vašem kódu můžete zakázat agenta Azure Mobile zapojení při dalším otevření klávesnice a svůj kód znovu aktivovat agenta Azure Mobile zapojení po zavření klávesnice tak, aby klávesnice neovlivní vzhled oznámení (pouze v iOS).
- V testu simulace, ale jenom skutečné kampaní nefungují některé položky (označení, vyzvánění, vibrace, obrázek, atd.).
- Použijete-li na tlačítko "testování" posune zaznamenané žádná data straně serveru. Data jenom zaznamenávána reálné nabízených kampaní.
- Který vám pomůže určit problém, Poradce při potížích s: otestujte, simulace a s reálným kampaní vzhledem k tomu, že každé fungují trochu jinak.
- Časový interval, který vaše "v aplikaci" a "kdykoli" kampaní spuštění je naplánováno na mohou provést doručení čísla, protože s kampaní jenom do budete dostávat uživatelům, kteří jsou "v aplikaci" v průběhu kampaně (a uživatelé, kteří mají své zařízení nastavené pro příjem oznámení ve formě "mimo aplikaci").
- Rozdíly mezi jak iOS a Android zpracovat mimo oznamování v aplikaci je obtížné přímo porovnat nabízených statistiky mezi iOS a Android verzi aplikace. Další informace úroveň oznámení s operačním systémem než iOS obsahuje Android. Android sestavy při nativní oznámení je dostali, kliknutí nebo odstraněný v Centru pro oznámení, ale iOS není hlásí tyto informace, pokud po kliknutí na oznámení. 
- Hlavní důvod, proč "po" čísla jsou jiné než jiné než "Doručená" čísel pro dosažení kampaní, že "v aplikaci" a "mimo aplikaci" oznámení počítají jinak. "V aplikaci" oznámení zpracovávají zapojení Mobile, ale "mimo aplikaci" oznámení zpracovávají centra oznámení v operační systém vašeho zařízení.

## <a name="push-targeting"></a>Nabízená zacílení

### <a name="issue"></a>Problém
- Integrované zacílení nefunguje očekávaným způsobem.
- Informace o aplikaci značku zacílení nefunguje očekávaným způsobem.
- Umístění GEO zacílení nefunguje očekávaným způsobem.
- Jazykové nastavení nefungují podle očekávání.

### <a name="causes"></a>Příčiny

- Ujistěte se, že jste nahráli značky informace o aplikaci prostřednictvím uživatelského rozhraní zapojení Mobile Azure nebo rozhraní API.
- Omezení nabízených rychlosti nebo nabízených kvóty na úrovni aplikace nebo omezit cílové skupiny na úrovni kampaně můžete osobě zabránit, aby přijímal konkrétní nabízených i v případě splňují vaše cílových kritérií. 
- Nastavení "Jazyka" je něco jiného než zacílení založený na země nebo národní prostředí, které se také liší od zacílení na základě Geo umístění na telefonu umístění na nebo GPS.
- Zpráva "nastavení výchozího jazyka" odeslaný zákazníkovi, kdo nemá svého zařízení nastavený na jednu z alternativní jazyky, které zadáte.


## <a name="push-scheduling"></a>Nabízená plánování

### <a name="issue"></a>Problém
- Plánování nabízených nefunguje očekávaným způsobem (odesláno příliš raných nebo opožděnou).

### <a name="causes"></a>Příčiny

- Časová pásma můžete problémy s plánováním, zvlášť když podle časového pásma koncoví uživatelé.
- Funkce Upřesnit nabízených zpozdit posune.
- Zaměření na podle telefonního zpozdit nastavení (místo značky informace o aplikaci) posune od Azure Mobile zapojení možná bude potřeba požadovat data v reálném čase telefonu před odesláním push.
- Kampaní vytvořená bez koncové datum ukládání push místně do zařízení a jeho vyjádření při příštím otevření aplikace i v případě, že se ručně ukončí kampaně.
- Spuštění víc než jedné kampaně ve stejnou dobu může trvat delší dobu skenování základním uživatele (zkuste pouze začínat jedné kampaně současně nejvýše čtyři, taky cíl jenom aktivní uživatelé tak, aby staré uživatele nemusíte kontrolovány).
- Pokud použijete možnost "Cílová skupina ignorovat nabízených se pošle uživatelů prostřednictvím rozhraní API" v části "Kampaně" Reach kampaně kampaně není odešle automaticky, musíte odeslat ručně pomocí rozhraní API dosáhla.
- Pokud používáte vlastní kategorii v Reach zobrazení v aplikaci oznámení, nepotřebujete sledovat správné životního cyklu oznámení, jinak nemusí zrušeno oznámení při uživatel zavřete ho.

 
