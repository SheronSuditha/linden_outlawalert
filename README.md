# mdt_outlawalert
Intended for use with [ESX] Hypaste RP’s Mobile Data Terminal by distritic, but it is not required
<img src="https://i.imgur.com/4wZ6YhR.png"></img>  
Note: If upgrading from v1 you will need to update the mdt:newCall event once again. Any custom alerts will also need updating.


### Instructions
  Download and setup Hypaste RP's MDT (link in credits)  
  Open mdt/sv_mdt.lua and locate RegisterServerEvent("mdt:newCall"), replace with:  
  ```
RegisterServerEvent("mdt:newCall")
AddEventHandler("mdt:newCall", function(details, caller, coords, sendNotification)
  call_index = call_index + 1
  local xPlayers = ESX.GetPlayers()
  for i= 1, #xPlayers do
  	local source = xPlayers[i]
  	local xPlayer = ESX.GetPlayerFromId(source)
  	if xPlayer.job.name == 'police' then
		if sendNotification ~= false then
			TriggerClientEvent("InteractSound_CL:PlayOnOne", source, 'demo', 1.0)
			TriggerClientEvent("mythic_notify:client:SendAlert", source, {type="inform", text="You have received a new call.", 5000, style = { ['background-color'] = '#ffffff', ['color'] = '#000000' }})
		end
  		TriggerClientEvent("mdt:newCall", source, details, caller, coords, call_index)
  	end
  end
end)
```  
### Custom alerts  
Firstly, go into server.lua and add the new alert into the dispatchTables table.
```
    vangelico = {displayCode = '211', description = 'Robbery', isImportant = 0, recipientList = {'police'}, length = '10000',
    infoM = 'fa-info-circle', info = 'Vangelico Jewelry Store'},
```
You can also define infoM2 and info2 for an additional line of text.  

Create your event trigger
```
    data = {dispatchCode = 'vangelico', caller = 'Alarm', street = 'Portola Dr, Rockford Hills', coords = vector3(-633.9, -241.7, 38.1)}
    TriggerEvent('wf-alerts:svNotify', data)
```


# Credits:

  Jager_bom for esx_outlawalert  
  Stroudy for WF_Alerts https://forum.cfx.re/t/dev-release-standalone-wf-alerts/1029331  
  distritic for Hypaste RP's MDT https://forum.cfx.re/t/esx-hypaste-rps-mobile-data-terminal-reports-warrants-calls-searches-more/1701472/1 
