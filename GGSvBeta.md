-- Criação da Interface UI-GUI
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local ESPButton = Instance.new("TextButton")
local ColorButton = Instance.new("TextButton")

-- Configurações básicas da UI
ScreenGui.Name = "MENU_ESP"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.new(0, 0, 0)
MainFrame.BackgroundTransparency = 0.5
MainFrame.Size = UDim2.new(0, 200, 0, 100)
MainFrame.Position = UDim2.new(0.5, -100, 0, 20)

local MainFrameCorner = Instance.new("UICorner")
MainFrameCorner.CornerRadius = UDim.new(0, 10) -- Botões arredondados
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
ColorButton.Position = UDim2.new(0, 10, 0, 50)
ColorButton.Text = "COR+"
ColorButton.TextColor3 = Color3.new(1, 1, 1)

local ColorButtonCorner = Instance.new("UICorner")
ColorButtonCorner.CornerRadius = UDim.new(0, 10)
ColorButtonCorner.Parent = ColorButton

-- Variáveis para ESP
local ESPEnabled = false
local ESPColor = Color3.new(1, 1, 1) -- Branco

-- Função para ativar/desativar ESP
local function toggleESP()
    ESPEnabled = not ESPEnabled
    ESPButton.Text = ESPEnabled and "ESP-" or "ESP+"
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer and player.Character then
            for _, part in pairs(player.Character:GetChildren()) do
                if part:IsA("BasePart") then
                    if ESPEnabled then
                        local highlight = Instance.new("Highlight")
                        highlight.Name = "ESPHighlight"
                        highlight.Parent = part
                        highlight.FillTransparency = 1
                        highlight.OutlineColor = ESPColor
                        highlight.OutlineTransparency = 0
                    else
                        if part:FindFirstChild("ESPHighlight") then
                            part.ESPHighlight:Destroy()
                        end
                    end
                end
            end
        end
    end
end

-- Função para mudar a cor do ESP
local function changeESPColor()
    ESPColor = ESPColor == Color3.new(1, 1, 1) and Color3.new(1, 0, 0) or Color3.new(1, 1, 1) -- Branco ou Vermelho
    ColorButton.Text = ESPColor == Color3.new(1, 1, 1) and "COR+" or "COR-"
    if ESPEnabled then
        toggleESP()
        toggleESP() -- Atualiza bordas com nova cor
    end
end

-- Conexão dos botões às funções
ESPButton.MouseButton1Click:Connect(toggleESP)
ColorButton.MouseButton1Click:Connect(changeESPColor)
