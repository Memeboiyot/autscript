local Knit = require(game:GetService("ReplicatedStorage").ReplicatedModules.KnitPackage.Knit)
local TraitService = Knit.GetService("TraitService")



local traitHandConnection



-- Function to start listening for the trait data event
local function startTraitConnection()
    -- If there's already a connection, disconnect it first
    if traitHandConnection then
        traitHandConnection:Disconnect()
        print("Previous TraitHand connection disconnected.")
    end

    -- Now, create the new connection
    traitHandConnection = TraitService.TraitHand:Connect(function(traitData)
        -- Log the trait data to the output for debugging
        print("Received traitData:", traitData)
        
        -- Handle the trait data as before
        if traitData[1] then 
            getgenv().Trait1 = traitData[1].Trait
            print("Trait1:", getgenv().Trait1)
        end
        if traitData[2] then 
            getgenv().Trait2 = traitData[2].Trait
            print("Trait2:", getgenv().Trait2)
        end
        if traitData[3] then 
            getgenv().Trait3 = traitData[3].Trait
            print("Trait3:", getgenv().Trait3)
        end
    end)

    print("New TraitHand connection established.")
end

-- Call this function to start or restart the connection
startTraitConnection()



getgenv().SecureMode = true
local Rayfield = loadstring(game:HttpGet('https://raw.githubusercontent.com/SiriusSoftwareLtd/Rayfield/main/source.lua'))()

_G.chest = false
_G.GroundItems = false

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local PromptButtonHoldBegan = nil
local ProximityPromptService = game:GetService("ProximityPromptService")
PromptButtonHoldBegan = ProximityPromptService.PromptButtonHoldBegan:Connect(function(prompt)
			fireproximityprompt(prompt)
		end)

-- Folder containing NPCs
local npcFolder = workspace:WaitForChild("NPCS") -- Change "NPCFolder" to your folder's name
local delayBetweenTeleports = 0.00001 -- Time in seconds between teleports

local function teleportToNPCs()
    for _, npc in ipairs(npcFolder:GetChildren()) do
        if npc:IsA("Model") then
            local targetCFrame = npc:GetPivot()
          --  character:PivotTo(targetCFrame)
            print("Teleported to:", npc.Name)
            task.wait(delayBetweenTeleports)
        else
            print("Skipping:", npc.Name, "- Missing HumanoidRootPart")
        end
    end
end




local function domove(val)


    local args = { [1] = val }
    game:GetService("ReplicatedStorage"):WaitForChild("ReplicatedModules")
        :WaitForChild("KnitPackage"):WaitForChild("Knit")
        :WaitForChild("Services"):WaitForChild("MoveInputService")
        :WaitForChild("RF"):WaitForChild("FireInput"):InvokeServer(unpack(args))
    task.wait(0.235)
 
end

local Window = Rayfield:CreateWindow({
    Name = "Syntax AUT",
    LoadingTitle = "ur a hoe",
    LoadingSubtitle = "by Null",
    ConfigurationSaving = {
        Enabled = false,
        FolderName = nil,
        FileName = "Big Hub"
    },
    Discord = {
        Enabled = false,
        Invite = "noinvitelink",
        RememberJoins = true
    },
    KeySystem = false,
    KeySettings = {
        Title = "Untitled",
        Subtitle = "Key System",
        Note = "No method of obtaining the key is provided",
        FileName = "Key",
        SaveKey = true,
        GrabKeyFromSite = false,
        Key = {"Hello"}
    }
})

Window.ModifyTheme('Default')


-- Start teleportation
teleportToNPCs()


local Tab = Window:CreateTab("Main", 4483362458)
getgenv().Enemy = "Curses"

local runService = game:GetService("RunService")
local HeartbeatConnections = {}

local function disconnectAllHeartbeats()
    for _, conn in ipairs(HeartbeatConnections) do
        conn:Disconnect()
    end
    HeartbeatConnections = {}
end

local function debounce(callback, delay)
    local lastCall = 0
    return function(...)
        local now = tick()
        if now - lastCall >= delay then
            lastCall = now
            callback(...)
        end
    end
end

local function clearHeartbeatConnections()
    for _, connection in ipairs(HeartbeatConnections) do
        if connection then
            connection:Disconnect()
        end
    end
    table.clear(HeartbeatConnections)  -- Reset the table
end


clearHeartbeatConnections()

local function createHeartbeatConnection(callback, interval)
    local connection = runService.Heartbeat:Connect(debounce(callback, interval))
    table.insert(HeartbeatConnections, connection)
    return connection
end

local function createToggle(name, interval, Tab, action)
    return Tab:CreateToggle({
        Name = name,
        CurrentValue = false,
        Callback = function(Value)
            getgenv()[name] = Value

            if getgenv()[name] then
                local actionConn = createHeartbeatConnection(function()
                    pcall(action)
                end, interval)

                getgenv()[name .. "Connection"] = actionConn
            else
                if getgenv()[name .. "Connection"] then
                    getgenv()[name .. "Connection"]:Disconnect()
                    getgenv()[name .. "Connection"] = nil
                end
            end
        end
    })
end

local farmingHeartbeatConnection

local function disconnectFarmingHeartbeat()
    if farmingHeartbeatConnection then
        farmingHeartbeatConnection:Disconnect()
        farmingHeartbeatConnection = nil
    end
end

local function findLowestHealthEnemy()
    local player = game.Players.LocalPlayer
    local lowestHealthEnemy = nil
    local lowestHealth = math.huge

    -- Skip if chest handling is active
    if _G.chest  then
        return nil
    end

if _G.GroundItems  then
        return nil
    end

    -- Priority Boss handling
    if getgenv().PrioBoss then
        for _, v in ipairs(game:GetService("Workspace").Living:GetChildren()) do
          if v:IsA("Model") and v:FindFirstChild('HumanoidRootPart') and v:FindFirstChild('Humanoid') then
                local humanoid = v.Humanoid
                if table.find(getgenv().PrioBossEnemy, v.Name) and humanoid.Health > 0 and v.Name ~= player.Name then
                    if humanoid.Health < lowestHealth then
                        lowestHealth = humanoid.Health
                        lowestHealthEnemy = v
                    end
                end
            end
        end

        -- Return if a priority boss is found
        if lowestHealthEnemy then
            return lowestHealthEnemy
        end
    end

    -- Regular enemy handling
    for _, v in ipairs(game:GetService("Workspace").Living:GetChildren()) do
        if v:IsA("Model") and v:FindFirstChild('Humanoid') then
            local humanoid = v.Humanoid
            local validEnemy = false

            -- Check for specific enemy types based on settings
            if getgenv().Enemy == "Pirates" and v.Name == "Pirate" then
                validEnemy = true
            elseif getgenv().Enemy == "Curses" and (v.Name == "Jujutsu Sorcerer" or v.Name == "Flyhead" or v.Name == "Mantis Curse" or v.Name == "Roppongi Curse" or v.Name == "Curse User") then
                validEnemy = true
            elseif getgenv().Enemy == "Tower" and (v.Name == "Jujutsu Sorcerer" or v.Name == "Flyhead" or v.Name == "Mantis Curse" or v.Name == "Roppongi Curse" or v.Name == "Curse User" or v.Name == "Thug" or v.Name == "Star Platinum Stand User"  or v.Name == "Magician's Red Stand User" or v.Name == "King Crimson Stand User"  or v.Name == "Hooligan" )  then
                validEnemy = true
            end

            -- Find lowest health among valid enemies
            if validEnemy and v.Name ~= player.Name then
                if humanoid.Health < lowestHealth then
                    lowestHealth = humanoid.Health
                    lowestHealthEnemy = v
                end
            end
        end
    end

    -- Return the enemy with the lowest health
    return lowestHealthEnemy
end



local runService = game:GetService("RunService")
local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local humanoid = char:WaitForChild("Humanoid")

local canEvade = true
local isEvading = false

local damageEvadeTriggered = false

local function hasDamageIndicator()
    for _, gui in ipairs(char:GetDescendants()) do
        if gui:IsA("BillboardGui") and gui.Name == "DamageIndicator" then
            return true
        end
    end
    return false
end

local function startFarming()
    disconnectFarmingHeartbeat()

    if getgenv().EnmFarm and not _G.chest and not _G.GroundItems then
        farmingHeartbeatConnection = runService.Heartbeat:Connect(debounce(function()
            pcall(function()
                if _G.chest or _G.GroundItems then return end
                local rootPart = char:FindFirstChild("HumanoidRootPart")
                local lowestHealthEnemy = findLowestHealthEnemy()

                -- Trigger evade ONCE per DamageIndicator
                if not damageEvadeTriggered and canEvade and hasDamageIndicator() and lowestHealthEnemy and rootPart and getgenv().evaded == true then
                    canEvade = false
                    isEvading = true
                    damageEvadeTriggered = true



                    local originalCFrame = rootPart.CFrame
                    local evadeCFrame = lowestHealthEnemy.HumanoidRootPart.CFrame * CFrame.new(0, 10000, 0)
               
                    rootPart.CFrame = evadeCFrame
                    task.wait(0.2)
     rootPart.Anchored = true

                    task.delay(getgenv().EvadeTime, function()
                        if rootPart and char and char.Parent then
                            rootPart.CFrame = originalCFrame
                            rootPart.Anchored = false
                            isEvading = false
                        end
                    end)

                    task.delay(getgenv().EvadeCool, function()
                        canEvade = true
                        damageEvadeTriggered = false
                    end)
                end

            
                if isEvading then return end

                if lowestHealthEnemy then

local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local humanoid = char:WaitForChild("Humanoid")

                    workspace.CurrentCamera.CameraSubject = humanoid
                    humanoid:ChangeState(11)
                    local destinationCFrame = lowestHealthEnemy:GetPivot()
                    player.Character:PivotTo(destinationCFrame * CFrame.new(0, 0, getgenv().Distance))
                    domove("MouseButton1")
                else
                    local farmingLocations = {
                        Pirates = CFrame.new(-3400.68848, 917.345886, 15200.5371),
                        Curses = CFrame.new(1997.1637, 943.199585, -1491.11816),
                        Tower = CFrame.new(27.7380543, 958.250732, -2153.10376)
                    }
                    local farmCFrame = farmingLocations[getgenv().Enemy]
                    if farmCFrame then
                        rootPart.CFrame = farmCFrame
                    end
                end
            end)
        end, 0.01))
    end
end





local EnemyDropdown = Tab:CreateDropdown({
    Name = "Enemy Select",
    Options = {"Curses", "Pirates", "Tower"},
    CurrentOption = "Curses",
    Callback = function(Dropdown)
        local selectedEnemy = table.concat(Dropdown, "%")
        print(selectedEnemy)
        getgenv().Enemy = selectedEnemy

        -- Stop and restart farming when enemy type is switched
        disconnectFarmingHeartbeat()
        if getgenv().EnmFarm and not _G.chest and not _G.GroundItems then
            startFarming()
        end
    end
})

