local Library = loadstring(game:HttpGet("https://pastebin.com/raw/KNBp0LRy"), "Coastified UI")()
repeat wait() until game:IsLoaded()
game:GetService("Players").LocalPlayer.Idled:connect(function()
game:GetService("VirtualUser"):ClickButton2(Vector2.new())
end)

local Window = Library:NewWindow("Fruit Seas")
local Section = Window:NewSection("AutoFarm")

local PlayerTP1
local TweenService = game:GetService("TweenService")

local NPCS = {}

    for i, v in pairs(workspace.NPC.Fight:GetDescendants()) do
        if v:IsA "Model" and v:FindFirstChild("HumanoidRootPart") then
            if not table.find(NPCS, tostring(v)) then
            table.insert(NPCS, tostring(v))
        end
    end
end
local Dropdown = Section:CreateDropdown("Select Mobs !", NPCS, 0, function(t)
    PlayerTP1 = t
end)
local Button = Section:CreateButton("Refresh [Mob]", function(Value)
    table.clear(NPCS)
    for i, v in pairs(workspace.NPC.Fight:GetDescendants()) do
        if v:IsA "Model" and v:FindFirstChild("HumanoidRootPart") then
            if not table.find(NPCS, tostring(v)) then
                table.insert(NPCS, tostring(v))
                ii:SetOptions(NPCS)
            end
        end
    end
end)
local Toggle = Section:CreateToggle("Auto [Hit]", function(Value)
Hit = Value
while Hit do task.wait()  -- Wait for 1 second before checking for enemies
pcall(function()
for i,v in pairs(workspace.NPC.Fight:GetDescendants()) do
if v.Name == PlayerTP1 then
if v.Humanoid.Health > 0 then
repeat
    local args = {
        [1] = "Combat",
        [2] = 999,
        [3] = "left",
        [4] = 999,
        [5] = "Combat"
    }
    
    game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("ServerMove"):FireServer(unpack(args))
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.HumanoidRootPart.CFrame * CFrame.new(0,0,4)
wait()  -- Wait a short time before checking again
until not _G.Hit or v.Humanoid.Health <= 0
end
end
end
end)
local Player = game.Players.LocalPlayer
local function onCharacterAdded(character)
    character.Archivable = false
    character:WaitForChild("HumanoidRootPart").Anchored = false
    if Player.Character then
        onCharacterAdded(Player.Character)
    end
end

Player.CharacterAdded:Connect(onCharacterAdded)
end
end)
