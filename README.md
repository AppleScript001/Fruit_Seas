local Library = loadstring(game:HttpGet("https://pastebin.com/raw/KNBp0LRy"))()

repeat
    wait()
until game:IsLoaded()

game:GetService("Players").LocalPlayer.Idled:connect(function()
    game:GetService("VirtualUser"):ClickButton2(Vector2.new())
end)

local Window = Library:NewWindow("Fruit Seas")
local Section = Window:NewSection("AutoFarm")

local NPCS = {}

-- Function to populate NPCS table with valid NPCs
local function populateNPCs()
    table.clear(NPCS)
    for _, npc in ipairs(workspace.NPC.Fight:GetChildren()) do
        if npc:IsA("Model") and npc:FindFirstChild("HumanoidRootPart") then
            table.insert(NPCS, npc.Name)
        end
    end
end

-- Initial population of NPCs
populateNPCs()

local Dropdown = Section:CreateDropdown("Select Mob", NPCS, 1, function(Value)
    -- Update selected mob name
    mobname = Value
end)

local function getNPC()
    local nearestNPC
    local minDist = math.huge

    for _, npc in ipairs(workspace.NPC.Fight:GetChildren()) do
        if npc:IsA("Model") and npc:FindFirstChild("HumanoidRootPart") and npc.Name == mobname then
            local dist = (game.Players.LocalPlayer.Character.HumanoidRootPart.Position - npc.HumanoidRootPart.Position).magnitude
            if dist < minDist then
                minDist = dist
                nearestNPC = npc
            end
        end
    end

    return nearestNPC
end

local Button = Section:CreateButton("Refresh Mobs", function()
    -- Refresh NPC list
    populateNPCs()
    Dropdown:SetOptions(NPCS)
end)

local Toggle = Section:CreateToggle("Auto Hit", function(Value)
    local Hit = Value
    while Hit do
        task.wait(1) -- Adjust the wait time if needed

        -- Attempt to find and attack the nearest NPC
        pcall(function()
            local npc = getNPC()
            if npc then
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = npc.HumanoidRootPart.CFrame
            end
        end)
    end
end)