local EnemyToggle = Tab:CreateToggle({
    Name = "Enemy Farm",
    CurrentValue = false,
    Callback = function(Value)
        getgenv().EnmFarm = Value
        if Value and not _G.chest or not _G.GroundItems then
            startFarming()
        else
            disconnectFarmingHeartbeat()
        end
    end
})


local Bosssssssss = Tab:CreateSection("Bosses")


local Label = Tab:CreateLabel("Make sure Enemy Farm is turned ON", 4483362458, Color3.fromRGB(255, 255, 255), false) -- Title, Icon, Color, IgnoreTheme

local PrioBoss = Tab:CreateToggle({
    Name = "Priority Boss",
    CurrentValue = false,
    Callback = function(Value)
        getgenv().PrioBoss = Value
        print(getgenv().PrioBossEnemy)
    end
})


getgenv().PrioBossEnemy = {}
local PrioBossDropdown = Tab:CreateDropdown({
    Name = "Boss Select",
    Options = {"Surgeon of Death", "The Bearer", "Kars", "Whitebeard", "Diavolo, The Boss","The Clown", "Shanks", "Luffy", "The Honored One", "The Strongest Of Today", "The Strongest In History", "The Vessel", "The Sorcerer Killer", "Eight-Handled Sword Divergent Sila Divine General Mahoraga", "Grimpus", "The Sovereign", "The Spy"},
    CurrentOption = {},
    MultipleOptions = true,
    Callback = function(Dropdown)
        getgenv().PrioBossEnemy = Dropdown -- Directly assign the selected bosses
        print(table.concat(getgenv().PrioBossEnemy, ", "))
    end
})

local evade = Tab:CreateSection("Evading Settings")


getgenv().evaded = false
local evadetog = Tab:CreateToggle({
   Name = "Toggle Evade",
   CurrentValue = false,
   Callback = function(Value)
   
getgenv().evaded = Value

   end,
})


getgenv().EvadeTime = 6
local EvadeTime = Tab:CreateSlider({
   Name = "Evade Time",
   Range = {1, 15},
   Increment = 1,
   Suffix = "Seconds",
   CurrentValue = 6,
   Callback = function(Value)
        getgenv().EvadeTime = Value
   end,
})


getgenv().EvadeCool = 1
local EvadeCool = Tab:CreateSlider({
   Name = "Evade Cooldown",
   Range = {1, 15},
   Increment = 1,
   Suffix = "Seconds",
   CurrentValue = 1,
   Callback = function(Value)
        getgenv().EvadeCool = Value
   end,
})




local others = Tab:CreateSection("FarmExtra")



getgenv().Distance = 6
local Slider = Tab:CreateSlider({
   Name = "Distance",
   Range = {1, 15},
   Increment = 1,
   Suffix = "Bananas",
   CurrentValue = 6,
   Flag = "Slider1", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
   Callback = function(Value)
        getgenv().Distance = Value
   end,
})

local oNESHOT = Tab:CreateToggle({
    Name = "OneShot Semi",
    CurrentValue = false,
    Callback = function(Value)
getgenv().AutoOneShotting = Value

    local connection
    connection = game:GetService("RunService").RenderStepped:Connect(function()
        if getgenv().AutoOneShotting == true then
            pcall(function()
sethiddenproperty(game.Players.LocalPlayer, "SimulationRadius", 112412400000)
sethiddenproperty(game.Players.LocalPlayer, "MaxSimulationRadius", 112412400000)

                for _,k in ipairs(workspace.Living:GetChildren()) do
                    if k:IsA("Model") and k:FindFirstChild("Head") and k.Head:IsA("Part") and k.Head.Name == "Head" and k.Head ~= game.Players.LocalPlayer.Character.Head then
                        if (k.Head.Position - game.Players.LocalPlayer.Character.Head.Position).magnitude <= 75 then
                            if k:FindFirstChildOfClass("Humanoid").Health ~= k:FindFirstChildOfClass("Humanoid").MaxHealth then k:FindFirstChildOfClass("Humanoid").Health = 0;end
                        end
                    end
                end
            end)
        else
            connection:Disconnect();
        end
    end)



    end
})



local reset = Tab:CreateButton({
   Name = "Reset and Stop ",
   Callback = function()
        EnemyToggle:Set(false)
        local deathmanok = game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart").Position
        game.Players.LocalPlayer.Character:FindFirstChild("Humanoid").Health = 0
        wait(3.5)
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(deathmanok)
   end
})



local movess = Tab:CreateSection("Do Moves")




local AbilityAtuo = Tab:CreateToggle({
    Name = "Auto Equip Stand",
    CurrentValue = false,
    Callback = function(Value)
        getgenv().Equip = Value
  
task.spawn(function()
    while getgenv().Equip == true do
        pcall(function()
            if not workspace.Living[game.Players.LocalPlayer.Name].StatesFolder.StandOn.Value == true then
                local args = {[1] = "Q"};
                game:GetService("ReplicatedStorage"):WaitForChild("ReplicatedModules"):WaitForChild("KnitPackage"):WaitForChild("Knit"):WaitForChild("Services"):WaitForChild("MoveInputService"):WaitForChild("RF"):WaitForChild("FireInput"):InvokeServer(unpack(args)); 
            end
        end)
        task.wait(0.45);
    end
end)
    end
})



local Roomo = Tab:CreateToggle({
    Name = "Auto Ope Room",
    CurrentValue = false,
    Callback = function(Value)
        getgenv().Room = Value
  
task.spawn(function()
    while getgenv().Room == true do
        pcall(function()
     
           if not game.Players.LocalPlayer.Character:GetAttribute("Room") == true  then

local ohString1 = "END-Q"

game:GetService("ReplicatedStorage").ReplicatedModules.KnitPackage.Knit.Services.MoveInputService.RF.FireInput:InvokeServer(ohString1)

                local args = {[1] = "Q"};
                game:GetService("ReplicatedStorage"):WaitForChild("ReplicatedModules"):WaitForChild("KnitPackage"):WaitForChild("Knit"):WaitForChild("Services"):WaitForChild("MoveInputService"):WaitForChild("RF"):WaitForChild("FireInput"):InvokeServer(unpack(args)); 

wait(1)
        local ohString1 = "END-Q"
           game:GetService("ReplicatedStorage").ReplicatedModules.KnitPackage.Knit.Services.MoveInputService.RF.FireInput:InvokeServer(ohString1)
end
        end)
        task.wait(1);
    end
end)
    end
})


-- Key Mappings: The first value is the key, and the second is the wait time (interval) for that key
local keyMappings = {
    {"Q", 1},
    {"E", 0.2},
    {"R", 0.2},
    {"T", 0.2},
    {"Y", 0.2},
    {"G", 0.2},
    {"H", 0.2},
    {"Z", 0.2},
    {"B", 0.2},
    {"V", 0.2},
    {"U", 0.2},
}

local selectedKeys = {}  -- Holds the selected keys for the loop
local keyCooldowns = {}  -- Track cooldown for each key

-- Function to create the toggle for starting the loop
local function createKeyLoopToggle()
    getgenv().isLooping = false  -- Set initial state of the loop to false (off)

    return Tab:CreateToggle({
        Name = "Start Key Loop",
        CurrentValue = false,
        Callback = function(Value)
            getgenv().isLooping = Value  -- Toggle the state of the loop (on/off)
            
            -- Loop that handles key press and respects key intervals
            createHeartbeatConnection(function()
                pcall(function()
                    local hasEnemy = findLowestHealthEnemy() ~= nil
                    local isFarming = getgenv().EnmFarm
                    local noChest = _G.chest
                    local noGroundItems = _G.GroundItems

                    -- Check conditions to keep running the key press logic
                    if not noChest and not noGroundItems and getgenv().isLooping then
                        -- Loop through the selected keys and their respective intervals
                        for _, key in ipairs(selectedKeys) do
                            -- Find the key mapping in keyMappings
                            for _, mapping in ipairs(keyMappings) do
                                if mapping[1] == key then
                                    local waitTime = mapping[2]  -- Retrieve the wait time for the selected key
                                    
                                    -- Check if the key is still on cooldown
                                    if not keyCooldowns[key] or (tick() - keyCooldowns[key] >= waitTime) then
                                        -- Key press function with the respective wait time
                                        domove(key)
                                        keyCooldowns[key] = tick()  -- Set the cooldown time for this key

                                        wait(0.1)
                                        domove(key .. "+")
                                        wait(0.1)
                                        domove(key .. "^")
                                        wait(0.1)
                                        domove(key .. "-")
                                        wait(0.1)
                                    end
                                    break  -- Break after processing the key with its wait time
                                end
                            end
                        end
                    end
                end)
            end, 0.1)  -- Heartbeat interval (control the frequency of checks)
        end
    })
end

-- Create the dropdown for selecting keys
local Dropdown = Tab:CreateDropdown({
    Name = "Select Keys",
    Options = {"Q", "E", "R", "T", "Y", "G", "H", "Z", "B", "V", "U"},
    CurrentOption = {"Q"},  -- Default selected option
    MultipleOptions = true,  -- Allow multiple keys to be selected
    Flag = "KeyDropdown",  -- Unique flag for saving settings
    Callback = function(Options)
        selectedKeys = Options  -- Update selected keys
    end,
})

-- Create the toggle for starting the loop
createKeyLoopToggle()



_G.chest = false
local chest = Tab:CreateSection("Chests/Items")

                   
local Toggle = Tab:CreateToggle({
    Name = "Auto Open Chests",
    CurrentValue = false,
    Callback = function(Value)
        getgenv().AutoOpenChests = Value
        if getgenv().AutoOpenChests then
            local openchest = createHeartbeatConnection(function()
                pcall(function()
                    local foundChest = false
                    -- Loop through all descendants in workspace
                    for _, v in ipairs(game:GetService("Workspace"):GetDescendants()) do
                        -- Check if part name contains "_Chest" and is a BasePart
                        if string.find(v.Name, "_Chest") and v:IsA("BasePart") then
                            -- Look for ProximityAttachment in the part
                            local proximityAttachment = v:FindFirstChild("ProximityAttachment")
                            if proximityAttachment then
                                foundChest = true
 _G.chest = true
                                -- Move player to chest's location
                                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame
                                task.wait(0.4) -- Allow some time for movement
                                -- Start interaction with ProximityAttachment
                   

  proximityAttachment.Interaction:InputHoldBegin()
game:GetService("Players").LocalPlayer.PlayerGui.UI.Gameplay.ChestRoll.Visible = false
                                task.wait(1.5) -- Adjust timing as needed
                                -- Only destroy the chest after interacting with it
                                v:Destroy()

game:GetService("Players").LocalPlayer.PlayerGui.UI.Gameplay.ChestRoll.Visible = false
   _G.chest    = false
                                break -- Stop the loop once a chest is opened and destroyed
                            end
                        end
                    end
                end)
            end, 0.5) -- Check every 0.5 seconds
            getgenv().openchest = openchest
        else
            if getgenv().openchest then
                getgenv().openchest:Disconnect()
                getgenv().openchest = nil
            end
            _G.chest = false -- Reset chest state
        end
    end
})


 _G.GroundItems = false


