local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")

local player = Players.LocalPlayer

---------------------------------------------------
-- GUI
---------------------------------------------------

local gui = Instance.new("ScreenGui")
gui.Name = "HDGui"
gui.ResetOnSpawn = false
gui.Parent = player.PlayerGui
gui.IgnoreGuiInset = true

---------------------------------------------------
-- BOTÃO HD
---------------------------------------------------

local open = Instance.new("TextButton")
open.Parent = gui
open.Size = UDim2.fromOffset(55,55)
open.Position = UDim2.new(0,15,0.5,-27)

open.Text = "HD"
open.TextScaled = true
open.Font = Enum.Font.GothamBold

open.BackgroundColor3 = Color3.fromRGB(35,35,35)
open.TextColor3 = Color3.new(1,1,1)

Instance.new("UICorner",open)

---------------------------------------------------
-- HD BUTTON — ARRASTÁVEL NO PC E MOBILE
---------------------------------------------------

local draggingHD = false
local dragStartHD = nil
local startPosHD = nil

local movedHD = false
local ignoreNextClick = false

local activeInputHD = nil

---------------------------------------------------
-- INÍCIO DO ARRASTE
---------------------------------------------------

open.InputBegan:Connect(function(input)

	if input.UserInputType ~= Enum.UserInputType.MouseButton1
		and input.UserInputType ~= Enum.UserInputType.Touch then
		return
	end

	draggingHD = true
	movedHD = false

	dragStartHD = input.Position
	startPosHD = open.Position

	activeInputHD = input

end)

---------------------------------------------------
-- MOVIMENTO
---------------------------------------------------

UIS.InputChanged:Connect(function(input)

	if not draggingHD then
		return
	end

	local isMouseMovement =
		input.UserInputType == Enum.UserInputType.MouseMovement

	local isTouchMovement =
		input.UserInputType == Enum.UserInputType.Touch
		and input == activeInputHD

	if not isMouseMovement and not isTouchMovement then
		return
	end

	local delta = input.Position - dragStartHD

	if delta.Magnitude > 5 then
		movedHD = true

		open.Position = UDim2.new(
			startPosHD.X.Scale,
			startPosHD.X.Offset + delta.X,
			startPosHD.Y.Scale,
			startPosHD.Y.Offset + delta.Y
		)
	end

end)

---------------------------------------------------
-- FIM DO ARRASTE
---------------------------------------------------

UIS.InputEnded:Connect(function(input)

	local isMouseRelease =
		input.UserInputType == Enum.UserInputType.MouseButton1

	local isTouchRelease =
		input.UserInputType == Enum.UserInputType.Touch
		and input == activeInputHD

	if not isMouseRelease and not isTouchRelease then
		return
	end

	if movedHD then

		ignoreNextClick = true

		task.delay(0.15, function()
			ignoreNextClick = false
		end)

	end

	draggingHD = false
	activeInputHD = nil

end)

---------------------------------------------------
-- MENU
---------------------------------------------------

local frame = Instance.new("Frame")
frame.Parent = gui

frame.Size = UDim2.fromOffset(430, 500)

-- Centraliza a interface na tela
frame.AnchorPoint = Vector2.new(0.5, 0.5)
frame.Position = UDim2.fromScale(0.5, 0.5)

frame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
frame.Visible = false

Instance.new("UICorner", frame)

---------------------------------------------------
-- ABRIR E FECHAR O PAINEL
---------------------------------------------------

---------------------------------------------------
-- ABRIR / FECHAR HD ADMIN
-- CENTRALIZA SOMENTE NA PRIMEIRA ABERTURA
---------------------------------------------------

local hasOpenedBefore = false

open.MouseButton1Click:Connect(function()

	if ignoreNextClick then
		return
	end

	if frame.Visible then

		-- Fecha sem alterar a posição.
		frame.Visible = false

	else

		-- Centraliza somente na primeira abertura.
		if not hasOpenedBefore then

			frame.AnchorPoint = Vector2.new(0.5, 0.5)
			frame.Position = UDim2.fromScale(0.5, 0.5)

			hasOpenedBefore = true

		end

		frame.Visible = true

	end

end)

---------------------------------------------------
-- TOPO
---------------------------------------------------

local top = Instance.new("Frame")
top.Parent = frame
top.Position = UDim2.new(0, 0, 0, 0)
top.Size = UDim2.new(1,0,0,40)
top.Name = "Top"

top.BackgroundColor3 = Color3.fromRGB(18,18,18)

---------------------------------------------------

local title = Instance.new("TextLabel")
title.Parent = top
title.BackgroundTransparency = 1
title.Size = UDim2.new(1,-80,1,0)
title.Position = UDim2.new(0,15,0,0)

title.Text = "HD Script v1"
title.TextColor3 = Color3.new(1,1,1)
title.TextSize = 22
title.Font = Enum.Font.GothamBold
title.TextXAlignment = Enum.TextXAlignment.Left

local creator = Instance.new("TextLabel")
creator.Parent = top
creator.BackgroundTransparency = 1
creator.Size = UDim2.new(0.709, -80,0.325, 0)
creator.Position = UDim2.new(0, 180,0, 22)

creator.Text = "Made by LucasDaviSonic2016"
creator.TextColor3 = Color3.new(0.211765, 0.211765, 0.211765)
creator.TextScaled = true
creator.Name = "Creator"
creator.Font = Enum.Font.GothamBold
creator.TextXAlignment = Enum.TextXAlignment.Left

---------------------------------------------------
-- FECHAR
---------------------------------------------------

local close = Instance.new("TextButton")
close.Parent = top
close.Size = UDim2.fromOffset(30,30)
close.Position = UDim2.new(1,-35,.5,-15)

close.Text = "X"
close.TextColor3 = Color3.new(1,1,1)
close.Font = Enum.Font.GothamBold
close.BackgroundColor3 = Color3.fromRGB(170,0,0)

Instance.new("UICorner",close)

---------------------------------------------------
-- MINIMIZAR
---------------------------------------------------

local minimize = Instance.new("TextButton")
minimize.Parent = top
minimize.Size = UDim2.fromOffset(30,30)
minimize.Position = UDim2.new(1,-70,.5,-15)

minimize.Text = "_"
minimize.TextColor3 = Color3.new(1,1,1)
minimize.Font = Enum.Font.GothamBold
minimize.BackgroundColor3 = Color3.fromRGB(70,70,70)

Instance.new("UICorner",minimize)

---------------------------------------------------
-- SCROLL
---------------------------------------------------

-- SCROLLINGFRAME RESPONSIVO

local scroll = Instance.new("ScrollingFrame")
scroll.Name = "CommandScroll"
scroll.Parent = frame

scroll.Position = UDim2.new(0, 10, 0, 50)
scroll.Size = UDim2.new(1, -20, 1, -60)

scroll.BackgroundTransparency = 1
scroll.BorderSizePixel = 0

scroll.ScrollBarThickness = 6
scroll.ScrollBarImageColor3 = Color3.fromRGB(180, 180, 180)

scroll.Active = true
scroll.ScrollingEnabled = true
scroll.ScrollingDirection = Enum.ScrollingDirection.Y

scroll.AutomaticCanvasSize = Enum.AutomaticSize.Y
scroll.CanvasSize = UDim2.new(0, 0, 0, 0)

scroll.ElasticBehavior = Enum.ElasticBehavior.WhenScrollable
scroll.VerticalScrollBarInset = Enum.ScrollBarInset.ScrollBar

-- Layout dos comandos
local layout = Instance.new("UIListLayout")
layout.Parent = scroll

layout.SortOrder = Enum.SortOrder.LayoutOrder
layout.Padding = UDim.new(0, 6)

-- Atualiza a área rolável conforme os comandos são adicionados
layout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
	scroll.CanvasSize = UDim2.new(
		0,
		0,
		0,
		layout.AbsoluteContentSize.Y + 10
	)
end)

-- RESPONSIVIDADE AUTOMÁTICA DO HD ADMIN

local responsiveScale = frame:FindFirstChild("ResponsiveScale")

if not responsiveScale then
	responsiveScale = Instance.new("UIScale")
	responsiveScale.Name = "ResponsiveScale"
	responsiveScale.Parent = frame
end

local viewportConnection

local function UpdateResponsiveGUI()
	local camera = workspace.CurrentCamera
	if not camera then return end

	local viewport = camera.ViewportSize

	-- Detecta o tipo de controle utilizado pelo jogador
	local touchMode = UIS.PreferredInput == Enum.PreferredInput.Touch

	local marginX
	local marginY

	if touchMode then
		-- Celular e tablet
		marginX = 24
		marginY = 90
	else
		-- PC e outros dispositivos
		marginX = 40
		marginY = 60
	end

	-- Calcula o tamanho necessário para caber na tela
	local scaleX = (viewport.X - marginX) / 430
	local scaleY = (viewport.Y - marginY) / 500

	local scale = math.min(1, scaleX, scaleY)

	responsiveScale.Scale = math.clamp(scale, 0.55, 1)
end

local function ConnectCamera()
	if viewportConnection then
		viewportConnection:Disconnect()
		viewportConnection = nil
	end

	local camera = workspace.CurrentCamera

	if camera then
		viewportConnection = camera:GetPropertyChangedSignal("ViewportSize"):Connect(UpdateResponsiveGUI)
	end

	UpdateResponsiveGUI()
end

-- Atualiza quando o jogador muda o tipo de controle
UIS:GetPropertyChangedSignal("PreferredInput"):Connect(UpdateResponsiveGUI)

-- Atualiza quando a resolução ou câmera muda
workspace:GetPropertyChangedSignal("CurrentCamera"):Connect(ConnectCamera)

ConnectCamera()

---------------------------------------------------
-- HD ADMIN FRAME: ARRASTE E CENTRALIZAÇÃO
-- PC + MOBILE
---------------------------------------------------

local draggingFrame = false
local dragStartFrame = nil
local startPosFrame = nil
local activeInputFrame = nil

---------------------------------------------------
-- LIMITAR O PAINEL À TELA
---------------------------------------------------

local function KeepFrameOnScreen()

	if not frame or not frame.Parent then
		return
	end

	local camera = workspace.CurrentCamera

	if not camera then
		return
	end

	local viewport = camera.ViewportSize
	local size = frame.AbsoluteSize
	local anchor = frame.AnchorPoint

	-- Converte a posição atual para coordenadas da tela.
	local absolutePosition = frame.AbsolutePosition

	local x = absolutePosition.X + size.X * anchor.X
	local y = absolutePosition.Y + size.Y * anchor.Y

	-- Limites horizontais.
	local minX = size.X * anchor.X
	local maxX = viewport.X - size.X * (1 - anchor.X)

	-- Limites verticais.
	local minY = size.Y * anchor.Y
	local maxY = viewport.Y - size.Y * (1 - anchor.Y)

	-- Se o painel for maior que a tela, centraliza nesse eixo.
	if minX > maxX then
		minX = viewport.X / 2
		maxX = minX
	end

	if minY > maxY then
		minY = viewport.Y / 2
		maxY = minY
	end

	x = math.clamp(x, minX, maxX)
	y = math.clamp(y, minY, maxY)

	-- Usa sempre a mesma referência de coordenadas.
	-- Limita a posição durante o arraste, sem saltos ao soltar.
	local camera = workspace.CurrentCamera

	if camera then
		local viewport = camera.ViewportSize
		local size = frame.AbsoluteSize
		local anchor = frame.AnchorPoint

		local minX = size.X * anchor.X
		local maxX = viewport.X - size.X * (1 - anchor.X)

		local minY = size.Y * anchor.Y
		local maxY = viewport.Y - size.Y * (1 - anchor.Y)

		-- Garante limites válidos mesmo em telas pequenas.
		if minX > maxX then
			minX = viewport.X / 2
			maxX = minX
		end

		if minY > maxY then
			minY = viewport.Y / 2
			maxY = minY
		end

		x = math.clamp(x, minX, maxX)
		y = math.clamp(y, minY, maxY)
	end

	frame.Position = UDim2.fromOffset(x, y)

