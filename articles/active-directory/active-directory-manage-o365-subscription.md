<properties
   pageTitle="Správa v adresáři vašeho předplatného Office 365 v Azure | Microsoft Azure"
   description="Správa adresáře předplatného Office 365 pomocí služby Azure Active Directory a Azure klasické portálu"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="manage-the-directory-for-your-office-365-subscription-in-azure"></a>Správa v adresáři vašeho předplatného Office 365 v Azure

Tento článek popisuje, jak spravovat adresář, který byl vytvořený pro předplatné Office 365, na portálu Azure klasické. Musíte se přihlásit k portálu Azure klasické Správce služeb nebo spolu správce Azure předplatného. Pokud ještě nemáte předplatné Azure, můžete registraci [Uvolnit 30denní zkušební](https://azure.microsoft.com/trial/get-started-active-directory/) dnes a nasazení prvního cloudové řešení za v části pět minut, pomocí tohoto odkazu. Ujistěte se, že jste pomocí pracovního nebo školního účtu, který používáte pro přihlášení k Office 365.

Po dokončení Azure předplatného se můžete přihlásit k portálu Azure klasické a přístup ke službám Azure. Klikněte na položku rozšíření služby Active Directory pro správu stejné adresář, který ověřuje uživatele Office 365.

Pokud už máte předplatné Azure, postupuje při správě dalších adresářů je také jednoduché. Petr Novák může mít například předplatné Office 365 pro Contoso.com. Má Azure předplatné, které si zaregistrovali pomocí svého účtu Microsoft, msmith@hotmail.com. V tomto případě spravuje dva adresáře.

  Předplatné |  Office 365  |  Azure
  -------------- | ------------- | -------------------------------
  Zobrazované jméno |  Contoso  |     Výchozí Azure Active Directory (Azure AD) adresář
  Název domény  |  contoso.com  | msmithhotmail.onmicrosoft.com

Chce správy identit uživatelů v adresáři společnosti Contoso během mu je přihlášený Azure pomocí svého účtu Microsoft, takže si můžete povolit Azure AD funkce, například vícefaktorové ověřování. Následující diagram mohou pomoci ke znázornění procesu.

![Diagram ke správě dva nezávislé adresáře](./media/active-directory-manage-o365-subscription/AAD_O365_03.png)

V tomto případě jsou dva adresáře nezávisle na sobě.

## <a name="to-manage-two-independent-directories"></a>Ke správě dva nezávislé adresáře
Aby Petr Novák ke správě oba adresáře, když si je přihlášený Azure jako msmith@hotmail.com, si musí proveďte následující kroky:

> [AZURE.NOTE]
> Tyto kroky můžete provést pouze v případě, že uživatel přihlášenými pomocí účtu Microsoft. Pokud je uživatel přihlášenými pomocí pracovní nebo školní účet, možnost **použít existující adresář** není k dispozici. Pracovní nebo školní účet podpisům pouze v jeho adresáři (to znamená adresáři kde je uložený pracovní nebo školní účet, a aby podnik nebo škola vlastní).

1.  Přihlaste se k [portálu Azure klasické](https://manage.windowsazure.com) jako msmith@hotmail.com.
2.  Klikněte na **Nový** > **aplikace služby** > **Služby Active Directory** > **adresáře** > **Vytvořit vlastní**.
3.  Klikněte na použít existující adresář a zaškrtněte políčko **určený k podpisu teď už můžu** .
4.  Přihlaste se k portálu Azure klasické jako globální správce Contoso.onmicrosoft.com (například msmith@contoso.com).
5.  Po zobrazení výzvy k **directory společnosti Contoso pomocí Azure?**, klikněte na **pokračovat**.
6.  Klikněte na tlačítko **Odhlásit se**.
7.  Přihlaste se k portálu Azure klasické jako msmith@hotmail.com. Directory společnosti Contoso a výchozího adresáře se zobrazí v koncovku služby Active Directory.

Po dokončení těchto kroků msmith@hotmail.com globálního správce v adresáři společnosti Contoso.

## <a name="to-administer-resources-as-the-global-admin"></a>Ke správě zdrojů jako globální správce
Nyní předpokládejme, že Petra Karásek vyžaduje spravovat weby a databáze prostředky, které jsou přidružené k Azure předplatné pro msmith@hotmail.com. Předtím, než si můžete to udělat, Petr Novák potřebuje proveďte následující kroky:

1.  Přihlaste se k [Azure klasické portál](https://manage.windowsazure.com) pomocí účtu správce služby Azure předplatného (v tomto příkladu msmith@hotmail.com).
2.  Přenášet předplatné directory společnosti Contoso: klikněte na **Nastavení** > **předplatná** > vyberte předplatné > **Upravit adresáře** > Vybrat **Contoso (Contoso.com)**. V rámci přenosu, všechny pracovní nebo školní účty, které jsou dalších správců předplatného se odeberou.
3.  Přidání Petra Karásek jako spolu správce předplatné: klepněte na tlačítko **Nastavení** > **Správci** > vyberte předplatné > **Přidat** > typ **JohnDoe@Contoso.com**.

## <a name="next-steps"></a>Další kroky
Další informace o vztah mezi předplatná a adresáře najdete v článku [jak předplatné souvisí s adresář](active-directory-how-subscriptions-associated-directory.md).
