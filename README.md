local Library = loadstring(game:HttpGet("https://pastebin.com/raw/KNBp0LRy"))()

repeat
    wait()
until game:IsLoaded()

game:GetService("Players").LocalPlayer.Idled:connect(function()
    game:GetService("VirtualUser"):ClickButton2(Vector2.new())
end)

local Window = Library:NewWindow("Fruit Seas")
local Section = Window:NewSection("AutoFarm")

local Hit = false
local NPCS = {}

-- Function to get the nearest NPC based on the current target mobname
local function getNPC(mobname)
    local dist, nearestNPC
    for i, v in pairs(workspace.NPC.Fight:GetDescendants()) do
        if v:IsA("Model") and v:FindFirstChild("HumanoidRootPart") and v.Name == mobname then
            local mag = (game.Players.LocalPlayer.Character.HumanoidRootPart.Position - v.HumanoidRootPart.Position).magnitude
            if mag < dist or not dist then
                dist = mag
                nearestNPC = v
            end
        end
    end
    return nearestNPC
end

-- Initial population of NPCS table
for i, v in pairs(workspace.NPC.Fight:GetDescendants()) do
    if v:IsA("Model") and v:FindFirstChild("HumanoidRootPart") then
        table.insert(NPCS, v.Name) -- Assuming NPC names are unique and can be used as identifiers
    end
end

local Dropdown = Section:CreateDropdown("Select Mobs", NPCS, 1, function(Value)
    -- Update the selected mobname
    mobname = Value
end)

local Button = Section:CreateButton("Refresh Mobs", function()
    -- Clear and update NPCS table
    NPCS = {}
    for i, v in pairs(workspace.NPC.Fight:GetDescendants()) do
        if v:IsA("Model") and v:FindFirstChild("HumanoidRootPart") then
            table.insert(NPCS, v.Name)
        end
    end
    Dropdown:SetOptions(NPCS) -- Update dropdown options
end)

local Toggle = Section:CreateToggle("Auto Hit", function(Value)
    Hit = Value
    while Hit do
        task.wait(1) -- Wait for 1 second before checking for enemies
        pcall(function()
            local npc = getNPC(mobname)
            if npc then
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = npc.HumanoidRootPart.CFrame
            end
        end)
    end
end)
