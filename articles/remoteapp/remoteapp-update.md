<properties
   pageTitle="Aktualizovat kolekci Azure RemoteApp | Microsoft Azure"
   description="Naučte se aktualizovat kolekci Azure RemoteApp"
   services="remoteapp"
   documentationCenter=""
   authors="lizap"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="update-a-collection-in-azure-remoteapp"></a>Aktualizace kolekce v Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Chodily čas, nevyhnutelně, když potřebujete aktualizovat aplikace nebo obrázky v kolekci Azure RemoteApp. Pokud používáte jeden z obrázků součástí předplatného Azure RemoteApp v cloudu nebo hybridní kolekci všechny aktualizace jsou uskutečněných jednotlivými Azure RemoteApp sebe sama tak můžete umístíte snadné.

Ale pokud používáte vlastní obrázek (integrované od začátku nebo vytvořil upravit některou z našich obrázky), jste zodpovědní za zachování obrázek a aplikace. Pokud potřebujete aktualizovat obrázek nebo na jakékoli stránce aplikace uvnitř, musíte k vytvoření nové a aktualizované verze obrázku a nahradit stávající obrázek ve vaší kolekci nového aktualizované obrázku.

Ano jak přejdete o aktualizaci kolekci? Je velmi jednoduché:

1. Aktualizujte obrázek, který jste použili v kolekci. Použití opravy nebo aktualizace potřeby a uložit ho pod novým názvem.
2. [Nahrání](remoteapp-uploadimage.md) nebo [importovat](remoteapp-image-on-azurevm.md) , obrázek a RemoteApp.
3. Teď na stránce kolekci klikněte na **Aktualizovat**.
4. **Obrázek šablony** seznamu vyberte nový obrázek.
4. Sem zadejte tu část záludné: je potřeba rozhodnout, jak zacházet s uživateli, které aktuálně používáte aplikaci v kolekci. Máte následující možnosti:
    - **Uživatelům 60 minut po instalaci aktualizace**. Hned po dokončení aktualizace Azure RemoteApp zobrazí zprávu s žádní aktivní uživatelé informací, uložte svou práci a odhlásit a znova se přihlaste. Po 60 minut aktivní uživatelé, kteří jste se nepřihlásili vypnout automatické odhlášeni. Uživatele můžete okamžitě znovu se přihlaste.
    - **Přihlásit uživatelé se okamžitě**. Hned po dokončení aktualizace odhlásíte všichni uživatelé automaticky bez varování. Pokud vyberete tuto možnost, uživatelé může dojít ke ztrátě dat. Ale budou se můžete připojit k aplikaci okamžitě.

1. Klikněte na značku zaškrtnutí aktualizaci zahájíte.
