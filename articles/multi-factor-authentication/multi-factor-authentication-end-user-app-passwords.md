<properties
    pageTitle="Jaké aplikace hesla v Azure MFA?"
    description="Tato stránka bude uživatelům pomůže pochopit, jaké aplikace hesla a co jsou použity pro s ohledem na Azure MFA."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>



# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a>Jaké aplikace hesla v Azure Vícefaktorové ověřování?

Některé prohlížení aplikací, třeba nativní e-mailovým klientem Apple, který používá Exchange Active Sync, aktuálně nepodporují vícefaktorové ověřování. Vícefaktorové ověřování aktivované za uživatele. To znamená, že pokud uživatel nemá povolené vícefaktorové ověřování a se pokoušíte použít jiné prohlížeče, nebude možné provést. Zadání hesla aplikace umožňuje tuto funkci používat.

>[AZURE.NOTE] Moderní ověřování pro klienty Office 2013
>
> Klienty Office 2013 (včetně aplikace Outlook) nyní podporuje nové ověřování protokoly a lze povolit pro podporu Vícefaktorové ověřování.  To znamená, že po povolení aplikace hesla nejsou potřebné pro práci s klienty Office 2013.  Další informace najdete v článku [Office 2013 moderní ověřování veřejné preview se ohlásí](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

## <a name="how-to-use-app-passwords"></a>Jak používat aplikaci hesla

Toto jsou některé věci informace o používání aplikace hesla.

- Skutečné heslo je automaticky generované a není zadán uživatelem. Protože je automaticky generované heslo pro mohl uhodnout složitější a lepší zabezpečení.
- Momentálně je maximálně 40 hesla uživatele. Pokud budete chtít vytvořit po dosáhli limitu, budete vyzváni k odstranit jednoho z existující aplikace hesla k vytvoření nové.
- Je vhodné vytvořit aplikaci hesla podle zařízení a ne podle aplikace. Můžete například vytvořit jedno heslo aplikace přenosný počítač a použít toto heslo aplikace pro všechny aplikace v této přenosném počítači.
- Budete mít k zadání hesla aplikace při prvním přihlášení.  Pokud potřebujete další z nich, můžete je vytvořit.

![Nastavení](./media/multi-factor-authentication-end-user-app-passwords/app.png)

Až budete mít aplikaci heslo, použijete tuto místo aktuální heslo se tyto aplikace jiné prohlížeče.  Tak například, pokud používáte vícefaktorové ověřování a nativní e-mailovým klientem Apple na telefonu.  Pomocí aplikace hesla, aby mohli obejít vícefaktorové ověřování a pokračovat v práci.

## <a name="creating-and-deleting-app-passwords"></a>Vytváření a odstraňování aplikací hesla
Během vaše počáteční přihlašovací jsou uvedeny aplikace heslo, které můžete použít.  Kromě toho můžete vytvořit a odstranění hesla aplikaci později.  Tento postup závisí na tom, jak používat vícefaktorové ověřování.  Zvolte této nejčastěji se vás týká.

Jak používat Multi-Factor ověřování|Popis
:------------- | :------------- |
[Použití s Office 365](#creating-and-deleting-app-passwords-with-office-365)|  To znamená, že budete chtít vytvořit hesla aplikaci portálu Office 365.
[Nevím](#creating-and-deleting-app-passwords-with-myapps-portal)|To znamená, že budete moci vytvářet aplikace hesla prostřednictvím [https://myapps.microsoft.com](https://myapps.microsoft.com)
[Můžu používat u Microsoft Azure](#create-app-passwords-in-the-azure-portal)| To znamená, že budete moci vytváření hesel aplikaci portálu Azure.

## <a name="creating-and-deleting-app-passwords-with-office-365"></a>Vytváření a odstraňování aplikací hesla s Office 365

Pokud používáte vícefaktorové ověřování s Office 365 můžete vytvářet a odstraňovat hesla aplikaci portálu Office 365.

### <a name="to-create-app-passwords-in-the-office-365-portal"></a>Vytvoření aplikace hesla v portálu Office 365
--------------------------------------------------------------------------------

1. Přihlaste se k [portálu Office 365](https://login.microsoftonline.com/).
2. V pravém horním rohu vyberte ovládacím prvku a zvolte nastavení Office 365.
3. Klikněte na další zabezpečení ověření.
4. Na pravé straně klikněte na odkaz, že **Aktualizovat telefonní čísla zabezpečení účtu.** 
 ![Nastavení](./media/multi-factor-authentication-end-user-manage/o365a.png)
5. Tím přejdete na stránku, která vám umožní změnit nastavení.
![Cloud](./media/multi-factor-authentication-end-user-manage/o365b.png)
6. V horní vedle ověření větší zabezpečení klikněte na **aplikace hesel.**
7. Klikněte na **vytvořit**.
![Cloud](./media/multi-factor-authentication-end-user-app-passwords-create-o365/apppass.png)
8. Zadejte název aplikace heslo a klikněte na tlačítko **Další**.
![Vytváření aplikací hesla](./media/multi-factor-authentication-end-user-app-passwords/create1.png)
9. Zkopírujte heslo aplikace do schránky a vložte je do aplikace.
![Vytváření aplikací hesla](./media/multi-factor-authentication-end-user-app-passwords/create2.png)


### <a name="to-delete-app-passwords-using-the-office-365-portal"></a>Chcete-li odstranit hesla aplikace na portálu Office 365
--------------------------------------------------------------------------------


1. Přihlaste se k [portálu Office 365](https://login.microsoftonline.com/).
2. V pravém horním rohu vyberte ovládacím prvku a zvolte nastavení Office 365.
3. Klikněte na další zabezpečení ověření.
4. Na pravé straně klikněte na odkaz, že **Aktualizovat telefonní čísla zabezpečení účtu.** 
 ![Nastavení](./media/multi-factor-authentication-end-user-manage/o365a.png)
5. Tím přejdete na stránku, která vám umožní změnit nastavení.
![Cloud](./media/multi-factor-authentication-end-user-manage/o365b.png)
6. V horní vedle ověření větší zabezpečení klikněte na **aplikace hesel.**
7. Vedle aplikace heslo, které chcete odstranit klikněte na **Odstranit**.
![Odstranění aplikace hesla](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)
8. Odstranění potvrďte kliknutím na tlačítko **Ano**.
![Potvrďte odstranění](./media/multi-factor-authentication-end-user-app-passwords/delete2.png)
9. Po odstranění hesla aplikace klepnutím na tlačítko **Zavřít**.
![Zavřete](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)


## <a name="creating-and-deleting-app-passwords-with-myapps-portal"></a>Vytváření a odstraňování aplikací hesla portálem Moje aplikace.
Pokud si nejste jisti, jak používat vícefaktorové ověřování, pak můžete vždy vytvořit a odstranění hesla aplikaci portálu Moje aplikace.

### <a name="to-create-an-app-password-using-the-myapps-portal"></a>Vytvoření heslo k aplikaci portálu Moje aplikace

1. Přihlaste se k [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Nahoře vyberte profil.
3. Vyberte další zabezpečení ověření.
![Cloud](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. Tím přejdete na stránku, která vám umožní změnit nastavení.
![Nastavení](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)
5. V horní vedle ověření větší zabezpečení klikněte na **aplikace hesel.**
6. Klikněte na **vytvořit**.
![Vytváření aplikací hesla](./media/multi-factor-authentication-end-user-app-passwords/create3.png)
7. Zadejte název aplikace heslo a klikněte na tlačítko **Další**.
![Vytváření aplikací hesla](./media/multi-factor-authentication-end-user-app-passwords/create1.png)
8. Zkopírujte heslo aplikace do schránky a vložte je do aplikace.
![Vytváření aplikací hesla](./media/multi-factor-authentication-end-user-app-passwords/create2.png)

### <a name="to-delete-an-app-password-using-the-myapps-portal"></a>Chcete-li odstranit heslo k aplikaci portálu Moje aplikace

1. Přihlaste se k [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Nahoře vyberte profil.
3. Vyberte další zabezpečení ověření.
![Cloud](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. Tím přejdete na stránku, která vám umožní změnit nastavení.
![Nastavení](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)
5. V horní vedle ověření další zabezpečení klikněte na **aplikace hesel.**
6. Vedle aplikace heslo, které chcete odstranit klikněte na **Odstranit**.
![Odstranění aplikace hesla](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)
7. Odstranění potvrďte kliknutím na tlačítko **Ano**.
![Potvrďte odstranění](./media/multi-factor-authentication-end-user-app-passwords/delete2.png)
8. Po odstranění hesla aplikace klepnutím na tlačítko **Zavřít**.
![Zavřete](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)


## <a name="create-app-passwords-in-the-azure-portal"></a>Vytvoření aplikace hesla v portálu Azure

Pokud používáte vícefaktorové ověřování s Azure budete chtít vytvořit hesla aplikaci portálu Azure.

### <a name="to-create-app-passwords-in-the-azure-portal"></a>Vytvoření aplikace hesla v portálu Azure

1. Přihlaste se k portálu Správa Azure.
2. Nahoře klikněte pravým tlačítkem myši na své uživatelské jméno a vyberte další ověření zabezpečení.
3. Na stránce proofup nahoře vyberte aplikaci hesla
4. Klikněte na **vytvořit**.
5. Zadejte název aplikace heslo a klikněte na tlačítko **Další**
6. Zkopírujte heslo aplikace do schránky a vložte je do aplikace.
![Cloud](./media/multi-factor-authentication-end-user-app-passwords-create-azure/app2.png)

### <a name="to-delete-app-passwords-in-the-azure-portal"></a>Chcete-li odstranit hesla aplikace na portálu Azure

1. Přihlaste se k portálu Správa Azure.
2. Nahoře klikněte pravým tlačítkem myši na své uživatelské jméno a vyberte další ověření zabezpečení.
3. V horní vedle ověření větší zabezpečení klikněte na **aplikace hesel.**
4. Vedle aplikace heslo, které chcete odstranit klikněte na **Odstranit**.
5. Odstranění potvrďte kliknutím na tlačítko **Ano**.
6. Po odstranění hesla aplikace klepnutím na tlačítko **Zavřít**.
![Zavřete](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)
