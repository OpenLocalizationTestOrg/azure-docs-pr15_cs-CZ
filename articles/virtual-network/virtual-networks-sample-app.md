<properties
   pageTitle="Ukázka aplikaci pro použití s prostředím hranice zabezpečení | Microsoft Azure"
   description="Nasazení tento jednoduchý webové aplikace po vytvoření DMZ otestovat přenosy toku scénáře"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/01/2016"
   ms.author="jonor"/>

# <a name="sample-application-for-use-with-security-boundary-environments"></a>Ukázka aplikace pro použití s prostředím hranice zabezpečení

[Vraťte se na stránku osvědčené postupy zabezpečení okraj][HOME]

Tyto skriptů Powershellu můžete spustit místně na serverech IIS01 a AppVM01 instalace a nastavení velmi jednoduché webovou aplikaci, která se zobrazí stránka html ze serveru IIS01 front-end obsahu z back-end AppVM01 serveru.

Tato aplikace nebude poskytuje jednoduché testování prostředí pro v mnoha příkladech DMZ a jak změny na koncové body, NSGs, UDR a brány Firewall pravidel účinku tok.

## <a name="firewall-rule-to-allow-icmp"></a>Brána firewall pravidlo umožňuje ICMP
Tento jednoduchý příkazu Powershellu je možné spouštět na libovolnou OM Windows přenosy ICMP (Ping). Toto nastavení umožňuje jednodušší testování a řešení problémů s tím, že povolí protokol ping předat bráně firewall systému windows (pro většinu distros Linux ICMP je zapnuta ve výchozím nastavení).

    # Turn On ICMPv4
    New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" `
        -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow

**Poznámka:** Pokud používáte skripty, toto pravidlo přidání brány firewall následuje první použití příkazu funkce.

## <a name="iis01---web-application-installation-script"></a>IIS01 - skript instalace webové aplikace
Tento skript udělejte toto:

1.  Otevřete IMCPv4 (použití příkazu Ping) na brána windows firewall místního serveru pro snadnější testování
2.  Instalace služby IIS a .net Framework v4.5
3.  Vytvoření webová stránka ASP.NET a Web.config souboru
4.  Změna výchozí fondu aplikací pro usnadnění přístupu k souborům
5.  Nastavení pro anonymní uživatele do svého účtu správce a hesla

