# PowerShell MicroSoftTeams

This is my general guide to particularly useful scripts in powershell for MicrosoftTeams in my current role as an educational developer at UNSW Sydney.

IMPORTANT: set up your powershell and use the preview version 1.1.3 to get all the cmdlets

> Reference: [Teams Powershell Docs](https://docs.microsoft.com/en-us/microsoftteams/teams-powershell-install#install-teams-powershell-public-preview)
> Cmdlets: [Teams Powershell Cmdlets](https://docs.microsoft.com/en-us/powershell/module/teams/?view=teams-ps)

=========================================================

## Adding a list of users to your MS Team via a csv

Notes:
- use a csv with header 'email'
- the email you have to use in the csv is "zID@ad.unsw.edu.au"

```
Connect-MicrosoftTeams

Get-Team -user "zID@ad.unsw.edu.au" // copy the Group ID you want from the list

Import-Csv -Path "users.csv" | foreach{Add-TeamUser -GroupId GROUPID -user $_.email}
```

=========================================================

## Getting a list of users from your MS team as a csv
```
Connect-MicrosoftTeams

Get-Team -user "zID@ad.unsw.edu.au" // copy the Group ID you want from the list

Get-TeamUser -GroupId YOURGROUPID | export-csv C:\something something\teamusers.csv
```

=========================================================

## Creating Multiple channels both Standard and Private from a csv

> Reference: [Creating bulk Channel for teams](https://www.ntweekly.com/2020/04/11/create-multiple-microsoft-teams-channels-powershell/)

Notes:
- set up a csv with your channel name (e.g. cname) in column 1 and membership type (e.g. ctype) in column 2.
- channels names can only used once, and the membership type is either "standard" or "private"

```
Connect-MicrosoftTeams
	
Get-Team -user "zID@ad.unsw.edu.au" // copy the Group ID you want from the list

Import-csv channels.csv | foreach{New-TeamChannel -GroupId YOURGROUPID -DisplayName $_.cname -MembershipType $_.ctype}
```

=========================================================

## Adding Users to Private Channels via CSV

> Reference: [Adding users to private teams](https://medium.com/@joaquin.guerrero/adding-bulk-users-to-teams-private-channels-8c9c8e563900)

Notes:
- You can only add users to channels you have created and are the owner.
- use a csv with header 'cname' for channel name and 'email' for emails.
- the email you have to use in the csv is "zID@ad.unsw.edu.au" (same as from the user import list above).
- be sure to add your relative tutors and teachers at this set, you can promote them to Owners of their relative channel afterwards.

```
Connect-MicrosoftTeams

Get-Team -User "zID@ad.unsw.edu.au" // then copy the GroupId that you want to add private channels to

Import-Csv -Path “YOUR_FILE_PATH” | foreach{Add-TeamChannelUser -GroupId YOUR_TEAM_ID -DisplayName $_.cname -user $_.email}
```

=========================================================

## Promoting Users to "Owners" in Private channels via csv

> Reference: [Add then promote]()

Notes:
- You will need to set up a separate csv that contains the users you want to promote in relative channels as Owners.
- use a csv with header 'cname' for the private channel name they already exist in and 'email' for emails.

```
Connect-MicrosoftTeams

Get-Team -User "zID@ad.unsw.edu.au" // then copy the GroupId that you want to add private channels to

Import-Csv -Path “YOUR_FILE_PATH” | foreach{Add-TeamChannelUser -GroupId YOUR_TEAM_ID -DisplayName $_.cname -user $_.email -Role Owner}

```