local Toggle = Tab:CreateToggle({
    Name = "Auto Ground Items",
    CurrentValue = false,
    Callback = function(Value)
        getgenv().GroundItems = Value

        if getgenv().GroundItems then
            local groundItemConnection = createHeartbeatConnection(function()
                pcall(function()
                    for _, v in ipairs(workspace.ItemSpawns.StandardItems:GetDescendants()) do     
    if v:IsA("BasePart") then      
   local proximityAttachment = v:FindFirstChild("ProximityAttachment")
                            if proximityAttachment then

                           _G.GroundItems = true
                            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.Parent.CFrame
                            task.wait(0.4) -- Allow some time for movement

                           proximityAttachment.Interaction:InputHoldBegin()
Rayfield:Notify({
   Title = v.Name,
   Content = "Picked upp",
   Duration = 6.5,
   Image = 4483362458,
})

task.wait(1.5)
 _G.GroundItems = false
                            task.wait(0.1) -- Adjust timing as needed
                            break -- Stop after interacting with one item
end
end
                    end
                end)
            end, 0.7) -- Check every 0.5 seconds
            getgenv().groundItemConnection = groundItemConnection
        else
            if getgenv().groundItemConnection then
                getgenv().groundItemConnection:Disconnect()
                getgenv().groundItemConnection = nil
            end
            _G.GroundItems = false
        end
    end
})


local Extra = Window:CreateTab("Extra", 4483362458)





local Traito = Extra:CreateSection("Traits")


getgenv().WantedThoseTraits = {"None"}
local Dropdown = Extra:CreateDropdown({
   Name = "Traits",
   Options = {"Poisonous","Prime","Overconfident Prime","Solar","Icarus Solar","Cursed","Undying Cursed","Vampiric","Ancient Vampiric","Gluttonous","Festering Gluttonous","Voided","Abyssal Voided","Gambler","Idle Death Gambler","Overflowing","Torrential Overflowing","Deferred","Fractured Deferred","True","Vitriolic True","Cultivation","Soul Reaping Cultivation","Economic","Greedy Economic","Angelic","Fallen Angelic","Godly","Egotistic Godly","Temporal","FTL Temporal","Spiritual","Mastered Spiritual","Ryoiki","Heavenly Restricted Ryoiki","RCT","Automatic RCT","Adaptation","Unbound Adaptation","Bypassing"},
   CurrentOption = {},
   MultipleOptions = true,
   Callback = function(Option)
getgenv().WantedThoseTraits = Option
   end,
})


getgenv().LoadPerdic = false


-- Initialize the processLog table if it's not already initialized
local processLog = processLog or {}

local function printProcessLog()
    if #processLog == 0 then
        print("No process logs available.")
        return
    end

    for _, entry in ipairs(processLog) do
        -- Ensure the entry has the expected fields
        print(string.format("Trait: %s, Action: %s, Success: %s, Time: %s",
            entry.Trait or "Unknown Trait", 
            entry.Action or "Unknown Action", 
            tostring(entry.Success), 
            os.date("%X", entry.Time)
        ))
    end
end


-- Create the main ScreenGui for the traits UI
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "TraitsGui"
screenGui.Parent = game.Players.LocalPlayer.PlayerGui  -- Parent to the player's GUI

-- Create a Frame to hold the labels for the traits
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 300, 0, 180)  -- Increased height for "Trait Acquired" message
frame.Position = UDim2.new(0.5, -150, 0.5, -90)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BackgroundTransparency = 0.5
frame.Parent = screenGui

-- Create the Trait Labels (for Trait1, Trait2, and Trait3)
local traitLabel1 = Instance.new("TextLabel")
traitLabel1.Size = UDim2.new(0, 280, 0, 40)
traitLabel1.Position = UDim2.new(0, 10, 0, 10)
traitLabel1.BackgroundTransparency = 1
traitLabel1.Text = "Trait 1: None"
traitLabel1.TextColor3 = Color3.fromRGB(255, 255, 255)
traitLabel1.TextSize = 20
traitLabel1.Parent = frame

local traitLabel2 = Instance.new("TextLabel")
traitLabel2.Size = UDim2.new(0, 280, 0, 40)
traitLabel2.Position = UDim2.new(0, 10, 0, 50)
traitLabel2.BackgroundTransparency = 1
traitLabel2.Text = "Trait 2: None"
traitLabel2.TextColor3 = Color3.fromRGB(255, 255, 255)
traitLabel2.TextSize = 20
traitLabel2.Parent = frame

local traitLabel3 = Instance.new("TextLabel")
traitLabel3.Size = UDim2.new(0, 280, 0, 40)
traitLabel3.Position = UDim2.new(0, 10, 0, 90)
traitLabel3.BackgroundTransparency = 1
traitLabel3.Text = "Trait 3: None"
traitLabel3.TextColor3 = Color3.fromRGB(255, 255, 255)
traitLabel3.TextSize = 20
traitLabel3.Parent = frame

-- Text label for the "Trait Acquired" message
local traitAcquiredLabel = Instance.new("TextLabel")
traitAcquiredLabel.Size = UDim2.new(0, 280, 0, 40)
traitAcquiredLabel.Position = UDim2.new(0, 10, 0, 130)
traitAcquiredLabel.BackgroundTransparency = 1
traitAcquiredLabel.Text = ""
traitAcquiredLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
traitAcquiredLabel.TextSize = 24
traitAcquiredLabel.Font = Enum.Font.SourceSansBold
traitAcquiredLabel.TextXAlignment = Enum.TextXAlignment.Center
traitAcquiredLabel.Visible = false
traitAcquiredLabel.Parent = frame

-- Variable to hold the last selected trait to avoid repeating the notification
local lastSelectedTrait = nil

-- Process Log Table
local processLog = {}

-- Function to update the trait labels and check if a trait matches
local function updateTraitLabels()
    traitLabel1.Text = "Trait 1: " .. (getgenv().Trait1 or "None")
    traitLabel2.Text = "Trait 2: " .. (getgenv().Trait2 or "None")
    traitLabel3.Text = "Trait 3: " .. (getgenv().Trait3 or "None")
end

-- Function to check if any of the traits match the wanted traits
local function hasWantedTrait()
    print("Checking traits:", getgenv().Trait1, getgenv().Trait2, getgenv().Trait3)
    return table.find(getgenv().WantedThoseTraits, getgenv().Trait1) or 
           table.find(getgenv().WantedThoseTraits, getgenv().Trait2) or 
           table.find(getgenv().WantedThoseTraits, getgenv().Trait3)
end

local function logProcess(trait, action, success)
    -- Ensure the trait is not nil before logging it
    if trait == nil then
        trait = "Unknown Trait"  -- Default to a safe value if trait is nil
    end

    -- Insert the log entry into the processLog table
    table.insert(processLog, {
        Trait = trait,
        Action = action,
        Success = success,
        Time = os.time()
    })


end