end

---------------------------------------------------
-- INÍCIO DO ARRASTE
---------------------------------------------------

top.InputBegan:Connect(function(input)

	if input.UserInputType ~= Enum.UserInputType.MouseButton1
		and input.UserInputType ~= Enum.UserInputType.Touch then
		return
	end

	draggingFrame = true
	dragStartFrame = input.Position
	startPosFrame = frame.Position
	activeInputFrame = input

end)

---------------------------------------------------
-- MOVIMENTO
---------------------------------------------------

UIS.InputChanged:Connect(function(input)

	if not draggingFrame then
		return
	end

	local isMouseMovement =
		input.UserInputType == Enum.UserInputType.MouseMovement

	local isTouchMovement =
		input.UserInputType == Enum.UserInputType.Touch
		and input == activeInputFrame

	if not isMouseMovement and not isTouchMovement then
		return
	end

	local delta = input.Position - dragStartFrame

	-- Calcula a posição usando o ponto inicial do arraste.
	local viewport = workspace.CurrentCamera.ViewportSize

	local x =
		startPosFrame.X.Scale * viewport.X
		+ startPosFrame.X.Offset
		+ delta.X

	local y =
		startPosFrame.Y.Scale * viewport.Y
		+ startPosFrame.Y.Offset
		+ delta.Y

	-- Atualiza a posição calculada durante o arraste.
	-- Limita a posição durante o arraste, sem saltos ao soltar.
	local camera = workspace.CurrentCamera

	if camera then
		local viewport = camera.ViewportSize
		local size = frame.AbsoluteSize
		local anchor = frame.AnchorPoint

		local minX = size.X * anchor.X
		local maxX = viewport.X - size.X * (1 - anchor.X)

		local minY = size.Y * anchor.Y
		local maxY = viewport.Y - size.Y * (1 - anchor.Y)

		-- Garante limites válidos mesmo em telas pequenas.
		if minX > maxX then
			minX = viewport.X / 2
			maxX = minX
		end

		if minY > maxY then
			minY = viewport.Y / 2
			maxY = minY
		end

		x = math.clamp(x, minX, maxX)
		y = math.clamp(y, minY, maxY)
	end

	frame.Position = UDim2.fromOffset(x, y)
end)

---------------------------------------------------
-- FIM DO ARRASTE — SEM PULO DE POSIÇÃO
---------------------------------------------------

UIS.InputEnded:Connect(function(input)

	
	local isMouseRelease =
		input.UserInputType == Enum.UserInputType.MouseButton1

	local isTouchRelease =
		input.UserInputType == Enum.UserInputType.Touch
		and input == activeInputFrame

	if not isMouseRelease and not isTouchRelease then
		return
	end

	draggingFrame = false
	activeInputFrame = nil

	-- Não recalcula a posição ao soltar.
	-- O painel já foi limitado durante o movimento
	-- quando necessário por outras partes do sistema.
	

end)


---------------------------------------------------
-- RESOLUÇÃO ALTERADA
---------------------------------------------------

local connectedCamera = nil
local viewportConnection = nil

local function ConnectHDFrameCamera()

	if viewportConnection then
		viewportConnection:Disconnect()
		viewportConnection = nil
	end

	connectedCamera = workspace.CurrentCamera

	if connectedCamera then

		viewportConnection =
			connectedCamera:GetPropertyChangedSignal("ViewportSize"):Connect(function()

				KeepFrameOnScreen()

			end)

	end

end

workspace:GetPropertyChangedSignal("CurrentCamera"):Connect(
	ConnectHDFrameCamera
)

ConnectHDFrameCamera()

---------------------------------------------------
-- BOTÕES
---------------------------------------------------

close.MouseButton1Click:Connect(function()
	frame.Visible = false
end)

---------------------------------------------------
-- MINIMIZAR / RESTAURAR SEM TELETRANSPORTE
---------------------------------------------------

local minimized = false

local expandedSize = UDim2.fromOffset(430, 500)
local minimizedSize = UDim2.fromOffset(430, 40)

---

 -- MINIMIZAR / RESTAURAR SEM DESLOCAR A BARRA

minimize.MouseButton1Click:Connect(function()

	
	-- Guarda a posição visual do topo antes de mudar o tamanho.
	local oldTopY = frame.AbsolutePosition.Y

	minimized = not minimized

	if minimized then
		scroll.Visible = false
		frame.Size = minimizedSize
	else
		frame.Size = expandedSize
		scroll.Visible = true
	end

	-- Espera o Roblox atualizar o tamanho visual.
	task.defer(function()

		if not frame or not frame.Parent then
			return
		end

		local camera = workspace.CurrentCamera
		if not camera then
			return
		end

		local newHeight = frame.AbsoluteSize.Y
		local anchorY = frame.AnchorPoint.Y

		-- Calcula a posição necessária para preservar o topo.
		local positionY = oldTopY + newHeight * anchorY

		frame.Position = UDim2.new(
			frame.Position.X.Scale,
			frame.Position.X.Offset,
			0,
			positionY
		)

		KeepFrameOnScreen()

	end)
	

end)


---------------------------------------------------
-- VELOCIDADE
---------------------------------------------------

local speedFrame = Instance.new("Frame")
speedFrame.Parent = scroll
speedFrame.Size = UDim2.new(1,-10,0,90)
speedFrame.BackgroundColor3 = Color3.fromRGB(35,35,35)
Instance.new("UICorner",speedFrame)

local speedTitle = Instance.new("TextLabel")
speedTitle.Parent = speedFrame
speedTitle.BackgroundTransparency = 1
speedTitle.Position = UDim2.new(0,10,0,5)
speedTitle.Size = UDim2.new(0,150,0,25)
speedTitle.Text = "Speed"
speedTitle.TextColor3 = Color3.new(1,1,1)
speedTitle.Font = Enum.Font.GothamBold
speedTitle.TextSize = 18
speedTitle.TextXAlignment = Enum.TextXAlignment.Left

local speedBox = Instance.new("TextBox")
speedBox.Parent = speedFrame
speedBox.Position = UDim2.new(0,10,0,35)
speedBox.Size = UDim2.new(0,120,0,35)
speedBox.PlaceholderText = "16"
speedBox.Text = ""
speedBox.BackgroundColor3 = Color3.fromRGB(55,55,55)
speedBox.TextColor3 = Color3.new(1,1,1)
speedBox.Font = Enum.Font.Gotham
speedBox.TextSize = 18
Instance.new("UICorner",speedBox)

local speedButton = Instance.new("TextButton")
speedButton.Parent = speedFrame
speedButton.Position = UDim2.new(1,-140,0,25)
speedButton.Size = UDim2.new(0,120,0,40)
speedButton.Text = "OFF"
speedButton.BackgroundColor3 = Color3.fromRGB(255,0,0)
speedButton.TextColor3 = Color3.new(1,1,1)
speedButton.Font = Enum.Font.GothamBold
speedButton.TextSize = 20
Instance.new("UICorner",speedButton)

local speedEnabled = false
local oldSpeed = 16

speedButton.MouseButton1Click:Connect(function()

	local character = player.Character or player.CharacterAdded:Wait()
	local humanoid = character:FindFirstChildOfClass("Humanoid")

	if not humanoid then
		return
	end

	speedEnabled = not speedEnabled

	if speedEnabled then

		oldSpeed = humanoid.WalkSpeed
		humanoid.WalkSpeed = tonumber(speedBox.Text) or 16

		speedButton.Text = "ON"
		speedButton.BackgroundColor3 = Color3.fromRGB(0,255,0)

	else

		humanoid.WalkSpeed = oldSpeed

		speedButton.Text = "OFF"
		speedButton.BackgroundColor3 = Color3.fromRGB(255,0,0)

	end

end)

player.CharacterAdded:Connect(function(character)

	task.wait(1)

	if speedEnabled then

		local humanoid = character:WaitForChild("Humanoid")
		humanoid.WalkSpeed = tonumber(speedBox.Text) or 16

	end

end)

---------------------------------------------------
-- SUPER PULO
---------------------------------------------------

local jumpFrame = Instance.new("Frame")
jumpFrame.Parent = scroll
jumpFrame.Size = UDim2.new(1,-10,0,90)
jumpFrame.BackgroundColor3 = Color3.fromRGB(35,35,35)
Instance.new("UICorner",jumpFrame)

local jumpTitle = Instance.new("TextLabel")
jumpTitle.Parent = jumpFrame
jumpTitle.BackgroundTransparency = 1
jumpTitle.Position = UDim2.new(0,10,0,5)
jumpTitle.Size = UDim2.new(0,150,0,25)
jumpTitle.Text = "Jump Power"
jumpTitle.TextColor3 = Color3.new(1,1,1)
jumpTitle.Font = Enum.Font.GothamBold
jumpTitle.TextSize = 18
jumpTitle.TextXAlignment = Enum.TextXAlignment.Left

local jumpBox = Instance.new("TextBox")
jumpBox.Parent = jumpFrame
jumpBox.Position = UDim2.new(0,10,0,35)
jumpBox.Size = UDim2.new(0,120,0,35)
jumpBox.PlaceholderText = "50"
jumpBox.Text = ""
jumpBox.BackgroundColor3 = Color3.fromRGB(55,55,55)
jumpBox.TextColor3 = Color3.new(1,1,1)
jumpBox.Font = Enum.Font.Gotham
jumpBox.TextSize = 18
Instance.new("UICorner",jumpBox)

local jumpButton = Instance.new("TextButton")
jumpButton.Parent = jumpFrame
jumpButton.Position = UDim2.new(1,-140,0,25)
jumpButton.Size = UDim2.new(0,120,0,40)
jumpButton.Text = "OFF"
jumpButton.BackgroundColor3 = Color3.fromRGB(255,0,0)
jumpButton.TextColor3 = Color3.new(1,1,1)
jumpButton.Font = Enum.Font.GothamBold
jumpButton.TextSize = 20
Instance.new("UICorner",jumpButton)

local jumpEnabled = false
local oldJump = 50

jumpButton.MouseButton1Click:Connect(function()

	local character = player.Character or player.CharacterAdded:Wait()
	local humanoid = character:FindFirstChildOfClass("Humanoid")

	if not humanoid then
		return
	end

	jumpEnabled = not jumpEnabled

	if jumpEnabled then

		oldJump = humanoid.JumpPower

		humanoid.UseJumpPower = true
		humanoid.JumpPower = tonumber(jumpBox.Text) or 50

		jumpButton.Text = "ON"
		jumpButton.BackgroundColor3 = Color3.fromRGB(0,255,0)

	else

		humanoid.UseJumpPower = true
		humanoid.JumpPower = oldJump

		jumpButton.Text = "OFF"
		jumpButton.BackgroundColor3 = Color3.fromRGB(255,0,0)

	end

end)

