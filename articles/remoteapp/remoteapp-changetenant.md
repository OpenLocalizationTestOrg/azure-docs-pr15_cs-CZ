
<properties
    pageTitle="Změna klienta služby Azure Active Directory v Azure RemoteApp | Microsoft Azure"
    description="Naučte se měnit klienta služby Azure Active Directory spojené s Azure RemoteApp"
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



# <a name="change-the-azure-active-directory-tenant-in-azure-remoteapp"></a>Změna klienta služby Azure Active Directory v Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Azure RemoteApp používá přístup uživatelům Azure Active Directory (Azure AD). Pouze Azure AD klienta, který můžete použít v Azure RemoteApp je přidružený k předplatnému Azure. Přidružená předplatného můžete zobrazit na stránce **Nastavení** na portálu. Na kartě **předplatná** prohlédněte **adresáře** ve sloupci.

> [AZURE.NOTE] Změna úspěšné nejdřív odeberte všechny uživatele stávajícímu klientovi Azure Active Directory ze všech kolekcí Azure RemoteApp. K tomuto účelu přejděte na portál Azure, přejděte na kartu **Azure RemoteApp** a otevřete každé kolekce Azure RemoteApp. Přejděte na kartu **uživatelů** a odebrání uživatelů, kteří patří do vašeho aktuálního tenanta služby Azure Active Directory. Opakujte pro všechny existující kolekce Azure RemoteApp. Aniž byste museli dělat, nebudete moct vytvářet nebo oprava kolekcí.

Pokud chcete použít jiném klientovi, tyto kroky použijte ke změně přidružení k vašemu předplatnému:

1. Na portálu odeberte všem uživatelům Azure AD, ke kterým jste mít přístup k Azure RemoteApp kolekcí. (Viz poznámka nad postup o tom, jak to udělat.)


2. Nastavení účtu Microsoft (dříve označovaného jako účet Live ID) jako správce služby. (Nevíte, pokud už jste správce služeb? Je můžete zjistit tak, že kliknete na **-Nastavení > správci**.) Teď tady je způsob změny, které:
    1. Klikněte v pravém horním rohu uživateli a potom klikněte na **zobrazení faktury**.
    2. Klikněte na předplatné. Na stránce Nová přejděte dolů a klikněte na **Upravit podrobnosti předplatného** vpravo. (Druh střední spodním pravém rohu, jestliže vám pomůže najít ho.)
    3. Zadejte účet Microsoft pro uživatele, který by měl být správce služby.

3. Nyní z portálu odhlásit a pak se přihlaste zpátky pomocí účtu Microsoft, které jste zadali v předchozím kroku.


4. Klikněte na **nové -> aplikace služby -> Active Directory -> adresář -> vlastní vytvořit**.
5. V části **adresář**vyberte **použít existující adresář**. Ukážeme muset odhlásit se z portálu nyní, takže klikněte na **že jsem připraven určený k podpisu teď**.
6. Přihlaste se zpátky do portálu jako globální správce na adresář, který chcete přidat. (Pokud již nebyly jste globální správce, bude po zaokrouhlit o přihlášení a potom na odhlásit.)
7. Budete požádáni při přihlášení Pokud chcete použít existující klienta AD k vašemu předplatnému. Klikněte na **pokračovat**a potom klikněte na **Odhlásit se**.
5. Znovu se přihlásit a vraťte se do **Nastavení -> předplatná**. Vyberte předplatné a pak klikněte na **Upravit adresář**. Vyberte Azure AD klienta, který chcete použít.



Můžete nyní používat novou Azure AD klienta pro řízení přístupu k Azure předplatné a při konfiguraci přístupu uživatelů v Azure RemoteApp.
