-- Criação da Interface UI-GUI
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local ESPButton = Instance.new("TextButton")
local ColorButton = Instance.new("TextButton")
local MenuButton = Instance.new("TextButton")
local FOVButton = Instance.new("TextButton")
local SpeedButton = Instance.new("TextButton") -- Botão de Velocidade

-- Configurações básicas da UI
ScreenGui.Name = "MENU_ESP"
ScreenGui.ResetOnSpawn = false -- Mantém a interface mesmo após a morte
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.new(0, 0, 0)
MainFrame.BackgroundTransparency = 0.5
MainFrame.Size = UDim2.new(0, 200, 0, 250) -- Ajustado para incluir todos os botões
MainFrame.Position = UDim2.new(0.5, -100, 0, -300) -- Inicialmente fora da tela
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
FOVButton.Position = UDim2.new(0, 10, 0, 110)
FOVButton.Text = "FOV: PADRÃO"
FOVButton.TextColor3 = Color3.new(1, 1, 1)

local FOVButtonCorner = Instance.new("UICorner")
FOVButtonCorner.CornerRadius = UDim.new(0, 10)
FOVButtonCorner.Parent = FOVButton

SpeedButton.Name = "SpeedButton"
SpeedButton.Parent = MainFrame
SpeedButton.BackgroundColor3 = Color3.new(0, 0, 0)
SpeedButton.Size = UDim2.new(0, 180, 0, 40)
SpeedButton.Position = UDim2.new(0, 10, 0, 160)
SpeedButton.Text = "VELOCIDADE: NORMAL"
SpeedButton.TextColor3 = Color3.new(1, 1, 1)

local SpeedButtonCorner = Instance.new("UICorner")
SpeedButtonCorner.CornerRadius = UDim.new(0, 10)
SpeedButtonCorner.Parent = SpeedButton

MenuButton.Name = "MenuButton"
MenuButton.Parent = ScreenGui
MenuButton.BackgroundColor3 = Color3.new(0, 0, 0)
MenuButton.Size = UDim2.new(0, 80, 0, 50) -- Tamanho aumentado para maior visibilidade
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
local defaultWalkSpeed = 16 -- Velocidade padrão
local increasedWalkSpeed = 50 -- Velocidade aumentada
local isSpeedBoosted = false -- Para alternar entre velocidades

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

-- Função para alternar cores adicionais do ESP
local function changeESPColor()
    local colors = {
        {color = Color3.new(1, 1, 1), name = "BRANCO"},
        {color = Color3.new(1, 0, 0), name = "VERMELHO"},
        {color = Color3.new(0, 0, 1), name = "AZUL"},
        {color = Color3.new(0, 1, 0), name = "VERDE"}
    }
    local currentIndex
    for i, c in ipairs(colors) do
        if ESPColor == c.color then
            currentIndex = i
            break
        end
    end
    local nextIndex = (currentIndex % #colors) + 1
    ESPColor = colors[nextIndex].color
    ColorButton.Text = colors[nextIndex].name
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

-- Função para alternar velocidade
local function toggleSpeed()
    local character = game.Players.LocalPlayer.Character
    if character and character:FindFirstChild("Humanoid") then
        isSpeedBoosted = not isSpeedBoosted
        character.Humanoid.WalkSpeed = isSpeedBoosted and increasedWalkSpeed or defaultWalkSpeed
        SpeedButton.Text = isSpeedBoosted and "VELOCIDADE: RÁPIDA" or "VELOCIDADE: NORMAL"
    end
end

-- Função para animar o menu (deslizar suavemente)
local function animateMenu()
    if MainFrame.Visible then
        -- Ocultar o menu com animação
        for i = 1, 10 do
            MainFrame.Position = MainFrame.Position - UDim2.new(0, 0, 0.1, 0)
            wait(0.02) -- Controla a velocidade da animação
        end
        MainFrame.Visible = false
    else
        -- Exibir o menu com animação
        MainFrame.Visible = true
        for i = 1, 10 do
            MainFrame.Position = MainFrame.Position + UDim2.new(0, 0, 0.1, 0)
            wait(0.02) -- Controla a velocidade da animação
        end
    end
end

-- Conexões
ESPButton.MouseButton1Click:Connect(toggleESP)
ColorButton.MouseButton1Click:Connect(changeESPColor)
FOVButton.MouseButton1Click:Connect(toggleFOV)
SpeedButton.MouseButton1Click:Connect(toggleSpeed)
MenuButton.MouseButton1Click:Connect(animateMenu)
