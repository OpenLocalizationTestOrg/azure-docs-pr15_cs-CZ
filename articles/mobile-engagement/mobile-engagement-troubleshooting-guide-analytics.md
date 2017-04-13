<properties 
   pageTitle="Azure mobilní zapojení Poradce při potížích s Průvodce - analýzy" 
   description="Technologie pro analýzu, sledování, segmentace a řídicích panelů řešení problémů s v Azure Mobile zapojení" 
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

# <a name="troubleshooting-guide-for-analytics-monitoring-segmentation-and-dashboard-issues"></a>Příručka pro řešení potíží pro problémy s analýzy, sledování, segmentace a řídicích panelů

Možné problémy jsou následující může dojít u jak Azure Mobile zapojení shromažďuje informace o aplikacích, zařízení a uživatelů.

## <a name="missingdelayed-information"></a>Chybějící nebo zpoždění informace

### <a name="issue"></a>Problém
- Informace o zpoždění v zobrazené v analýzy, segmentace nebo řídicího panelu.
- Sledování chybí informace.
- Informace chybí analýzy, segmentace nebo řídicího panelu.
- Snadná práce segmentace omezení.

### <a name="causes"></a>Příčiny

- Můžete použít technologie pro analýzu API rozhraní API monitoru a rozhraní API segmentů a podíváte všechna data, můžete se chybí uživatelské rozhraní se zobrazuje prostřednictvím rozhraní API.
- Pokud SDK zapojení Azure Mobile není správně integrována do aplikace pak nebudete moct vidět informace ve analýzy, segmentace, sledování a řídicích panelů.
- Nesmí být segmentů po jejich vytvoření změnit segmentů může být "klonovaný" (zkopírované) nebo "odstraněna" (odstranit). Segmentů může obsahovat pouze 10 kritéria.
- K nastavení zařízení test, odinstalujte nebo znovu nainstalujte aplikaci na zařízení test se nejlíp seznámíte test chybějících údajů ze sledování.
- Informace o aktualizaci každých 24 hodin pro analýzy, segmentace nebo řídicí panely.
- Informace v nových segmentů se nemusí zobrazit až 24 hodin po jejich vytvoření i v případě segmentu vychází z předchozího informace.
- Filtrování technologie pro analýzu dat v uživatelském rozhraní zobrazí všechny příklady tohoto typu bez ohledu na verzi aplikace (například "dojde k chybě" filtrované podle název se zobrazí z verze 1 a 2 verzi aplikace).
- Časové období analýzy je na základě data z nastavení zařízení uživatelů, aby mohl uživatele, jehož telefon obsahuje datum nastavené nesprávně zobrazovaly ve špatném časové období.
- Posune žádné na straně serveru, který data zaznamenané po pomocí tlačítka "testování", data jenom přihlásili skutečné nabízených kampaní.

## <a name="cant-locate-items-in-ui"></a>Nemůžete najít položky v uživatelském rozhraní

### <a name="issue"></a>Problém
- Nelze vytvořit segmenty podle určitých integrované nebo vlastní aplikaci informace označení kritéria.
- Nelze najít některé integrované nebo vlastní aplikaci informace označení kritéria v analýzy, sledování a řídicích panelů.
- Nelze interpretace dat v analýzy, sledování, segmentace nebo řídicí panely.

### <a name="causes"></a>Příčiny

- Některé integrované položek a značky informace aplikace jsou dostupné jako kritérií nabízených jenom ale nemusí být přidaného segmentu nebo viditelný z analýzy, sledování nebo řídicího panelu. 
- Předdefinovaný položek a značky aplikace informace, které nejde přidat segment musíte se nastavení seznamu zacílení kritéria v jednotlivých kampaní provádět stejnou funkci jako zacílení podle segment.
- V tématu kontextové nabídky v částech technologie pro analýzu, sledování, segmentace a řídicí panely Azure Mobile zapojení uživatelského rozhraní pro další pomoc a jak informace.

## <a name="crash-troubleshooting"></a>Poradce při potížích s selhat

### <a name="issue"></a>Problém
- Aplikace v analýzy, sledování nebo řídicímu panelu dojde k chybě.

### <a name="causes"></a>Příčiny

- Řešení problémů s aplikace na Sharepointu se zobrazí v analýzy, sledování nebo řídicího panelu projděte poznámky k verzi známé problémy s předchozími verzemi aplikace v SDK.
- Další řešení potíží s selhání aplikace popisují, jak událost z test zařízení s nainstalovanou aplikací a vyhledat ID zařízení v části "Sledovat – události" uživatelského rozhraní Azure Mobile Engagement. Proveďte události, který způsobuje aplikaci selhat a vyhledat další informace v části "Monitor – dojde k chybě" uživatelského rozhraní Azure Mobile Engagement. 

 
