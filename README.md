# Office 365 External User File Access Report

![PowerShell](https://img.shields.io/badge/PowerShell-%235391FE.svg?style=for-the-badge&logo=powershell&logoColor=white)
![Microsoft SharePoint](https://img.shields.io/badge/Microsoft_SharePoint-0078D4?style=for-the-badge&logo=microsoft-sharepoint&logoColor=white)
![Microsoft OneDrive](https://img.shields.io/badge/Microsoft_OneDrive-0078D4?style=for-the-badge&logo=microsoft-onedrive&logoColor=white)

A PowerShell script to audit and report on external user file access in SharePoint Online and OneDrive for Business.

## Features

- 🔍 **Comprehensive Access Reporting**:
  - Sharing invitations
  - Anonymous links
  - Secure links
- 🔐 **Multiple Authentication Methods**:
  - Interactive login (MFA supported)
  - Credential parameters for automation
  - Automatic module installation
- 📊 **Flexible Filtering**:
  - SharePoint-only reports
  - OneDrive-only reports
  - Custom date ranges
- 📁 **CSV Export**:
  - Timestamped output files
  - Automatic file opening option

## Prerequisites

- PowerShell 5.1 or later
- Exchange Online PowerShell V2 module
- Global Admin or SharePoint Admin permissions

## Installation

1. Clone the repository:
   ```powershell
   git clone https://github.com/RapidScripter/office365-external-sharing-report.git
   cd office365-external-sharing-report

2. Install the required module:
   ```powershell
   Install-Module ExchangeOnlineManagement -Scope CurrentUser -Force
   ```

## Usage

### Basic Commands

```powershell
# Interactive MFA authentication (last 90 days)
.\ExternalSharingReport.ps1

# Specific date range
.\ExternalSharingReport.ps1 -StartDate "2023-01-01" -EndDate "2023-01-31"

# SharePoint-only report
.\ExternalSharingReport.ps1 -SharePointOnline

# OneDrive-only report with automation
.\ExternalSharingReport.ps1 -OneDrive -AdminName "admin@domain.com" -Password "yourpassword"
```

### Advanced Options

| Parameter            | Description                     | Example                           |
|--------------------  |---------------------------------|---------------------------------- |
| `-StartDate`         | Report start date               | `-StartDate "2023-01-01`          |
| `-EndDate`           | Report end date                 | `-EndDate "2023-01-31`            |
| `-SharePointOnline`  | SharePoint-only report          | `-SharePointOnline`               |
| `-OneDrive`          | OneDrive-only report            | `-OneDrive`                       |
| `-AdminName`         | Admin username for automation   | `-AdminName "admin@domain.com"`   |
| `-Password`          | Password for automation         | `-Password "yourpassword"`        |

## Output

The script generates a CSV report with these columns:

- **Shared Time**: When sharing occurred
- **Shared Time**: User who shared the content
- **Shared With**: Recipient ("Anyone with link" for anonymous)
- **Shared Resource Type**: File, Folder, etc.
- **Shared Resource**: URL to the shared item
- **Site URL**: Parent site collection URL
- **Sharing Type**: SharingInvitationCreated/AnonymousLinkCreated/AddedToSecureLink
- **Workload**: SharePoint or OneDrive
- **More Info**: Full audit log details (JSON)

Sample output filename: `ExternalSharingReport_2023-08-15_143022.csv`

## Example Output

| Shared Time       | Shared By          | Shared With               | Shared Resource Type | Shared Resource | Site URL | Sharing Type | Workload |
|-------------------|--------------------|---------------------------|----------------------|-----------------|----------|--------------|----------|
| 2023-11-15 09:30 | john.doe@contoso.com | vendor@external.com | File | Budget2023.xlsx | https://contoso.sharepoint.com/sites/finance | SharingInvitationCreated | SharePoint |
| 2023-11-16 14:15 | jane.smith@contoso.com | Anyone with the link | Folder | ProjectDocs | https://contoso-my.sharepoint.com/personal/jane_smith | AnonymousLinkCreated | OneDrive |
| 2023-11-17 11:20 | mike.jones@contoso.com | partner@external.org | File | Contract.pdf | https://contoso.sharepoint.com/sites/legal | AddedToSecureLink | SharePoint |

## Troubleshooting

| Error/Symptom | Solution |
|--------------|----------|
| "Cannot connect to Exchange Online" | Verify admin permissions |
| "No sharing events found" | Check your date range |
| "Module not found" | Run `Install-Module ExchangeOnlineManagement` |
| "Date range too large" | Maximum 90 days historical data |

## Best Practices

1. **Regular Audits**: Run monthly to detect unauthorized sharing
2. **Review Anonymous Links**: Identify sensitive content shared publicly
3. **Automate Reports**: Schedule with credential parameters
4. **Combine with DLP**: Use reports to inform Data Loss Prevention policies
