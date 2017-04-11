1. Ve Visual Studio **Průzkumník řešení**klikněte pravým tlačítkem myši projektu a vyberte **Přidat > Podpora Docker** v místní nabídce.

    ![Přidání Docker podpory místní nabídky](media/vs-azure-tools-docker-add-docker-support/docker-support-context-menu.png)

1. Přidání Docker podpory ASP.NET základní web projektu za následek přidání více souborů Docker vás někdo přidá projektu, včetně vytvořte Docker soubory, skripty Windows Powershellu nasazení a Docker vlastnost soubory. 

    ![Soubory docker přidané do projektu](media/vs-azure-tools-docker-add-docker-support/docker-files-added.png)
    
> [AZURE.NOTE]Pokud používáte [Docker pro Windows Beta](https://beta.docker.com), otevřete Properties\Docker.props, odebrat nebo rovná hodnotě a restartujte aplikaci Visual Studio hodnoty se projeví.
> 
> ```
> <DockerMachineName Condition="'$(DockerMachineName)'=='' "></DockerMachineName>
> ```
