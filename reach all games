-- Serviços
local player = game.Players.LocalPlayer
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

-- Variáveis principais
local reach = 3.5
local hitPower = 1
local MAX_REACH = 9999
local MAX_HIT = 9999
local oneHitReachEnabled = false
local shapeView = 1

-- GUI principal
local screenGui = Instance.new("ScreenGui", game.CoreGui)
screenGui.ResetOnSpawn = false

local frame = Instance.new("Frame", screenGui)
frame.Size = UDim2.new(0, 240, 0, 310)
frame.Position = UDim2.new(0, 30, 0.5, -155)
frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
frame.BorderSizePixel = 2
frame.Active = true
frame.Draggable = true
frame.Visible = true

-- Botão flutuante "R" para abrir/fechar
local toggleBtn = Instance.new("TextButton", screenGui)
toggleBtn.Size = UDim2.new(0, 40, 0, 40)
toggleBtn.Position = UDim2.new(0, 5, 0.5, -20)
toggleBtn.BackgroundColor3 = Color3.fromRGB(100, 0, 0)
toggleBtn.TextColor3 = Color3.new(1, 1, 1)
toggleBtn.Text = "R"
toggleBtn.Font = Enum.Font.SourceSansBold
toggleBtn.TextScaled = true
toggleBtn.AutoButtonColor = true
toggleBtn.Active = true
toggleBtn.Draggable = true

toggleBtn.MouseButton1Click:Connect(function()
	frame.Visible = not frame.Visible
end)

-- Notificação "Script Executado!"
local notification = Instance.new("TextLabel", screenGui)
notification.Size = UDim2.new(0, 200, 0, 50)
notification.Position = UDim2.new(0.5, -100, 0.1, 0)
notification.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
notification.TextColor3 = Color3.new(1, 1, 1)
notification.Text = "✅ Script Executado!"
notification.TextScaled = true
notification.Font = Enum.Font.SourceSansBold
notification.Visible = true

task.delay(3, function()
	notification:Destroy()
end)

-- Função para criar campos numéricos
local function createSmartBox(parent, title, posY, defaultVal, callback)
	local container = Instance.new("Frame", parent)
	container.Size = UDim2.new(1, 0, 0.12, 0)
	container.Position = UDim2.new(0, 0, posY, 0)
	container.BackgroundTransparency = 1

	local label = Instance.new("TextLabel", container)
	label.Size = UDim2.new(0.6, 0, 1, 0)
	label.Position = UDim2.new(0, 0, 0, 0)
	label.BackgroundTransparency = 1
	label.TextColor3 = Color3.new(1,1,1)
	label.TextScaled = true
	label.Text = title

	local input = Instance.new("TextBox", container)
	input.Size = UDim2.new(0.4, 0, 1, 0)
	input.Position = UDim2.new(0.6, 0, 0, 0)
	input.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
	input.TextColor3 = Color3.new(1,1,1)
	input.TextScaled = true
	input.Text = tostring(defaultVal)
	input.ClearTextOnFocus = true
	input.Font = Enum.Font.SourceSansBold

	input.FocusLost:Connect(function(enter)
		if enter then
			local num = tonumber(input.Text)
			if num then
				callback(num)
			end
			input.Text = tostring(callback())
		end
	end)

	return function(newVal)
		input.Text = tostring(newVal)
	end
end

-- Campos
local updateReachText = createSmartBox(frame, "Reach:", 0.00, reach, function(val)
	if val then reach = math.clamp(val, 1, MAX_REACH) end
	return reach
end)

local updateHitText = createSmartBox(frame, "Força do Hit:", 0.12, hitPower, function(val)
	if val then hitPower = math.clamp(val, 1, MAX_HIT) end
	return hitPower
end)

local updateShapeViewText = createSmartBox(frame, "Visualizador (1=C,2=Q,3=L):", 0.24, shapeView, function(val)
	if val then shapeView = math.clamp(val, 1, 3) end
	return shapeView
end)

-- Função para criar botões
local function createButton(text, posX, posY, callback)
	local btn = Instance.new("TextButton", frame)
	btn.Size = UDim2.new(0.5, 0, 0.12, 0)
	btn.Position = UDim2.new(posX, 0, posY, 0)
	btn.Text = text
	btn.Font = Enum.Font.SourceSansBold
	btn.TextScaled = true
	btn.TextColor3 = Color3.new(1, 1, 1)
	btn.BackgroundColor3 = Color3.fromRGB(60, 0, 0)
	btn.MouseButton1Click:Connect(callback)
	return btn
