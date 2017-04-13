<properties
    pageTitle="Správa nastavení dvoustupňového ověření | Microsoft Azure"
    description="Správa používání Azure Vícefaktorové ověřování, včetně změny kontaktních informací nebo konfiguraci vašeho zařízení."
    services="multi-factor-authentication"
    keywords = "vícefaktorové ověřování klienta, ověřování problém, ID korelace"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="manage-your-settings-for-two-step-verification"></a>Správa nastavení dvoustupňového ověření

V tomto článku najdete odpovědi na otázky o tom, jak aktualizovat nastavení dvoustupňového ověření nebo multi-Factor ověření. Pokud máte potíže s přihlášením ke svému účtu, podívejte se do [máte potíže s dvoustupňového ověření](multi-factor-authentication-end-user-troubleshoot.md) nápovědu k řešení potíží.


## <a name="where-to-find-the-settings-page"></a>Kde najdete na stránce nastavení
V závislosti na vaší společnosti nastavení Azure Vícefaktorové ověřování existuje několik místa, kde můžete změnit nastavení jako svého telefonního čísla.

Pokud váš správce IT odesláno konkrétní adresu URL nebo kroky ke správě dvoustupňového ověření, postupujte podle těchto pokynů. Následující pokyny by jinak práce pro všechny ostatní. Pokud tyto kroky, ale není vidět stejné možnosti, to znamená, že váš pracovní nebo školní přizpůsobit své vlastní portálu. Požádejte správce pro odkaz na portálu Azure Vícefaktorové ověřování.


1. Přihlaste se k [https://myapps.microsoft.com](https://myapps.microsoft.com)  
2. Nahoře vyberte **profil**.  
3. Vyberte **Další zabezpečení ověření**.  

    ![Moje aplikace](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. Na stránce ověření větší zabezpečení načte s nastavením.

    ![Proofup](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)


## <a name="i-want-to-change-my-phone-number-or-add-a-secondary-number"></a>Chci změnit telefonní číslo nebo přidání sekundární čísla

Je důležité Konfigurace sekundárního ověřování telefonního čísla.  Protože primární telefonní číslo a mobilní aplikace jsou pravděpodobně na stejné telefonu, sekundární telefonní číslo je jediný způsob, jak bude moct se zpátky ke svému účtu případě ztráty nebo krádeže telefonu.

> [AZURE.NOTE]
> Pokud nechcete mít taky přístup k primární telefonní číslo a nepotřebujete pomoc se zprovozněním ke svému účtu, najdete v článku naší témata nápovědy v [máte potíže s dvoustupňového ověření](multi-factor-authentication-end-user-troubleshoot.md).

**Chcete-li změnit primární telefonní číslo:**  

1. Na stránce ověření větší zabezpečení vyberte textové pole s aktuální telefonní číslo a upravovat pomocí nové telefonní číslo.  
2. Vyberte **Uložit**.  
3. Pokud je číslo, které používáte pro možnost upřednostňované ověření, budete muset před uložením ověřit nové číslo.  


**Chcete-li přidat sekundární telefonní číslo:**  

1. Na stránce ověření větší zabezpečení zaškrtněte políčko vedle **alternativní ověřování telefonní.**  
2. Sekundární telefonní číslo zadejte do textového pole.  
3. Dokončení vyberte **Uložit** a změny.  


## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-to-a-new-one"></a>Jak vyčistit Microsoft Authenticator ze starého zařízení a přesunutí na novou?
Po odinstalaci aplikace z vašeho zařízení nebo resetujte neodebere aktivaci na back-end. Používejte kroků uvedených v [Přechod na nové zařízení](multi-factor-authentication-microsoft-authenticator.md#how-to-move-to-the-new-microsoft-authenticator-app).

## <a name="next-steps"></a>Další kroky
- Získejte tipy pro odstraňování potíží a Nápověda k [máte potíže s dvoustupňového ověření](multi-factor-authentication-end-user-troubleshoot.md)
- Nastavení [aplikace hesla](multi-factor-authentication-end-user-app-passwords.md) pro všechny aplikace, které nepodporují dvoustupňového ověření.
