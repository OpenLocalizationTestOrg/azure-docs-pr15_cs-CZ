<properties 
   pageTitle="Poradce při potížích s nasazeném StorSimple zařízení | Microsoft Azure"
   description="Popisuje, jak Diagnostika a oprava chyb probíhajících na StorSimple zařízení, která je aktuálně nasazeném a funkční."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/16/2016"
   ms.author="v-sharos" />

# <a name="troubleshoot-an-operational-storsimple-device"></a>Poradce při potížích s provozní StorSimple zařízení

## <a name="overview"></a>Základní informace

Tento článek obsahuje užitečné Poradce při potížích pokyny pro řešení problémů s konfigurací, že se můžete setkat po nasazeném a provozní StorSimple zařízení. Popisuje běžné problémy s možných příčin a doporučené kroky k řešení problémů, ke kterým může dojít při spuštění aplikace Microsoft Azure StorSimple. Tyto informace platí pro zařízení fyzicky místní StorSimple a StorSimple virtuální zařízení.

Na konci tohoto článku, které najdete seznam kódy chyb, které se můžete setkat v průběhu operace Microsoft Azure StorSimple i kroků můžete za účelem vyřešení chyby. 

## <a name="setup-wizard-process-for-operational-devices"></a>Průvodce instalací provozní zařízení

Použití Průvodce nastavením ([Vyvolat HcsSetupWizard][1]) Kontrola konfigurace zařízení a akce korekční v případě potřeby.

Když spustíte Průvodce nastavením na dříve konfigurované a provozní zařízení, se liší tok procesu. Můžete změnit následující položky:

- IP adresy, podsítě masky a brány
- Primární DNS server
- Primární NTP server
- Konfigurace proxy volitelné webu

Průvodce nastavením neprovádí operace související s registrace shromažďování a zařízení heslo.

## <a name="errors-that-occur-during-subsequent-runs-of-the-setup-wizard"></a>Chyby, ke kterým dochází při následné se spustí Průvodce instalací

Následující tabulka popisuje chyby setkat při spuštění na Průvodce nastavením na provozní zařízení, která je možné příčiny chyb a doporučené akce vyřešit. 

| Ne. | Chybová zpráva nebo podmínky | Možné příčiny | Doporučené akce |
|:--- |:-------------------------- |:--------------- |:------------------ |
|  1  | Chyba 350032: Toto zařízení již byly deaktivovat. | Tato chyba se zobrazí, pokud spustíte Průvodce nastavením v zařízení, které je deaktivovat. | [Kontaktování podpory společnosti Microsoft](storsimple-contact-microsoft-support.md) k dalším krokům. Deaktivovaný zařízení nelze vložit do služby. Před zařízení můžete znovu aktivovat, může být potřeba obnovit factory. |
|  2  | Vyvolání HcsSetupWizard: ERROR_INVALID_FUNCTION (výjimky z HRESULT: 0x80070001) | Když nefunguje aktualizace serveru DNS. Nastavení DNS se globálních nastavení a, použijí se přes všechny rozhraní povolena sítě. | Povolit rozhraní a znovu použít nastavení DNS. Protože tato nastavení jsou globální to může přerušit síť pro ostatní povolené rozhraní. |
|  3  | Zařízení vypadá to online na portálu správce StorSimple služby, ale při pokusu o dokončit minimální nastavení a konfiguraci uložte se nezdaří. | Během počáteční instalace web proxy nebyl nakonfigurováno, i když jste udělali skutečné proxy serveru v místě. | Získáte pomocí [rutiny Test HcsmConnection] [ 2] vyhledejte chyby do schránky. [Kontaktování podpory společnosti Microsoft](storsimple-contact-microsoft-support.md) Pokud nemůžete vyřešit. |
|  4  | Vyvolání HcsSetupWizard: Hodnota není spadat očekávaný rozsah. | Maska nesprávné podsítě vytvoří tato chyba. Tohle jsou možné příčiny: <ul><li> Maska podsítě nebyla nalezena nebo prázdná.</li><li>Formát předponu Ipv6 není správný.</li><li>Rozhraní povolena cloudu, ale brány chybějící nebo nesprávné.</li></ul>Všimněte si, že DATA 0 automaticky mraků s podporou případě nakonfigurování Průvodce nastavením. | Ke zjištění problému, pomocí podsítě 0.0.0.0 nebo 256.256.256.256 a podívejte se na výstupu. Zadejte správné hodnoty masky podsítě, bránu a předponu Ipv6 podle potřeby. |
 
## <a name="error-codes"></a>Kódy chyb

Chyby jsou uvedeny v číselných pořadí.

|Číslo chyby|Text chybové zprávy nebo popis|Akce doporučené uživatele|
|:---|:---|:---|
|10502|Při přístupu k účtu úložiště došlo k chybě.|Počkejte několik minut a zkuste to znovu. Pokud potíže potrvají, prosím podporu společnosti Microsoft kontakt k dalším krokům.|
|40017|Zálohování selže jako hlasitost podle zásady zálohování nebyl nalezen na zařízení.|Opakovat zálohování operace, pokud potíže potrvají, obraťte se na Microsoft Support. Další kroky.|
|40018|Zálohování selže jako na zařízení nebyl nalezen žádný svazky podle zásady zálohování. |Opakovat zálohování operace, pokud potíže potrvají, obraťte se na Microsoft Support. Další kroky.|
|390061|Je systém zaneprázdněn nebo není k dispozici.|Počkejte několik minut a zkuste to znovu. Pokud potíže potrvají, prosím podporu společnosti Microsoft kontakt k dalším krokům.|
|390143|Došlo k chybě s kódem chyby 390143. (Neznámé chybě.)|Pokud potíže potrvají, obraťte se prosím Microsoft Support k dalším krokům.|

## <a name="next-steps"></a>Další kroky

Pokud jste se nepodařilo problém vyřešit, [Kontaktujte podporu od Microsoftu](storsimple-contact-microsoft-support.md) , aby vám. 


[1]: https://technet.microsoft.com/en-us/%5Clibrary/Dn688135(v=WPS.630).aspx
[2]: https://technet.microsoft.com/en-us/%5Clibrary/Dn715782(v=WPS.630).aspx
