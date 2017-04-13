<properties
    pageTitle="Jak migrovat z RemoteApp VNET Azure VNET | Microsoft Azure"
    description="Zjistěte, jak migrovat z RemoteApp VNET do Azure VNET"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="how-to-migrate-a-hybrid-collection-from-a-remoteapp-vnet-to-an-azure-vnet"></a>Jak migrovat kolekce hybridní z RemoteApp VNET Azure VNET

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Dobrá zpráva! Jsme jste povolili nasazení hybridní RemoteApp kolekce přímo do vaší stávající Azure virtuální sítích (VNETs) místo abyste vytvářeli RemoteApp specifické VNETs. To vám umožní využít výhod nejnovější funkce VNET (třeba ExpressRoute) a dát vaše hybridní kolekce přímý přístup k síti jiných Azure služeb a virtuálních počítačích používaný této VNET.  (Toto získá je lepší výkon a jednodušší nastavení než VNET VNET konfigurace).


Řekněme, že jste už vytvořili hybridním RemoteApp kolekce s názvem *OriginalCollection* s RemoteApp VNET s názvem *RemoteAppVNET*. Tady najdete postup, jak migrovat do nového VNET Azure s názvem *AzureVNET*.

1.  Na kartě **sítí** na [portálu Správa](http://manage.windowsazure.com/)vytvořte VNET s názvem *AzureVNET*, jaký byl použit pro *RemoteAppVNET*pomocí stejného umístění, konfigurace DNS a adresní prostor (pro alespoň jedno podsítí *AzureVNET* ).
2.  Konfigurace *AzureVNET* na některý z hostitele nebo máte připojení k síti pro nasazení služby Active Directory, která *OriginalCollection* je doména připojen k.
3.  Na kartě **aplikace RemoteApp** vytvoří novou kolekci RemoteApp s názvem *Novou kolekci*. (Použijte možnost **vytvořit s VNET** **Vytvořit**.)
3.  Konfigurace *NewCollection* nasazeny podsítě v *AzureVNET*.
4.  Konfigurace *NewCollection* používat stejný obrázek a informace o připojení k domény jaký byl použit pro *OriginalCollection*.
5.  Po několik hodin *NewCollection* se zobrazí v seznamu kolekce s aktivního stavu.

Teď Pokud není potřeba migrovat všechny informace o uživatelích z původní kolekci nové kolekce, postupujte takto:

6.  Odstranění *OriginalCollection*.
7.  Odstranění *RemoteAppVNET*.

A budete mít?

Můžete také pokud potřebujete migrovat informace o uživateli z původní kolekci nové kolekce, postupujte takto:

6.  Odeslání e-mailu pro [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) s ID Azure předplatného, na název vaší původní kolekci a název nové kolekce webů a požádáte ho, které se mají migrovat informace o uživateli.
7.  Do 2 pracovních dní týmu RemoteApp přesune přístup uživatelů ze seznamu a všechny dokumenty uživatelů a nastavení uživatelů z původní kolekci nové kolekce.
8.  Odstranění *OriginalCollection*.
9.  Odstranění *RemoteAppVNET*.

A teď budete mít?

Pokud máte nějaké dotazy nebo potřebujete pomoc s jinak, e-mailem [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help).
