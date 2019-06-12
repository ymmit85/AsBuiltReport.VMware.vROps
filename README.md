# VMware vROps As Built Report

## Sample Reports
### Sample Report 1 - Default Style
Sample vSphere As Built report with health checks, using default report style.

![Sample vSphere Report 1](https://github.com/AsBuiltReport/AsBuiltReport.VMware.vROps/blob/master/Samples/Sample_vSphere_Report_1.png "Sample vSphere Report 1")

### Sample Report 2 - Custom Style
Sample vSphere As Built report with health checks, using custom report style.

![Sample vSphere Report 2](https://github.com/AsBuiltReport/AsBuiltReport.VMware.vROps/blob/master/Samples/Sample_vSphere_Report_2.png "Sample vSphere Report 2")

# Getting Started
Below are the instructions on how to install, configure and generate a VMware vSphere As Built report.

## Pre-requisites
The following PowerShell modules are required for generating a VMware vSphere As Built report.

Each of these modules can be easily downloaded and installed via the PowerShell Gallery 

- [AsBuiltReport Module](https://www.powershellgallery.com/packages/AsBuiltReport/)
- [VMware PowerCLI Module](https://www.powershellgallery.com/packages/VMware.PowerCLI/)
- [PowervROPs Module](https://github.com/ymmit85/PowervROps/)

### Module Installation

Open a Windows PowerShell terminal window and install each of the required modules as follows;
```powershell
install-module AsBuiltReport
install-module VMware.PowerCLI
import-module PowervROPs
```

### Required Privileges

To generate a VMware vSphere report, a user account with Read-Only rights to all objects within vROps. Ideally a Local User Account is used.

## Configuration
The vROps As Built Report utilises a JSON file to allow configuration of report information, options, detail and healthchecks. 

A vROps report configuration file can be generated by executing the following command;
```powershell
New-AsBuiltReportConfig -Report VMware.vROps -Path <User specified folder> -Name <Optional> 
```

Executing this command will copy the default vROps report JSON configuration to a user specified folder. 

All report settings can then be configured via the JSON file.

The following provides information of how to configure each schema within the report's JSON file.

### Report
The **Report** sub-schema provides configuration of the vSphere report information

| Schema | Sub-Schema | Description |
| ------ | ---------- | ----------- |
| Report | Name | The name of the As Built Report
| Report | Version | The report version
| Report | Status | The report release status

### Options
The **Options** sub-schema allows certain options within the report to be toggled on or off

| Schema | Sub-Schema | Setting | Description |
| ------ | ---------- | ------- | ----------- |
| Options | AuthSource | Local/Authentication Source Name | Set location of where to authenticate user running report.
| Options | AlertFilter | String value | Used to report only on Alerts with specified value in name.


### InfoLevel
The **InfoLevel** sub-schema allows configuration of each section of the report at a granular level. The following sections can be set

| Schema | Sub-Schema | Default Setting |
| ------ | ---------- | --------------- |
| InfoLevel | Authentication | 3
| InfoLevel | Roles | 3
| InfoLevel | Groups | 3
| InfoLevel | Users | 3
| InfoLevel | Adapters | 3
| InfoLevel | RemoteCollectors | 3
| InfoLevel | Alerts | 3
| InfoLevel | SuperMetrics | 3
| InfoLevel | ServiceStatus | 3
| InfoLevel | CustomGroups | 3

There are 6 levels (0-5) of detail granularity for each section as follows;

| Setting | InfoLevel | Description |
| ------- | ---- | ----------- |
| 0 | Disabled | does not collect or display any information
| 1 | Summary** | provides summarised information for a collection of objects
| 2 | Informative | provides condensed, detailed information for a collection of objects
| 3 | Detailed | provides detailed information for individual objects
| 4 | Adv Detailed | provides detailed information for individual objects, as well as information for associated objects
| 5 | Comprehensive | provides comprehensive information for individual objects, such as advanced configuration settings

\*\* *future release*

### Healthcheck
The **Healthcheck** sub-schema is used to toggle health checks on or off. Currently none configured.

## Examples 
- Generate HTML & Word reports with Timestamp
Generate a vROps As Built Report for vCenter Server 'vrops-01.corp.local' using specified credentials. Export report to HTML & DOC formats. Use default report style. Append timestamp to report filename. Save reports to 'C:\Users\Tim\Documents'
```powershell
New-AsBuiltReport -Target 'vrops-01.corp.local' -Username 'admin' -Password 'VMware1!' -Report VMware.vROps -Format Html,Word -OutputPath 'C:\Users\Tim\Documents' -Timestamp
```
- Generate HTML & Text reports with Health Checks
Generate a vSphere As Built Report for vCenter Server 'vrops-01.corp.local' using stored credentials. Export report to HTML & Text formats. Use default report style. Highlight environment issues within the report. Save reports to 'C:\Users\Tim\Documents'
```powershell
New-AsBuiltReport -Target 'vrops-01.corp.local' -Credentials $Creds -Report VMware.vROps -Format Html,Text -OutputPath 'C:\Users\Tim\Documents' -EnableHealthCheck
```
- Generate report with multiple vROps Servers using Custom Style
Generate a single vSphere As Built Report for vCenter Servers 'vrops-01.corp.local' and 'vrops-02.corp.local' using specified credentials. Report exports to WORD format by default. Apply custom style to the report. Reports are saved to the user profile folder by default.
```powershell
New-AsBuiltReport -Target 'vrops-01.corp.local','vcenter-02.corp.local' -Username 'admin' -Password 'VMware1!' -Report VMware.vROps -StylePath C:\Scripts\Styles\MyCustomStyle.ps1
```
- Generate HTML & Word reports, attach and send reports via e-mail
Generate a vROps As Built Report for vCenter Server 'vrops-01.corp.local' using specified credentials. Export report to HTML & DOC formats. Use default report style. Reports are saved to the user profile folder by default. Attach and send reports via e-mail.
```powershell
New-AsBuiltReport -Target 'vrops-01.corp.local' -Username 'admin' -Password 'VMware1!' -Report VMware.vROps -Format Html,Word -OutputPath C:\Users\Tim\Documents -SendEmail
```

## Known Issues