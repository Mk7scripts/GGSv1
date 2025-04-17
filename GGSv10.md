-- Criação da Interface UI-GUI
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local ESPButton = Instance.new("TextButton")
local ColorButton = Instance.new("TextButton")
local MenuButton = Instance.new("TextButton")
local FOVButton = Instance.new("TextButton") -- Botão de FOV

-- Configurações básicas da UI
ScreenGui.Name = "MENU_ESP"
ScreenGui.ResetOnSpawn = false -- Mantém a interface mesmo após a morte
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.new(0, 0, 0)
MainFrame.BackgroundTransparency = 0.5
MainFrame.Size = UDim2.new(0, 200, 0, 160) -- Tamanho ajustado para incluir o botão de FOV
MainFrame.Position = UDim2.new(0.5, -100, 0, 20)
MainFrame.Visible = false -- Inicialmente oculto

local MainFrameCorner = Instance.new("UICorner")
MainFrameCorner.CornerRadius = UDim.new(0, 10)
MainFrameCorner.Parent = MainFrame

ESPButton.Name = "ESPButton"
ESPButton.Parent = MainFrame
ESPButton.BackgroundColor3 = Color3.new(0, 0, 0)
ESPButton.Size = UDim2.new(0, 180, 0, 40)
ESPButton.Position = UDim2.new(0, 10, 0, 10)
ESPButton.Text = "ESP+"
ESPButton.TextColor3 = Color3.new(1, 1, 1)

local ESPButtonCorner = Instance.new("UICorner")
ESPButtonCorner.CornerRadius = UDim.new(0, 10)
ESPButtonCorner.Parent = ESPButton

ColorButton.Name = "ColorButton"
ColorButton.Parent = MainFrame
ColorButton.BackgroundColor3 = Color3.new(0, 0, 0)
ColorButton.Size = UDim2.new(0, 180, 0, 40)
ColorButton.Position = UDim2.new(0, 10, 0, 60)
ColorButton.Text = "BRANCO"
ColorButton.TextColor3 = Color3.new(1, 1, 1)

local ColorButtonCorner = Instance.new("UICorner")
ColorButtonCorner.CornerRadius = UDim.new(0, 10)
ColorButtonCorner.Parent = ColorButton

FOVButton.Name = "FOVButton"
FOVButton.Parent = MainFrame
FOVButton.BackgroundColor3 = Color3.new(0, 0, 0)
FOVButton.Size = UDim2.new(0, 180, 0, 40)
FOVButton.Position = UDim2.new(0, 10, 0, 110) -- Posição abaixo dos outros botões
FOVButton.Text = "FOV: PADRÃO"
FOVButton.TextColor3 = Color3.new(1, 1, 1)

local FOVButtonCorner = Instance.new("UICorner")
FOVButtonCorner.CornerRadius = UDim.new(0, 10)
FOVButtonCorner.Parent = FOVButton

MenuButton.Name = "MenuButton"
MenuButton.Parent = ScreenGui
MenuButton.BackgroundColor3 = Color3.new(0, 0, 0)
MenuButton.Size = UDim2.new(0, 50, 0, 30)
MenuButton.Position = UDim2.new(0, 20, 0, 20) -- Canto superior esquerdo
MenuButton.Text = "M"
MenuButton.TextColor3 = Color3.new(1, 1, 1)

local MenuButtonCorner = Instance.new("UICorner")
MenuButtonCorner.CornerRadius = UDim.new(0, 10)
MenuButtonCorner.Parent = MenuButton

-- Variáveis e funcionalidades
local ESPEnabled = false
local ESPColor = Color3.new(1, 1, 1) -- Branco
local FOVDefault = 70
local FOVZoomed = 120

-- Função para alternar ESP
local function toggleESP()
    ESPEnabled = not ESPEnabled
    ESPButton.Text = ESPEnabled and "ESP-" or "ESP+"
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer and player.Character then
            if ESPEnabled then
                if not player.Character:FindFirstChild("HighlightESP") then
                    local highlight = Instance.new("Highlight")
                    highlight.Name = "HighlightESP"
                    highlight.Parent = player.Character
                    highlight.FillTransparency = 1
                    highlight.OutlineTransparency = 0
                    highlight.OutlineColor = ESPColor
                end
            else
                if player.Character:FindFirstChild("HighlightESP") then
                    player.Character.HighlightESP:Destroy()
                end
            end
        end
    end
end

-- Função para mudar cor do ESP
local function changeESPColor()
    ESPColor = ESPColor == Color3.new(1, 1, 1) and Color3.new(1, 0, 0) or Color3.new(1, 1, 1)
    ColorButton.Text = ESPColor == Color3.new(1, 1, 1) and "BRANCO" or "VERMELHO"
    if ESPEnabled then
        toggleESP()
        toggleESP()
    end
end

-- Função para alternar FOV
local function toggleFOV()
    local camera = workspace.CurrentCamera
    if camera.FieldOfView == FOVDefault then
        camera.FieldOfView = FOVZoomed
        FOVButton.Text = "FOV: AMPLIADO"
    else
        camera.FieldOfView = FOVDefault
        FOVButton.Text = "FOV: PADRÃO"
    end
end

-- Função para alternar menu
local function toggleMenu()
    MainFrame.Visible = not MainFrame.Visible
end

-- Conexões
ESPButton.MouseButton1Click:Connect(toggleESP)
ColorButton.MouseButton1Click:Connect(changeESPColor)
FOVButton.MouseButton1Click:Connect(toggleFOV)
MenuButton.MouseButton1Click:Connect(toggleMenu)
