<properties 
    pageTitle="Nastavení registru zjišťování aplikace služby Proxy cloud | Microsoft Azure" 
    description="Cílem toto téma je poskytnout kroky, které potřebujete provést nastavení požadovaný port v počítačích s agenta zjišťování aplikace cloudu." 
    services="active-directory" 
    documentationCenter="" 
    authors="markusvi" 
    manager="femila"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="markusvi"/>

# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a>Nastavení registru aplikace zjišťování cloudové služby Proxy

Ve výchozím nastavení agenta cloudu aplikace zjišťování nakonfigurovaný tak, aby použít pouze porty 80 a 443. Pokud se chystáte o instalaci aplikace zjišťování cloudu v prostředí s proxy server, který používá vlastní port (80 ani 443), musíte nakonfigurovat agentů tento port. Konfigurace vychází z klíče registru.


Cílem toto téma je poskytnout kroky, které potřebujete provést nastavení požadovaný port v počítačích s agenta zjišťování aplikace cloudu.



**Chcete-li změnit port používaný počítači se systémem agenta cloudu aplikace zjišťování, proveďte následující kroky:**


1. Spusťte editor registru. <br> ![Spuštění](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)

2. Přejděte na nebo vytvořte následujícímu klíči registru: <br> **HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud aplikace Discovery\Endpoint** 

3. Vytvořte novou hodnotu **řetězců** s názvem **porty**. ![Nový](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)

4. Otevřete dialogové okno **Upravit řetězců** , poklikejte na hodnotu porty.


5. Hodnotu data do textového pole zadejte následující hodnoty a přidejte všechny vlastní porty používané vaší organizací: <br><br>
**80** <br>
**8080** <br>
**8118** <br>
**8888** <br>
**81** <br>
**12080** <br>
**6999** <br>
**30606** <br>
**31595** <br>
**4080** <br>
**443** <br>
**číslo 1110** <br><br>
![Úprava řetězců](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)

6. Klikněte na **OK** zavřete dialogové okno **Upravit řetězců** .



**Další zdroje informací**


* [Jak lze zjistit unsanctioned cloudu aplikace, které se používají v rámci naší organizace](active-directory-cloudappdiscovery-whatis.md) 