-- Auto reroll function
local function autoReroll()
    while getgenv().Jex do  -- While auto trait is active
        local selectedTrait = nil
        local success = false

        -- First, try to select a trait (regardless of whether it's wanted)
        if getgenv().Trait1 and table.find(getgenv().WantedThoseTraits, getgenv().Trait1) then
            print("Trying trait 1:", getgenv().Trait1)
            game:GetService("ReplicatedStorage").ReplicatedModules.KnitPackage.Knit.Services.TraitService.RF.PickTrait:InvokeServer(1)
            selectedTrait = getgenv().Trait1
            success = true
        elseif getgenv().Trait2 and table.find(getgenv().WantedThoseTraits, getgenv().Trait2) then
            print("Trying trait 2:", getgenv().Trait2)
            game:GetService("ReplicatedStorage").ReplicatedModules.KnitPackage.Knit.Services.TraitService.RF.PickTrait:InvokeServer(2)
            selectedTrait = getgenv().Trait2
            success = true
        elseif getgenv().Trait3 and table.find(getgenv().WantedThoseTraits, getgenv().Trait3) then
            print("Trying trait 3:", getgenv().Trait3)
            game:GetService("ReplicatedStorage").ReplicatedModules.KnitPackage.Knit.Services.TraitService.RF.PickTrait:InvokeServer(3)
            selectedTrait = getgenv().Trait3
            success = true
        end

        -- Now check if the picked trait is one of the wanted traits
        if hasWantedTrait() then
            -- If we have a wanted trait, we notify and update the UI
            if selectedTrait and selectedTrait ~= lastSelectedTrait then
                print("Selected wanted trait:", selectedTrait)
                -- Update the acquired trait label
                traitAcquiredLabel.Text = "Trait Acquired: " .. selectedTrait
                traitAcquiredLabel.Visible = true
                -- Notify via Rayfield
                Rayfield:Notify({
                    Title = "Trait",
                    Content = "Selecting trait: " .. selectedTrait,
                    Duration = 3,
                    Image = 4483362458,
                })
                lastSelectedTrait = selectedTrait
                logProcess(selectedTrait, "Select", true)  -- Log successful selection
            end
        else
            -- If no wanted trait, discard and reroll
            print("Discarding traits...")
            game:GetService("ReplicatedStorage").ReplicatedModules.KnitPackage.Knit.Services.TraitService.RF.DiscardTraits:InvokeServer()
       --     logProcess(selectedTrait, "Discard", false)  -- Log unsuccessful discard
        end
   --     printProcessLog()
        updateTraitLabels()  -- Update the trait labels after every check
        task.wait(0.001)  -- Delay before checking again
    end
end



-- Example: Starting the Auto Trait function with a toggle
local ToggleTrait = Extra:CreateToggle({
    Name = "Auto Trait",
    CurrentValue = false,
    Callback = function(Value)
        getgenv().Jex = Value

        if getgenv().Jex then
            -- Re-run the trait checking process after toggling
            autoReroll()  -- Start rerolling if the toggle is true
        else
            print("Auto Trait stopped.")
        end
    end
})



print("a3")

local Keybind = Extra:CreateKeybind({
   Name = "TraitStart/Stop Keybind",
   CurrentKeybind = "L",
   HoldToInteract = false,
  
   Callback = function(Keybind)
  
if getgenv().Jex then
ToggleTrait:Set(false)
else
ToggleTrait:Set(true)
end

   end,
})

local LevelServices = Extra:CreateSection("LevelService")

getgenv().Rollled = false
getgenv().AutoShards = false

-- Toggle for Auto Delete Skins with improved performance
local AutoShardsTogle = Extra:CreateToggle({
    Name = "Auto Shards into Level(Uses USHARDS for XP)",
    CurrentValue = false,
    Callback = function(Value)
        getgenv().AutoShards = Value
        getgenv().currentToggle = Value

        local lastUpdate = tick()

    function recursive(tbl)
 for i,v in pairs(tbl) do
  if typeof(v) == "table" then
   recursive(v)
  end
  print(i," | ",v)
 end
end


local module = {
    instanceTypes = {
        ['boolean'] = 'BoolValue',
        ['string'] = 'StringValue',
        ['number'] = 'NumberValue',
    },

    ToInstance = function(self, tbl, parent)
        for key, value in pairs(tbl) do
            if type(value) == 'table' then
                local folder = Instance.new("Folder")
                folder.Name = key
                folder.Parent = parent
                self:ToInstance(value, folder)
            else
                local valueType = self.instanceTypes[type(value)]
                if valueType then
                    local val = Instance.new(valueType)
                    val.Name = key
                    val.Value = value
                    val.Parent = parent
                end
            end
        end
    end,

    ToTable = function(self, parent)
        local function recurse(folder)
            local data = {}
            for _, child in ipairs(folder:GetChildren()) do
                data[child.Name] = child:IsA("Folder") and recurse(child) or child.Value
            end
            return data
        end
        return recurse(parent)
    end
}



local function waitForEmptyTable(tbl)
    while next(tbl) ~= nil do  -- Wait until the table has no elements
        task.wait()  -- Yield to prevent freezing the script
    end
end



local function Levelupshards()
    if not getgenv().AutoShards then return end  -- Stop if toggle is off
if game:GetService("Players").LocalPlayer:WaitForChild("Data"):WaitForChild("Ability"):GetAttribute("AbilityLevel")==200  then return end

    task.wait()

    getgenv().Rollled = true
    print("Rolling...")

    local bannerIndex = 1
    local currencyType = "UShards"
    local rollAmount = 10

    local rollResults = game:GetService("ReplicatedStorage")
        .ReplicatedModules.KnitPackage.Knit.Services.ShopService.RF
        .RollBanner:InvokeServer(bannerIndex, currencyType, rollAmount)

    -- Create visual result folder
    local resultFolder = Instance.new("Folder")
    resultFolder.Name = "RollResults"
    module:ToInstance(rollResults, resultFolder)

    -- Build shard spend table FROM the surviving roll results
    local shardsToSpend = {}

    for _, item in ipairs(resultFolder:GetChildren()) do
        local abilityId = item:FindFirstChild("AbilityId")
        local shards = item:FindFirstChild("Shards")
        if abilityId and shards then
            shardsToSpend["ABILITY_" .. abilityId.Value] = shards.Value
        end
    end

    -- Process one ability at a time
    for ability, amount in pairs(shardsToSpend) do
        while amount > 0 do
            print("Consuming shards for:", ability, "Amount:", amount)

            -- Send only the current ability
            local singleAbility = { [ability] = math.min(amount, 10) }  -- Consume in chunks (adjust as needed)
            
            game:GetService("ReplicatedStorage")
                .ReplicatedModules.KnitPackage.Knit.Services.LevelService.RF
                .ConsumeShardsForXP:InvokeServer(singleAbility)

            amount = amount - singleAbility[ability]  -- Reduce amount processed
            task.wait(0.0000000000001)  -- Give the server time to process
        end
    end

    print("All shards consumed!")

    getgenv().Rollled = false
    print("Roll complete.")
end




 
      
              while  task.wait(getgenv().StatSpeed) do
             if getgenv().AutoShards and getgenv().Rollled == false then
                    Levelupshards()
                    end
                end

    end
})























local StatSpeed = Extra:CreateSlider({
   Name = "StatSpeed",
   Range = {0.001, 1},
   Increment = 0.001,
   Suffix = "StatSpeed",
   CurrentValue = 0.1,
   Flag = "StatSpeed", 
   Callback = function(Value)
getgenv().StatSpeed = Value
print(getgenv().StatSpeed)
   end,
})



			local module = { instanceTypes = { ['boolean'] = 'BoolValue', ['string'] = 'StringValue', ['number'] = 'NumberValue' }, ToInstance = function(self, tbl, parent)
				for i, v in pairs(tbl) do
					local number = tonumber(v)
					if number then tbl[i] = number end
					if type(v) == 'table' then
						local currentFolder = Instance.new('Folder') currentFolder.Parent = parent currentFolder.Name = i self:ToInstance(v, currentFolder)
					else
						for t, class in pairs(self.instanceTypes) do
							if type(v) == t then
								local inst = Instance.new(class) inst.Name = i inst.Value = v inst.Parent = parent
							end
						end
					end
				end
			end,
			ToTable = function(self, parent)
				local function parse(newParent)
					local result = {}
					for _, v in pairs(newParent:GetChildren()) do
						if v:IsA('Folder') then result[v.Name] = parse(v)
						else result[v.Name] = v.Value end
					end
					return result
				end
				return parse(parent)
			end }

getgenv().Press = function(Path)
    if Path then
        pcall(function()
            game:GetService("GuiService").SelectedObject = Path;
            if game:GetService("GuiService").SelectedObject == Path then
                game:GetService("VirtualInputManager"):SendKeyEvent(true,Enum.KeyCode.Return,false,game);
                game:GetService("VirtualInputManager"):SendKeyEvent(false,Enum.KeyCode.Return,false,game);
            end
        end)
    else
        game:GetService("GuiService").SelectedObject = nil;
    end
end


getgenv().Storderd = false 
getgenv().IsSelling = false
local bonessssssssssssss = Extra:CreateToggle({
	Name = "UCOINS INTO USHARDS",
	CurrentValue = false,
	Callback = function(Value)
		getgenv().Bonees = Value
		getgenv().currentToggle = Value

		local lastUpdate = tick()

		local function Bonesss()


        getgenv().Storderd = false 
getgenv().IsSelling = false
        
local vm = game:GetService("Players").LocalPlayer.PlayerGui:WaitForChild("Shop"):WaitForChild("Outline"):WaitForChild("item"):WaitForChild("ViewportFrame"):FindFirstChild("WorldModel")
if vm then
	vm:Destroy()
end

local screenGui = game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui"):WaitForChild("Shop")


for _, obj in pairs(screenGui:GetDescendants()) do
	if obj:IsA("ImageLabel") or obj:IsA("ImageButton") then
		obj.ImageTransparency = 1
		obj.BackgroundTransparency = 1
	elseif obj:IsA("TextLabel") or obj:IsA("TextButton") then
		obj.TextTransparency = 1
		obj.BackgroundTransparency = 1
		if obj:FindFirstChildOfClass("UIStroke") then
			obj:FindFirstChildOfClass("UIStroke").Transparency = 1
		end
	elseif obj:IsA("Frame") then
		obj.BackgroundTransparency = 1
	elseif obj:IsA("ScrollingFrame") then
		obj.BackgroundTransparency = 1
		obj.ScrollBarImageTransparency = 1
	elseif obj:IsA("ViewportFrame") then
		obj.BackgroundTransparency = 1
		for _, v in pairs(obj:GetDescendants()) do
			if v:IsA("BasePart") then
				v.Transparency = 1
			elseif v:IsA("Decal") then
				v.Transparency = 1
			elseif v:IsA("Tool") then
				for _, part in pairs(v:GetDescendants()) do
					if part:IsA("BasePart") then
						part.Transparency = 1
					end
				end
			end
		end
	end
end



-- Resize the first frame found
local firstFrame = screenGui:FindFirstChildWhichIsA("Frame", true)
if firstFrame then
	firstFrame.Size = UDim2.new(0, 500, 0, 100) -- Adjust size as needed
end


			local player = game:GetService("Players").LocalPlayer
			local gui = player:WaitForChild("PlayerGui")
			local shop = gui:WaitForChild("Shop"):WaitForChild("Outline")
			local itemsFolder = shop:WaitForChild("items")

			shop.Position = UDim2.new(0, 500, 0, 0)
			local children = itemsFolder:GetChildren()
			if #children < 6 then return end

			local boneItem = children[6]
			if not boneItem then return end
			boneItem.Name = "bone"

			local boneButton = boneItem:FindFirstChild("but")
			local buyButton = shop:FindFirstChild("buy")
			if not boneButton or not buyButton then return end

if getgenv().Storderd == false and getgenv().IsSelling == false then


	
	local vim = game:GetService("VirtualInputManager")
	vim:SendMouseButtonEvent(boneButton.AbsolutePosition.X + boneButton.AbsoluteSize.X / 1.5, boneButton.AbsolutePosition.Y + 60, 0, true, boneButton, 4.5)
	vim:SendMouseButtonEvent(boneButton.AbsolutePosition.X + boneButton.AbsoluteSize.X / 1.5, boneButton.AbsolutePosition.Y + 60, 0, false, boneButton, 4.5)
	task.wait(0.2)

	vim:SendMouseButtonEvent(buyButton.AbsolutePosition.X + buyButton.AbsoluteSize.X / 1.5, buyButton.AbsolutePosition.Y + 60, 0, true, buyButton, 4.5)
	vim:SendMouseButtonEvent(buyButton.AbsolutePosition.X + buyButton.AbsoluteSize.X / 1.5, buyButton.AbsolutePosition.Y + 60, 0, false, buyButton, 4.5)


end


		
		
		task.wait(0.1)


for _, v in pairs(player.Backpack:GetChildren()) do

	if v:IsA("Tool") and v.Name == "Bone" and not player.PlayerGui.UI.Gameplay.Character.Info.CombatTag.Visible and  getgenv().IsSelling == false then
		local uuid = v:GetAttribute("UUID")
		if uuid == nil or uuid == "0" then continue end

     	task.wait(0.1)


		v.Name = "UNWASHEDMONEY"


		v.Parent = player.Backpack
		task.wait(0.01)
		v.Parent = player.Character
		task.wait(0.1)

task.wait(0.1)
		-- Store request
		local args = {
			[1] = {
				["AddItems"] = true
			}
		}
		game:GetService("ReplicatedStorage").ReplicatedModules.KnitPackage.Knit.Services.InventoryService.RE.ItemInventory:FireServer(unpack(args))


local removed = true

			local stillThere = false
			for _, tool in ipairs(player.Backpack:GetChildren()) do
				if tool:IsA("Tool") and tool:GetAttribute("UUID") == uuid then
					stillThere = true
				
				end
			end
			if not stillThere then
				removed = true
         
			end
			task.wait(0.1)
	


		if not removed then
	
			getgenv().Storderd = false
            task.wait(0.1)
            continue
		else
	
			getgenv().Storderd = true
			task.wait(0.1)
		end


	end
end

			-- ✅ SELL

			
        

			local invData = game:GetService("ReplicatedStorage").ReplicatedModules.KnitPackage.Knit.Services.InventoryService.RF.GetItems:InvokeServer("ItemInventory")
			local folder = Instance.new("Folder")
			module:ToInstance(invData, folder)

			for _, v in ipairs(folder:GetDescendants()) do
				if v.Name == "_ItemId" and v.Value == "ITEM_25" then
					local uuidVal = v.Parent:FindFirstChild("_UUID")
					if uuidVal then
						local uuid = uuidVal.Value
						getgenv().IsSelling = true
                          
							game:GetService("ReplicatedStorage").ReplicatedModules.KnitPackage.Knit.Services.InventoryService.RE.ItemInventory:FireServer({ ["UUID"] = uuid, ["ItemId"] = "ITEM_25" })
					task.wait(0.1)
						  print("💸💸💸")
						

                        for _, v in pairs(player.Backpack:GetChildren()) do


	if v:IsA("Tool") and v:GetAttribute("UUID") == uuid then
        v.Name = "WASHEDMONEY"
									
     local LongAssRemote = game:GetService("ReplicatedStorage")
            :WaitForChild("ReplicatedModules"):WaitForChild("KnitPackage"):WaitForChild("Knit")
            :WaitForChild("Services"):WaitForChild("ShopService"):WaitForChild("RE"):WaitForChild("Signal");
            local args = {[1] = "BlackMarketBulkSellItems",[2] = {}};
  
                    table.insert(args[2],{"ITEM_25",uuid,1});
          
            LongAssRemote:FireServer(unpack(args));
    

										
                                        task.wait(0.1)
                                        getgenv().IsSelling = false
                                           getgenv().Storderd = false
                                              task.wait(0.1)
folder:Destroy()
folder = nil

										end
									end
                                    getgenv().Storderd = false 
getgenv().IsSelling = false
				end
                end
			end
		end

		while getgenv().Bonees do
        task.wait(0.2);
			pcall(function()
				if getgenv().Bonees and  getgenv().IsSelling == false then
					Bonesss()
				end
			end)
		end

	end
})



local Other = Extra:CreateSection("Other")



createToggle("Remove Effects", 0.00045, Extra, function()
    for i = #workspace.Effects:GetChildren(), 1, -1 do
        local v = workspace.Effects:GetChildren()[i]
        v:Destroy()
    end
end)





createToggle("Auto Ascend", 0.5, Extra, function()
if game:GetService("Players").LocalPlayer:WaitForChild("Data"):WaitForChild("Ability"):GetAttribute("AbilityLevel")==200  then
    local initialPosition = game:GetService("Players").LocalPlayer.Data.Ability.Value
    local args = { [1] = initialPosition }
    game:GetService("ReplicatedStorage"):WaitForChild("ReplicatedModules"):WaitForChild("KnitPackage"):WaitForChild("Knit"):WaitForChild("Services"):WaitForChild("LevelService"):WaitForChild("RF"):WaitForChild("AscendAbility"):InvokeServer(unpack(args))
end
end)



createToggle("Auto Playtime", 1, Extra, function()
    local testArray = {300, 900, 1800, 2700, 3600, 5400, 7200, 9000, 10800, 14400}
   
for i = 1,10,1 do

local args = {
    [1] = testArray[i]
}

game:GetService("ReplicatedStorage"):WaitForChild("ReplicatedModules"):WaitForChild("KnitPackage"):WaitForChild("Knit"):WaitForChild("Services"):WaitForChild("ShopService"):WaitForChild("RF"):WaitForChild("ClaimPlaytimeReward"):InvokeServer(unpack(args))
end
end)







createToggle("Auto Stat Attack", 0.00045, Extra, function()
   


local args = {
    [1] = game:GetService("Players").LocalPlayer.Data.Ability.Value,
    [2] = {
        ["Special"] = 0,
        ["Defense"] = 0,
        ["Health"] = 0,
        ["Attack"] = 1
    }
}

game:GetService("ReplicatedStorage"):WaitForChild("ReplicatedModules"):WaitForChild("KnitPackage"):WaitForChild("Knit"):WaitForChild("Services"):WaitForChild("StatService"):WaitForChild("RF"):WaitForChild("ApplyStats"):InvokeServer(unpack(args))

end)


createToggle("Auto Sell Items", 0.00045, Extra, function()
    local player = game:GetService("Players").LocalPlayer
    local backpack = player.Backpack
    local shopService = game:GetService("ReplicatedStorage")
        :WaitForChild("ReplicatedModules"):WaitForChild("KnitPackage"):WaitForChild("Knit")
        :WaitForChild("Services"):WaitForChild("ShopService"):WaitForChild("RE"):WaitForChild("Signal")

    for _, item in ipairs(backpack:GetChildren()) do
        if item.Name == "Sanji's Cookbook" then
            item:Destroy()
            print("Deleted:", item.Name)
            return  -- Stop here and wait for the next loop iteration
        elseif item:IsA("Tool") and not table.find(getgenv().Wantedthositems, item.Name) and
            item.Name ~= "Mount" and item.Name ~= "Sanji's Cookbook" and item.Name ~= "Candy Bag" and 
            item.Name ~= "Simple Domain Essence" and item.Name ~= "Chain of Thousand Miles" and 
            item.Name ~= "Playful Cloud" and item.Name ~= "Wheel of Dharma" and 
            item.Name ~= "Heavenly Restriction Awakening" and item.Name ~= "Inverted Spear of Heaven" and 
            item.Name ~= "Chest Key" and item.Name ~= "Cursed Apple" and item.Name ~= "Stocking" and 
            item.Name ~= "Cursed Arm" and item.Name ~= "Death Painting" and item.Name ~= "Shrine Item" and 
            item.Name ~= "Altered Steel Ball" and item.Name ~= "Limitless Technique Scroll" then
                
            local args = { "BlackMarketBulkSellItems", { { item:GetAttribute("ItemId"), item:GetAttribute("UUID"), 1 } } }
            shopService:FireServer(unpack(args))
            print("Sold:", item.Name)

            task.wait(0.00045)  -- Allow server time to process before next iteration
            return  -- Stop here and wait for the next loop iteration
        end
    end
end)


createToggle("Auto Store Items", 0.00045, Extra, function()
 for i, v in pairs(game:GetService("Players").LocalPlayer.Backpack:GetChildren()) do

     
if v:IsA("Tool") and v.Parent ~= game:GetService("Players").LocalPlayer.Character and table.find(getgenv().Wantedthositems, v.Name) and game:GetService("Players").LocalPlayer.PlayerGui.UI.Gameplay.Character.Info.CombatTag.Visible == false then
 print(v.Name)
         v.Parent = game:GetService("Players").LocalPlayer.Character
task.wait(0.001)
 v.Parent = game:GetService("Players").LocalPlayer.Backpack
task.wait(0.001)
v.Parent = game:GetService("Players").LocalPlayer.Character
task.wait(0.001)
                if v.Parent == game:GetService("Players").LocalPlayer.Character and game:GetService("Players").LocalPlayer.PlayerGui.UI.Gameplay.Character.Info.CombatTag.Visible == false then




                        local args = {

                            [1] = {

                                ["AddItems"] = true

                            }   

                        }

                    game:GetService("ReplicatedStorage").ReplicatedModules.KnitPackage.Knit.Services.InventoryService.RE.ItemInventory:FireServer(unpack(args))


                end




 task.wait(0.02)

                end
            end
end)



getgenv().Wantedthositems = {"None"}
local Dropdown = Extra:CreateDropdown({
   Name = "Dont Sell items",
   Options = {"Cursed Orb", "Wheel of Dharma", "Cursed Arm", "Shrine Item", "Sukuna's Finger", "Gun Parts", "Dragon Ball", "Requiem Arrow", "Bone"},
   CurrentOption = {},
   MultipleOptions = true,
   Callback = function(itomo)
getgenv().Wantedthositems = itomo
   end,
})



local module = {
                instanceTypes = {
                    ['boolean'] = 'BoolValue';
                    ['string'] = 'StringValue';
                    ['number'] = 'NumberValue';
                };
                ToInstance = function(self, tbl, parent)
                    for i,v in pairs(tbl) do
                        local number = tonumber(v)
                        if number then
                            tbl[i] = number
                        end
                        if type(v) == 'table' then
                            local currentFolder = Instance.new('Folder')
                            currentFolder.Parent = parent
                            currentFolder.Name = i
                            self:ToInstance(v, currentFolder)
                        else
                            for i2,v2 in pairs(self.instanceTypes) do
                                if type(v) == i2 then
                                    local currentValue = Instance.new(v2)
                                    currentValue.Parent = parent
                                    currentValue.Name = i
                                    currentValue.Value = v
                                end
                            end
                        end
                    end
                end;

                ToTable = function(self, parent)
                    local function currentFunction(newParent)
                        local returnedData = {}
                        for i,v in pairs(newParent:GetChildren()) do
                            if v:IsA('Folder') then
                                returnedData[v.Name] = currentFunction(v)
                            else
                                returnedData[v.Name] = v.Value
                            end
                        end
                        return returnedData
                    end
                    local constructedTable = currentFunction(parent)
                    return constructedTable
                end;
            };




local Qua = Window:CreateTab("Quests", 4483362458)



local ExtraBosses = Qua:CreateSection("Slayer Bosses")



getgenv().Slayer = {"None"}
local Dropdown = Qua:CreateDropdown({
   Name = "Slayer Boss",
   Options = {"Gojo","FingerMan"},
   CurrentOption =  "",
   Callback = function(bosss)
    local yesString = table.concat(bosss, "%")
        getgenv().Slayer = yesString

   end,
})




createToggle("Auto Slayer", 1, Qua, function()
 if getgenv().Slayer == "Gojo" then
local args = {
    [1] = "Slayer_Quest",
    [2] = "Gojo"
}

game:GetService("ReplicatedStorage"):WaitForChild("ReplicatedModules"):WaitForChild("KnitPackage"):WaitForChild("Knit"):WaitForChild("Services"):WaitForChild("DialogueService"):WaitForChild("RF"):WaitForChild("CheckDialogue"):InvokeServer(unpack(args))
end

if getgenv().Slayer == "FingerMan" then
local args = {
    [1] = "Slayer_Quest",
    [2] = "Finger Bearer"
}

game:GetService("ReplicatedStorage"):WaitForChild("ReplicatedModules"):WaitForChild("KnitPackage"):WaitForChild("Knit"):WaitForChild("Services"):WaitForChild("DialogueService"):WaitForChild("RF"):WaitForChild("CheckDialogue"):InvokeServer(unpack(args))
end

end)




local otherquests = Qua:CreateSection("Other Quests")



local Busoshoku = Qua:CreateButton({
   Name = "Auto Busoshoku Haki",
   Callback = function()
Busoshoku = false
if Busoshoku  == false then
Busoshoku = true

local ohString1 = "Agent's Secret"

game:GetService("ReplicatedStorage").ReplicatedModules.KnitPackage.Knit.Services.DialogueService.RF.CheckDialogue:InvokeServer(ohString1)

repeat task.wait()
EnemyDropdown:Set({"Pirates"})
EnemyToggle:Set(true)
until game:GetService("Players").LocalPlayer.QuestLines["Agent's Secret"]["Agent's Secret"]["Defeat [Pirate]"].Value >= 100 

EnemyToggle:Set(false)

local args = {
    [1] = "Save_The_Village_Adventure"
}

game:GetService("ReplicatedStorage"):WaitForChild("ReplicatedModules"):WaitForChild("KnitPackage"):WaitForChild("Knit"):WaitForChild("Services"):WaitForChild("DialogueService"):WaitForChild("RF"):WaitForChild("CheckDialogue"):InvokeServer(unpack(args))
wait(1)
local args = {
    [1] = "Save_The_Village_Adventure"
}

game:GetService("ReplicatedStorage"):WaitForChild("ReplicatedModules"):WaitForChild("KnitPackage"):WaitForChild("Knit"):WaitForChild("Services"):WaitForChild("DialogueService"):WaitForChild("RF"):WaitForChild("CheckDialogue"):InvokeServer(unpack(args))
wait(2)
local args = {
    [1] = "Save_The_Village_Adventure"
}

game:GetService("ReplicatedStorage"):WaitForChild("ReplicatedModules"):WaitForChild("KnitPackage"):WaitForChild("Knit"):WaitForChild("Services"):WaitForChild("DialogueService"):WaitForChild("RF"):WaitForChild("CheckDialogue"):InvokeServer(unpack(args))

wait(.5)
   game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(10009.0762, -48.4100723, 30129.7754, 0, 0, -1, 0, 1, 0, 1, 0, 0)


repeat task.wait()

for i,v in pairs(game:GetService("Workspace").Living:GetDescendants()) do
        if v:IsA("Model") and v:FindFirstChild('HumanoidRootPart') and v:FindFirstChild('Humanoid') and v.Humanoid.Health > 0 and v.Name ~=  game.Players.LocalPlayer.Name and v.Name == "Kuro of a Hundred Plans" then
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.HumanoidRootPart.CFrame * CFrame.new(0,0,5)
       warn("attack")


local args = {
    [1] = "MouseButton1"
}

game:GetService("ReplicatedStorage"):WaitForChild("ReplicatedModules"):WaitForChild("KnitPackage"):WaitForChild("Knit"):WaitForChild("Services"):WaitForChild("MoveInputService"):WaitForChild("RF"):WaitForChild("FireInput"):InvokeServer(unpack(args))

end

        end
until game:GetService("Players").LocalPlayer.QuestLines["Agent's Secret"]["Agent's Secret"]["Defeat [Kuro]"].Value == 1 

wait(1)
local ohString1 = "Agent's Secret"

game:GetService("ReplicatedStorage").ReplicatedModules.KnitPackage.Knit.Services.DialogueService.RF.CheckDialogue:InvokeServer(ohString1)

Busoshoku = false
end

   end
})







local Kenbunshoku = Qua:CreateButton({
   Name = "Auto Kenbunshoku Haki",
   Callback = function()
Kenbunshoku = false
if Kenbunshoku == false then
Kenbunshoku = true

local ohString1 = "Muri's Venture"

game:GetService("ReplicatedStorage").ReplicatedModules.KnitPackage.Knit.Services.DialogueService.RF.CheckDialogue:InvokeServer(ohString1)




repeat task.wait()
EnemyDropdown:Set({"Pirates"})
EnemyToggle:Set(true)
until game:GetService("Players").LocalPlayer.QuestLines["Muri's Venture"]["Muri's Venture"]["Defeat [Pirate]"].Value >= 250

EnemyToggle:Set(false)



   game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-69063.8594, 3350.4209, 19868.2402, -0.91139704, -1.05728866e-07, 0.41152814, -8.2438838e-08, 1, 7.43432906e-08, -0.41152814, 3.38303572e-08, -0.91139704)


repeat task.wait()

local function removeNumbersFromString(str)
    return str:gsub("%d", "")
end

for _, part in pairs(game.Workspace.Living:GetChildren()) do
    if part.Name ~= game.Players.LocalPlayer.Name then
        part.Name = removeNumbersFromString(part.Name)
    end
end





for i,v in pairs(game:GetService("Workspace").Living:GetDescendants()) do
        if v:IsA("Model") and v:FindFirstChild('HumanoidRootPart') and v:FindFirstChild('Humanoid') and v.Humanoid.Health > 0 and v.Name ~=  game.Players.LocalPlayer.Name and v.Name == "Surgeon of Death" then
        game.Players.LocalPlayer.Character:WaitForChild('HumanoidRootPart').CFrame = v.HumanoidRootPart.CFrame * CFrame.new(0,0,5)
       warn("attack")


local args = {
    [1] = "MouseButton1"
}

game:GetService("ReplicatedStorage"):WaitForChild("ReplicatedModules"):WaitForChild("KnitPackage"):WaitForChild("Knit"):WaitForChild("Services"):WaitForChild("MoveInputService"):WaitForChild("RF"):WaitForChild("FireInput"):InvokeServer(unpack(args))

end

        end
until game:GetService("Players").LocalPlayer.QuestLines["Muri's Venture"]["Muri's Venture"]["Defeat [Law]"].Value == 1 



   game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(2181.17041, 914.19696, -2550.84668, 0.903624654, -0, -0.428325176, 0, 1, -0, 0.428325176, 0, 0.903624654)




wait(1)

repeat task.wait()


for i,v in pairs(game:GetService("Workspace").Living:GetDescendants()) do
        if v:IsA("Model") and v:FindFirstChild('HumanoidRootPart') and v:FindFirstChild('Humanoid') and v.Humanoid.Health > 0 and v.Name ~=  game.Players.LocalPlayer.Name and v.Name == "Crocodile" then
        game.Players.LocalPlayer.Character:WaitForChild('HumanoidRootPart').CFrame = v.HumanoidRootPart.CFrame * CFrame.new(0,0,5)

local args = {
    [1] = "MouseButton1"
}

game:GetService("ReplicatedStorage"):WaitForChild("ReplicatedModules"):WaitForChild("KnitPackage"):WaitForChild("Knit"):WaitForChild("Services"):WaitForChild("MoveInputService"):WaitForChild("RF"):WaitForChild("FireInput"):InvokeServer(unpack(args))

end

        end
until game:GetService("Players").LocalPlayer.QuestLines["Muri's Venture"]["Muri's Venture"]["Defeat [Crocodile]"].Value == 1 



wait(1)
local ohString1 = "Muri's Venture"

game:GetService("ReplicatedStorage").ReplicatedModules.KnitPackage.Knit.Services.DialogueService.RF.CheckDialogue:InvokeServer(ohString1)





Kenbunshoku = false
end

   end
})








