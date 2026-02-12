This Tutorial will show you the way to integrate PH_Vehiclekeys into JG-Dealership and JG_AdvancedGarages.

Lets start with JG-AdvancedGarages:
1. Open the config-cl.lua
2. insert 
```
local function normPlate(p)
  p = (p or ''):gsub('^%s+',''):gsub('%s+$','')
  p = p:gsub('%s+', ''):upper()
  return p
end

local function hasKeyViaExport(plate)
  if GetResourceState('PH_VehicleKeys') ~= 'started' then
    print('[JG-AG] PH_VehicleKeys not started skipped Keycheck.')
    return true
  end
  local ok, res = pcall(function()
    return exports['PH_VehicleKeys']:HasKeyClient(plate)
  end)
  return ok and res == true
end
```

this on the very top of config-cl.lua

3. Open config.lua and replace yours with that ```Config.Vehiclekeys = "PH_Vehiclekeys"```

4.restart your server and test.

Now lets integrate it into JG-Dealerships

1.open your config.lua and change ```Config.VehicleKeys =""```  to  ```Config.VehicleKeys = "PH_Vehiclekeys"```

2. Open cl-functions.lua and find ```exports["Renewed-Vehiclekeys"]:addKey(plate)``` and directly below add this 
```elseif Config.VehicleKeys == "PH_Vehiclekeys" then
    local isKeyless = false
if exports['PH_Vehiclekeys'] and exports['PH_Vehiclekeys'].IsKeylessModel then
    isKeyless = exports['PH_Vehiclekeys']:IsKeylessModel(model)
elseif Config and Config.KeylessModels then
    -- Fallback: check if table exists
    local hash = (type(model) == 'number') and model or GetHashKey(model)
    isKeyless = (Config.KeylessModels[hash] == true)
        or (type(model) == 'string' and Config.KeylessModels[model] == true)
end

local keyType = isKeyless and 'keyless' or 'standard'

--client to Server so you get the keys.
local ok, why = exports['PH_Vehiclekeys']:DealerGiveKey(plate, keyType, true)
```

if you have JS-Dealerships stock that will be in line 298-312 now.

4. restart your server and test if you get the keys.

If you have trouble installing it or you get errors feel free to open a ticket :slight_smile:


sadly i currently need to give you this as a Install file since i do not have a Docs or Website currently. 
But since i look forward to make my scripts easy to install i think we are good with this for now.
