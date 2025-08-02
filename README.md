# Script-brainrot-
-- AVISO: Este script é apenas para fins educacionais. Usar cheats em jogos online é contra os termos de serviço da Roblox e pode resultar em banimento.

local player = game:GetService("Players").LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

-- Função para tornar o jogador invisível
function makeInvisible()
    for _, part in ipairs(character:GetDescendants()) do
        if part:IsA("BasePart") then
            part.Transparency = 1
            part.CastShadow = false
        end
    end
    
    -- Esconder acessórios
    for _, accessory in ipairs(character:GetAccessories()) do
        accessory.Handle.Transparency = 1
    end
end

-- Função para obter todos os equipamentos
function getAllGear()
    local backpack = player:WaitForChild("Backpack")
    local replicatedStorage = game:GetService("ReplicatedStorage")
    
    -- Verificar onde os equipamentos estão armazenados no jogo
    local gearLocation = game:GetService("Workspace"):FindFirstChild("Gear") or replicatedStorage:FindFirstChild("Gear")
    
    if gearLocation then
        for _, gear in ipairs(gearLocation:GetChildren()) do
            if gear:IsA("Tool") then
                local clone = gear:Clone()
                clone.Parent = backpack
            end
        end
    end
end

-- Função de teletransporte para a base do jogador
function teleportToBase()
    -- Verificar se o jogador tem uma base marcada
    local baseMarker = workspace:FindFirstChild(player.Name .. "_Base")
    
    if not baseMarker then
        -- Criar uma base se não existir
        baseMarker = Instance.new("Part")
        baseMarker.Name = player.Name .. "_Base"
        baseMarker.Size = Vector3.new(10, 1, 10)
        baseMarker.Position = Vector3.new(0, 100, 0) -- Posição padrão no céu
        baseMarker.Anchored = true
        baseMarker.Transparency = 0.5
        baseMarker.BrickColor = BrickColor.new("Bright red")
        baseMarker.Parent = workspace
    end
    
    -- Teleportar para a base
    local rootPart = character:FindFirstChild("HumanoidRootPart")
    if rootPart then
        rootPart.CFrame = baseMarker.CFrame + Vector3.new(0, 3, 0)
    end
end

-- Função para invadir bases roubadas
function raidStolenBases()
    for _, obj in ipairs(workspace:GetChildren()) do
        if string.find(obj.Name, "StolenBase") or string.find(obj.Name, "Brainrot") then
            -- Teleportar para a base roubada
            local rootPart = character:FindFirstChild("HumanoidRootPart")
            if rootPart then
                rootPart.CFrame = obj.CFrame + Vector3.new(0, 3, 0)
                break
            end
        end
    end
end

-- Ativar todas as habilidades
makeInvisible()
getAllGear()
teleportToBase()

-- Criar interface de usuário
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player.PlayerGui

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 200, 0, 150)
frame.Position = UDim2.new(0, 10, 0, 10)
frame.BackgroundTransparency = 0.7
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.Parent = screenGui

local title = Instance.new("TextLabel")
title.Text = "Brainrot Hacks"
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundTransparency = 1
title.TextColor3 = Color3.fromRGB(255, 0, 0)
title.Parent = frame

local invisBtn = Instance.new("TextButton")
invisBtn.Text = "Toggle Invisibility"
invisBtn.Size = UDim2.new(0.9, 0, 0, 30)
invisBtn.Position = UDim2.new(0.05, 0, 0, 40)
invisBtn.Parent = frame

local gearBtn = Instance.new("TextButton")
gearBtn.Text = "Get All Gear"
gearBtn.Size = UDim2.new(0.9, 0, 0, 30)
gearBtn.Position = UDim2.new(0.05, 0, 0, 80)
gearBtn.Parent = frame

local raidBtn = Instance.new("TextButton")
raidBtn.Text = "Raid Bases"
raidBtn.Size = UDim2.new(0.9, 0, 0, 30)
raidBtn.Position = UDim2.new(0.05, 0, 0, 120)
raidBtn.Parent = frame

-- Conectar botões às funções
invisBtn.MouseButton1Click:Connect(function()
    makeInvisible()
end)

gearBtn.MouseButton1Click:Connect(function()
    getAllGear()
end)

raidBtn.MouseButton1Click:Connect(function()
    raidStolenBases()
end)

-- Loop para manter a invisibilidade (alguns jogos tentam resetar)
while true do
    wait(1)
    if character then
        makeInvisible()
    end
end