local UI = Window:CreateTab("UI", 4483362458)






local Traito = UI:CreateSection("UI")


local ToggleInv = UI:CreateToggle({
    Name = "Inventory",
    CurrentValue = false,
    Callback = function(Value)
        getgenv().Nex = Value
        getgenv().currentToggle = Value

        local lastUpdate = tick()

        local function invon()
       game:GetService("Players").LocalPlayer.PlayerGui.UI.Menus.Visible = true
       game:GetService("Players").LocalPlayer.PlayerGui.UI.Menus.Inventory.Visible = true
        end

  local function invoff()
  game:GetService("Players").LocalPlayer.PlayerGui.UI.Menus.Visible = false
              game:GetService("Players").LocalPlayer.PlayerGui.UI.Menus.Inventory.Visible = false
        end

        createHeartbeatConnection(function()
            pcall(function()
                if getgenv().Nex and tick() - lastUpdate >= .4 then
                    lastUpdate = tick()
                   invon()
end
            end)
        end, 0.4) -- Adjust interval as needed
    end
})


local AbilityUI = UI:CreateButton({
   Name = "Open Ability",
   Callback = function()
    game:GetService("Players").LocalPlayer.PlayerGui.UI.Menus.Visible = true
   game:GetService("Players").LocalPlayer.PlayerGui.UI.Menus.Ability.Visible = true
   end
})

