local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local hrp = char:WaitForChild("HumanoidRootPart")

local RunService = game:GetService("RunService")

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "PlataformaUI"
screenGui.Parent = player:WaitForChild("PlayerGui")

local btn = Instance.new("TextButton")
btn.Size = UDim2.new(0, 150, 0, 50)
btn.Position = UDim2.new(0.4, 0, 0.85, 0)
btn.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
btn.TextColor3 = Color3.fromRGB(255,255,255)
btn.Font = Enum.Font.SourceSansBold
btn.TextSize = 24
btn.Text = "Ativar"
btn.Parent = screenGui
btn.MouseButton1Click:Connect(function()
	if btn.Text == "Ativar" then
 
 local plataforma = Instance.new("Part")
		plataforma.Size = Vector3.new(6,1,6)
		plataforma.Color = Color3.fromRGB(255,0,0)
		plataforma.Anchored = true
		plataforma.CanCollide = true
		plataforma.Parent = workspace
  
local ativo = true
		local tempo = 5

local conn
		conn = RunService.RenderStepped:Connect(function()
			if ativo and hrp and hrp.Parent then
				-- Sempre embaixo do personagem
				plataforma.Position = hrp.Position - Vector3.new(0,3,0)
			end
		end)

for i = tempo,1,-1 do
			btn.Text = tostring(i).."s"
			wait(1)
		end

ativo = false
		conn:Disconnect()
		plataforma:Destroy()
		btn.Text = "Ativar"
	end
end)
