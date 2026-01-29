-- Services
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

-- Variables
local player = Players.LocalPlayer
local mouse = player:GetMouse()
local screenGui = nil
local isDragging = false
local dragStart = nil
local dragStartPos = nil

-- Variáveis para controle
local lagTracks = {}
local speedActive = false
local speedConnection = nil  -- Conexão para o Heartbeat

-- Função para criar a UI
local function createUI()
	-- Criar ScreenGui
	screenGui = Instance.new("ScreenGui")
	screenGui.Name = "AhansLagBombUI"
	screenGui.Parent = player:WaitForChild("PlayerGui")
	screenGui.ResetOnSpawn = false
	screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

	-- Main Frame
	local mainFrame = Instance.new("Frame")
	mainFrame.Name = "MainFrame"
	mainFrame.Size = UDim2.new(0, 350, 0, 220)
	mainFrame.Position = UDim2.new(0.5, -175, 0.5, -110)
	mainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
	mainFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
	mainFrame.BorderSizePixel = 0
	mainFrame.Parent = screenGui
	
	-- Arredondar bordas
	local UICorner = Instance.new("UICorner")
	UICorner.CornerRadius = UDim.new(0, 12)
	UICorner.Parent = mainFrame
	
	-- Sombra
	local UIStroke = Instance.new("UIStroke")
	UIStroke.Color = Color3.fromRGB(50, 50, 50)
	UIStroke.Thickness = 2
	UIStroke.Parent = mainFrame

	-- Barra de título
	local titleBar = Instance.new("Frame")
	titleBar.Name = "TitleBar"
	titleBar.Size = UDim2.new(1, 0, 0, 30)
	titleBar.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
	titleBar.BorderSizePixel = 0
	titleBar.Parent = mainFrame
	
	local titleBarCorner = Instance.new("UICorner")
	titleBarCorner.CornerRadius = UDim.new(0, 12, 0, 0)
	titleBarCorner.Parent = titleBar

	-- Texto do título
	local titleLabel = Instance.new("TextLabel")
	titleLabel.Name = "TitleLabel"
	titleLabel.Size = UDim2.new(1, -40, 1, 0)
	titleLabel.Position = UDim2.new(0, 10, 0, 0)
	titleLabel.BackgroundTransparency = 1
	titleLabel.Text = "Aham's Lag Bomb"
	titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
	titleLabel.Font = Enum.Font.GothamBold
	titleLabel.TextSize = 16
	titleLabel.TextXAlignment = Enum.TextXAlignment.Left
	titleLabel.Parent = titleBar

	-- Botão de fechar (opcional)
	local closeButton = Instance.new("TextButton")
	closeButton.Name = "CloseButton"
	closeButton.Size = UDim2.new(0, 30, 0, 30)
	closeButton.Position = UDim2.new(1, -30, 0, 0)
	closeButton.BackgroundTransparency = 1
	closeButton.Text = "X"
	closeButton.TextColor3 = Color3.fromRGB(255, 100, 100)
	closeButton.Font = Enum.Font.GothamBold
	closeButton.TextSize = 18
	closeButton.Parent = titleBar
	
	closeButton.MouseButton1Click:Connect(function()
		screenGui:Destroy()
	end)

	-- Área dos botões
	local buttonContainer = Instance.new("Frame")
	buttonContainer.Name = "ButtonContainer"
	buttonContainer.Size = UDim2.new(1, -40, 1, -70)
	buttonContainer.Position = UDim2.new(0, 20, 0, 50)
	buttonContainer.BackgroundTransparency = 1
	buttonContainer.Parent = mainFrame

	-- Função para criar botões
	local function createButton(text, position, callback)
		local button = Instance.new("TextButton")
		button.Name = text .. "Button"
		button.Size = UDim2.new(1, 0, 0, 40)
		button.Position = position
		button.BackgroundColor3 = Color3.fromRGB(0, 100, 255)
		button.Text = text
		button.TextColor3 = Color3.fromRGB(255, 255, 255)
		button.Font = Enum.Font.GothamBold
		button.TextSize = 14
		button.Parent = buttonContainer
		
		local buttonCorner = Instance.new("UICorner")
		buttonCorner.CornerRadius = UDim.new(0, 8)
		buttonCorner.Parent = button
		
		local buttonStroke = Instance.new("UIStroke")
		buttonStroke.Color = Color3.fromRGB(0, 150, 255)
		buttonStroke.Thickness = 2
		buttonStroke.Parent = button
		
		-- Efeito hover
		button.MouseEnter:Connect(function()
			TweenService:Create(button, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(0, 120, 255)}):Play()
		end)
		
		button.MouseLeave:Connect(function()
			TweenService:Create(button, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(0, 100, 255)}):Play()
		end)
		
		button.MouseButton1Click:Connect(callback)
		
		return button
	end

	-- Botão Lag Bomb
	local lagBombButton = createButton("Lag Bomb", UDim2.new(0, 0, 0, 0), function()
		lagBomb()
	end)

	-- Botão Stop Lag
	local stopLagButton = createButton("Stop Lag", UDim2.new(0, 0, 0, 50), function()
		stopLag()
	end)

	-- Botão Speed
	local speedButton = createButton("Speed", UDim2.new(0, 0, 0, 100), function()
		activateSpeed()
	end)

	-- Função de arrastar a UI
	local function startDragging(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 then
			isDragging = true
			dragStart = input.Position
			dragStartPos = mainFrame.Position
			
			local connection
			connection = input.Changed:Connect(function()
				if input.UserInputState == Enum.UserInputState.End then
					isDragging = false
					connection:Disconnect()
				end
			end)
		end
	end

	local function updateDrag(input)
		if isDragging then
			local delta = input.Position - dragStart
			mainFrame.Position = UDim2.new(
				dragStartPos.X.Scale,
				dragStartPos.X.Offset + delta.X,
				dragStartPos.Y.Scale,
				dragStartPos.Y.Offset + delta.Y
			)
		end
	end

	titleBar.InputBegan:Connect(startDragging)
	titleBar.InputChanged:Connect(updateDrag)
	
	-- Permitir arrastar também pelo título
	titleLabel.InputBegan:Connect(startDragging)
	titleLabel.InputChanged:Connect(updateDrag)
end

-- Função Lag Bomb EXATA ORIGINAL sem texto visível
function lagBomb()
	print("hehe")
	
	-- A função original completa começa aqui:
	local player = Players.LocalPlayer
	local playerGui = player:WaitForChild("PlayerGui")
	
	-- Criar a ScreenGui mas desabilitar para ficar invisível
	local ScreenGui = Instance.new("ScreenGui")
	ScreenGui.Name = "AhansLagBombEffect"
	ScreenGui.Parent = playerGui
	ScreenGui.ResetOnSpawn = false
	ScreenGui.Enabled = false -- TORNA INVISÍVEL PARA O CLIENT
	
	local Frame = Instance.new("Frame")
	Frame.Size = UDim2.new(1, 0, 1, 0)
	Frame.BackgroundTransparency = 1
	Frame.Parent = ScreenGui
	
	-- Manter o TextLabel mas ele não será visível porque o ScreenGui está desabilitado
	local TextLabel = Instance.new("TextLabel")
	TextLabel.Parent = Frame
	TextLabel.AnchorPoint = Vector2.new(0.5, 0.5)
	TextLabel.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	TextLabel.BackgroundTransparency = 1
	TextLabel.BorderSizePixel = 0
	TextLabel.Position = UDim2.new(0.5, 0, 0.5, 0)
	TextLabel.Size = UDim2.new(0, 408, 0, 152)
	TextLabel.Font = Enum.Font.FredokaOne
	TextLabel.Text = "Aham's Lag Bomb" -- Texto alterado mas não visível
	TextLabel.TextColor3 = Color3.fromRGB(98, 17, 148)
	TextLabel.TextScaled = true
	TextLabel.TextWrapped = true
	
	-- Função anti-crash original
	spawn(function()
		function anticrashSkid(player)
			function anticrashChar(char)
				local hum = char:WaitForChild("Humanoid", 5)
				local anim = hum:WaitForChild("Animator", 5)
				anim.AnimationPlayed:Connect(function(track)
					track:AdjustSpeed(-math.huge)
					track:Stop()
				end)
				for _, v in anim:GetPlayingAnimationTracks() do
					v:AdjustSpeed(-math.huge)
					v:Stop()
				end
			end
			player.CharacterAdded:Connect(anticrashChar)
			if player.Character then
				anticrashChar(player.Character)
			end
		end
		
		local plrs = game:GetService("Players")
		for i, v in plrs:GetPlayers() do
			if v ~= plrs.LocalPlayer then
				anticrashSkid(v)
			end
		end
		plrs.PlayerAdded:Connect(anticrashSkid)
	end)
	
	-- Sistema de animações original
	spawn(function()
		while task.wait() do
			for i = 1, 5 do
				local anim = Instance.new("Animation")
				anim.AnimationId = `http{game:GetService("HttpService"):GenerateGUID()}=113437017590496`
				
				pcall(function()
					local track = game:GetService("Players").LocalPlayer.Character.Humanoid.Animator:LoadAnimation(anim)
					track:Play(9e9, 9e9, 9e9)
					game:GetService("RunService").PreRender:Wait()
					track:AdjustSpeed(-math.huge)
					table.insert(lagTracks, track) -- Adicionar para controle
				end)
			end
		end
	end)
	
	print("Lag Bomb ativado!")
end

-- Função Stop Lag
function stopLag()
	print("Lag parado!")
	
	-- Parar todas as animações ativas
	for _, track in ipairs(lagTracks) do
		pcall(function()
			track:Stop()
		end)
	end
	
	-- Limpar tabela
	lagTracks = {}
	
	-- Remover as UIs de lag
	local playerGui = player:WaitForChild("PlayerGui")
	for _, gui in ipairs(playerGui:GetChildren()) do
		if gui.Name == "AhansLagBombEffect" then
			gui:Destroy()
		end
	end
end

-- Função Speed - Usando Heartbeat
function activateSpeed()
	print("Speed ativado!")
	
	-- Se já estiver ativo, desativa
	if speedActive then
		speedActive = false
		
		-- Desconectar o Heartbeat
		if speedConnection then
			speedConnection:Disconnect()
			speedConnection = nil
		end
		
		-- Restaurar walkspeed normal
		local char = player.Character
		if char then
			local humanoid = char:FindFirstChild("Humanoid")
			if humanoid then
				humanoid.WalkSpeed = 16 -- Valor padrão do Roblox
				print("Speed desativado! WalkSpeed restaurado para 16")
			end
		end
		
		return
	end
	
	speedActive = true
	
	-- Conectar ao Heartbeat para aplicar walkspeed 45
	speedConnection = RunService.Heartbeat:Connect(function()
		if speedActive then
			local char = player.Character
			if char then
				local humanoid = char:FindFirstChild("Humanoid")
				
				-- Aplicar walkspeed 45
				if humanoid then
					humanoid.WalkSpeed = 45
				end
			end
		end
	end)
	
	print("Speed ativado! WalkSpeed ajustado para 45 usando Heartbeat")
end

-- Inicializar a UI quando o script rodar
createUI()

-- Notificação de inicialização
print("Aham's Lag Bomb carregado!")