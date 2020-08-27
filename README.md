# PSMSTeams

This is my general guide to particularly useful scripts in powershell for MicrosoftTeams in my current role.

/*IMPORTANT set up your powershell and use the preview version to get all the cmdlets*/
Reference: https://docs.microsoft.com/en-us/microsoftteams/teams-powershell-install#install-teams-powershell-public-preview

==================================================================================================================================
/* Getting a list of users from your MS team */

	Connect-MicrosoftTeams
	
	Get-Team -user "zID@ad.unsw.edu.au" // copy the Group ID you want from the list
	
	Get-TeamUser -GroupId YOURGROUPID | export-csv C:\something something\teamusers.csv

==================================================================================================================================

/*Creating Multiple channels both Standard and Private from a csv NOT AVAILABLE YET*/

Reference: https://www.ntweekly.com/2020/04/11/create-multiple-microsoft-teams-channels-powershell/

prepare - set up a csv saved as a filename channels.csv with your channel name (cname) in column 1

	Connect-MicrosoftTeams
	
	Get-Team -user "zID@ad.unsw.edu.au" // copy the Group ID you want from the list

	Import-csv channels.csv | foreach{New-TeamChannel -GroupId YOURGROUPID -DisplayName $_.cname -MembershipType $_.ctype}

==================================================================================================================================

/*Adding Users to Private Channels via CSV (have you csv ready beforehand use header cname for channel name and email for emails)*/
Reference: https://medium.com/@joaquin.guerrero/adding-bulk-users-to-teams-private-channels-8c9c8e563900


	Connect-MicrosoftTeams

	Get-Team -User "zID@ad.unsw.edu.au" // then copy the GroupId that you want to add private channels to

	Import-Csv -Path “YOUR_FILE_PATH” | foreach{Add-TeamChannelUser -GroupId YOUR_TEAM_ID -DisplayName $_.cname -user $_.email}

==================================================================================================================================
