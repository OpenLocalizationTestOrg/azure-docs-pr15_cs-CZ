<properties
   pageTitle="Testování OM nabídky pro Marketplace | Microsoft Azure"
   description="Pochopte, jak k testování OM obrázek z webu Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/01/2016"
   ms.author="hascipio" />

# <a name="test-your-vm-offer-for-the-azure-marketplace-in-staging"></a>Otestujte vaše OM nabídka z Azure Marketplace v pracovní

Pracovní znamená, že nasazení SKU v soukromé "sandboxová", kde můžete otestujte a ověřte jeho funkcí před nasazením na Tržiště. Skladovou se zobrazí v přípravu stejně jako u ji zákazníkovi, který má nasazení. Obrázek OM musí být oprávnění posune pracovní.

## <a name="step-1-push-your-offer-to-staging"></a>Krok 1: Nabízená vaši nabídku pracovní

1. Na kartě **Publikovat** klikněte na **použít pro pracovní**.

    ![Kreslení](media/marketplace-publishing-vm-image-test-in-staging/vm-image-push-to-staging.png)

2. Pokud portál publikování vás upozorní na všechny chyby, opravte.
3.  V **kdo má přístup k vaší fázované nabídky?** dialogové okno Zadejte seznamu Azure předplatných, která se používají k náhledu vaší nabídky na [portál Azure náhled](https://portal.azure.com).

    >[AZURE.NOTE] V případě virtuálních počítačích a řešení šablon, ujistěte se, **co nedělat** povolených předplatných typu CSP, DreamSpark nebo Azure otevřené.


    > V případě virtuálních počítačích po kliknutí na tlačítko **NABÍZENÁ pracovní**podle těchto kroků provádí za scény. Budete moct zobrazit průběh jednotlivými kroky na kartě Publikovat ve publikování portálu. Je třeba zkontrolovat tuto stránku v pravidelných intervalech (až stav zobrazuje FÁZOVANÁ) pro všechny selhání informace, které potřebujete oprav od vašeho konce.

    > - Nejdřív žádosti o pracovní půjde certifikační týmu, který ověřit virtuální pevný disk. Ale pokud žádosti o má máte jenom marketingové změnit, pak krok certifikační vynechán.
    > - Po dokončení certifikace replikace nabídky start přes Azure datacentrech. Obecně trvá 24 48hours replikace dokončete ale můžou projevit až po týdnu v závislosti na velikosti virtuální pevný disk. Pokud však žádosti o má máte jenom marketingové změnit, pak replikace je rychlejší.
    > - Po dokončení replikace nabídky budou k dispozici v [Azure portálu](http:/portal.azure.com). V této době stav budou uloženy publikování portálu. Fázovanou nabídky se zobrazuje v [Azure portál](http:/portal.azure.com) jenom pomocí e-mailu ID přidružený k předplatnému, ke kterému je připraven nabídky.

4. Přihlaste se k [Azure náhled portál](https://portal.azure.com) pomocí jedné z Azure předplatných uvedené v předchozím kroku.
5. Najděte vaší nabídky a ověřte body OM obrázek:
  - Ujistěte se, že marketingového obsahu se objeví správně v tržiště.
  - Nasazení začátku do konce OM obrázku.

      ![img mapu portálu](media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg)

> [AZURE.IMPORTANT] Vaše nabídka zůstane v přípravu dokud oznámit, že budete Microsoftu s použitím portál publikování [kartu**Publikovat** > klikněte na tlačítko **"požádat o schválení k nabízených k výrobní"**], že budete chtít použít pro výroby. Toto je ideální čas pro všechny členy týmu zaškrtnutí přes všechno, co při přípravě na vaši nabídku přejdete uvedené.

> Přípravná platformy slouží k testování nabídky v režimu náhledu vydavatelem. Důrazně zamezilo pomocí tohoto platofrm pro obchodní účely.

## <a name="next-steps"></a>Další kroky
Vaše nabídka "připraven" a testování jeho funkcí a marketingového obsahu, můžete přejít na poslední publikování fáze, **Krok 4**: [nasazení vaši nabídku Tržiště](marketplace-publishing-push-to-production.md).

## <a name="see-also"></a>Viz taky
- [Začínáme: jak publikovat nabídka z Azure Marketplace](marketplace-publishing-getting-started.md)
