<properties
   pageTitle="Povolení přístupu veřejné aplikaci ještě ACS | Microsoft Azure"
   description="Jak povolit přístup k veřejným do kontejneru služby Azure."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, kontejnerů, Micro služby, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="timlt"/>

# <a name="enable-public-access-to-an-azure-container-service-application"></a>Povolení přístupu ke veřejné aplikace služby Azure kontejneru

Libovolný Datacentrum/OS kontejner ACS [veřejné agent fondu](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) je automaticky vystaven na Internetu. Ve výchozím nastavení, porty **80**, **443** **8080** jsou otevřené a jsou dostupné (veřejné) kontejneru listening na tyto porty. Tento článek popisuje, jak otevřít další porty pro aplikace služby Azure kontejner.

## <a name="open-a-port-portal"></a>Otevření portu (portál) 

Nejdřív potřeba otevřete portu, které chceme.

1. Přihlaste se k portálu.
2. Najděte skupina zdroje, který nasazení služby Azure kontejner.
3. Vyberte Vyrovnávání zatížení agent (což je s názvem podobný **XXXX agent lb XXXX**).

    ![Vyrovnávání zatížení služby Azure kontejneru](media/container-service-dcos-agents/agent-load-balancer.png)

4. Klikněte na **sond** a potom **Přidat**.

    ![Vyrovnávání zatížení služby Azure kontejneru sond](media/container-service-dcos-agents/add-probe.png)

5. Vyplňování formuláře zkušební a klikněte na **OK**.

  	| Pole | Popis |
  	| ----- | ----------- |
  	| Jméno  | Popisný název zkušební. |
  	| Port  | Port kontejneru otestovat. |
  	| Cesta  | (Pokud v režimu HTTP) Relativní webu cesta k probe. Protokol HTTPS v nejsou podporované. |
  	| Interval | Dobu mezi zkušební pokusů v sekundách. |
  	| Chybná prahová hodnota | Počet po sobě jdoucí zkušební pokusí před vzhledem k tomu kontejneru chybná. | 
    

6. Zpět na vlastnosti Vyrovnávání zatížení agent klikněte na **načíst vyrovnávající pravidla** a potom na **Přidat**.

    ![Pravidla Vyrovnávání zatížení služby Azure kontejneru](media/container-service-dcos-agents/add-balancer-rule.png)

7. Vyplňování formuláře Vyrovnávání zatížení a klikněte na **OK**.

  	| Pole | Popis |
  	| ----- | ----------- |
  	| Jméno  | Popisný název Vyrovnávání zatížení. |
  	| Port  | Veřejné příchozí port. |
  	| Port back-end | Vnitřní veřejný port kontejner směrovat přenosy v síti na. |
  	| Fond back-end | Kontejnery v této oblasti budou cíle pro tento Vyrovnávání zatížení. |
  	| Probe | Zkušební umožňuje určit, zda je cílový ve **fondu back-end** správný. |
  	| Zachování relace | Určuje způsob zpracování přenosy z klienta po celou dobu relace.<br><br>**Žádný**: po sobě jdoucí žádosti ze stejného klienta můžete uskutečněných jednotlivými kontejneru.<br>**IP klienta**: po sobě jdoucí žádosti ze stejného IP klienta zpracovávají stejným kontejner.<br>**IP klienta a Protocol (protokol)**: po sobě jdoucí žádosti o stejné kombinaci IP a protokol klienta jsou uskutečněných jednotlivými kontejneru stejné. |
  	| Časový limit nečinnosti | TCP (pouze) V minut, otevřete bez ohledu na čas zachovat klienta TCP/HTTP *udržování* zprávy. |

## <a name="add-a-security-rule-portal"></a>Přidání pravidla zabezpečení (portál)

Pak potřebujeme přidat pravidlo zabezpečení, který přesměrovává přenosy z našich port otevřené přes bránu firewall.

1. Přihlaste se k portálu.
2. Najděte skupina zdroje, který nasazení služby Azure kontejner.
3. Vyberte **veřejný** agent sítě skupině zabezpečení (což je s názvem podobný **XXXX-agent veřejné nsg-XXXX**).

    ![Skupina zabezpečení síť služby Azure kontejneru](media/container-service-dcos-agents/agent-nsg.png)

4. Vyberte **Příchozí pravidla zabezpečení** a potom na **Přidat**.

    ![Pravidla skupiny zabezpečení sítě služby Azure kontejneru](media/container-service-dcos-agents/add-firewall-rule.png)

5. Vyplňte pravidlo brány firewall pro povolení veřejné port a klikněte na **OK**.

  	| Pole | Popis |
  	| ----- | ----------- |
  	| Jméno  | Popisný název pravidla brány firewall. |
  	| Priority (priorita) | Priorita pořadí pravidla. Čím nižší číslo vyšší priorita. |
  	| Zdroje | Omezte příchozí rozsahu IP adres povolení nebo odepření toto pravidlo. Pomocí **libovolné** není určete omezení. |
  	| Služba | Vyberte sadu předdefinovaných služeb, ke kterému se chcete toto pravidlo zabezpečení. V opačném případě pomocí **vlastní** můžete vytvořit vlastní. |
  	| Protocol (protokol) | Omezte podle **TCP** a **UDP**přenosy. Pomocí **libovolné** není určete omezení. |
  	| Oblast port | **Vlastní**po **služby** určuje oblast porty, které ovlivňuje toto pravidlo. Můžete použít jeden port, například **80**nebo oblasti jako **1 024 1 500**. |
  	| Akce | Povolit nebo odepřít přenosy, které splňují daná kritéria. |

## <a name="next-steps"></a>Další kroky

Přečtěte si o rozdílech mezi [veřejných a privátních agenti Datacentrum/OS](container-service-dcos-agents.md).

Přečtěte si další informace o [správě Datacentrum/OS kontejnery](container-service-mesos-marathon-ui.md).