player.CharacterAdded:Connect(function(character)

	task.wait(1)

	if jumpEnabled then

		local humanoid = character:WaitForChild("Humanoid")

		humanoid.UseJumpPower = true
		humanoid.JumpPower = tonumber(jumpBox.Text) or 50

	end

end)

---------------------------------------------------
-- TECLA E
-- SÓ FUNCIONA COM O BOTÃO ON
---------------------------------------------------

UIS.InputBegan:Connect(function(input,gameProcessed)

	if gameProcessed then
		return
	end

	if input.KeyCode ~= Enum.KeyCode.E then
		return
	end

	---------------------------------------------------
	-- BOTÃO ESTÁ OFF?
	-- ENTÃO E NÃO FAZ NADA
	---------------------------------------------------

	if not flyEnabled then
		return
	end

	---------------------------------------------------
	-- ESTÁ VOANDO?
	-- E DESLIGA SOMENTE O VOO
	---------------------------------------------------

	if flying then

		StopFlying()

		---------------------------------------------------
		-- NÃO ESTÁ VOANDO?
		-- E LIGA O VOO NOVAMENTE
		---------------------------------------------------

	else

		StartFlying()

	end

end)

---------------------------------------------------
-- RESPAWN
---------------------------------------------------

player.CharacterAdded:Connect(function(character)

	task.wait(1)

	if flyEnabled then
		StartFlying()
	end

end)

---------------------------------------------------
-- FLY
---------------------------------------------------

local RunService = game:GetService("RunService")

local flyFrame = Instance.new("Frame")
flyFrame.Parent = scroll
flyFrame.Size = UDim2.new(1,-10,0,90)
flyFrame.BackgroundColor3 = Color3.fromRGB(35,35,35)

Instance.new("UICorner",flyFrame)

---------------------------------------------------
-- TÍTULO
---------------------------------------------------

local flyTitle = Instance.new("TextLabel")
flyTitle.Parent = flyFrame
flyTitle.BackgroundTransparency = 1
flyTitle.Position = UDim2.new(0,10,0,5)
flyTitle.Size = UDim2.new(0,150,0,25)

flyTitle.Text = "Fly"
flyTitle.TextColor3 = Color3.new(1,1,1)
flyTitle.Font = Enum.Font.GothamBold
flyTitle.TextSize = 18
flyTitle.TextXAlignment = Enum.TextXAlignment.Left

---------------------------------------------------
-- TEXTBOX
---------------------------------------------------

local flyBox = Instance.new("TextBox")
flyBox.Parent = flyFrame
flyBox.Position = UDim2.new(0,10,0,35)
flyBox.Size = UDim2.new(0,120,0,35)

flyBox.PlaceholderText = "50"
flyBox.Text = ""

flyBox.BackgroundColor3 = Color3.fromRGB(55,55,55)
flyBox.TextColor3 = Color3.new(1,1,1)

flyBox.Font = Enum.Font.Gotham
flyBox.TextSize = 18

Instance.new("UICorner",flyBox)

---------------------------------------------------
-- BOTÃO
---------------------------------------------------

local flyButton = Instance.new("TextButton")
flyButton.Parent = flyFrame

flyButton.Position = UDim2.new(1,-140,0,25)
flyButton.Size = UDim2.new(0,120,0,40)

flyButton.Text = "OFF"

flyButton.BackgroundColor3 =
	Color3.fromRGB(255,0,0)

flyButton.TextColor3 =
	Color3.new(1,1,1)

flyButton.Font =
	Enum.Font.GothamBold

flyButton.TextSize = 20

Instance.new("UICorner",flyButton)

---------------------------------------------------
-- HD ADMIN FLY — PC + MOBILE
---------------------------------------------------

local flyEnabled = false
local flying = false

local flySpeed = 50

local bodyVelocity = nil
local bodyGyro = nil
local flyConnection = nil

---------------------------------------------------
-- STOP FLY
---------------------------------------------------

local function StopFlying()

	flying = false

	if flyConnection then
		flyConnection:Disconnect()
		flyConnection = nil
	end

	if bodyVelocity then
		bodyVelocity:Destroy()
		bodyVelocity = nil
	end

	if bodyGyro then
		bodyGyro:Destroy()
		bodyGyro = nil
	end

	local character = player.Character

	if character then

		local humanoid =
			character:FindFirstChildOfClass("Humanoid")

		local animate =
			character:FindFirstChild("Animate")

		if animate then
			animate.Disabled = false
		end

		if humanoid then
			humanoid:ChangeState(
				Enum.HumanoidStateType.Freefall
			)
		end

	end

end

---------------------------------------------------
-- START FLY
---------------------------------------------------

local function StartFlying()

	if not flyEnabled or flying then
		return
	end

	local character =
		player.Character or player.CharacterAdded:Wait()

	local humanoid =
		character:WaitForChild("Humanoid")

	local hrp =
		character:WaitForChild("HumanoidRootPart")

	---------------------------------------------------
	-- VELOCIDADE
	---------------------------------------------------

	flySpeed = tonumber(flyBox.Text) or 50

	if flySpeed < 1 then
		flySpeed = 50
	end

	flying = true

	---------------------------------------------------
	-- PARA ANIMAÇÕES
	---------------------------------------------------

	local animator =
		humanoid:FindFirstChildOfClass("Animator")

	if animator then

		for _, track in ipairs(
			animator:GetPlayingAnimationTracks()
			) do
			track:Stop(0.1)
		end

	end

	---------------------------------------------------
	-- DESLIGA ANIMATE
	---------------------------------------------------

	local animate =
		character:FindFirstChild("Animate")

	if animate then
		animate.Disabled = true
	end

	---------------------------------------------------
	-- BODY GYRO
	---------------------------------------------------

	bodyGyro = Instance.new("BodyGyro")

	bodyGyro.Name = "HDFlyGyro"
	bodyGyro.MaxTorque = Vector3.new(
		math.huge,
		math.huge,
		math.huge
	)

	bodyGyro.P = 10000
	bodyGyro.D = 500
	bodyGyro.CFrame = workspace.CurrentCamera.CFrame
	bodyGyro.Parent = hrp

	---------------------------------------------------
	-- BODY VELOCITY
	---------------------------------------------------

	bodyVelocity = Instance.new("BodyVelocity")

	bodyVelocity.Name = "HDFlyVelocity"
	bodyVelocity.MaxForce = Vector3.new(
		math.huge,
		math.huge,
		math.huge
	)

	bodyVelocity.P = 10000
	bodyVelocity.Velocity = Vector3.zero
	bodyVelocity.Parent = hrp

	---------------------------------------------------
	-- MOVIMENTO
	---------------------------------------------------

	flyConnection = RunService.RenderStepped:Connect(function()

		if not flyEnabled or not flying then
			return
		end

		if not character.Parent or not hrp.Parent then
			StopFlying()
			return
		end

		if not bodyGyro or not bodyVelocity then
			return
		end

		local camera = workspace.CurrentCamera

		if not camera then
			return
		end

		---------------------------------------------------
		-- ROTAÇÃO DA CÂMERA
		---------------------------------------------------

		bodyGyro.CFrame = camera.CFrame

		---------------------------------------------------
		-- MOVIMENTO RELATIVO À CÂMERA
		---------------------------------------------------

		local velocity = Vector3.zero

		if not UIS:GetFocusedTextBox() then

			-- PC: teclado
			local forwardInput = 0
			local rightInput = 0

			if UIS:IsKeyDown(Enum.KeyCode.W) then
				forwardInput += 1
			end

			if UIS:IsKeyDown(Enum.KeyCode.S) then
				forwardInput -= 1
			end

			if UIS:IsKeyDown(Enum.KeyCode.D) then
				rightInput += 1
			end

			if UIS:IsKeyDown(Enum.KeyCode.A) then
				rightInput -= 1
			end

			-- Mobile e controles: joystick
			local moveDirection = humanoid.MoveDirection

			if moveDirection.Magnitude > 0.05 then

				local cameraForward = Vector3.new(
					camera.CFrame.LookVector.X,
					0,
					camera.CFrame.LookVector.Z
				)

				local cameraRight = Vector3.new(
					camera.CFrame.RightVector.X,
					0,
					camera.CFrame.RightVector.Z
				)

				if cameraForward.Magnitude > 0 then
					cameraForward = cameraForward.Unit
				end

				if cameraRight.Magnitude > 0 then
					cameraRight = cameraRight.Unit
				end

				forwardInput =
					moveDirection:Dot(cameraForward)

				rightInput =
					moveDirection:Dot(cameraRight)

			end

			-- Usa a direção COMPLETA da câmera,
			-- incluindo a inclinação vertical.
			velocity +=
				camera.CFrame.LookVector * forwardInput

			velocity +=
				camera.CFrame.RightVector * rightInput

			---------------------------------------------------
			-- SUBIR E DESCER NO PC
			---------------------------------------------------

			if UIS:IsKeyDown(Enum.KeyCode.Space) then
				velocity += Vector3.new(0, 1, 0)
			end

			if UIS:IsKeyDown(Enum.KeyCode.LeftControl) then
				velocity -= Vector3.new(0, 1, 0)
			end

		end

		---------------------------------------------------
		-- NORMALIZA A VELOCIDADE
		---------------------------------------------------

		if velocity.Magnitude > 1 then
			velocity = velocity.Unit
		end

		bodyVelocity.Velocity = velocity * flySpeed

	end)

end

---------------------------------------------------
-- BOTÃO ON/OFF
---------------------------------------------------

flyButton.MouseButton1Click:Connect(function()

	---------------------------------------------------
	-- DESLIGAR COMPLETAMENTE
	---------------------------------------------------

	if flyEnabled then

		flyEnabled = false

		StopFlying()

		flyButton.Text = "OFF"

		flyButton.BackgroundColor3 =
			Color3.fromRGB(255,0,0)

		return

	end

	---------------------------------------------------
	-- LIGAR
	---------------------------------------------------

	flyEnabled = true

	flyButton.Text = "ON"

	flyButton.BackgroundColor3 =
		Color3.fromRGB(0,255,0)

	StartFlying()

end)

---------------------------------------------------
-- TECLA E
---------------------------------------------------
-- NÃO ALTERA O BOTÃO
---------------------------------------------------

UIS.InputBegan:Connect(function(input,gameProcessed)

	if gameProcessed then
		return
	end

	if input.KeyCode ~= Enum.KeyCode.E then
		return
	end

	---------------------------------------------------
	-- SE O BOTÃO ESTIVER OFF, NÃO FAZ NADA
	---------------------------------------------------

	if not flyEnabled then
		return
	end

	---------------------------------------------------
	-- E DESLIGA/LIGA SOMENTE O VOO
	---------------------------------------------------

	if flying then

		StopFlying()

	else

		StartFlying()

	end

end)

---------------------------------------------------
-- RESPAWN
---------------------------------------------------

player.CharacterAdded:Connect(function(character)

	task.wait(1)

	if flyEnabled then

		StartFlying()

	end

end)

