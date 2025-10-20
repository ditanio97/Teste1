--// DRAGON CANNON - Versão Original \\--

-- Serviços
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local hrp = character:WaitForChild("HumanoidRootPart")

-- Variáveis
local cooldown = false
local COOLDOWN_TIME = 5
local FORCE = 350
local UP_ANGLE = 0.1 -- inclinação pra cima

-- Cria GUI
local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
screenGui.ResetOnSpawn = false

local button = Instance.new("TextButton", screenGui)
button.Size = UDim2.new(0, 160, 0, 50)
button.Position = UDim2.new(0.5, -80, 0, 20)
button.BackgroundColor3 = Color3.fromRGB(255, 180, 0)
button.Text = "⚡ Dragon Cannon ⚡"
button.TextScaled = true
button.Font = Enum.Font.GothamBold
button.TextColor3 = Color3.new(0, 0, 0)
button.AutoButtonColor = false
button.BackgroundTransparency = 0.1
button.Visible = true

-- Efeito de aura no jogador
local aura = Instance.new("ParticleEmitter")
aura.Texture = "rbxassetid://3170169887"
aura.Rate = 15
aura.Speed = NumberRange.new(0)
aura.Size = NumberSequence.new(1)
aura.Lifetime = NumberRange.new(1)
aura.Transparency = NumberSequence.new({ NumberSequenceKeypoint.new(0, 0.3), NumberSequenceKeypoint.new(1, 1) })
aura.Color = ColorSequence.new(Color3.fromRGB(255, 220, 50))
aura.Parent = hrp

-- Função do Dragon Cannon
local function dragonCannon()
	if cooldown then return end
	cooldown = true

	local ball = workspace:FindFirstChild("Football")
	if ball and ball:IsA("BasePart") then
		-- Efeito visual na bola
		local trail = Instance.new("Trail")
		trail.Attachment0 = Instance.new("Attachment", ball)
		trail.Attachment1 = Instance.new("Attachment", ball)
		trail.Color = ColorSequence.new(Color3.fromRGB(255, 220, 50))
		trail.Lifetime = 0.3
		trail.LightEmission = 1
		trail.Enabled = true
		trail.Parent = ball

		-- Faz o personagem "chutar" a bola
		local direction = (hrp.CFrame.LookVector + Vector3.new(0, UP_ANGLE, 0)).Unit
		local bodyVelocity = Instance.new("BodyVelocity")
		bodyVelocity.MaxForce = Vector3.new(1e5, 1e5, 1e5)
		bodyVelocity.Velocity = direction * FORCE
		bodyVelocity.Parent = ball

		game.Debris:AddItem(bodyVelocity, 0.3)
		game.Debris:AddItem(trail, 1)
	end

	-- Efeito de cooldown
	button.Text = "Aquecendo 🔥"
	button.BackgroundColor3 = Color3.fromRGB(255, 130, 0)

	local tween = TweenService:Create(button, TweenInfo.new(COOLDOWN_TIME, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
		BackgroundColor3 = Color3.fromRGB(255, 180, 0)
	})
	tween:Play()

	task.wait(COOLDOWN_TIME)
	cooldown = false
	button.Text = "⚡ Dragon Cannon ⚡"
end

-- Conexão
button.MouseButton1Click:Connect(dragonCannon)
