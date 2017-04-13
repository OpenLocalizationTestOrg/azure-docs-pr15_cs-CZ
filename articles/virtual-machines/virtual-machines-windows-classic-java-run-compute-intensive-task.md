<properties
    pageTitle="Aplikace Java náročné na virtuálního počítače | Microsoft Azure"
    description="Naučte se vytvářet Azure virtuální počítač, který spustí aplikaci Java náročné, která můžete sledovat jinou aplikací Java."
    services="virtual-machines-windows"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-run-a-compute-intensive-task-in-java-on-a-virtual-machine"></a>Jak spustit náročné úkolu v Java na virtuálního počítače

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Azure můžete pomocí virtuálního počítače zpracovávání náročné úkoly. Virtuální počítač můžete například zpracování úkolů a výsledky doručit klientské počítače a mobilní aplikace. Po přečtení v tomto článku, budete mít pochopení toho, jak vytvořit virtuální počítač, který spustí aplikaci Java náročné, která můžete sledovat jinou aplikací Java.

Tento kurz předpokládá vědět, jak vytvořit aplikace konzoly Java, můžete importovat knihovny do aplikace Java a můžete generovat Java archivu (SKLENICE). Žádná znalost Microsoft Azure se dosadí.

Naučíte se:

* Jak vytvořit virtuální počítač s Java Development Kit (JDK) už nainstalovaný.
* Jak vzdáleně přihlášení do virtuálního počítače.
* Jak vytvořit obor názvů bus služby.
* Jak vytvořit aplikaci Java, která provádí náročné úkolu.
* Jak vytvořit aplikaci Java, která sleduje průběh úkolu náročné.
* Postup pro spuštění aplikace Java.
* Jak zabránit aplikace Java.

Tento kurz použije problém Traveling prodejce pro daný úkol náročné. Následující obrázek je příkladem aplikace Java náročné úkolu.

![Řešení problému Traveling prodejce][solver_output]

Následující obrázek je příkladem aplikace Java sledování úkolů náročné.

![Prodejce Traveling problém klienta][client_output]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Vytvoření virtuálního počítače