---------------------------------------------------
-- NOCLIP
---------------------------------------------------

local noclipFrame = Instance.new("Frame")
noclipFrame.Parent = scroll
noclipFrame.Size = UDim2.new(1,-10,0,70)
noclipFrame.BackgroundColor3 = Color3.fromRGB(35,35,35)

Instance.new("UICorner",noclipFrame)

local noclipTitle = Instance.new("TextLabel")
noclipTitle.Parent = noclipFrame
noclipTitle.BackgroundTransparency = 1
noclipTitle.Position = UDim2.new(0,10,0,5)
noclipTitle.Size = UDim2.new(0,150,0,25)

noclipTitle.Text = "Noclip"
noclipTitle.TextColor3 = Color3.new(1,1,1)
noclipTitle.Font = Enum.Font.GothamBold
noclipTitle.TextSize = 18
noclipTitle.TextXAlignment = Enum.TextXAlignment.Left

local noclipButton = Instance.new("TextButton")
noclipButton.Parent = noclipFrame
noclipButton.Position = UDim2.new(1,-140,0,15)
noclipButton.Size = UDim2.new(0,120,0,40)

noclipButton.Text = "OFF"
noclipButton.BackgroundColor3 = Color3.fromRGB(255,0,0)
noclipButton.TextColor3 = Color3.new(1,1,1)
noclipButton.Font = Enum.Font.GothamBold
noclipButton.TextSize = 20

Instance.new("UICorner",noclipButton)

local noclip = false
local noclipConnection

noclipButton.MouseButton1Click:Connect(function()

	noclip = not noclip

	---------------------------------------------------
	-- NOCLIP ON
	---------------------------------------------------

	if noclip then

		noclipButton.Text = "ON"
		noclipButton.BackgroundColor3 = Color3.fromRGB(0,255,0)

		noclipConnection = game:GetService("RunService").Stepped:Connect(function()

			local character = player.Character

			if character then

				for _,v in ipairs(character:GetDescendants()) do

					if v:IsA("BasePart") then
						v.CanCollide = false
					end

				end

			end

		end)

		---------------------------------------------------
		-- NOCLIP OFF
		---------------------------------------------------

	else

		noclipButton.Text = "OFF"
		noclipButton.BackgroundColor3 = Color3.fromRGB(255,0,0)

		if noclipConnection then
			noclipConnection:Disconnect()
			noclipConnection = nil
		end

		-- Reativa a colisão do personagem
		local character = player.Character

		if character then

			for _,v in ipairs(character:GetDescendants()) do

				if v:IsA("BasePart") then
					v.CanCollide = true
				end

			end

			-- Faz o personagem voltar para um estado normal
			local humanoid = character:FindFirstChildOfClass("Humanoid")

			if humanoid then
				humanoid:ChangeState(Enum.HumanoidStateType.GettingUp)
			end

		end

	end

end)

---------------------------------------------------
-- ESP
---------------------------------------------------

local espFrame = Instance.new("Frame")
espFrame.Parent = scroll
espFrame.Size = UDim2.new(1,-10,0,70)
espFrame.BackgroundColor3 = Color3.fromRGB(35,35,35)
Instance.new("UICorner", espFrame)

local espTitle = Instance.new("TextLabel")
espTitle.Parent = espFrame
espTitle.BackgroundTransparency = 1
espTitle.Position = UDim2.new(0,10,0,5)
espTitle.Size = UDim2.new(0,200,0,25)
espTitle.Text = "ESP"
espTitle.Font = Enum.Font.GothamBold
espTitle.TextSize = 18
espTitle.TextColor3 = Color3.new(1,1,1)
espTitle.TextXAlignment = Enum.TextXAlignment.Left

local espButton = Instance.new("TextButton")
espButton.Parent = espFrame
espButton.Position = UDim2.new(1,-140,0,15)
espButton.Size = UDim2.new(0,120,0,40)
espButton.Text = "OFF"
espButton.Font = Enum.Font.GothamBold
espButton.TextSize = 20
espButton.TextColor3 = Color3.new(1,1,1)
espButton.BackgroundColor3 = Color3.fromRGB(255,0,0)
Instance.new("UICorner", espButton)

local espEnabled = false

local nameEspEnabled = false
local nameLabels = {}

local function RemoveESP()

	for _, plr in ipairs(Players:GetPlayers()) do

		if plr.Character then

			local highlight = plr.Character:FindFirstChild("HDESP")
			if highlight then
				highlight:Destroy()
			end

			local beam = plr.Character:FindFirstChild("ESPBeam")
			if beam then
				beam:Destroy()
			end

			local root = plr.Character:FindFirstChild("HumanoidRootPart")
			if root then
				local att = root:FindFirstChild("ESPAttachment1")
				if att then
					att:Destroy()
				end
			end

			local nameESP = plr.Character:FindFirstChild("HDNameESP")
			if nameESP then
				nameESP:Destroy()
			end

		end

	end

	-- Remove o attachment do seu personagem
	local myCharacter = player.Character

	if myCharacter then
		local myRoot = myCharacter:FindFirstChild("HumanoidRootPart")

		if myRoot then
			local att = myRoot:FindFirstChild("ESPAttachment0")
			if att then
				att:Destroy()
			end
		end
	end

end

local function AddESP()

	local myCharacter = player.Character
	if not myCharacter then return end

	local myRoot = myCharacter:FindFirstChild("HumanoidRootPart")
	if not myRoot then return end

	-- Remove attachment antigo do jogador
	local oldAttachment = myRoot:FindFirstChild("ESPAttachment0")
	if oldAttachment then
		oldAttachment:Destroy()
	end

	local myAttachment = Instance.new("Attachment")
	myAttachment.Name = "ESPAttachment0"
	myAttachment.Parent = myRoot

	for _, targetPlayer in pairs(game.Players:GetPlayers()) do

		if targetPlayer ~= player then

			local character = targetPlayer.Character

			if character then

				local targetRoot = character:FindFirstChild("HumanoidRootPart")

				if targetRoot then

					-- Remove ESP antigo
					local oldHighlight = character:FindFirstChild("HDESP")
					if oldHighlight then
						oldHighlight:Destroy()
					end

					local oldBeam = character:FindFirstChild("ESPBeam")
					if oldBeam then
						oldBeam:Destroy()
					end

					local oldAttachment1 = targetRoot:FindFirstChild("ESPAttachment1")
					if oldAttachment1 then
						oldAttachment1:Destroy()
					end

					-------------------------------------------------
					-- COR DO TIME
					-------------------------------------------------

					local espColor = Color3.fromRGB(255,0,0)

					if targetPlayer.Team and not targetPlayer.Neutral then
						espColor = targetPlayer.Team.TeamColor.Color
					end

					-------------------------------------------------
					-- HIGHLIGHT
					-------------------------------------------------

					local highlight = Instance.new("Highlight")
					highlight.Name = "HDESP"
					highlight.FillColor = espColor
					highlight.OutlineColor = Color3.new(1,1,1)
					highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
					highlight.FillTransparency = 0.45
					highlight.Parent = character

					-------------------------------------------------
					-- LINHA
					-------------------------------------------------

					local targetAttachment = Instance.new("Attachment")
					targetAttachment.Name = "ESPAttachment1"
					targetAttachment.Parent = targetRoot

					local beam = Instance.new("Beam")
					beam.Name = "ESPBeam"

					beam.Attachment0 = myAttachment
					beam.Attachment1 = targetAttachment

					beam.FaceCamera = true

					beam.Width0 = 0.12
					beam.Width1 = 0.12

					beam.Color = ColorSequence.new(espColor)

					beam.LightEmission = 1

					beam.Parent = character

				end

			end

		end

	end

end

espButton.MouseButton1Click:Connect(function()

	espEnabled = not espEnabled

	if espEnabled then

		espButton.Text = "ON"
		espButton.BackgroundColor3 = Color3.fromRGB(0,255,0)

		AddESP()

	else

		espButton.Text = "OFF"
		espButton.BackgroundColor3 = Color3.fromRGB(255,0,0)

		RemoveESP()

	end

end)

local function UpdateESP()

	if not espEnabled then
		RemoveESP() -- Garante que remove tudo
		return
	end

	RemoveESP()
	task.wait()
	AddESP()
end

-- Quando alguém entra
Players.PlayerAdded:Connect(function(plr)

	plr.CharacterAdded:Connect(function()
		task.wait(0.5)
		UpdateESP()
	end)

	-- Atualiza quando trocar de time
	plr:GetPropertyChangedSignal("Team"):Connect(function()
		UpdateESP()
	end)

	-- Atualiza quando virar neutro/deixar de ser neutro
	plr:GetPropertyChangedSignal("Neutral"):Connect(function()
		UpdateESP()
	end)

	task.wait(0.5)
	UpdateESP()

end)

-- Para quem já estava no servidor
for _, plr in ipairs(Players:GetPlayers()) do

	if plr ~= player then

		plr.CharacterAdded:Connect(function()
			task.wait(0.5)
			UpdateESP()
		end)

		plr:GetPropertyChangedSignal("Team"):Connect(function()
			UpdateESP()
		end)

		plr:GetPropertyChangedSignal("Neutral"):Connect(function()
			UpdateESP()
		end)

	end

end

-- Quando VOCÊ respawnar
player.CharacterAdded:Connect(function()
	task.wait(0.5)
	UpdateESP()
end)

---------------------------------------------------
-- VER VIDA
---------------------------------------------------

local healthFrame = Instance.new("Frame")
healthFrame.Parent = scroll
healthFrame.Size = UDim2.new(1,-10,0,70)
healthFrame.BackgroundColor3 = Color3.fromRGB(35,35,35)
Instance.new("UICorner", healthFrame)

local healthTitle = Instance.new("TextLabel")
healthTitle.Parent = healthFrame
healthTitle.BackgroundTransparency = 1
healthTitle.Position = UDim2.new(0,10,0,5)
healthTitle.Size = UDim2.new(0,200,0,25)
healthTitle.Text = "ESP Health"
healthTitle.Font = Enum.Font.GothamBold
healthTitle.TextSize = 18
healthTitle.TextColor3 = Color3.new(1,1,1)
healthTitle.TextXAlignment = Enum.TextXAlignment.Left

local healthButton = Instance.new("TextButton")
healthButton.Parent = healthFrame
healthButton.Position = UDim2.new(1,-140,0,15)
healthButton.Size = UDim2.new(0,120,0,40)
healthButton.Text = "OFF"
healthButton.Font = Enum.Font.GothamBold
healthButton.TextSize = 20
healthButton.TextColor3 = Color3.new(1,1,1)
healthButton.BackgroundColor3 = Color3.fromRGB(255,0,0)
Instance.new("UICorner", healthButton)

local healthEnabled = false
local healthLabels = {}

local function RemoveHealthESP()

	for _,v in ipairs(healthLabels) do

		if v and v.Parent then
			v:Destroy()
		end

	end

	table.clear(healthLabels)

end

