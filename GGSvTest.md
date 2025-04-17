-- Criação da Interface UI-GUI
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local ESPButton = Instance.new("TextButton")
local ColorButton = Instance.new("TextButton")

ScreenGui.Name = "MENU_ESP"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.new(0, 0, 0)
MainFrame.BackgroundTransparency = 0.5
MainFrame.Size = UDim2.new(0, 200, 0, 100)
MainFrame.Position = UDim2.new(0.5, -100, 0, 20)

ESPButton.Name = "ESPButton"
ESPButton.Parent = MainFrame
ESPButton.BackgroundColor3 = Color3.new(0, 0, 0)
ESPButton.Size = UDim2.new(0, 180, 0, 40)
ESPButton.Position = UDim2.new(0, 10, 0, 10)
ESPButton.Text = "ESP+"
ESPButton.TextColor3 = Color3.new(1, 1, 1)

ColorButton.Name = "ColorButton"
ColorButton.Parent = MainFrame
ColorButton.BackgroundColor3 = Color3.new(0, 0, 0)
ColorButton.Size = UDim2.new(0, 180, 0, 40)
ColorButton.Position = UDim2.new(0, 10, 0, 50)
ColorButton.Text = "COR+"
ColorButton.TextColor3 = Color3.new(1, 1, 1)

-- Função para ativar/desativar ESP
local ESPEnabled = false
local ESPColor = Color3.new(1, 1, 1) -- Branco

local function toggleESP()
    ESPEnabled = not ESPEnabled
    ESPButton.Text = ESPEnabled and "ESP-" or "ESP+"
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            if ESPEnabled then
                local box = Instance.new("BoxHandleAdornment")
                box.Name = "ESPBox"
                box.Parent = player.Character.HumanoidRootPart
                box.Adornee = player.Character.HumanoidRootPart
                box.Size = Vector3.new(4, 6, 4)
                box.Color3 = ESPColor
                box.Transparency = 0.5
                box.ZIndex = 0
                box.AlwaysOnTop = true
            else
                if player.Character.HumanoidRootPart:FindFirstChild("ESPBox") then
                    player.Character.HumanoidRootPart.ESPBox:Destroy()
                end
            end
        end
    end
end

-- Função para mudar a cor do ESP
local function changeESPColor()
    ESPColor = ESPColor == Color3.new(1, 1, 1) and Color3.new(1, 0, 0) or Color3.new(1, 1, 1) -- Alterna entre Branco e Vermelho
    ColorButton.Text = ESPColor == Color3.new(1, 1, 1) and "COR+" or "COR-"
    if ESPEnabled then
        toggleESP()
        toggleESP()
    end
end

ESPButton.MouseButton1Click:Connect(toggleESP)
ColorButton.MouseButton1Click:Connect(changeESPColor)