local roll = UI:CreateSection("Stats")

getgenv().StatCheck = false
getgenv().GotStat = false

local AutoStatRoll = UI:CreateToggle({
    Name = "Auto Stat Reroll",
    CurrentValue = false,
    Callback = function(WackValue)
        getgenv().EStatReroll = WackValue
       
if getgenv().GotStat == true then
getgenv().GotStat = false
end

    while getgenv().EStatReroll == true and getgenv().StatCheck == false and getgenv().GotStat == false do

      local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TraitService = ReplicatedStorage.ReplicatedModules.KnitPackage.Knit.Services.TraitService

local AbilityUUID = Players.LocalPlayer:WaitForChild("Data"):WaitForChild("Ability"):GetAttribute("UUID")

-- Thresholds (0 = skip)
getgenv().HEALTH = getgenv().HEALTH or 0
getgenv().ATTACK = getgenv().ATTACK or 0
getgenv().DEFENSE = getgenv().DEFENSE or 0
getgenv().SPECIAL = getgenv().SPECIAL or 0



 getgenv().StatCheck = true

-- Conversion module
local module = {
	instanceTypes = {
		['boolean'] = 'BoolValue',
		['string'] = 'StringValue',
		['number'] = 'NumberValue',
	},

	ToInstance = function(self, tbl, parent)
		for key, value in pairs(tbl) do
			if type(value) == 'table' then
				local folder = Instance.new("Folder")
				folder.Name = key
				folder.Parent = parent
				self:ToInstance(value, folder)
			else
				local valueType = self.instanceTypes[type(value)]
				if valueType then
					local val = Instance.new(valueType)
					val.Name = key
					val.Value = value
					val.Parent = parent
				end
			end
		end
	end,
}


	local rollResults = TraitService.RF.RerollTraitStats:InvokeServer(AbilityUUID, false)

	local resultFolder = Instance.new("Folder")
	resultFolder.Name = "RollResults"
	module:ToInstance(rollResults, resultFolder)

	local statFolder = resultFolder:FindFirstChild("StatBonuses")
	if not statFolder then
		warn("Missing StatBonuses, rerolling...")
		task.wait(0.1) 
        getgenv().GotStat = false
        getgenv().StatCheck = false
        continue
	end

local function safeStat(name)
	local val = statFolder:FindFirstChild(name)
	return typeof(val) == "Instance" and typeof(val.Value) == "number" and val.Value or 0 or val.Value == 0 or val.Value == nil or val.Value == ""
    end

	local health = safeStat("Health")
	local attack = safeStat("Attack")
	local defense = safeStat("Defence")
	local special = safeStat("Special")

	print("Rolling... Health:", health, "| Attack:", attack, "| Defense:", defense, "| Special:", special)

	Rayfield:Notify({
		Title = "Reroll",
		Content = "Health: " .. tostring(health) .. ", Attack: " .. tostring(attack) ..
				  ", Defense: " .. tostring(defense) .. ", Special: " .. tostring(special),
		Duration = 0.8,
		Image = 4483362458,
	})


local function shouldSkip(statValue, minValue)
    return minValue == 0 or minValue == nil or statValue == nil
end

if shouldSkip(health, getgenv().HEALTH) and 
   shouldSkip(attack, getgenv().ATTACK) and 
   shouldSkip(defense, getgenv().DEFENSE) and 
   shouldSkip(special, getgenv().SPECIAL) then

    Rayfield:Notify({
        Title = "Reroll",
        Content = "⚠️ All selected stats are missing or zero, skipping this roll",
        Duration = 0.8,
        Image = 4483362458,
    })

    print("⚠️ All selected stats are missing or zero, skipping this roll")
    task.wait(0.1)
    getgenv().StatCheck = false
    getgenv().GotStat = false
    continue
end


local passed = true

if getgenv().HEALTH > 0 and (typeof(health) ~= "number" or health < getgenv().HEALTH) then passed = false end
if getgenv().ATTACK > 0 and (typeof(attack) ~= "number" or attack < getgenv().ATTACK) then passed = false end
if getgenv().DEFENSE > 0 and (typeof(defense) ~= "number" or defense < getgenv().DEFENSE) then passed = false end
if getgenv().SPECIAL > 0 and (typeof(special) ~= "number" or special < getgenv().SPECIAL) then passed = false end



	if passed then
		print("✅ Desired stats met!")

		Rayfield:Notify({
			Title = "Reroll",
			Content = "✅ Desired stats met!",
			Duration = 0.8,
			Image = 4483362458,
		})

		resultFolder:Destroy()
	 getgenv().GotStat = true
        wait(.2)
         getgenv().StatCheck = false
		return
	end

	task.wait(getgenv().Stat2Speed)
 getgenv().StatCheck = false

    end

    end
})