local function CreateHealthESP()

	RemoveHealthESP()

	for _,plr in ipairs(Players:GetPlayers()) do

		if plr ~= player and plr.Character then

			local head = plr.Character:FindFirstChild("Head")
			local hum = plr.Character:FindFirstChildOfClass("Humanoid")

			if head and hum then

				local nameEspEnabled = false
				local nameLabels = {}

				local bill = Instance.new("BillboardGui")
				bill.Name = "HealthESP"
				bill.Size = UDim2.new(0,120,0,30)
				bill.StudsOffset = Vector3.new(0,4,0)
				bill.AlwaysOnTop = true
				bill.Parent = head

				local txt = Instance.new("TextLabel")
				txt.Parent = bill
				txt.Size = UDim2.new(1,0,1,0)
				txt.BackgroundTransparency = 1
				txt.Font = Enum.Font.GothamBold
				txt.TextScaled = true

				task.spawn(function()

					while bill.Parent and healthEnabled do

						local hp = hum.Health
						local max = hum.MaxHealth

						txt.Text = "HP: "..math.floor(hp).."/"..math.floor(max)

						local p = hp/max

						if p > .75 then
							txt.TextColor3 = Color3.fromRGB(0,255,0)
						elseif p > .25 then
							txt.TextColor3 = Color3.fromRGB(255,255,0)
						else
							txt.TextColor3 = Color3.fromRGB(255,0,0)
						end

						task.wait(.1)

					end

				end)

				table.insert(healthLabels,bill)

			end

		end

	end

end

healthButton.MouseButton1Click:Connect(function()

	healthEnabled = not healthEnabled

	if healthEnabled then

		healthButton.Text = "ON"
		healthButton.BackgroundColor3 = Color3.fromRGB(0,255,0)

		CreateHealthESP()

	else

		healthButton.Text = "OFF"
		healthButton.BackgroundColor3 = Color3.fromRGB(255,0,0)

		RemoveHealthESP()

	end

end)

Players.PlayerAdded:Connect(function(plr)

	plr.CharacterAdded:Connect(function()

		task.wait(1)

		if espEnabled then
			AddESP()
		end

		if healthEnabled then
			CreateHealthESP()
		end

	end)

end)

player.CharacterAdded:Connect(function()

	task.wait(1)

	if espEnabled then
		AddESP()
	end

	if healthEnabled then
		CreateHealthESP()
	end

end)

---------------------------------------------------
-- MOSTRAR ITENS
---------------------------------------------------

local itemsFrame = Instance.new("Frame")
itemsFrame.Parent = scroll
itemsFrame.Size = UDim2.new(1,-10,0,70)
itemsFrame.BackgroundColor3 = Color3.fromRGB(35,35,35)
Instance.new("UICorner", itemsFrame)

local itemsTitle = Instance.new("TextLabel")
itemsTitle.Parent = itemsFrame
itemsTitle.BackgroundTransparency = 1
itemsTitle.Position = UDim2.new(0,10,0,5)
itemsTitle.Size = UDim2.new(0,200,0,25)
itemsTitle.Text = "ESP Itens"
itemsTitle.Font = Enum.Font.GothamBold
itemsTitle.TextSize = 18
itemsTitle.TextColor3 = Color3.new(1,1,1)
itemsTitle.TextXAlignment = Enum.TextXAlignment.Left

local itemsButton = Instance.new("TextButton")
itemsButton.Parent = itemsFrame
itemsButton.Position = UDim2.new(1,-140,0,15)
itemsButton.Size = UDim2.new(0,120,0,40)
itemsButton.Text = "OFF"
itemsButton.Font = Enum.Font.GothamBold
itemsButton.TextSize = 20
itemsButton.TextColor3 = Color3.new(1,1,1)
itemsButton.BackgroundColor3 = Color3.fromRGB(255,0,0)
Instance.new("UICorner",itemsButton)

local itemsEnabled = false
local itemLabels = {}

---------------------------------------------------
-- REMOVE
---------------------------------------------------

local function RemoveItemESP()

	for _,gui in ipairs(itemLabels) do
		if gui and gui.Parent then
			gui:Destroy()
		end
	end

	table.clear(itemLabels)

end

---------------------------------------------------
-- CRIA
---------------------------------------------------

local function CreateItemESP()

	RemoveItemESP()

	for _,plr in ipairs(Players:GetPlayers()) do

		if plr ~= player and plr.Character then

			local head = plr.Character:FindFirstChild("Head")

			if head then

				local bill = Instance.new("BillboardGui")
				bill.Name = "HDItemESP"
				bill.Size = UDim2.new(0,170,0,90)
				bill.StudsOffset = Vector3.new(0,5.8,0)
				bill.AlwaysOnTop = true
				bill.Parent = head

				local txt = Instance.new("TextLabel")
				txt.Parent = bill
				txt.Size = UDim2.new(1,0,1,0)
				txt.BackgroundTransparency = 1
				txt.TextWrapped = true
				txt.Font = Enum.Font.GothamBold
				txt.TextScaled = true
				txt.TextColor3 = Color3.new(1,1,1)

				task.spawn(function()

					while bill.Parent and itemsEnabled do

						local text = ""

						local backpack = plr:FindFirstChild("Backpack")

						if backpack then

							for _,tool in ipairs(backpack:GetChildren()) do

								if tool:IsA("Tool") then

									if text == "" then
										text = tool.Name
									else
										text = text.."\n"..tool.Name
									end

								end

							end

						end

						local character = plr.Character

						if character then

							for _,tool in ipairs(character:GetChildren()) do

								if tool:IsA("Tool") then

									if text == "" then
										text = "[Equipado]\n"..tool.Name
									else
										text = text.."\n[Equipado] "..tool.Name
									end

								end

							end

						end

						if text == "" then
							text = "Nenhum item"
						end

						txt.Text = text

						task.wait(.3)

					end

				end)

				table.insert(itemLabels,bill)

			end

		end

	end

end

---------------------------------------------------
-- BOTÃO
---------------------------------------------------

itemsButton.MouseButton1Click:Connect(function()

	itemsEnabled = not itemsEnabled

	if itemsEnabled then

		itemsButton.Text = "ON"
		itemsButton.BackgroundColor3 = Color3.fromRGB(0,255,0)

		CreateItemESP()

	else

		itemsButton.Text = "OFF"
		itemsButton.BackgroundColor3 = Color3.fromRGB(255,0,0)

		RemoveItemESP()

	end

end)

---------------------------------------------------
-- ATUALIZA
---------------------------------------------------

local function UpdateItemESP()

	if not itemsEnabled then
		return
	end

	CreateItemESP()

end

Players.PlayerAdded:Connect(function(plr)

	plr.CharacterAdded:Connect(function()

		task.wait(1)

		UpdateItemESP()

	end)

	plr.ChildAdded:Connect(function()

		task.wait(.2)

		UpdateItemESP()

	end)

end)

for _,plr in ipairs(Players:GetPlayers()) do

	if plr ~= player then

		plr.CharacterAdded:Connect(function()

			task.wait(1)

			UpdateItemESP()

		end)

		local backpack = plr:FindFirstChild("Backpack")

		if backpack then

			backpack.ChildAdded:Connect(UpdateItemESP)
			backpack.ChildRemoved:Connect(UpdateItemESP)

		end

	end

end

player.CharacterAdded:Connect(function()

	task.wait(1)

	UpdateItemESP()

end)

---------------------------------------------------
-- TP TOOL
---------------------------------------------------

local tpToolFrame = Instance.new("Frame")
tpToolFrame.Parent = scroll
tpToolFrame.Size = UDim2.new(1,-10,0,70)
tpToolFrame.BackgroundColor3 = Color3.fromRGB(35,35,35)
Instance.new("UICorner", tpToolFrame)

local tpToolTitle = Instance.new("TextLabel")
tpToolTitle.Parent = tpToolFrame
tpToolTitle.BackgroundTransparency = 1
tpToolTitle.Position = UDim2.new(0,10,0,5)
tpToolTitle.Size = UDim2.new(0,200,0,25)
tpToolTitle.Text = "TP Tool"
tpToolTitle.Font = Enum.Font.GothamBold
tpToolTitle.TextSize = 18
tpToolTitle.TextColor3 = Color3.new(1,1,1)
tpToolTitle.TextXAlignment = Enum.TextXAlignment.Left

local tpToolButton = Instance.new("TextButton")
tpToolButton.Parent = tpToolFrame
tpToolButton.Position = UDim2.new(1,-140,0,15)
tpToolButton.Size = UDim2.new(0,120,0,40)
tpToolButton.Text = "PEGAR"
tpToolButton.Font = Enum.Font.GothamBold
tpToolButton.TextSize = 20
tpToolButton.TextColor3 = Color3.new(1,1,1)
tpToolButton.BackgroundColor3 = Color3.fromRGB(0,170,255)
Instance.new("UICorner", tpToolButton)

---------------------------------------------------
-- TP TOOL — PEGAR PELO PAINEL E RECUPERAR AO MORRER
---------------------------------------------------

local tpTool = nil
local tpToolRequested = false

---------------------------------------------------
-- CRIAR A TP TOOL
---------------------------------------------------

local function CreateTPTool()

	local backpack = player:WaitForChild("Backpack")

	-- Se a ferramenta já estiver no inventário, reutiliza.
	local existingTool = backpack:FindFirstChild("HD TP Tool")

	if existingTool then
		tpTool = existingTool
		return
	end

	-- Se estiver equipada, não cria outra cópia.
	local character = player.Character

	if character then

		local equippedTool = character:FindFirstChild("HD TP Tool")

		if equippedTool then
			tpTool = equippedTool
			return
		end

	end

	---------------------------------------------------
	-- CRIAR FERRAMENTA
	---------------------------------------------------

	local newTool = Instance.new("Tool")

	newTool.Name = "HD TP Tool"
	newTool.RequiresHandle = false
	newTool.CanBeDropped = false

	---------------------------------------------------
	-- TELETRANSPORTE
	---------------------------------------------------

	newTool.Activated:Connect(function()

		local mouse = player:GetMouse()

		local currentCharacter = player.Character

		local hrp = currentCharacter
			and currentCharacter:FindFirstChild("HumanoidRootPart")

		if hrp then

			hrp.CFrame = CFrame.new(
				mouse.Hit.Position + Vector3.new(0, 3, 0)
			)

		end

	end)

	---------------------------------------------------
	-- ENTREGAR AO JOGADOR
	---------------------------------------------------

	tpTool = newTool
	newTool.Parent = backpack

end

---------------------------------------------------
-- BOTÃO PEGAR
---------------------------------------------------

tpToolButton.MouseButton1Click:Connect(function()

	-- Registra que o jogador solicitou a ferramenta.
	tpToolRequested = true

	CreateTPTool()

	tpToolButton.Text = "PEGO!"
	tpToolButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)

	task.delay(1, function()

		if tpToolButton.Parent then

			tpToolButton.Text = "PEGAR"
			tpToolButton.BackgroundColor3 = Color3.fromRGB(0, 170, 255)

		end

	end)

end)

---------------------------------------------------
-- RECUPERAR APÓS RENASCER
---------------------------------------------------

player.CharacterAdded:Connect(function()

	task.wait(0.5)

	-- Só recupera automaticamente se já tiver solicitado
	-- a TP Tool pelo botão PEGAR anteriormente.
	if tpToolRequested then
		CreateTPTool()
	end

end)

---------------------------------------------------
-- RESPAWN
---------------------------------------------------

player.CharacterAdded:Connect(function()

	task.wait(1)

	if tpTool and not tpTool.Parent then
		tpTool.Parent = player.Backpack
	end

end)

