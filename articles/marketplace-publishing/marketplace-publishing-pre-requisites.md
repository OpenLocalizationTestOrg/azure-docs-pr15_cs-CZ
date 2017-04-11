<properties
   pageTitle="Netechnických předpoklady pro vytvoření nabídky pro Azure Marketplace | Microsoft Azure"
   description="Porozumět tomu, že splníte požadavky pro vytvoření a nasazení nabídka z Azure Marketplace pro jiné uživatele k nákupu."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
  ms.service="marketplace"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="Azure"
  ms.workload="na"
  ms.date="08/18/2016"
  ms.author="hascipio"/>

# <a name="general-prerequisites-for-creating-an-offer-for-the-azure-marketplace"></a>Obecné požadavky pro vytváření nabídka z Azure Marketplace
Princip Obecné, proces – zaměřeném požadavky, které jsou potřebné k přesunutí vpřed s vytvářením nabídky.

## <a name="ensure-that-you-are-registered-as-a-seller-with-microsoft"></a>Ujistěte se, že jsou registrovány jako prodejce u Microsoftu
Podrobné pokyny k registraci prodejce účtu Microsoft přejděte na [Vytvoření účtu a registrace](marketplace-publishing-accounts-creation-registration.md).

- **Pokud** vaše společnost již registrována jako prodejce v Centru pro vývojáře a chcete vytvořit novou nabídku, a přihlaste se k publikování portálu se stejným id e-mailu s které Centrum pro vývojáře registrace probíhá. Tento krok je potřeba, aby portál Centrum pro vývojáře a publikování jsou propojené s překrývajícími.
- **Pokud vaše společnost již registrována jako prodejce v Centru pro vývojáře a kterou chcete upravit stávající nabídky,** a pak buď přihlášení k publikování portál pomocí účtu správce nebo pomocí účtu, který se přidá ve formě co správce publikování portálu. Kroky potřebné k přidání účtu správce co je uveden níže.

## <a name="steps-to-add-a-co-admin-in-the-publishing-portal"></a>Pokyny pro přidání dalších správce na portálu pro publikování
Správci portál publikování můžete přidat ostatní členové společnost, kteří pracují na aplikaci jako co správce publikování portálu. **Za předpokladu, že jste správce,** uvedeny níže najdete postup, jak přidat co správce.

>[AZURE.NOTE] Pro nové uživatele před přidání dalších správců v publikování portál, ujistěte se, že jste vytvořili aspoň jeden aplikaci v publikování portálu. Tento postup je vyžadován jako na kartě **VYDAVATELÉ** se zobrazí pouze po vytvoření alespoň jeden aplikace v publikování portálu.

1. Zajistěte, aby byl id co správce e-mailu Microsoft account(MSA). V opačném případě zaregistrovat jako MSA pomocí tohoto [odkazu](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).
2. Zajistěte, aby alespoň jednu žádost pod účtem správce před pokusem o přidání dalších – správce.
3. Po výše uvedené kroky hotovi, přihlaste se k publikování portálu s id co správce e-mailu a znovu přihlásit se.
4. Teď přihlášení k publikování portálu s id správce e-mailu.
5. Přejděte na vydavatelé -> Vybrat -> váš účet správce -> přidat správce co (snímek uvedeny níže)

    ![Kreslení](media/marketplace-publishing-pre-requisites/imgAddAdmin_05.png)

6. Zajištěna ID e-mailu v různých fáze procesu publikování (například Centrum pro vývojáře, portál publikování) pro veškeré komunikace od Microsoftu.
7. Centrum pro vývojáře registrace eliminování možnosti použití účtu přidružené k jedné osobě. To je navržené pro závislost odebráním jednu osobu.
8. Pokud čelit jakýchkoli problémů s Centrum pro vývojáře registrace vyvolejte lístek pomocí tohoto [odkazu](https://developer.microsoft.com/en-us/windows/support).

## <a name="steps-to-delete-a-co-admin-in-the-publishing-portal"></a>Postup odstranění co správce publikování portálu
**Za předpokladu, že jste správce,** uvedeny níže najdete postup, jak odstranit co – správce.

1. Přihlaste se k publikování portálu s id správce e-mailu.
2. Přejděte na **Vydavatelé** -> Vybrat účtu -> **Správci** -> **Dalších správců**.
3. Klikněte na tlačítko **X** vedle na co – správce chcete odstranit tot (snímek uvedeny níže).

    ![Kreslení](media/marketplace-publishing-pre-requisites/imgDeleteAdmin_03.png)

> [AZURE.IMPORTANT] Nemáte zadejte společnosti daň a banky informace, pokud se chystáte publikovat pouze bezplatné nabídky (nebo přenést vlastní licence).

> Registrace společnosti musí být dokončen začít pracovat. Však během vaše organizace pracuje informace o daňové a bankovní účet Microsoft Developer, vývojáři můžete začít pracovat týkající se vytváření virtuálních počítačů obrázek na [Portál publikování](https://publish.windowsazure.com), získání oprávnění a testování v Azure testovacím prostředí. Budete potřebovat schválení účtu dokončení prodejce jenom pro poslední krok publikování vaše nabídka z Azure Marketplace.

## <a name="acquire-an-azure-pay-as-you-go-subscription"></a>Získání Azure "systému průběžného financování" předplatného
Toto je předplatné, které se používají k vytvoření OM obrázků a předá obrázky na [Webu Azure Marketplace](https://azure.microsoft.com/marketplace/). Pokud nemáte stávajícímu předplatnému, pak zaregistrujte na https://account.windowsazure.com/signup?offer=ms-azr-0003p.

## <a name="sell-from-countries"></a>"Zákazník od" země
> [AZURE.WARNING]
K prodeji vašich služeb na webu Azure Marketplace, ujistěte se, že je vaše registrovaných entita z jedné země schválené "zákazník od". Toto omezení je pro výběr a daně důvodů. Aktivně chceme nevidět rozbalte seznam země, takže zůstávají sestavenou. Úplný seznam najdete v článku oddíl 1b [zásady účast Azure Marketplace](http://go.microsoft.com/fwlink/?LinkID=526833).

## <a name="next-steps"></a>Další kroky
Jakmile netechnických předpoklady jsou splněny, jsou další nabídky specifické technické požadavky. Klikněte na odkaz na článek odpovídajících nabídka typ, který chcete vytvořit pro Azure Marketplace.

- [Předpoklady technické OM](marketplace-publishing-vm-image-creation-prerequisites.md)
- [Řešení šablony technické předpoklady](marketplace-publishing-solution-template-creation-prerequisites.md)

## <a name="see-also"></a>Viz taky
- [Začínáme: jak publikovat nabídka z Azure Marketplace](marketplace-publishing-getting-started.md)