end

-- Botões
createButton("-", 0, 0.36, function()
	reach = math.max(1, reach - 0.5)
	updateReachText(reach)
end)

createButton("+", 0.5, 0.36, function()
	reach = math.min(MAX_REACH, reach + 0.5)
	updateReachText(reach)
end)

createButton("-", 0, 0.48, function()
	hitPower = math.max(1, hitPower - 1)
	updateHitText(hitPower)
end)

createButton("+", 0.5, 0.48, function()
	hitPower = math.min(MAX_HIT, hitPower + 1)
	updateHitText(hitPower)
end)

createButton("-", 0, 0.60, function()
	shapeView = math.max(1, shapeView - 1)
	updateShapeViewText(shapeView)
end)

createButton("+", 0.5, 0.60, function()
	shapeView = math.min(3, shapeView + 1)
	updateShapeViewText(shapeView)
end)

local oneHitBtn
oneHitBtn = createButton("One Hit Reach: OFF", 0, 0.72, function()
	oneHitReachEnabled = not oneHitReachEnabled
	if oneHitReachEnabled then
		oneHitBtn.Text = "One Hit Reach: ON"
		oneHitBtn.BackgroundColor3 = Color3.fromRGB(0, 120, 0)
	else
		oneHitBtn.Text = "One Hit Reach: OFF"
		oneHitBtn.BackgroundColor3 = Color3.fromRGB(120, 0, 0)
	end
end)
oneHitBtn.Size = UDim2.new(1, 0, 0.12, 0)
oneHitBtn.Position = UDim2.new(0, 0, 0.72, 0)
oneHitBtn.BackgroundColor3 = Color3.fromRGB(120, 0, 0)

local resetBtn = Instance.new("TextButton", frame)
resetBtn.Size = UDim2.new(1, 0, 0.12, 0)
resetBtn.Position = UDim2.new(0, 0, 0.84, 0)
resetBtn.Text = "Centralizar GUI"
resetBtn.TextScaled = true
resetBtn.TextColor3 = Color3.new(1,1,1)
resetBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
resetBtn.MouseButton1Click:Connect(function()
	frame.Position = UDim2.new(0, 30, 0.5, -155)
end)

-- Visualizador
local visualizer = Instance.new("Part", workspace)
visualizer.Anchored = true
visualizer.CanCollide = false
visualizer.Material = Enum.Material.Neon
visualizer.BrickColor = BrickColor.Red()
visualizer.Transparency = 0.6

-- Loop de ataque
RunService.RenderStepped:Connect(function()
	local char = player.Character
	local tool = char and char:FindFirstChildOfClass("Tool")
	if tool and tool:FindFirstChild("Handle") then
		local handle = tool.Handle

		if shapeView == 1 then
			visualizer.Shape = Enum.PartType.Ball
			visualizer.Size = Vector3.new(reach, reach, reach)
			visualizer.Position = handle.Position
		elseif shapeView == 2 then
			visualizer.Shape = Enum.PartType.Block
			visualizer.Size = Vector3.new(reach, reach, reach)
			visualizer.Position = handle.Position
		else
			visualizer.Shape = Enum.PartType.Block
			visualizer.Size = Vector3.new(0.3, 5, reach)
			local lookVector = handle.CFrame.LookVector
			local centerPos = handle.Position + (lookVector * (reach / 2))
			visualizer.CFrame = CFrame.new(centerPos, centerPos + lookVector)
		end

		for _, other in ipairs(game.Players:GetPlayers()) do
			if other ~= player and other.Character and other.Character:FindFirstChild("HumanoidRootPart") then
				local dist = (handle.Position - other.Character.HumanoidRootPart.Position).Magnitude
				if dist <= reach then
					local hits = oneHitReachEnabled and 1 or hitPower
					for _, part in ipairs(other.Character:GetChildren()) do
						if part:IsA("BasePart") then
							for i = 1, hits do
								firetouchinterest(part, handle, 0)
								firetouchinterest(part, handle, 1)
							end
						end
					end
				end
			end
		end
	else
		visualizer.Size = Vector3.new(0, 0, 0)
	end
end)
