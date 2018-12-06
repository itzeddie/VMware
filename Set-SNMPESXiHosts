###############################################################################################
# Configure SNMP for All Hosts
###############################################################################################

$ErrorAction = "Stop"

$UserAdminCredentials = Get-Credential -Message "Enter Admin Account Credentials"
$ESXiAdminCredentials = Get-Credential -Message "Enter ESXi Root User and Password"
$vCenter = Read-Host "Enter the FQDN of the vCenter Server"

try {
	Connect-VIServer -Server $vCenter -Credential $UserAdminCredentials -ErrorAction $ErrorAction -Force
	}
catch {
	Write-Host "Unable to connect to the vCenter Server" -ForegroundColor Red
	Write-Host $_.Exception.Message
	}
	
$VMHosts = Get-VMHost | Where-Object {$_.PowerState -eq 'PoweredOn'}

ForEach ($VMHost in $VMHosts) {
	Try {
		Connect-VIServer -Server $VMHost -Credential $ESXiAdminCredentials -ErrorAction $ErrorAction -Force
		$SNMPStatus = Get-VMHostSnmp
		If ($SNMPStatus.Enabled -eq $false) {
			Write-Host "SNMP Not Configured for $VMHost." -ForegroundColor Green
			Get-VMHostSnmp | Set-VMHostSnmp -enabled:$true -ReadOnlyCommunity "piNo73Nevv"
			}
		Else {
			Write-Host "SNMP Configured for $VMHost. Skipping to the next host." -ForegroundColor Yellow
			
			}
		Disconnect-VIServer -Confirm:$false
		}
	Catch {
		Write-Host "Unable to connect to $VMHost. Skipping to the next host." -ForegroundColor Red
		Write-Host $_.Exception.Message -ForegroundColor Red
	}
}