1. Přihlaste se k [Azure klasické portálu](https://manage.windowsazure.com).
2. Klikněte na **Nový**, klikněte na **Výpočet**, klikněte na **virtuálního počítače**a klikněte na **Z Galerie**.
3. V dialogovém okně **Vybrat obrázek virtuálního počítače** vyberte **JDK 7 systému Windows Server 2012**.
Všimněte si, že **JDK 6 systému Windows Server 2012** dostupné v případě, že máte starší verze aplikace, které nejsou zatím spustit ve JDK 7.
4. Klikněte na tlačítko **Další**.
4. V dialogovém okně **Konfigurace virtuálního počítače** :
    1. Zadejte název pro virtuální počítač.
    2. Určete velikost pro účely virtuální počítač.
    3. Do pole **Uživatelské jméno** zadejte název pro správce. Nezapomeňte tento název a heslo, které pak zadáte, budete je používat při vzdáleně přihlášení do virtuálního počítače.
    4. Zadejte heslo do pole **heslo** a znovu ho zadat do pole **potvrzení** . To je heslo účtu správce.
    5. Klikněte na tlačítko **Další**.
5. V dalším dialogovém okně **Konfigurace virtuálního počítače** :
    1. Ke **cloudové služby**použijte výchozí **vytvořit nové cloudové služby**.
    2. Hodnotu **cloudové služby DNS název** musí být jedinečná přes cloudapp.net. V případě potřeby upravte tuto hodnotu tak, že Azure označuje, že bude jedinečný.
    2. Zadejte oblast, spřažení skupiny nebo virtuální sítě. Pro účely tohoto kurzu zadejte oblast například **Západní USA**.
    2. **Úložiště účet**vyberte **použít účet automaticky generované úložiště**.
    3. **Nastavte dostupnost**vyberte **(žádné)**.
    4. Klikněte na tlačítko **Další**.
5. V konečném dialogové okno **Konfigurace virtuálního počítače** :
    1. Přijměte výchozí koncový bod položky.
    2. Klikněte na **dokončení**.

## <a name="to-remotely-log-in-to-your-virtual-machine"></a>K vzdáleně přihlášení do virtuálního počítače

1. Přihlaste se k [Azure klasické portálu](https://manage.windowsazure.com).
2. Klikněte na **virtuálních počítačích**.
3. Klikněte na název virtuální počítač, který chcete přihlásit k.
4. Klikněte na **Připojit**.
5. Odpovězte na dotazy v případě potřeby se připojit k počítači virtuální. Po zobrazení výzvy k jména a hesla správce použijte hodnoty, které jste zadali při vytvoření virtuálního počítače.

Všimněte si, že funkce Bus služby Azure vyžadují Baltimore CyberTrust kořenového certifikátu jako součást vašeho JRE **cacerts** úložiště je třeba nainstalovat. Certifikát automaticky zahrnuta v prostředí Java Runtime (JRE) použité v tomto kurzu. Pokud nemáte tento certifikát v úložišti **cacerts** JRE, najdete v článku [Přidání certifikátu do úložiště certifikátů CA Java] [ add_ca_cert] informace o přidávání ho (stejně jako informace o zobrazení certifikátů v úložišti cacerts).

## <a name="how-to-create-a-service-bus-namespace"></a>Jak vytvořit obor názvů bus služby

Chcete-li v práci začít používat služby Bus fronty Azure, je nutné nejprve vytvořit obor názvů služby. Obor názvů služby poskytuje oboru kontejner adresování Bus služby zdrojů v aplikaci.

Při vytváření názvů služby:

1.  Přihlaste se k [Azure klasické portálu](https://manage.windowsazure.com).
2.  V levém navigačním podokně portálu Azure klasické klikněte na **službu Bus, řízení přístupu a ukládání do mezipaměti**.
3.  V podokně levou portálu Azure klasické klikněte na uzel **Bus služby** a potom klikněte na tlačítko **Nový** .  
    ![Snímek obrazovky služby Bus uzel][svc_bus_node]
4.  V dialogovém okně **vytvořit nový Namespace služby** zadejte **Namespace**a abyste měli jistotu, že bude jedinečný, klikněte na tlačítko **Kontrola dostupnosti** .  
    ![Vytvoření nového Namespace snímek][create_namespace]
5.  Po provedení, že oboru název je k dispozici, vyberte zemi nebo oblasti, ve kterém by měly být hostované oboru názvů a potom klikněte na tlačítko **Vytvořit Namespace** .  

    Obor názvů, který jste vytvořili zobrazí na portálu Azure klasické a trvá chvíli trvat, než aktivovat. Počkejte, dokud stav je **aktivní** pokračujte dalším krokem.

## <a name="obtain-the-default-management-credentials-for-the-namespace"></a>Získejte oprávnění správy výchozí obor názvů

K provedení správy operací, jako je vytvoření do fronty na nový obor názvů, budete muset získejte Správa oprávnění pro obor.

1.  V levém navigačním podokně klikněte na uzel **Služby Bus** zobrazíte seznam dostupných obory názvů.
    ![Snímek obrazovky dostupné obory názvů][avail_namespaces]
2.  Vyberte obor názvů, který jste právě vytvořili v seznamu zobrazeny.
    ![Snímek obrazovky seznamu Namespace][namespace_list]
3.  V pravém podokně **Vlastnosti** seznamy vlastností pro nový obor názvů.
    ![Snímek obrazovky podokna Vlastnosti][properties_pane]
4.  **Výchozí klíč** skryté. Klikněte na tlačítko **zobrazení** pro zobrazení zabezpečovací přihlašovací údaje.
    ![Snímek obrazovky výchozí klíč][default_key]
5.  Poznamenejte si **Výchozí Vystavitel** a **Výchozí klíč** jako provádět operace s oboru použijete následující informace.

## <a name="how-to-create-a-java-application-that-performs-a-compute-intensive-task"></a>Jak vytvořit aplikaci Java, která provede úkol náročné

1. V tomto počítači vývoj (který nemusí být virtuální počítač, který jste vytvořili) stáhněte si [Azure SDK jazyka Java](https://azure.microsoft.com/develop/java/).
2. Vytvoření aplikace konzoly Java pomocí příklad na konci této části. V tomto kurzu budeme používat **TSPSolver.java** jako název souboru Java. Změnit **vaše\_služby\_bus\_obor názvů**, **vaše\_služby\_bus\_vlastník**, a **vaší\_služby\_bus\_klíč** zástupných symbolů k použití služby bus **názvů**, **Výchozí Vystavitel** a **Klíč výchozí** hodnoty, respektive.
3. Po označení, export aplikace do archivu spustitelného Java (SKLENICE) a balíček požadované knihovny do vygenerovaných SKLENICE. V tomto kurzu budeme používat **TSPSolver.jar** jako název vygenerovaných SKLENICE.

<p/>

    // TSPSolver.java

    import com.microsoft.windowsazure.services.core.Configuration;
    import com.microsoft.windowsazure.services.core.ServiceException;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import java.io.*;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import java.util.ArrayList;
    import java.util.Date;
    import java.util.List;

    public class TSPSolver {

        //  Value specifying how often to provide an update to the console.
        private static long loopCheck = 100000000;  

        private static long nTimes = 0, nLoops=0;

        private static double[][] distances;
        private static String[] cityNames;
        private static int[] bestOrder;
        private static double minDistance;
        private static ServiceBusContract service;

        private static void buildDistances(String fileLocation, int numCities) throws Exception{
            try{
                BufferedReader file = new BufferedReader(new InputStreamReader(new DataInputStream(new FileInputStream(new File(fileLocation)))));
                double[][] cityLocs = new double[numCities][2];
                for (int i = 0; i<numCities; i++){
                    String[] line = file.readLine().split(", ");
                    cityNames[i] = line[0];
                    cityLocs[i][0] = Double.parseDouble(line[1]);
                    cityLocs[i][1] = Double.parseDouble(line[2]);
                }
                for (int i = 0; i<numCities; i++){
                    for (int j = i; j<numCities; j++){
                        distances[i][j] = Math.hypot(Math.abs(cityLocs[i][0] - cityLocs[j][0]), Math.abs(cityLocs[i][1] - cityLocs[j][1]));
                        distances[j][i] = distances[i][j];
                    }
                }
            } catch (Exception e){
                throw e;
            }
        }

        private static void permutation(List<Integer> startCities, double distSoFar, List<Integer> restCities) throws Exception {

            try
            {
                nTimes++;
                if (nTimes == loopCheck)
                {
                    nLoops++;
                    nTimes = 0;
                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.print("Current time is " + dateFormat.format(date) + ". ");
                    System.out.println(  "Completed " + nLoops + " iterations of size of " + loopCheck + ".");
                }

                if ((restCities.size() == 1) && ((minDistance == -1) || (distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-1)] < minDistance))){
                    startCities.add(restCities.get(0));
                    newBestDistance(startCities, distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-2)]);
                    startCities.remove(startCities.size()-1);
                }
                else{
                    for (int i=0; i<restCities.size(); i++){
                        startCities.add(restCities.get(0));
                        restCities.remove(0);
                        permutation(startCities, distSoFar + distances[startCities.get(startCities.size()-1)][startCities.get(startCities.size()-2)],restCities);
                        restCities.add(startCities.get(startCities.size()-1));
                        startCities.remove(startCities.size()-1);
                    }
                }
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        private static void newBestDistance(List<Integer> cities, double distance) throws ServiceException, Exception {
            try
            {
                minDistance = distance;
                String cityList = "Shortest distance is "+minDistance+", with route: ";
                for (int i = 0; i<bestOrder.length; i++){
                    bestOrder[i] = cities.get(i);
                    cityList += cityNames[bestOrder[i]];
                    if (i != bestOrder.length -1)
                        cityList += ", ";
                }
                System.out.println(cityList);
                service.sendQueueMessage("TSPQueue", new BrokeredMessage(cityList));
            }
            catch (ServiceException se)
            {
                throw se;
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        public static void main(String args[]){

            try {

                Configuration config = ServiceBusConfiguration.configureWithWrapAuthentication(
                        "your_service_bus_namespace", "your_service_bus_owner",
                        "your_service_bus_key",
                        ".servicebus.windows.net",
                        "-sb.accesscontrol.windows.net/WRAPv0.9");

                service = ServiceBusService.create(config);

                int numCities = 10;  // Use as the default, if no value is specified at command line.
                if (args.length != 0)
                {
                    if (args[0].toLowerCase().compareTo("createqueue")==0)
                    {
                        // No processing to occur other than creating the queue.
                        QueueInfo queueInfo = new QueueInfo("TSPQueue");

                        service.createQueue(queueInfo);

                        System.out.println("Queue named TSPQueue was created.");

                        System.exit(0);
                    }

                    if (args[0].toLowerCase().compareTo("deletequeue")==0)
                    {
                        // No processing to occur other than deleting the queue.
                        service.deleteQueue("TSPQueue");

                        System.out.println("Queue named TSPQueue was deleted.");

                        System.exit(0);
                    }

                    // Neither creating or deleting a queue.
                    // Assume the value passed in is the number of cities to solve.
                    numCities = Integer.valueOf(args[0]);  
                }

                System.out.println("Running for " + numCities + " cities.");

                List<Integer> startCities = new ArrayList<Integer>();
                List<Integer> restCities = new ArrayList<Integer>();
                startCities.add(0);
                for(int i = 1; i<numCities; i++)
                    restCities.add(i);
                distances = new double[numCities][numCities];
                cityNames = new String[numCities];
                buildDistances("c:\\TSP\\cities.txt", numCities);
                minDistance = -1;
                bestOrder = new int[numCities];
                permutation(startCities, 0, restCities);
                System.out.println("Final solution found!");
                service.sendQueueMessage("TSPQueue", new BrokeredMessage("Complete"));
            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }
        }

    }



## <a name="how-to-create-a-java-application-that-monitors-the-progress-of-the-compute-intensive-task"></a>Jak vytvořit aplikaci Java, která sleduje průběh úkolu náročné

1. V počítači vývoj vytvoření aplikace konzoly Java pomocí příklad na konci této části. V tomto kurzu budeme používat **TSPClient.java** jako název souboru Java. Viz dříve, upravit **vaše\_služby\_bus\_obor názvů**, **vaše\_služby\_bus\_vlastník**, a **vaše\_služby\_bus\_klíč** zástupné symboly používat službu bus **názvů**, **Výchozí Vystavitel** a **Klíč výchozí** hodnoty v.
2. Export aplikace do spustitelného SKLENICE a balíček požadované knihovny do vygenerovaných SKLENICE. V tomto kurzu budeme používat **TSPClient.jar** jako název vygenerovaných SKLENICE.

<p/>

    // TSPClient.java

    import java.util.Date;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import com.microsoft.windowsazure.services.core.*;

    public class TSPClient
    {

        public static void main(String[] args)
        {
                try
                {

                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.println("Starting at " + dateFormat.format(date) + ".");

                    String namespace = "your_service_bus_namespace";
                    String issuer = "your_service_bus_owner";
                    String key = "your_service_bus_key";

                    Configuration config;
                    config = ServiceBusConfiguration.configureWithWrapAuthentication(
                            namespace, issuer, key,
                            ".servicebus.windows.net",
                            "-sb.accesscontrol.windows.net/WRAPv0.9");

                    ServiceBusContract service = ServiceBusService.create(config);

                    BrokeredMessage message;

                    int waitMinutes = 3;  // Use as the default, if no value is specified at command line.
                    if (args.length != 0)
                    {
                        waitMinutes = Integer.valueOf(args[0]);  
                    }

                    String waitString;

                    waitString = (waitMinutes == 1) ? "minute." : waitMinutes + " minutes.";

                    // This queue must have previously been created.
                    service.getQueue("TSPQueue");

                    int numRead;

                    String s = null;

                    while (true)
                    {

                        ReceiveQueueMessageResult resultQM = service.receiveQueueMessage("TSPQueue");
                        message = resultQM.getValue();

                        if (null != message && null != message.getMessageId())
                        {

                            // Display the queue message.
                            byte[] b = new byte[200];

                            System.out.print("From queue: ");

                            s = null;
                            numRead = message.getBody().read(b);
                            while (-1 != numRead)
                            {
                                s = new String(b);
                                s = s.trim();
                                System.out.print(s);
                                numRead = message.getBody().read(b);
                            }
                            System.out.println();
                            if (s.compareTo("Complete") == 0)
                            {
                                // No more processing to occur.
                                date = new Date();
                                System.out.println("Finished at " + dateFormat.format(date) + ".");
                                break;
                            }
                        }
                        else
                        {
                            // The queue is empty.
                            System.out.println("Queue is empty. Sleeping for another " + waitString);
                            Thread.sleep(60000 * waitMinutes);
                        }
                    }

            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }

        }

    }

## <a name="how-to-run-the-java-applications"></a>Spuštění aplikace Java
Spusťte aplikaci náročné vytvořit fronty, potom na cestách Saleseman problém vyřešit, která přidá aktuální nejlepší směrování do fronty bus služby. Při spuštění (nebo později), aplikaci náročné spustit klienta zobrazíte výsledky bus frontě.

### <a name="to-run-the-compute-intensive-application"></a>Spusťte aplikaci náročné

1. Přihlaste se do virtuálního počítače.
2. Vytvořte složku, kde se spustí aplikace. Například **c:\TSP**.
3. Zkopírujte **TSPSolver.jar** **c:\TSP**
4. Vytvoření souboru s názvem **c:\TSP\cities.txt** s tímto obsahem.

        City_1, 1002.81, -1841.35
        City_2, -953.55, -229.6
        City_3, -1363.11, -1027.72
        City_4, -1884.47, -1616.16
        City_5, 1603.08, -1030.03
        City_6, -1555.58, 218.58
        City_7, 578.8, -12.87
        City_8, 1350.76, 77.79
        City_9, 293.36, -1820.01
        City_10, 1883.14, 1637.28
        City_11, -1271.41, -1670.5
        City_12, 1475.99, 225.35
        City_13, 1250.78, 379.98
        City_14, 1305.77, 569.75
        City_15, 230.77, 231.58
        City_16, -822.63, -544.68
        City_17, -817.54, -81.92
        City_18, 303.99, -1823.43
        City_19, 239.95, 1007.91
        City_20, -1302.92, 150.39
        City_21, -116.11, 1933.01
        City_22, 382.64, 835.09
        City_23, -580.28, 1040.04
        City_24, 205.55, -264.23
        City_25, -238.81, -576.48
        City_26, -1722.9, -909.65
        City_27, 445.22, 1427.28
        City_28, 513.17, 1828.72
        City_29, 1750.68, -1668.1
        City_30, 1705.09, -309.35
        City_31, -167.34, 1003.76
        City_32, -1162.85, -1674.33
        City_33, 1490.32, 821.04
        City_34, 1208.32, 1523.3
        City_35, 18.04, 1857.11
        City_36, 1852.46, 1647.75
        City_37, -167.44, -336.39
        City_38, 115.4, 0.2
        City_39, -66.96, 917.73
        City_40, 915.96, 474.1
        City_41, 140.03, 725.22
        City_42, -1582.68, 1608.88
        City_43, -567.51, 1253.83
        City_44, 1956.36, 830.92
        City_45, -233.38, 909.93
        City_46, -1750.45, 1940.76
        City_47, 405.81, 421.84
        City_48, 363.68, 768.21
        City_49, -120.3, -463.13
        City_50, 588.51, 679.33

5. Na příkazovém řádku přejděte do adresáře c:\TSP.
6. Zkontrolujte, zda že je složka Koš JRE v prostředí Path.
7. Potřebujete vytvořit bus frontě před spuštěním permutací TSP Řešitele. Spusťte tento příkaz Vytvořit bus frontě.

        java -jar TSPSolver.jar createqueue

8. Vytvoření fronty můžete spustit permutací TSP Řešitele. Například spusťte tento příkaz spustit řešitele 8 města.

        java -jar TSPSolver.jar 8

 Pokud nezadáte číslo, bude spouštět 10 města. Jak Řešitel najde aktuální nejkratší trasy, přidá je do fronty.

> [AZURE.NOTE]
> Větší číslo, které zadáte, tím déle Řešitel se spustí. Například spuštěn 14 měst může trvat několik minut a spuštění 15 města může trvat několik hodin. Zvětšení 16 či více měst může vést k dnů runtime (postupně týdny, měsíce a roky). Toto je kvůli rychlé zvýšení počtu permutací vyhodnocen Řešitel jako počet poštovních města zvýšení.

### <a name="how-to-run-the-monitoring-client-application"></a>Postup při spuštění aplikace pro sledování klienta
1. Přihlaste se k počítači, kde se spustí klientské aplikaci. To nemusí být stejném počítači spuštěna **TSPSolver** aplikace, i když může být.
2. Vytvořte složku, kde se spustí aplikace. Například **c:\TSP**.
3. Zkopírujte **TSPClient.jar** **c:\TSP**
4. Zkontrolujte, zda že je složka Koš JRE v prostředí Path.
5. Na příkazovém řádku přejděte do adresáře c:\TSP.
6. Spusťte tento příkaz.

        java -jar TSPClient.jar

    Volitelně určete počet minut spánek mezi Kontrola fronty předáním argument příkazového řádku. Výchozí spánku dobu trvání Kontrola fronty je 3 minut, které využívá Pokud žádný argument příkazového řádku předána **TSPClient**. Pokud chcete použít jinou hodnotu pro interval spánku, například minutu, spusťte tento příkaz.

        java -jar TSPClient.jar 1

    Dokud uvidí zprávu fronty "Hotovo" se spustí klienta. Všimněte si, že pokud víckrát Řešitel spustit bez spuštění klienta, budete muset spustit klienta tisknutím úplně Vyprázdnit fronty. Můžete taky můžete odstranit fronty a potom ho znovu vytvářet. Pokud chcete odstranit fronty, spusťte tento příkaz **TSPSolver** (ne **TSPClient**).

        java -jar TSPSolver.jar deletequeue

    Řešitel se spustí, dokud neskončí zkoumání všechny trasy.

## <a name="how-to-stop-the-java-applications"></a>Jak zabránit aplikace Java
Řešitel i klientských aplikacích můžete stisknout **Kombinaci kláves Ctrl + C** ukončíte, pokud chcete ukončit před běžným dokončením.


[solver_output]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/WA_JavaTSPSolver.png
[client_output]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/WA_JavaTSPClient.png
[svc_bus_node]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_02_SvcBusNode.jpg
[create_namespace]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_03_CreateNewSvcNamespace.jpg
[avail_namespaces]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[namespace_list]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_05_NamespaceList.jpg
[properties_pane]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_06_PropertiesPane.jpg
[default_key]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_07_DefaultKey.jpg
[add_ca_cert]: ../java-add-certificate-ca-store.md
