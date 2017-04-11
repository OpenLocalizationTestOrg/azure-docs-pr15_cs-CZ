<properties
    pageTitle="Akce: v případě Azure služby narušení ovlivňující Azure klíč trezoru | Microsoft Azure"
    description="Přečtěte si postup v případě přerušení služby Azure, ovlivňující Azure klíč trezoru."
    services="key-vault"
    documentationCenter=""
    authors="adamglick"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="key-vault"
    ms.workload="key-vault"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="sumedhb;aglick"/>


# <a name="azure-key-vault-availability-and-redundancy"></a>Azure klíč trezoru dostupnost a redundance

Azure trezoru klíč funkcemi více vrstev redundance, abyste měli jistotu, že klíče a tajemství zůstávají k dispozici aplikaci i v případě jednotlivých součástí službu nezdaří.

Obsah trezoru klíčové replikace v oblasti a vedlejší oblasti aspoň 150 mil jinam, ale v rámci stejné zeměpisná oblast. To udržuje vysoké životnost klíče a tajemství.

Pokud se nezdaří jednotlivé součásti v rámci služby klíčové trezoru alternativní součásti v oblasti krok v a bude předávat vaši žádost, abyste měli jistotu, že je žádné snížení funkčnosti. Se nemusí provádět žádnou akci to aktivovat. Se automaticky stane a budou průhledné vám.

Méně častých událost, že je k dispozici celá Azure oblast požadavky, které uděláte z trezoru klíč Azure v dané oblasti způsoby automaticky směrované (*selhání*) do vedlejší oblasti. Když oblasti primární neexistuje znovu, jsou požadavky na směrovány zpět (*se nepodařilo zpět*) na primární oblast. Ještě jednou se nemusí provádět žádnou akci, protože k tomu dojde automaticky.

Existuje několik upozornění je potřeba vědět:

* V případě selhání oblast může trvat několik minut selhání prostřednictvím služby. Požadavky, které byly během této doby může selhat, dokud nebude dokončena záložní.
* Po dokončení přepojení trezoru klíčové je v režimu jen pro čtení. Požadavky, které jsou podporované v tomto režimu jsou:
 * Seznam klíčových trezorů
 * Zobrazí vlastnosti klíčové trezorů
 * Seznam tajemství
 * Získání tajemství
 * Seznam klíče
 * Získat klíče (vlastností)
 * Šifrování
 * Dešifrování
 * Obtékání
 * Zrušit zalomení
 * Ověření
 * Sign
 * Zálohování
* Po přepojení se nezdařila zpět, jsou k dispozici všechny typy oslovení (včetně čtení *a* zápis požadavky).
