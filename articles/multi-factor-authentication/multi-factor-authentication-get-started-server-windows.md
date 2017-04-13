<properties 
    pageTitle="Ověřování systému Windows a Azure Vícefaktorové ověřování serveru"
    description="Tohle je stránka Azure Multi-Factor ověřování, které vám pomohou při nasazení ověřování systému Windows a Azure Multi-Factor ověřování serveru."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a>Ověřování systému Windows a Azure Vícefaktorové ověřování serveru

V části ověřování systému Windows umožňuje správcům povolení a konfigurace ověřování systému Windows pro jednu nebo více aplikací.  Následuje seznam co byste měli myslet před nastavení ověřování systému Windows.

-  před Vícefaktorové ověřování Azure Terminálové služby bude platit, je nutné restartovat počítač.
-  Pokud je zaškrtnuté políčko "Vyžadují Azure Vícefaktorové ověřování uživatele POZVYHLEDAT" a nejste v seznamu uživatelů, nebudete moct Přihlaste se k počítači po restartování.
-  Důvěryhodné IP adresy je závisí na tom, jestli aplikaci umožňují IP klient s ověřování. Je podporován aktuálně jenom Terminálová služba.  







>[AZURE.NOTE]Tato funkce není podporována zabezpečené Terminálová služba Windows Server 2012 R2.




## <a name="to-secure-an-application-with-windows-authentication-use-the-following-procedure"></a>Zabezpečení aplikace pomocí ověřování Windows, použijte následující postup.

1. Na Server Azure Multi-Factor Authentication klepněte na ikonu ověřování systému Windows.
![Ověřování systému Windows](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)
2. Zaškrtněte políčko Povolit Windows ověřování. Ve výchozím nastavení je zaškrtnuté toto políčko.
3. Karta aplikace umožňuje správci konfigurovat jednu nebo více aplikací pro ověřování systému Windows.
4. Vyberte server nebo aplikace – určit, zda je povolena aplikace. Klikněte na OK.
5. Klikněte na Přidat. tlačítko.
6. Na kartě Důvěryhodní IP adresy umožňuje přeskočit Azure Multi-Factor ověřování systému Windows relace pocházející z konkrétní IP adresy. Například používáte zaměstnanců aplikaci z office a z domovské stránky, můžete se rozhodnout, že nechcete telefonech vyzvánění pro Azure Vícefaktorové ověřování v kanceláři. V takovém případě zadáte podsítě office jako důvěryhodné IP adresy položku.
7. Klikněte na Přidat. tlačítko.
8. Pokud chcete přeskočit jednu IP adresu, vyberte jeden IP.
9. Pokud chcete přeskočit celé rozsahu IP vyberte rozsah IP. Příklad 10.63.193.1-10.63.193.100.
10. Pokud byste chtěli zadat oblasti z adresy IP pomocí zápisu podsítě zaškrtněte podsítě. Zadejte počáteční IP podsítě a vyberte odpovídající masky z rozevíracího seznamu.
11. Klikněte na tlačítko OK.