local Stat2Speed = UI:CreateSlider({
   Name = "Stat Loop Speed",
   Range = {0.1, 1},
   Increment = 0.1,
      Suffix = "SPEED",
   CurrentValue = 0.1,
   Callback = function(Value)
getgenv().Stat2Speed = Value
print(getgenv().Stat2Speed)
   end,
})

getgenv().HEALTH = 1

local healthslid = UI:CreateSlider({
   Name = "Health Stat",
   Range = {0, 45},
   Increment = 1,
   Suffix = "Health",
   CurrentValue = 1,
   Flag = "Health", 
   Callback = function(Value)
getgenv().HEALTH = Value
print(getgenv().HEALTH)
   end,
})

getgenv().ATTACK = 0
local attack = UI:CreateSlider({
   Name = "Attack Stat",
   Range = {0, 45},
   Increment = 1,
   Suffix = "Attack",
   CurrentValue = 0,
   Flag = "Attack", 
   Callback = function(Value)
getgenv().ATTACK = Value
print(getgenv().ATTACK)
   end,
})

getgenv().DEFENSE = 0
local Defencesl = UI:CreateSlider({
   Name = "Defence Stat",
   Range = {0, 45},
   Increment = 1,
   Suffix = "Defence",
   CurrentValue = 0,
   Flag = "Defence", 
   Callback = function(Value)
getgenv().DEFENSE = Value
print(getgenv().DEFENSE)
   end,
})


getgenv().SPECIAL = 0
local specialslid = UI:CreateSlider({
   Name = "Special Stat",
   Range = {0, 45},
   Increment = 1,
   Suffix = "Special",
   CurrentValue = 0,
   Flag = "Dpecial", 
   Callback = function(Value)
getgenv().SPECIAL = Value
print(getgenv().SPECIAL)
   end,
})


local Crateo = UI:CreateSection("Crate Stuff")

-- Toggle for Auto Normal with reduced server calls
local AutoNorm = UI:CreateToggle({
    Name = "Auto Normal",
    CurrentValue = false,
    Callback = function(Value)
        getgenv().ETEex = Value
        getgenv().currentToggle = Value

        local lastUpdate = tick()

        local args10 = { [1] = "Skin_Crate", [2] = "UShards", [3] = 10 }
        local args1 = { [1] = "Skin_Crate", [2] = "UShards", [3] = 1 }

        -- Consolidate server call to avoid redundant InvokeServer
        local function autonormal()
            game:GetService("ReplicatedStorage"):WaitForChild("ReplicatedModules"):WaitForChild("KnitPackage"):WaitForChild("Knit"):WaitForChild("Services"):WaitForChild("ShopService"):WaitForChild("RF"):WaitForChild("BuySkinCrate"):InvokeServer(unpack(args10))
            game:GetService("ReplicatedStorage"):WaitForChild("ReplicatedModules"):WaitForChild("KnitPackage"):WaitForChild("Knit"):WaitForChild("Services"):WaitForChild("ShopService"):WaitForChild("RF"):WaitForChild("BuySkinCrate"):InvokeServer(unpack(args1))
        end

        -- Adjust heartbeat frequency
        createHeartbeatConnection(function()
            pcall(function()
                if getgenv().ETEex and tick() - lastUpdate >= 0.5 then
                    lastUpdate = tick()
                    autonormal()
                end
            end)
        end, 0.5)
    end
})

-- Toggle for Auto Delete Skins with improved performance
local AutoDelete = UI:CreateToggle({
    Name = "Auto Delete Skins",
    CurrentValue = false,
    Callback = function(Value)
        getgenv().Delete = Value
        getgenv().currentToggle = Value

        local lastUpdate = tick()

        local module = {
            instanceTypes = { ['boolean'] = 'BoolValue', ['string'] = 'StringValue', ['number'] = 'NumberValue' },
            ToInstance = function(self, tbl, parent)
                for i, v in pairs(tbl) do
                    if type(v) == 'table' then
                        local currentFolder = Instance.new('Folder')
                        currentFolder.Parent = parent
                        currentFolder.Name = i
                        self:ToInstance(v, currentFolder)
                    else
                        local instanceType = self.instanceTypes[type(v)]
                        if instanceType then
                            local valueInstance = Instance.new(instanceType)
                            valueInstance.Parent = parent
                            valueInstance.Name = i
                            valueInstance.Value = v
                        end
                    end
                end
            end,
            ToTable = function(self, parent)
                local function recurse(folder)
                    local data = {}
                    for _, child in ipairs(folder:GetChildren()) do
                        data[child.Name] = child:IsA('Folder') and recurse(child) or child.Value
                    end
                    return data
                end
                return recurse(parent)
            end
        }

        local function DeleteSkins()
            local args = { [1] = "SkinInventory" }
            local items = game:GetService("ReplicatedStorage")
                :WaitForChild("ReplicatedModules")
                :WaitForChild("KnitPackage")
                :WaitForChild("Knit")
                :WaitForChild("Services")
                :WaitForChild("InventoryService")
                :WaitForChild("RF")
                :WaitForChild("GetItems")
                :InvokeServer(unpack(args))

            local ExampleFolder = Instance.new("Folder")
            module:ToInstance(items, ExampleFolder)

            -- Collect unwanted items in one pass
            local toDelete = {}
            for _, v in ipairs(ExampleFolder:GetDescendants()) do
                if v.Name == "_UnusualInfo" or (v.ClassName ~= "Folder" and v.Name ~= "_Rarity") then
                    table.insert(toDelete, v.Parent)
                elseif v.Name == "_Rarity" and not (v.Value == 3 or v.Value == 4 or v.Value == 6) then
                    table.insert(toDelete, v.Parent)
                end
            end

            -- Delete collected items
            for _, item in ipairs(toDelete) do
                if item and item.Parent then item:Destroy() end
            end

            -- Prepare deletion request for UUIDs
            local uuidsToDelete = {}
            for _, child in ipairs(ExampleFolder:GetChildren()) do
                table.insert(uuidsToDelete, child.Name)
            end

            if #uuidsToDelete > 0 then
                local deleteArgs = { [1] = { ["UUIDS"] = uuidsToDelete, ["Remove"] = true } }
                game:GetService("ReplicatedStorage")
                    :WaitForChild("ReplicatedModules")
                    :WaitForChild("KnitPackage")
                    :WaitForChild("Knit")
                    :WaitForChild("Services")
                    :WaitForChild("InventoryService")
                    :WaitForChild("RE")
                    :WaitForChild("SkinInventory")
                    :FireServer(unpack(deleteArgs))
            end
        end

        createHeartbeatConnection(function()
            pcall(function()
                if getgenv().Delete and tick() - lastUpdate >= 0.5 then
                    lastUpdate = tick()
                    DeleteSkins()
                end
            end)
        end, 0.5)
    end
})