Tento skript Powershellu by měla běžet místně, zatímco RDP měli do IIS01.

    # IIS Server Post Build Config Script
    # Get Admin Account and Password
        Write-Host "Please enter the admin account information used to create this VM:" -ForegroundColor Cyan
        $theAdmin = Read-Host -Prompt "The Admin Account Name (no domain or machine name)"
        $thePassword = Read-Host -Prompt "The Admin Password"
        
    # Turn On ICMPv4
        Write-Host "Creating ICMP Rule in Windows Firewall" -ForegroundColor Cyan
        New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow
        
    # Install IIS
        Write-Host "Installing IIS and .Net 4.5, this can take some time, like 15+ minutes..." -ForegroundColor Cyan
        add-windowsfeature Web-Server, Web-WebServer, Web-Common-Http, Web-Default-Doc, Web-Dir-Browsing, Web-Http-Errors, Web-Static-Content, Web-Health, Web-Http-Logging, Web-Performance, Web-Stat-Compression, Web-Security, Web-Filtering, Web-App-Dev, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Net-Ext, Web-Net-Ext45, Web-Asp-Net45, Web-Mgmt-Tools, Web-Mgmt-Console
        
    # Create Web App Pages
        Write-Host "Creating Web page and Web.Config file" -ForegroundColor Cyan
        $MainPage = '<%@ Page Language="vb" AutoEventWireup="false" %>
        <%@ Import Namespace="System.IO" %>
        <script language="vb" runat="server">
            Protected Sub Page_Load(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Load
                Dim FILENAME As String = "\\10.0.2.5\WebShare\Rand.txt"
                Dim objStreamReader As StreamReader
                objStreamReader = File.OpenText(FILENAME)
                Dim contents As String = objStreamReader.ReadToEnd()
                lblOutput.Text = contents
                objStreamReader.Close()
                lblTime.Text = Now()
            End Sub
        </script>
            
        <!DOCTYPE html>
        <html xmlns="http://www.w3.org/1999/xhtml">
        <head runat="server">
            <title>DMZ Example App</title>
        </head>
        <body style="font-family: Optima,Segoe,Segoe UI,Candara,Calibri,Arial,sans-serif;">
          <form id="frmMain" runat="server">
            <div>
              <h1>Looks like you made it!</h1>
              This is a page from the inside (a web server on a private network),<br />
              and it is making its way to the outside! (If you are viewing this from the internet)<br />
              <br />
              The following sections show:
              <ul style="margin-top: 0px;">
                <li> Local Server Time - Shows if this page is or isnt cached anywhere</li>
                <li> File Output - Shows that the web server is reaching AppVM01 on the backend subnet and successfully returning content</li>
                <li> Image from the Internet - Doesnt really show anything, but it made me happy to see this when the app worked</li>
              </ul>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>Local Web Server Time</b>: <asp:Label runat="server" ID="lblTime" /></div>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>File Output from AppVM01</b>: <asp:Label runat="server" ID="lblOutput" /></div>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>Image File Linked from the Internet</b>:<br />
                <br />
                <img src="http://sd.keepcalm-o-matic.co.uk/i/keep-calm-you-made-it-7.png" alt="You made it!" width="150" length="175"/></div>
            </div>
          </form>
        </body>
        </html>'
        
        $WebConfig ='<?xml version="1.0" encoding="utf-8"?>
        <configuration>
          <system.web>
            <compilation debug="true" strict="false" explicit="true" targetFramework="4.5" />
            <httpRuntime targetFramework="4.5" />
            <identity impersonate="true" />
            <customErrors mode="Off"/>
          </system.web>
          <system.webServer>
            <defaultDocument>
              <files>
                <add value="Home.aspx" />
              </files>
            </defaultDocument>
          </system.webServer>
        </configuration>'
            
        $MainPage | Out-File -FilePath "C:\inetpub\wwwroot\Home.aspx" -Encoding ascii
        $WebConfig | Out-File -FilePath "C:\inetpub\wwwroot\Web.config" -Encoding ascii
    
    # Set App Pool to Clasic Pipeline to remote file access will work easier
        Write-Host "Updaing IIS Settings" -ForegroundColor Cyan
        c:\windows\system32\inetsrv\appcmd.exe set app "Default Web Site/" /applicationPool:".NET v4.5 Classic"
        c:\windows\system32\inetsrv\appcmd.exe set config "Default Web Site/" /section:system.webServer/security/authentication/anonymousAuthentication /userName:$theAdmin /password:$thePassword /commit:apphost
        
    # Make sure the IIS settings take
        Write-Host "Restarting the W3SVC" -ForegroundColor Cyan
        Restart-Service -Name W3SVC
        
        Write-Host
        Write-Host "Web App Creation Successfull!" -ForegroundColor Green
        Write-Host


## <a name="appvm01---file-server-installation-script"></a>AppVM01 - soubor skriptu instalace serveru
Tento skript nastaví back-end k této jednoduché aplikaci. Tento skript udělejte toto:

1.  Otevřete IMCPv4 (použití příkazu Ping) v bráně firewall pro snadnější testování
2.  Vytvoření nového adresáře
3.  Vytvoření textového souboru je vzdálený přístup k webové stránce výše
4.  Nastavení oprávnění pro adresář a soubor, který chcete anonymního přístupu
5.  Vypnutí IE rozšířené zabezpečení umožňuje jednodušší procházení z tohoto serveru 

>[AZURE.IMPORTANT] **Doporučený postup**: nikdy vypnout rozšířené zabezpečení aplikace Internet Explorer na serveru výrobní plus je obecně špatný nápad procházet web ze serveru výroby. Otevření sdílených souborů pro anonymní přístup je také špatný nápad, ale Hotovo tady pro zjednodušení.

Tento skript Powershellu by měla běžet místně, zatímco RDP měli do AppVM01. Prostředí PowerShell je třeba spustit jako správce zajistit úspěšná spuštění.
    
    # AppVM01 Server Post Build Config Script
    # PowerShell must be run as Administrator for Net Share commands to work
    
    # Turn On ICMPv4
        New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow
    
    # Create Directory
        New-Item "C:\WebShare" -ItemType Directory
    
    # Write out Rand.txt
        $FileContent = "Hello, I'm the contents of a remote file on AppVM01."
        $FileContent | Out-File -FilePath "C:\WebShare\Rand.txt" -Encoding ascii
    
    # Set Permissions on share
        $Acl = Get-Acl "C:\WebShare"
        $AccessRule = New-Object system.security.accesscontrol.filesystemaccessrule("Everyone","ReadAndExecute, Synchronize","ContainerInherit, ObjectInherit","InheritOnly","Allow")
        $Acl.SetAccessRule($AccessRule)
        Set-Acl "C:\WebShare" $Acl
    
    # Create network share
        Net Share WebShare=C:\WebShare "/grant:Everyone,READ"
    
    # Turn Off IE Enhanced Security Configuration for Admins
        Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0
    
        Write-Host
        Write-Host "File Server Setup Successfull!" -ForegroundColor Green
        Write-Host
    

## <a name="dns01---dns-server-installation-script"></a>DNS01 - skript instalace serveru DNS
Existuje skriptu součástí Tato ukázková aplikace pro nastavení serveru DNS. Pokud testujete pravidel brány firewall, NSG nebo UDR musí obsahovat přenos DNS, serveru DNS01 bude třeba nastavení ručně. Soubor xml konfigurace sítě pro oba příklady obsahuje DNS01 jako primární DNS server a veřejné server DNS hostitelem úrovně 3 jako záložní serveru DNS. Server úrovně 3 DNS bude DNS server použitý pro přenos-local a s DNS01 není nastaven, bez místní DNS by dojít.

<!--Link References-->
[HOME]: ../best-practices-network-security.md
