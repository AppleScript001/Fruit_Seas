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

local function getNPC()
    local dist, thing = math.huge
    for i, v in pairs(workspace.NPC.Fight:GetDescendants()) do
        if v:IsA "Model" and v:FindFirstChild("HumanoidRootPart") and v.Name == mobname then
            local mag = (game.Players.LocalPlayer.Character.HumanoidRootPart.Position - v.Parent.Position).magnitude
            if mag < dist then
                dist = mag
                thing = v
            end
        end
    end
    return thing
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
while Hit do task.wait(1)  -- Wait for 1 second before checking for enemies
    pcall(function()
                    
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = getNPC().UpperTorso.CFrame
    end)
end
end)