---------------------------------------------------
-- TELEPORTAR PARA PLAYER
---------------------------------------------------

local tpFrame = Instance.new("Frame")
tpFrame.Parent = scroll
tpFrame.Size = UDim2.new(1,-10,0,95)
tpFrame.BackgroundColor3 = Color3.fromRGB(35,35,35)
Instance.new("UICorner",tpFrame)

local tpTitle = Instance.new("TextLabel")
tpTitle.Parent = tpFrame
tpTitle.BackgroundTransparency = 1
tpTitle.Position = UDim2.new(0,10,0,5)
tpTitle.Size = UDim2.new(0,220,0,25)
tpTitle.Text = "TP to player"
tpTitle.Font = Enum.Font.GothamBold
tpTitle.TextSize = 18
tpTitle.TextColor3 = Color3.new(1,1,1)
tpTitle.TextXAlignment = Enum.TextXAlignment.Left

local tpBox = Instance.new("TextBox")
tpBox.Parent = tpFrame
tpBox.Position = UDim2.new(0,10,0,40)
tpBox.Size = UDim2.new(0,180,0,40)
tpBox.PlaceholderText = "Nome ou DisplayName"
tpBox.Text = ""
tpBox.ClearTextOnFocus = false
tpBox.Font = Enum.Font.Gotham
tpBox.TextSize = 18
tpBox.TextColor3 = Color3.new(1,1,1)
tpBox.BackgroundColor3 = Color3.fromRGB(55,55,55)
Instance.new("UICorner",tpBox)

local tpButton = Instance.new("TextButton")
tpButton.Parent = tpFrame
tpButton.Position = UDim2.new(1,-140,0,40)
tpButton.Size = UDim2.new(0,120,0,40)
tpButton.Text = "TP"
tpButton.Font = Enum.Font.GothamBold
tpButton.TextSize = 20
tpButton.TextColor3 = Color3.new(1,1,1)
tpButton.BackgroundColor3 = Color3.fromRGB(0,170,255)
Instance.new("UICorner",tpButton)

---------------------------------------------------
-- TELEPORT
---------------------------------------------------