createToggle("Auto Recycle Skins", 5, UI, function()

    local module = {
        instanceTypes = {
            ['boolean'] = 'BoolValue',
            ['string'] = 'StringValue',
            ['number'] = 'NumberValue'
        },
        ToInstance = function(self, tbl, parent)
            for i, v in pairs(tbl) do
                local number = tonumber(v)
                if number then
                    tbl[i] = number
                end
                if type(v) == 'table' then
                    local currentFolder = Instance.new('Folder')
                    currentFolder.Parent = parent
                    currentFolder.Name = i
                    self:ToInstance(v, currentFolder)
                else
                    for i2, v2 in pairs(self.instanceTypes) do
                        if type(v) == i2 then
                            local currentValue = Instance.new(v2)
                            currentValue.Parent = parent
                            currentValue.Name = i
                            currentValue.Value = v
                        end
                    end
                end
            end
        end,

        ToTable = function(self, parent)
            local function currentFunction(newParent)
                local returnedData = {}
                for i, v in pairs(newParent:GetChildren()) do
                    if v:IsA('Folder') then
                        returnedData[v.Name] = currentFunction(v)
                    else
                        returnedData[v.Name] = v.Value
                    end
                end
                return returnedData
            end
            local constructedTable = currentFunction(parent)
            return constructedTable
        end
    }

    local args = {
        [1] = "SkinInventory"
    }

    local skinInventory = game:GetService("ReplicatedStorage")
        :WaitForChild("ReplicatedModules")
        :WaitForChild("KnitPackage")
        :WaitForChild("Knit")
        :WaitForChild("Services")
        :WaitForChild("InventoryService")
        :WaitForChild("RF")
        :WaitForChild("GetItems")
        :InvokeServer(unpack(args))

    local ExampleFolder = Instance.new("Folder")
    module:ToInstance(skinInventory, ExampleFolder)

    for _, v in pairs(ExampleFolder:GetDescendants()) do
        if v.Name == "_UnusualInfo" then
            v.Parent:Destroy()
        end
    end

    for _, v in pairs(ExampleFolder:GetDescendants()) do
        if v.ClassName ~= "Folder" and v.Name ~= "_Rarity" and v.Name ~= "_UnusualInfo" then
            v:Destroy()
        end
    end

    for _, v in pairs(ExampleFolder:GetDescendants()) do
        if v.Name == "_Rarity" then
            if v.Value ~= 3 and v.Value ~= 4 then
                v.Parent:Destroy()
            end
        end
    end

    -- Group recycling
    local skinGroup = {}
    for _, v in pairs(ExampleFolder:GetChildren()) do
        table.insert(skinGroup, v.Name)
    end

    -- Recycle in batches of 25
    if #skinGroup > 0 then
        print("Recycling the following skins in batches of 25:")

        local batchSize = 25
        for i = 1, #skinGroup, batchSize do
            local batch = {}
            
            for j = i, math.min(i + batchSize - 1, #skinGroup) do
                table.insert(batch, skinGroup[j])
            end

            -- Print the batch
            print("Recycling batch:")
            for _, skinName in ipairs(batch) do
                print(skinName)
            end

            -- Send the batch to the server
            game:GetService("ReplicatedStorage")
                .ReplicatedModules
                .KnitPackage
                .Knit
                .Services
                .ShopService
                .RF
                .RecycleSkins
                :InvokeServer(batch)
        end
    end
end)






local Banner = UI:CreateToggle({
    Name = "Auto Banner",
    CurrentValue = false,
    Callback = function(Value)
        getgenv().Banner = Value
        getgenv().currentToggle = Value

        local lastUpdate = tick()

        local function Banner()
           
local args = {
    [1] = 1,
    [2] = "UShards",
    [3] = 10
}

game:GetService("ReplicatedStorage"):WaitForChild("ReplicatedModules"):WaitForChild("KnitPackage"):WaitForChild("Knit"):WaitForChild("Services"):WaitForChild("ShopService"):WaitForChild("RF"):WaitForChild("RollBanner"):InvokeServer(unpack(args))


        end

        createHeartbeatConnection(function()
            pcall(function()
                if getgenv().Banner and tick() - lastUpdate >= 0.1 then
                    lastUpdate = tick()
                    Banner()
                end
            end)
        end, 0.1) -- Adjust interval as needed
    end
})



createToggle("Auto Event Crate",  0.00045, UI, function()
   
local args = {
    [1] = "Halloween_Crate_2024",
    [2] = "UHalloween",
    [3] = 10
}

game:GetService("ReplicatedStorage").ReplicatedModules.KnitPackage.Knit.Services.ShopService.RF.BuySkinCrate:InvokeServer(unpack(args))

local args = {
    [1] = "Halloween_Crate_2024",
    [2] = "UHalloween",
    [3] = 1
}

game:GetService("ReplicatedStorage").ReplicatedModules.KnitPackage.Knit.Services.ShopService.RF.BuySkinCrate:InvokeServer(unpack(args))


end)






createToggle("Auto Delete Event", 1, UI, function()
    local module = {
        instanceTypes = {
            ['boolean'] = 'BoolValue';
            ['string'] = 'StringValue';
            ['number'] = 'NumberValue';
        };
        ToInstance = function(self, tbl, parent)
            for i, v in pairs(tbl) do
                if tonumber(v) then tbl[i] = tonumber(v) end
                if type(v) == 'table' then
                    local folder = Instance.new('Folder')
                    folder.Parent = parent
                    folder.Name = i
                    self:ToInstance(v, folder)
                else
                    local typeInstance = self.instanceTypes[type(v)]
                    if typeInstance then
                        local valueInstance = Instance.new(typeInstance)
                        valueInstance.Parent = parent
                        valueInstance.Name = i
                        valueInstance.Value = v
                    end
                end
            end
        end;
        ToTable = function(self, parent)
            local function recurse(parent)
                local data = {}
                for _, child in ipairs(parent:GetChildren()) do
                    if child:IsA('Folder') then
                        data[child.Name] = recurse(child)
                    else
                        data[child.Name] = child.Value
                    end
                end
                return data
            end
            return recurse(parent)
        end;
    }

    local args = { [1] = "SkinInventory" }
    local inventory = game:GetService("ReplicatedStorage")
        :WaitForChild("ReplicatedModules")
        :WaitForChild("KnitPackage")
        :WaitForChild("Knit")
        :WaitForChild("Services")
        :WaitForChild("InventoryService")
        :WaitForChild("RF")
        :WaitForChild("GetItems")
        :InvokeServer(unpack(args))

    local folder = Instance.new("Folder")
    module:ToInstance(inventory, folder)

    -- Cache descendants for filtering
    local descendants = folder:GetDescendants()

    -- Remove unwanted "_UnusualInfo" and non-essential values
    for _, desc in ipairs(descendants) do
        if desc.Name == "_UnusualInfo" or (desc.ClassName ~= "Folder" and desc.Name ~= "_Rarity") then
            desc:Destroy()
        end
    end

  if desc.ClassName ~= "Folder" and desc.Name ~= "_Rarity" and desc.Name ~= "_UnusualInfo" then 
  
v:Destroy()
 
      end


    -- Filter items by rarity
    for _, desc in ipairs(folder:GetDescendants()) do
        if desc.Name == "_Rarity" and (desc.Value ~= 3 or desc.Value ~= 4 or desc.Value ~= 6) then
            desc.Parent:Destroy()
        end
    end

    -- Batch delete items by UUIDS
    local uuidsToDelete = {}
    for _, item in ipairs(folder:GetChildren()) do
        table.insert(uuidsToDelete, item.Name)
    end

    if #uuidsToDelete > 0 then
        local deleteArgs = {
            [1] = {
                ["UUIDS"] = uuidsToDelete,
                ["Remove"] = true
            }
        }
        game:GetService("ReplicatedStorage")
            :WaitForChild("ReplicatedModules")
            :WaitForChild("KnitPackage")
            :WaitForChild("Knit")
            :WaitForChild("Services")
            :WaitForChild("InventoryService")
            :WaitForChild("RE")
            :WaitForChild("SkinInventory")
            :FireServer(unpack(deleteArgs))
    end
end)


local Banner = UI:CreateToggle({
    Name = "Auto Candy",
    CurrentValue = false,
    Callback = function(Value)
        getgenv().candy = Value
        getgenv().currentToggle = Value

        local lastUpdate = tick()

        local function candy()
           
local args = {
    [1] = true
}

game:GetService("ReplicatedStorage").ReplicatedModules.KnitPackage.Knit.Services.HalloweenCandyService.RF.ExchangeCandy:InvokeServer(unpack(args))


        end

        createHeartbeatConnection(function()
            pcall(function()
                if getgenv().candy and tick() - lastUpdate >= 0.1 then
                    lastUpdate = tick()
                    candy()
                end
            end)
        end, 1) -- Adjust interval as needed
    end
})



if game.PlaceId == 6846458508 then

local TP = Window:CreateTab("TP", 4483362458)
local Areas = TP:CreateSection("Locations")



local sound = Instance.new("Sound")
sound.SoundId = "rbxassetid://5066021887"



local MainArea = TP:CreateButton({
   Name = "Center City",
   Callback = function()
game:GetService("SoundService"):PlayLocalSound(sound)
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(2002.75391, 1021.93994, 172.694946, 1, 0, 0, 0, 1, 0, 0, 0, 1)
   end
})

local Soc = TP:CreateButton({
   Name = "Football Field",
   Callback = function()
game:GetService("SoundService"):PlayLocalSound(sound)
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(1988.70386, 1021.93994, -476.829956, 1, 0, 0, 0, 1, 0, 0, 0, 1)
   end
})



local Wilds = TP:CreateButton({
   Name = "Center Wilds",
   Callback = function()
game:GetService("SoundService"):PlayLocalSound(sound)
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(2498.27881, 1021.93994, -542.429688, 1, 0, 0, 0, 1, 0, 0, 0, 1)
   end
})



local Aooriach = TP:CreateButton({
   Name = "Desert Approach",
   Callback = function()
game:GetService("SoundService"):PlayLocalSound(sound)
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(1988.70386, 1021.93994, -836.379883, 1, 0, 0, 0, 1, 0, 0, 0, 1)
   end
})


local Cairn = TP:CreateButton({
   Name = "Infernal Cairn",
   Callback = function()
game:GetService("SoundService"):PlayLocalSound(sound)
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(2814.81519, 1021.93994, -771.991943, 0.707134247, -0, -0.707079291, 0, 1, -0, 0.707079291, 0, 0.707134247)
   end
})


local Desert = TP:CreateButton({
   Name = "Desert",
   Callback = function()
game:GetService("SoundService"):PlayLocalSound(sound)
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(1845.552, 892.55188, -1815.76868, 0.945575476, 0, 0.325402796, 0, 1, 0, -0.325402796, 0, 0.945575476)
   end
})

local Punk = TP:CreateButton({
   Name = "Punk Hazard",
   Callback = function()
game:GetService("SoundService"):PlayLocalSound(sound)
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-68944.9453, 3489.41479, 20010.8789, 1, 0, 0, 0, 1, 0, 0, 0, 1)
   end
})



local Marineford = TP:CreateButton({
   Name = "Marineford",
   Callback = function()
game:GetService("SoundService"):PlayLocalSound(sound)
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-24526.3086, 1580.77393, 589.837036, 1, 0, 0, 0, 1, 0, 0, 0, 1)
   end
})

local Orange = TP:CreateButton({
   Name = "Orange Town",
   Callback = function()
game:GetService("SoundService"):PlayLocalSound(sound)
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-3155.26929, 1048.19897, 15144.7275, 1, 0, 0, 0, 1, 0, 0, 0, 1)
   end
})


local npcc = TP:CreateSection("NPCs")


local Players = game:GetService("Players")

-- Function to create a button
local function createButton(name, Tab, action)
    print("Creating button for: " .. name)
    return Tab:CreateButton({
        Name = name,
        Callback = function()
            -- Execute the action when the button is clicked
            pcall(action)
        end
    })
end

-- Function to create teleport buttons for each NPC in a folder
local function createButtonsForNPCs(folderName, Tab)
    -- Get the NPC folder
    local npcFolder = workspace:FindFirstChild(folderName)
    if not npcFolder then
        warn("Folder " .. folderName .. " not found in workspace!")
        return
    end

    -- Print folder details for debugging
    print("Found NPC folder: " .. folderName)
    
    -- Loop through each NPC in the folder
    for _, npc in pairs(npcFolder:GetChildren()) do
        -- Print NPC name for debugging
        print("Processing NPC: " .. npc.Name)
        
        -- Create a button for the NPC
        createButton(npc.Name, Tab, function()
            -- Teleport the player to the NPC's WorldPivot
            local player = Players.LocalPlayer
            local character = player.Character or player.CharacterAdded:Wait()
            
            if character then
                -- Find the NPC by name (button name) and teleport to its WorldPivot
                local targetNPC = npcFolder:FindFirstChild(npc.Name)
                if targetNPC then
game:GetService("SoundService"):PlayLocalSound(sound)
                    local destinationCFrame = targetNPC:GetPivot()
                    character:PivotTo(destinationCFrame)
                else
                    warn("Target NPC named " .. npc.Name .. " not found!")
                end
            end
        end)
    end
end



print("Starting button creation")
createButtonsForNPCs("NPCS", TP) -- Replace "NPCFolder" with your folder's name
end













Rayfield:LoadConfiguration()



