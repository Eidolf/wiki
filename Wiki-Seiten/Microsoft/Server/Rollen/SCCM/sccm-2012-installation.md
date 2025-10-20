---
title: sccm-2012-installation
description: 
published: true
date: 2023-12-31T13:34:31.269Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:34:28.120Z
---

# SCCM 2012 Installation

# <span class="mw-headline" id="bkmrk-voraussetzungen-1">Voraussetzungen</span>

<div class="vector-body" id="bkmrk-bits-iis6-wmi-kompat"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. BITS
2. IIS6 WMI Kompatibiliät (Rollenfeature)
3. WCF Aktivierung
4. Remotedifferenzialkomprimierung (Feature)
5. SQL Server 
    1. min. Version (2008 SP2 CU9 / 2008 SP3 CU4 / 2008R2 SP1 CU6
    2. Datenbank muss einen bestimmten Zeichensatz verwenden 
        - setup.exe /ACTION=REBUILDDATABASE /SQLCOLLATION=SQL\_Latin1\_General\_CP1\_CI\_AS /INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS=%username%

</div></div></div>## <span class="mw-headline" id="bkmrk-workstation-zertifik-1">Workstation Zertifikate</span>

Der SCCM benötigt für die Installation der Agents, Clientzertifikate an den einzelnen Servern in FQDN Form z.B. (Servername.Domain.etc)

<div class="vector-body" id="bkmrk-neue-vorlage-an-der-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Neue Vorlage an der Zertifizierungsstelle erstellen

1. On the member server that is running the Certification Authority console, right-click Certificate Templates, and then click Manage to load the Certificate Templates management console.
2. In the results pane, right-click the entry that displays Workstation Authentication in the column Template Display Name, and then click Duplicate Template.
3. In the Duplicate Template dialog box, ensure that Windows 2003 Server, Enterprise Edition is selected, and then click OK. ```
    Important Do not select Windows 2008 Server, Enterprise Edition.
    ```
4. In the Properties of New Template dialog box, on the General tab, enter a template name to generate the client certificates that will be used on Configuration Manager client computers, such as ConfigMgr Client Certificate.
5. Click the Security tab, select the Domain Computers group, and select the additional permissions of Read and Autoenroll. Do not clear Enroll.
6. Click OK and close Certificate Templates Console.
7. In the Certification Authority console, right-click Certificate Templates, click New, and then click Certificate Template to Issue.
8. In the Enable Certificate Templates dialog box, select the new template that you have just created, ConfigMgr Client Certificate, and then click OK.
9. If you do not need to create and issue any more certificate, close Certification Authority.

- Ausrollen des Zertifikats über Gruppenrichtlinie einrichten

1. On the domain controller, click Start, click Administrative Tools, and then click Group Policy Management.
2. Navigate to your domain, right-click the domain, and then select Create a GPO in this domain, and Link it here. <dl><dd>This step uses the best practice of creating a new Group Policy for custom settings rather than editing the</dd><dd>Default Domain Policy that is installed with Active Directory Domain Services. By assigning this Group Policy at the domain level, you will apply it #:to all computers in the domain. However, on a production environment, you can restrict the autoenrollment so that it enrolls on only selected #:computers by assigning the Group Policy at an organizational unit level, or you can filter the domain Group Policy with a security group so that it #:applies only to the computers in the group. If you restrict autoenrollment, remember to include the server that is configured as the management point.</dd></dl>
3. In the New GPO dialog box, enter a name for the new Group Policy, such as Autoenroll Certificates, and click OK.
4. In the results pane, on the Linked Group Policy Objects tab, right-click the new Group Policy, and then click Edit.
5. In the Group Policy Management Editor, expand Policies under Computer Configuration, and then navigate to Windows Settings / Security Settings / Public Key Policies.
6. Right-click the object type named Certificate Services Client – Auto-enrollment, and then click Properties.
7. From the Configuration Model drop-down list, select Enabled, select Renew expired certificates, update pending certificates, and remove revoked certificates, select Update certificates that use certificate templates, and then click OK.
8. Close Group Policy Management.

</div></div></div>### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://technet.microsoft.com/en-us/library/gg682023.aspx#BKMK_client2008_cm2012" rel="nofollow">http://technet.microsoft.com/en-us/library/gg682023.aspx#BKMK_client2008_cm2012</a>
```