local function FindPlayer(text)

	text = string.lower(text)

	for _,plr in ipairs(Players:GetPlayers()) do

		if string.lower(plr.Name) == text then
			return plr
		end

	end

	for _,plr in ipairs(Players:GetPlayers()) do

		if string.lower(plr.DisplayName) == text then
			return plr
		end

	end

	for _,plr in ipairs(Players:GetPlayers()) do

		if string.sub(string.lower(plr.Name),1,#text) == text then
			return plr
		end

		if string.sub(string.lower(plr.DisplayName),1,#text) == text then
			return plr
		end

	end

	return nil

end

tpButton.MouseButton1Click:Connect(function()

	if tpBox.Text == "" then
		return
	end

	local target = FindPlayer(tpBox.Text)

	if not target then

		tpButton.Text = "NÃO ACHOU"
		tpButton.BackgroundColor3 = Color3.fromRGB(255,0,0)

		task.wait(1)

		if tpButton.Parent then
			tpButton.Text = "TP"
			tpButton.BackgroundColor3 = Color3.fromRGB(0,170,255)
		end

		return

	end

	local myCharacter = player.Character
	local targetCharacter = target.Character

	if not myCharacter or not targetCharacter then
		return
	end

	local myRoot = myCharacter:FindFirstChild("HumanoidRootPart")
	local targetRoot = targetCharacter:FindFirstChild("HumanoidRootPart")

	if not myRoot or not targetRoot then
		return
	end

	myRoot.CFrame = targetRoot.CFrame + Vector3.new(0,3,0)

	tpButton.Text = "OK!"
	tpButton.BackgroundColor3 = Color3.fromRGB(0,255,0)

	task.wait(1)

	if tpButton.Parent then
		tpButton.Text = "TP"
		tpButton.BackgroundColor3 = Color3.fromRGB(0,170,255)
	end

end)

---------------------------------------------------
-- AIMBOT
---------------------------------------------------

local aimbotFrame = Instance.new("Frame")
aimbotFrame.Parent = scroll
aimbotFrame.Size = UDim2.new(1,-10,0,70)
aimbotFrame.BackgroundColor3 = Color3.fromRGB(35,35,35)

Instance.new("UICorner",aimbotFrame)

---------------------------------------------------
-- TÍTULO
---------------------------------------------------

local aimbotTitle = Instance.new("TextLabel")
aimbotTitle.Parent = aimbotFrame
aimbotTitle.BackgroundTransparency = 1
aimbotTitle.Position = UDim2.new(0,10,0,5)
aimbotTitle.Size = UDim2.new(0,200,0,25)

aimbotTitle.Text = "Aimbot"
aimbotTitle.TextColor3 = Color3.new(1,1,1)
aimbotTitle.Font = Enum.Font.GothamBold
aimbotTitle.TextSize = 18
aimbotTitle.TextXAlignment = Enum.TextXAlignment.Left

---------------------------------------------------
-- BOTÃO
---------------------------------------------------

local aimbotButton = Instance.new("TextButton")
aimbotButton.Parent = aimbotFrame
aimbotButton.Position = UDim2.new(1,-140,0,15)
aimbotButton.Size = UDim2.new(0,120,0,40)

aimbotButton.Text = "OFF"
aimbotButton.BackgroundColor3 = Color3.fromRGB(255,0,0)
aimbotButton.TextColor3 = Color3.new(1,1,1)
aimbotButton.Font = Enum.Font.GothamBold
aimbotButton.TextSize = 20

Instance.new("UICorner",aimbotButton)

---------------------------------------------------
-- VARIÁVEIS
---------------------------------------------------

local aimbotEnabled = false
local aimbotActive = false
local aimbotTarget = nil

-- Número usado para cancelar loops antigos
local aimbotLoopId = 0

---------------------------------------------------
-- PROCURAR ALVO
---------------------------------------------------

local function GetAimbotTarget()

	local camera = workspace.CurrentCamera

	if not camera then
		return nil
	end

	local viewport = camera.ViewportSize

	local screenCenter = Vector2.new(
		viewport.X / 2,
		viewport.Y / 2
	)

	local closestPlayer = nil
	local closestScreenDistance = math.huge

	for _, targetPlayer in ipairs(Players:GetPlayers()) do

		if targetPlayer ~= player then

			local character = targetPlayer.Character

			if character then

				local humanoid =
					character:FindFirstChildOfClass("Humanoid")

				local head =
					character:FindFirstChild("Head")

				if humanoid
					and head
					and humanoid.Health > 0 then

					---------------------------------------------------
					-- CONVERTE A POSIÇÃO DO PLAYER PARA A TELA
					---------------------------------------------------

					local screenPosition, visible =
						camera:WorldToViewportPoint(head.Position)

					---------------------------------------------------
					-- SÓ ACEITA SE ESTIVER APARECENDO NA CÂMERA
					---------------------------------------------------

					if visible and screenPosition.Z > 0 then

						local screenPoint = Vector2.new(
							screenPosition.X,
							screenPosition.Y
						)

						---------------------------------------------------
						-- DISTÂNCIA ATÉ O CENTRO DA CÂMERA
						---------------------------------------------------

						local distanceFromCenter =
							(screenPoint - screenCenter).Magnitude

						---------------------------------------------------
						-- PEGA O MAIS PRÓXIMO DO CENTRO
						---------------------------------------------------

						if distanceFromCenter < closestScreenDistance then

							closestScreenDistance =
								distanceFromCenter

							closestPlayer =
								targetPlayer

						end

					end

				end

			end

		end

	end

	return closestPlayer

end

---------------------------------------------------
-- START AIMBOT
---------------------------------------------------

local function StartAimbot()

	if not aimbotEnabled then
		return
	end

	if aimbotActive then
		return
	end

	-- Escolhe o alvo UMA VEZ
	aimbotTarget = GetAimbotTarget()

	if not aimbotTarget then
		return
	end

	aimbotActive = true
	aimbotLoopId += 1

	local thisLoopId = aimbotLoopId

	task.spawn(function()

		while aimbotEnabled
			and aimbotActive
			and thisLoopId == aimbotLoopId do

			local camera = workspace.CurrentCamera

			if camera then

				local character =
					aimbotTarget.Character

				if not character then
					break
				end

				local humanoid =
					character:FindFirstChildOfClass("Humanoid")

				local head =
					character:FindFirstChild("Head")

				-- Alvo morreu
				if not humanoid
					or humanoid.Health <= 0
					or not head then

					break

				end

				-- Olha SOMENTE para o alvo escolhido
				local cameraPosition =
					camera.CFrame.Position

				camera.CFrame =
					CFrame.lookAt(
						cameraPosition,
						head.Position
					)

			end

			task.wait()

		end

		-- Se o alvo morreu/saiu,
		-- para somente o funcionamento
		aimbotActive = false

	end)

end

---------------------------------------------------
-- STOP AIMBOT
-- SÓ PARA O AIMBOT
-- O BOTÃO CONTINUA ON
---------------------------------------------------

local function StopAimbot()

	aimbotActive = false
	aimbotTarget = nil

	-- Invalida qualquer loop antigo
	aimbotLoopId += 1

end

---------------------------------------------------
-- BOTÃO ON/OFF
---------------------------------------------------

aimbotButton.MouseButton1Click:Connect(function()

	aimbotEnabled = not aimbotEnabled

	if aimbotEnabled then

		aimbotButton.Text = "ON"
		aimbotButton.BackgroundColor3 =
			Color3.fromRGB(0,255,0)

		StartAimbot()

	else

		aimbotButton.Text = "OFF"
		aimbotButton.BackgroundColor3 =
			Color3.fromRGB(255,0,0)

		StopAimbot()

	end

end)

---------------------------------------------------
-- TECLA R
---------------------------------------------------

UIS.InputBegan:Connect(function(input,gameProcessed)

	if gameProcessed then
		return
	end

	if input.KeyCode ~= Enum.KeyCode.R then
		return
	end

	-- Se o botão estiver OFF, R não faz nada
	if not aimbotEnabled then
		return
	end

	-- Se estiver ativo, R desliga
	if aimbotActive then

		StopAimbot()

		-- Se estiver parado, R liga
	else

		StartAimbot()

	end

end)

---------------------------------------------------
-- RESPAWN DO AIMBOT
---------------------------------------------------

player.CharacterAdded:Connect(function()

	task.wait(1)

	-- Só volta automaticamente se o Aimbot
	-- estava realmente ativo antes do respawn.
	if aimbotEnabled and aimbotActive then

		aimbotActive = false
		aimbotTarget = nil

		StartAimbot()

	end

end)

---------------------------------------------------
-- PLAYER SAIU
---------------------------------------------------

Players.PlayerRemoving:Connect(function(targetPlayer)

	if targetPlayer == aimbotTarget then
		aimbotTarget = nil
	end

end)

---------------------------------------------------
-- PARTE 5.4
-- OTIMIZAÇÕES
---------------------------------------------------

---------------------------------------------------
-- UPDATE GERAL
---------------------------------------------------

local function UpdateEverything()

	if espEnabled then
		UpdateESP()
	end

	if healthEnabled then
		CreateHealthESP()
	end

	if itemsEnabled then
		CreateItemESP()
	end

end

---------------------------------------------------
-- PLAYER ENTROU
---------------------------------------------------

Players.PlayerAdded:Connect(function(plr)

	plr.CharacterAdded:Connect(function()

		task.wait(0.5)
		UpdateEverything()

		local backpack = plr:WaitForChild("Backpack")

		backpack.ChildAdded:Connect(function()
			task.wait(.1)
			UpdateEverything()
		end)

		backpack.ChildRemoved:Connect(function()
			task.wait(.1)
			UpdateEverything()
		end)

	end)

end)

---------------------------------------------------
-- PLAYERS QUE JÁ ESTAVAM
---------------------------------------------------

for _,plr in ipairs(Players:GetPlayers()) do

	if plr ~= player then

		plr.CharacterAdded:Connect(function()

			task.wait(.5)
			UpdateEverything()

		end)

		local backpack = plr:FindFirstChild("Backpack")

		if backpack then

			backpack.ChildAdded:Connect(function()

				task.wait(.1)
				UpdateEverything()

			end)

			backpack.ChildRemoved:Connect(function()

				task.wait(.1)
				UpdateEverything()

			end)

		end

	end

end

---------------------------------------------------
-- PLAYER LOCAL RESPAWN
---------------------------------------------------

player.CharacterAdded:Connect(function()

	task.wait(1)

	if speedEnabled then

		local hum = player.Character:WaitForChild("Humanoid")
		hum.WalkSpeed = tonumber(speedBox.Text) or 16

	end

	if jumpEnabled then

		local hum = player.Character:WaitForChild("Humanoid")
		hum.UseJumpPower = true
		hum.JumpPower = tonumber(jumpBox.Text) or 50

	end

	if tpTool and not tpTool.Parent then
		tpTool.Parent = player.Backpack
	end

	UpdateEverything()

end)

---------------------------------------------------
-- EVITA TP PARA VOCÊ MESMO
---------------------------------------------------

local oldFindPlayer = FindPlayer

FindPlayer = function(text)

	local plr = oldFindPlayer(text)

	if plr == player then
		return nil
	end

	return plr

end

---------------------------------------------------
-- REFRESH AUTOMÁTICO A CADA 5 SEGUNDOS
---------------------------------------------------

task.spawn(function()

	while task.wait(5) do

		UpdateEverything()

	end

end)

---------------------------------------------------
-- LIMPA ESP AO SAIR
---------------------------------------------------

Players.PlayerRemoving:Connect(function()

	task.wait(.1)

	UpdateEverything()

end)

---------------------------------------------------
-- REMOVE ITENS FANTASMAS
---------------------------------------------------

task.spawn(function()

	while task.wait(2) do

		if itemsEnabled then

			for i = #itemLabels,1,-1 do

				if not itemLabels[i] or not itemLabels[i].Parent then
					table.remove(itemLabels,i)
				end

			end

		end

	end

end)

---------------------------------------------------
-- REMOVE HP FANTASMA
---------------------------------------------------

task.spawn(function()

	while task.wait(2) do

		if healthEnabled then

			for i = #healthLabels,1,-1 do

				if not healthLabels[i] or not healthLabels[i].Parent then
					table.remove(healthLabels,i)
				end

			end

		end

	end

end)

---------------------------------------------------
-- MENSAGEM QUE NAO TEM KKKKK
---------------------------------------------------

---------------------------------------------------
-- PARTE 5.5
-- SERVIDORES
---------------------------------------------------

local TeleportService = game:GetService("TeleportService")

---------------------------------------------------
-- FRAME SERVER
---------------------------------------------------

local serverFrame = Instance.new("Frame")
serverFrame.Parent = scroll
serverFrame.Size = UDim2.new(1,-10,0,100)
serverFrame.BackgroundColor3 = Color3.fromRGB(35,35,35)

Instance.new("UICorner",serverFrame)


local serverTitle = Instance.new("TextLabel")
serverTitle.Parent = serverFrame
serverTitle.BackgroundTransparency = 1
serverTitle.Position = UDim2.new(0,10,0,5)
serverTitle.Size = UDim2.new(1,-20,0,25)

serverTitle.Text = "Servers"
serverTitle.TextColor3 = Color3.new(1,1,1)
serverTitle.Font = Enum.Font.GothamBold
serverTitle.TextSize = 18
serverTitle.TextXAlignment = Enum.TextXAlignment.Left


---------------------------------------------------
-- REENTRAR NO MESMO SERVER
---------------------------------------------------

local rejoinButton = Instance.new("TextButton")

rejoinButton.Parent = serverFrame
rejoinButton.Position = UDim2.new(0,10,0,45)
rejoinButton.Size = UDim2.new(0,180,0,40)

rejoinButton.Text = "Rejoin"
rejoinButton.Font = Enum.Font.GothamBold
rejoinButton.TextSize = 18
rejoinButton.TextColor3 = Color3.new(1,1,1)

rejoinButton.BackgroundColor3 = Color3.fromRGB(0,170,255)

Instance.new("UICorner",rejoinButton)


rejoinButton.MouseButton1Click:Connect(function()

	rejoinButton.Text = "Entrando..."

	task.wait(.5)

	TeleportService:Teleport(
		game.PlaceId,
		player
	)

end)


---------------------------------------------------
-- SERVER HOP
---------------------------------------------------

local hopButton = Instance.new("TextButton")

hopButton.Parent = serverFrame
hopButton.Position = UDim2.new(1,-190,0,45)
hopButton.Size = UDim2.new(0,180,0,40)

hopButton.Text = "Server Hop"
hopButton.Font = Enum.Font.GothamBold
hopButton.TextSize = 18
hopButton.TextColor3 = Color3.new(1,1,1)

hopButton.BackgroundColor3 = Color3.fromRGB(170,0,255)

Instance.new("UICorner",hopButton)


hopButton.MouseButton1Click:Connect(function()

	hopButton.Text = "Procurando..."

	local servers = {}

	local success,result = pcall(function()

		return game:GetService("HttpService"):JSONDecode(
			game:HttpGet(
				"https://games.roblox.com/v1/games/"..
					game.PlaceId..
					"/servers/Public?sortOrder=Asc&limit=100"
			)
		)

	end)


	if success and result and result.data then

		for _,server in pairs(result.data) do

			if server.id ~= game.JobId 
				and server.playing < server.maxPlayers then

				table.insert(
					servers,
					server.id
				)

			end

		end

	end


	if #servers > 0 then

		local randomServer =
			servers[
		math.random(1,#servers)
		]


		TeleportService:TeleportToPlaceInstance(
			game.PlaceId,
			randomServer,
			player
		)

	else

		hopButton.Text = "Nenhum server"

		task.wait(2)

	end


	hopButton.Text = "Trocar Server"

end)

---------------------------------------------------
-- AIMBOT NPC
---------------------------------------------------

local npcAimbotFrame = Instance.new("Frame")
npcAimbotFrame.Parent = scroll
npcAimbotFrame.Size = UDim2.new(1,-10,0,70)
npcAimbotFrame.BackgroundColor3 = Color3.fromRGB(35,35,35)

Instance.new("UICorner",npcAimbotFrame)

---------------------------------------------------
-- TÍTULO
---------------------------------------------------

local npcAimbotTitle = Instance.new("TextLabel")
npcAimbotTitle.Parent = npcAimbotFrame
npcAimbotTitle.BackgroundTransparency = 1
npcAimbotTitle.Position = UDim2.new(0,10,0,5)
npcAimbotTitle.Size = UDim2.new(0,200,0,25)

npcAimbotTitle.Text = "Aimbot NPC"
npcAimbotTitle.TextColor3 = Color3.new(1,1,1)
npcAimbotTitle.Font = Enum.Font.GothamBold
npcAimbotTitle.TextSize = 18
npcAimbotTitle.TextXAlignment = Enum.TextXAlignment.Left

---------------------------------------------------
-- BOTÃO
---------------------------------------------------

local npcAimbotButton = Instance.new("TextButton")
npcAimbotButton.Parent = npcAimbotFrame
npcAimbotButton.Position = UDim2.new(1,-140,0,15)
npcAimbotButton.Size = UDim2.new(0,120,0,40)

npcAimbotButton.Text = "OFF"
npcAimbotButton.BackgroundColor3 = Color3.fromRGB(255,0,0)
npcAimbotButton.TextColor3 = Color3.new(1,1,1)
npcAimbotButton.Font = Enum.Font.GothamBold
npcAimbotButton.TextSize = 20

Instance.new("UICorner",npcAimbotButton)

---------------------------------------------------
-- VARIÁVEIS
---------------------------------------------------

local npcAimbotEnabled = false
local npcAimbotActive = false
local npcAimbotTarget = nil
local npcAimbotLoopId = 0

---------------------------------------------------
-- PROCURA NPC MAIS PRÓXIMO DO CENTRO DA CÂMERA
---------------------------------------------------

local function GetClosestNPC()

	local camera = workspace.CurrentCamera

	if not camera then
		return nil
	end

	local viewport = camera.ViewportSize

	local screenCenter = Vector2.new(
		viewport.X / 2,
		viewport.Y / 2
	)

	local closestNPC = nil
	local closestScreenDistance = math.huge

	for _, object in ipairs(workspace:GetDescendants()) do

		if object:IsA("Model") then

			---------------------------------------------------
			-- IGNORA PERSONAGENS DE PLAYERS
			---------------------------------------------------

			local playerOwner = Players:GetPlayerFromCharacter(object)

			if not playerOwner then

				local humanoid =
					object:FindFirstChildOfClass("Humanoid")

				local head =
					object:FindFirstChild("Head")

				if humanoid
					and head
					and humanoid.Health > 0 then

					---------------------------------------------------
					-- VÊ SE O NPC ESTÁ APARECENDO NA CÂMERA
					---------------------------------------------------

					local screenPosition, visible =
						camera:WorldToViewportPoint(head.Position)

					if visible and screenPosition.Z > 0 then

						local screenPoint = Vector2.new(
							screenPosition.X,
							screenPosition.Y
						)

						local distanceFromCenter =
							(screenPoint - screenCenter).Magnitude

						---------------------------------------------------
						-- NPC MAIS PRÓXIMO DO CENTRO
						---------------------------------------------------

						if distanceFromCenter < closestScreenDistance then

							closestScreenDistance =
								distanceFromCenter

							closestNPC =
								object

						end

					end

				end

			end

		end

	end

	return closestNPC

end

---------------------------------------------------
-- START AIMBOT NPC
---------------------------------------------------

local function StartNPCAimbot()

	if not npcAimbotEnabled then
		return
	end

	if npcAimbotActive then
		return
	end

	---------------------------------------------------
	-- ESCOLHE O NPC UMA VEZ
	---------------------------------------------------

	npcAimbotTarget =
		GetClosestNPC()

	if not npcAimbotTarget then
		return
	end

	npcAimbotActive = true
	npcAimbotLoopId += 1

	local thisLoopId =
		npcAimbotLoopId

	task.spawn(function()

		while npcAimbotEnabled
			and npcAimbotActive
			and thisLoopId == npcAimbotLoopId do

			local camera =
				workspace.CurrentCamera

			if camera then

				local npc =
					npcAimbotTarget

				if not npc or not npc.Parent then
					break
				end

				local humanoid =
					npc:FindFirstChildOfClass("Humanoid")

				local head =
					npc:FindFirstChild("Head")

				if not humanoid
					or humanoid.Health <= 0
					or not head then

					break
				end

				---------------------------------------------------
				-- CÂMERA OLHA PARA O NPC
				---------------------------------------------------

				local cameraPosition =
					camera.CFrame.Position

				camera.CFrame =
					CFrame.lookAt(
						cameraPosition,
						head.Position
					)

			end

			task.wait()

		end

		npcAimbotActive = false

	end)

end

---------------------------------------------------
-- BOTÃO ON/OFF
---------------------------------------------------

npcAimbotButton.MouseButton1Click:Connect(function()

	npcAimbotEnabled =
		not npcAimbotEnabled

	if npcAimbotEnabled then

		npcAimbotButton.Text = "ON"

		npcAimbotButton.BackgroundColor3 =
			Color3.fromRGB(0,255,0)

		StartNPCAimbot()

	else

		npcAimbotActive = false
		npcAimbotTarget = nil
		npcAimbotLoopId += 1

		npcAimbotButton.Text = "OFF"

		npcAimbotButton.BackgroundColor3 =
			Color3.fromRGB(255,0,0)

	end

end)

---------------------------------------------------
-- TECLA F
---------------------------------------------------

UIS.InputBegan:Connect(function(input,gameProcessed)

	if gameProcessed then
		return
	end

	if input.KeyCode ~= Enum.KeyCode.F then
		return
	end

	---------------------------------------------------
	-- BOTÃO OFF = F NÃO FAZ NADA
	---------------------------------------------------

	if not npcAimbotEnabled then
		return
	end

	---------------------------------------------------
	-- PAUSA
	---------------------------------------------------

	if npcAimbotActive then

		npcAimbotActive = false
		npcAimbotLoopId += 1
		npcAimbotTarget = nil

		---------------------------------------------------
		-- RETOMA
		---------------------------------------------------

	else

		StartNPCAimbot()

	end

end)

---------------------------------------------------
-- RESPAWN
---------------------------------------------------

player.CharacterAdded:Connect(function()

	task.wait(1)

	-- Só volta sozinho se NÃO estava pausado
	if npcAimbotEnabled and npcAimbotActive then

		npcAimbotActive = false
		npcAimbotTarget = nil

		StartNPCAimbot()

	end

end)

---------------------------------------------------
-- TOUCH FLING
---------------------------------------------------

local touchFlingFrame = Instance.new("Frame")
touchFlingFrame.Parent = scroll
touchFlingFrame.Size = UDim2.new(1,-10,0,70)
touchFlingFrame.BackgroundColor3 = Color3.fromRGB(35,35,35)

Instance.new("UICorner",touchFlingFrame)

---------------------------------------------------
-- TÍTULO
---------------------------------------------------

local touchFlingTitle = Instance.new("TextLabel")
touchFlingTitle.Parent = touchFlingFrame
touchFlingTitle.BackgroundTransparency = 1
touchFlingTitle.Position = UDim2.new(0,10,0,5)
touchFlingTitle.Size = UDim2.new(0,200,0,25)

touchFlingTitle.Text = "Touch Fling (RISCO DE BAN)"
touchFlingTitle.TextColor3 = Color3.new(1,1,1)
touchFlingTitle.Font = Enum.Font.GothamBold
touchFlingTitle.TextSize = 18
touchFlingTitle.TextXAlignment = Enum.TextXAlignment.Left

---------------------------------------------------
-- BOTÃO
---------------------------------------------------

local touchFlingButton = Instance.new("TextButton")
touchFlingButton.Parent = touchFlingFrame
touchFlingButton.Position = UDim2.new(1,-140,0,15)
touchFlingButton.Size = UDim2.new(0,120,0,40)

touchFlingButton.Text = "OFF"
touchFlingButton.BackgroundColor3 = Color3.fromRGB(255,0,0)
touchFlingButton.TextColor3 = Color3.new(1,1,1)
touchFlingButton.Font = Enum.Font.GothamBold
touchFlingButton.TextSize = 20

Instance.new("UICorner",touchFlingButton)

---------------------------------------------------
-- LÓGICA
---------------------------------------------------

local character = player.Character or player.CharacterAdded:Wait()
local rootPart = character:WaitForChild("HumanoidRootPart")

local flingEnabled = false

local FLING_POWER = 1e6  -- força fixa somada
local FLING_MULT  = 10000     -- multiplicador da sua velocidade real
local SPIN_POWER  = 99999  -- giro no eixo Y (só o servidor vê)

-- Limites ANTI-ARRASTO (o que conta como "movimento normal" seu)
local MAX_SPEED = 40          -- velocidade horizontal máxima aceita
local MAX_JUMP  = 60          -- velocidade máxima para cima
local MAX_FALL  = 200         -- velocidade máxima de queda
local MAX_STEP  = 10          -- studs por frame; acima disso = foi arrastado
local TELEPORT_FRAMES = 30    -- frames até aceitar como teleporte do painel

local heartbeatConn, renderConn, steppedConn

-- Estado real (o que a SUA tela mostra)
local realCFrame, realLinear, realAngular = nil, Vector3.zero, Vector3.zero
local offsetSign = 1

-- Anti-arrasto
local lastSafeCFrame = nil
local rejectCount = 0

-- Colisão original das partes (para restaurar ao desligar)
local originalCollide = {}

local ARM_NAMES = {
	["LeftUpperArm"] = true, ["LeftLowerArm"] = true, ["LeftHand"] = true,
	["RightUpperArm"] = true, ["RightLowerArm"] = true, ["RightHand"] = true,
	["Left Arm"] = true, ["Right Arm"] = true, -- R6
}

local function getHumanoid()
	return character and character:FindFirstChildOfClass("Humanoid")
end

local function isValid()
	return character
		and rootPart
		and rootPart.Parent
		and getHumanoid() ~= nil
end

---------------------------------------------------
-- COLISÃO APENAS NOS BRAÇOS
---------------------------------------------------

local function applyArmCollision()
	if not character then return end
	for _, part in ipairs(character:GetDescendants()) do
		if part:IsA("BasePart") then
			if originalCollide[part] == nil then
				originalCollide[part] = part.CanCollide
			end
			part.CanCollide = ARM_NAMES[part.Name] == true
		end
	end
end

local function restoreCollision()
	for part, value in pairs(originalCollide) do
		if part and part.Parent then
			part.CanCollide = value
		end
	end
	table.clear(originalCollide)
end

---------------------------------------------------
-- CONTROLE DE CONEXÕES
---------------------------------------------------

local function disconnectAll()
	if heartbeatConn then heartbeatConn:Disconnect() heartbeatConn = nil end
	if renderConn then renderConn:Disconnect() renderConn = nil end
	if steppedConn then steppedConn:Disconnect() steppedConn = nil end
end

local function restoreReal()
	if realCFrame and rootPart and rootPart.Parent then
		rootPart.CFrame = realCFrame
		rootPart.AssemblyLinearVelocity = realLinear
		rootPart.AssemblyAngularVelocity = realAngular
	end
	realCFrame = nil
end

-- Limita a velocidade para o impacto não virar "velocidade sua"
local function clampVelocity(vel)
	local horizontal = Vector3.new(vel.X, 0, vel.Z)
	if horizontal.Magnitude > MAX_SPEED then
		horizontal = horizontal.Unit * MAX_SPEED
	end
	local y = math.clamp(vel.Y, -MAX_FALL, MAX_JUMP)
	return Vector3.new(horizontal.X, y, horizontal.Z)
end

local function startFling()
	disconnectAll()
	lastSafeCFrame = nil
	rejectCount = 0

	-- 1) HEARTBEAT: salva o estado real (já filtrado) e aplica o estado de fling
	heartbeatConn = RunService.Heartbeat:Connect(function()
		if not isValid() then return end

		local cf = rootPart.CFrame
		local lin = rootPart.AssemblyLinearVelocity

		-- ANTI-ARRASTO: se fui jogado longe num frame, volto pra posição segura
		if lastSafeCFrame then
			local dist = (cf.Position - lastSafeCFrame.Position).Magnitude
			if dist > MAX_STEP then
				rejectCount += 1
				if rejectCount < TELEPORT_FRAMES then
					cf = lastSafeCFrame
					lin = Vector3.zero
				else
					-- ficou muito tempo deslocado: foi teleporte do painel
					rejectCount = 0
					lastSafeCFrame = cf
				end
			else
				rejectCount = 0
				lastSafeCFrame = cf
			end
		else
			lastSafeCFrame = cf
		end

		-- Impacto não vira sua velocidade real
		lin = clampVelocity(lin)

		realCFrame = cf
		realLinear = lin
		realAngular = Vector3.zero

		-- Estado de fling (só o servidor enxerga)
		rootPart.AssemblyLinearVelocity = (lin * FLING_MULT) + Vector3.new(FLING_POWER, FLING_POWER, FLING_POWER)
		rootPart.AssemblyAngularVelocity = Vector3.new(0, SPIN_POWER, 0)
		rootPart.CFrame = cf * CFrame.Angles(0, math.rad(math.random(0, 360)), 0)
	end)

	-- 2) RENDERSTEPPED: devolve o estado real antes da câmera desenhar
	renderConn = RunService.RenderStepped:Connect(function()
		if not isValid() then return end
		restoreReal()
	end)

	-- 3) STEPPED: roda a cada passo de física
	steppedConn = RunService.Stepped:Connect(function()
		if not isValid() then return end

		-- Mantém colisão só nos braços (o Humanoid tenta resetar isso)
		applyArmCollision()

		local humanoid = getHumanoid()
		local climbing = humanoid and humanoid:GetState() == Enum.HumanoidStateType.Climbing

		if climbing then
			rootPart.AssemblyAngularVelocity = Vector3.new(0, SPIN_POWER, 0)
			rootPart.CFrame = rootPart.CFrame * CFrame.Angles(0, math.rad(math.random(0, 360)), 0)
		else
			offsetSign = -offsetSign
			rootPart.AssemblyLinearVelocity = rootPart.AssemblyLinearVelocity + Vector3.new(0, 0.1 * offsetSign, 0)
		end
	end)
end

local function stopFling()
	disconnectAll()
	restoreReal()
	restoreCollision()
	lastSafeCFrame = nil
	rejectCount = 0

	if rootPart and rootPart.Parent then
		rootPart.AssemblyLinearVelocity = Vector3.zero
		rootPart.AssemblyAngularVelocity = Vector3.zero
	end
end

local function updateFlingState()
	if flingEnabled then
		touchFlingButton.Text = "ON"
		touchFlingButton.BackgroundColor3 = Color3.fromRGB(0,255,0)
		startFling()
	else
		touchFlingButton.Text = "OFF"
		touchFlingButton.BackgroundColor3 = Color3.fromRGB(255,0,0)
		stopFling()
	end
end

touchFlingButton.MouseButton1Click:Connect(function()
	flingEnabled = not flingEnabled
	updateFlingState()
end)

player.CharacterAdded:Connect(function(newChar)
	disconnectAll()
	table.clear(originalCollide)
	character = newChar
	rootPart = character:WaitForChild("HumanoidRootPart")
	realCFrame = nil
	lastSafeCFrame = nil
	rejectCount = 0
	flingEnabled = false
	updateFlingState()
end)
