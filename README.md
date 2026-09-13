-- =========================================================
-- PAINEL CNP @ilxlucaa — SENHA IGUAL DA FOTO
-- SENHA: ilxlucaa11
-- COLOCAR EM: StarterGui → ScreenGui → LocalScript
-- =========================================================

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local Workspace = game:GetService("Workspace")

local player = Players.LocalPlayer
if not player then return end
local playerGui = player:WaitForChild("PlayerGui")
local mouse = player:GetMouse()

-- ==============================================
-- CORES
-- ==============================================
local VERMELHO_SENHA = Color3.fromRGB(90, 0, 0)        -- Fundo da senha
local VERMELHO_BOTAO_ENTRAR = Color3.fromRGB(130, 10, 10)
local FUNDO_PAINEL = Color3.fromRGB(24, 24, 26)         -- Cinza-escuro
local FUNDO_BOTAO = Color3.fromRGB(34, 34, 36)
local VERDE_BORDA = Color3.fromRGB(80, 160, 80)
local VERDE_ON = Color3.fromRGB(100, 200, 100)
local VERDE_OFF = Color3.fromRGB(60, 130, 60)
local BRANCO = Color3.fromRGB(255, 255, 255)
local CINZA_CLARO = Color3.fromRGB(200, 200, 200)

local SENHA_CORRETA = "ilxlucaa11"
local Modulos = {}
local Indicadores = {}

-- ==============================================
-- TELA DE SENHA — IGUAL A FOTO
-- ==============================================
local guiSenha = Instance.new("ScreenGui")
guiSenha.Name = "TelaSenha"
guiSenha.ResetOnSpawn = false
guiSenha.Parent = playerGui

-- Fundo escuro atrás
local escuroFundo = Instance.new("Frame")
escuroFundo.Name = "FundoEscuro"
escuroFundo.Size = UDim2.new(1, 0, 1, 0)
escuroFundo.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
escuroFundo.BackgroundTransparency = 0.4
escuroFundo.Parent = guiSenha

-- Caixa da senha
local caixaSenha = Instance.new("Frame")
caixaSenha.Name = "CaixaSenha"
caixaSenha.Size = UDim2.fromOffset(320, 220)
caixaSenha.Position = UDim2.new(0.5, 0, 0.5, 0)
caixaSenha.AnchorPoint = Vector2.new(0.5, 0.5)
caixaSenha.BackgroundColor3 = VERMELHO_SENHA
Instance.new("UICorner", caixaSenha).CornerRadius = UDim.new(0, 16)
caixaSenha.Parent = guiSenha

-- Título
local titSenha = Instance.new("TextLabel")
titSenha.Size = UDim2.new(1, 0, 0, 50)
titSenha.Position = UDim2.fromOffset(0, 15)
titSenha.BackgroundTransparency = 1
titSenha.Text = "Painel Cnp"
titSenha.TextColor3 = BRANCO
titSenha.Font = Enum.Font.GothamBold
titSenha.TextSize = 28
titSenha.Parent = caixaSenha

-- Caixa de digitação
local inputSenha = Instance.new("TextBox")
inputSenha.Name = "InputSenha"
inputSenha.Size = UDim2.new(1, -30, 0, 50)
inputSenha.Position = UDim2.fromOffset(15, 70)
inputSenha.BackgroundColor3 = Color3.fromRGB(40, 5, 5)
inputSenha.Text = ""
inputSenha.PlaceholderText = "Digite a senha..."
inputSenha.TextColor3 = BRANCO
inputSenha.PlaceholderColor3 = Color3.fromRGB(160, 160, 160)
inputSenha.Font = Enum.Font.Gotham
inputSenha.TextSize = 18
inputSenha.AutoLocalize = false
Instance.new("UICorner", inputSenha).CornerRadius = UDim.new(0, 10)
inputSenha.Parent = caixaSenha

-- Botão Entrar
local btnEntrar = Instance.new("TextButton")
btnEntrar.Name = "BotaoEntrar"
btnEntrar.Size = UDim2.new(1, -30, 0, 50)
btnEntrar.Position = UDim2.fromOffset(15, 140)
btnEntrar.BackgroundColor3 = VERMELHO_BOTAO_ENTRAR
btnEntrar.Text = "ENTRAR"
btnEntrar.TextColor3 = BRANCO
btnEntrar.Font = Enum.Font.GothamBold
btnEntrar.TextSize = 20
btnEntrar.AutoLocalize = false
Instance.new("UICorner", btnEntrar).CornerRadius = UDim.new(0, 10)
btnEntrar.Parent = caixaSenha

-- Mensagem de erro
local erroTexto = Instance.new("TextLabel")
erroTexto.Name = "ErroTexto"
erroTexto.Size = UDim2.new(1, 0, 0, 25)
erroTexto.Position = UDim2.fromOffset(0, 195)
erroTexto.BackgroundTransparency = 1
erroTexto.Text = ""
erroTexto.TextColor3 = Color3.fromRGB(255, 120, 120)
erroTexto.Font = Enum.Font.Gotham
erroTexto.TextSize = 14
erroTexto.Visible = false
erroTexto.Parent = caixaSenha

-- ==============================================
-- FUNÇÃO CRIAR PAINELZINHO (Vertex / Anim)
-- ==============================================
local function criarPainelzinho(nome, posX, posY, titulo, conteudo)
	local painel = Instance.new("Frame")
	painel.Name = "Painel_"..nome
	painel.Size = UDim2.fromOffset(180, 180)
	painel.Position = UDim2.new(posX, 15, posY, 15)
	painel.BackgroundColor3 = FUNDO_PAINEL
	painel.Visible = false
	painel.Parent = playerGui
	Instance.new("UICorner", painel).CornerRadius = UDim.new(0, 12)
	local borda = Instance.new("UIStroke")
	borda.Color = VERDE_BORDA; borda.Thickness = 2; borda.Parent = painel

	local cab = Instance.new("TextLabel")
	cab.Size = UDim2.new(1, -10, 0, 35)
	cab.Position = UDim2.fromOffset(10, 5)
	cab.BackgroundTransparency = 1
	cab.Text = titulo
	cab.TextColor3 = BRANCO
	cab.Font = Enum.Font.GothamBold
	cab.TextSize = 15
	cab.TextXAlignment = Enum.TextXAlignment.Left
	cab.Parent = painel

	local corpo = Instance.new("TextLabel")
	corpo.Size = UDim2.new(1, -20, 1, -50)
	corpo.Position = UDim2.fromOffset(10, 40)
	corpo.BackgroundTransparency = 1
	corpo.Text = conteudo
	corpo.TextColor3 = CINZA_CLARO
	corpo.Font = Enum.Font.Gotham
	corpo.TextSize = 13
	corpo.TextWrapped = true
	corpo.TextXAlignment = Enum.TextXAlignment.Left
	corpo.TextYAlignment = Enum.TextYAlignment.Top
	corpo.Parent = painel

	return painel
end

-- ==============================================
-- FUNÇÕES AUXILIARES
-- ==============================================
local function criarIndicador(nome, posY)
	local btn = Instance.new("TextButton")
	btn.Name = "Ind_"..nome
	btn.Size = UDim2.fromOffset(120, 40)
	btn.Position = UDim2.new(0.02, 0, posY, 0)
	btn.BackgroundColor3 = FUNDO_BOTAO
	btn.BorderSizePixel = 0
	btn.Text = nome..": OFF"
	btn.TextColor3 = CINZA_CLARO
	btn.Font = Enum.Font.GothamBold
	btn.TextSize = 14
	btn.Visible = false
	btn.Parent = playerGui
	Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 8)
	local s = Instance.new("UIStroke")
	s.Color = VERDE_BORDA; s.Thickness = 1.5; s.Parent = btn
	Indicadores[nome] = btn
end

local function atualizarIndicador(nome, ativo)
	local btn = Indicadores[nome]
	if not btn then return end
	btn.Visible = true
	btn.Text = nome..": "..(ativo and "ON" or "OFF")
	btn.BackgroundColor3 = ativo and VERDE_ON or VERDE_OFF
	btn.TextColor3 = ativo and BRANCO or CINZA_CLARO
end

-- ==============================================
-- GUI PRINCIPAL — SÓ APARECE APÓS SENHA
-- ==============================================
local function abrirPainelPrincipal()
	guiSenha:Destroy() -- Remove a tela de senha

	local gui = Instance.new("ScreenGui")
	gui.Name = "PainelCnp"
	gui.ResetOnSpawn = false
	gui.Parent = playerGui

	-- Botão abrir
	local openBtn = Instance.new("TextButton")
	openBtn.Name = "BotaoAbrir"
	openBtn.Size = UDim2.fromOffset(130, 45)
	openBtn.Position = UDim2.new(0, 10, 0.5, -22)
	openBtn.BackgroundColor3 = FUNDO_PAINEL
	openBtn.Text = "ABRIR PAINEL"
	openBtn.TextColor3 = BRANCO
	openBtn.Font = Enum.Font.GothamBold
	openBtn.TextSize = 13
	Instance.new("UICorner", openBtn).CornerRadius = UDim.new(0, 8)
	local bordaOpen = Instance.new("UIStroke")
	bordaOpen.Color = VERDE_BORDA; bordaOpen.Thickness = 2; bordaOpen.Parent = openBtn
	openBtn.Parent = gui

	local panel = Instance.new("Frame")
	panel.Name = "Painel"
	panel.Size = UDim2.fromOffset(310, 480)
	panel.Position = UDim2.new(0.5, 0, 0.5, 0)
	panel.AnchorPoint = Vector2.new(0.5, 0.5)
	panel.BackgroundColor3 = FUNDO_PAINEL
	Instance.new("UICorner", panel).CornerRadius = UDim.new(0, 14)
	local bordaPanel = Instance.new("UIStroke")
	bordaPanel.Color = VERDE_BORDA; bordaPanel.Thickness = 2; bordaPanel.Parent = panel
	panel.Visible = false
	panel.Parent = gui

	-- TÍTULO
	local tit = Instance.new("TextLabel")
	tit.Size = UDim2.new(1, -50, 0, 50)
	tit.Position = UDim2.fromOffset(12, 5)
	tit.BackgroundTransparency = 1
	tit.Text = "PAINEL CNP @ilxlucaa"
	tit.TextColor3 = BRANCO
	tit.Font = Enum.Font.GothamBold
	tit.TextSize = 17
	tit.TextXAlignment = Enum.TextXAlignment.Left
	tit.Parent = panel

	-- BOTÃO X DE FECHAR
	local fecharX = Instance.new("TextButton")
	fecharX.Name = "BotaoFecharX"
	fecharX.Size = UDim2.fromOffset(36, 36)
	fecharX.Position = UDim2.new(1, -44, 0, 8)
	fecharX.BackgroundColor3 = FUNDO_BOTAO
	fecharX.Text = "×"
	fecharX.TextColor3 = BRANCO
	fecharX.Font = Enum.Font.GothamBold
	fecharX.TextSize = 24
	fecharX.AutoLocalize = false
	Instance.new("UICorner", fecharX).CornerRadius = UDim.new(1, 0)
	local bordaX = Instance.new("UIStroke")
	bordaX.Color = VERDE_BORDA; bordaX.Thickness = 1.5; bordaX.Parent = fecharX
	fecharX.Parent = panel

	-- Botão fechar embaixo
	local fecharBaixo = Instance.new("TextButton")
	fecharBaixo.Size = UDim2.fromOffset(100, 35)
	fecharBaixo.Position = UDim2.new(0.5, -50, 1, -45)
	fecharBaixo.BackgroundColor3 = FUNDO_BOTAO
	fecharBaixo.Text = "FECHAR"
	fecharBaixo.TextColor3 = BRANCO
	fecharBaixo.Font = Enum.Font.GothamBold
	fecharBaixo.TextSize = 13
	Instance.new("UICorner", fecharBaixo).CornerRadius = UDim.new(0, 8)
	local bordaFechar = Instance.new("UIStroke")
	bordaFechar.Color = VERDE_BORDA; bordaFechar.Thickness = 1.5; bordaFechar.Parent = fecharBaixo
	fecharBaixo.Parent = panel

	-- ABRIR / FECHAR
	openBtn.Activated:Connect(function() panel.Visible = true; openBtn.Visible = false end)
	local function fecharPainel() panel.Visible = false; openBtn.Visible = true end
	fecharX.Activated:Connect(fecharPainel)
	fecharBaixo.Activated:Connect(fecharPainel)

	-- Arrastar
	local drag, dStart, pStart
	panel.InputBegan:Connect(function(i)
		if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then
			drag = true; dStart = i.Position; pStart = panel.Position
		end
	end)
	UserInputService.InputChanged:Connect(function(i)
		if drag and (i.UserInputType == Enum.UserInputType.MouseMovement or i.UserInputType == Enum.UserInputType.Touch) then
			local delta = i.Position - dStart
			panel.Position = UDim2.new(pStart.X.Scale, pStart.X.Offset+delta.X, pStart.Y.Scale, pStart.Y.Offset+delta.Y)
		end
	end)

	local scr = Instance.new("ScrollingFrame")
	scr.Size = UDim2.new(1, -20, 1, -95)
	scr.Position = UDim2.fromOffset(10, 55)
	scr.BackgroundTransparency = 1
	scr.ScrollBarThickness = 4
	scr.AutomaticCanvasSize = Enum.AutomaticSize.Y
	scr.Parent = panel

	local lay = Instance.new("UIListLayout")
	lay.Padding = UDim.new(0, 7)
	lay.HorizontalAlignment = Enum.HorizontalAlignment.Center
	lay.Parent = scr

	local function Bot(nome, lig, desl)
		local b = Instance.new("TextButton")
		b.Size = UDim2.new(1, -8, 0, 42)
		b.BackgroundColor3 = VERDE_OFF
		b.Text = nome..": OFF"
		b.TextColor3 = CINZA_CLARO
		b.Font = Enum.Font.GothamBold
		b.TextSize = 14
		Instance.new("UICorner", b).CornerRadius = UDim.new(0, 8)
		local bordaBot = Instance.new("UIStroke")
		bordaBot.Color = VERDE_BORDA; bordaBot.Thickness = 1.5; bordaBot.Parent = b
		b.Parent = scr
		local at = false
		b.Activated:Connect(function()
			at = not at
			b.Text = nome..": "..(at and "ON" or "OFF")
			b.BackgroundColor3 = at and VERDE_ON or VERDE_OFF
			b.TextColor3 = at and BRANCO or CINZA_CLARO
			task.spawn(at and lig or desl)
		end)
	end

	-- ==============================================
	-- 🔫 MIRA MOBILADOR
	-- ==============================================
	criarIndicador("Mira", 0.05)
	Bot("Mira Mobilador",
	function()
		atualizarIndicador("Mira", true)
		print("✅ MIRA LIGADA")
		local ID = "rbxassetid://130286880439702"
		local function ok(t)
			if not t:IsA("Tool") or t.Name ~= "Gun" or t:GetAttribute("M") then return end
			t:SetAttribute("M", true)
			t.Equipped:Connect(function() mouse.Icon = ID end)
			t.Unequipped:Connect(function() mouse.Icon = "" end)
		end
		local function go(c)
			mouse.Icon = ""
			for _,v in ipairs(c:GetChildren()) do ok(v) end
			c.ChildAdded:Connect(ok)
			local bp = player:WaitForChild("Backpack")
			for _,v in ipairs(bp:GetChildren()) do ok(v) end
			bp.ChildAdded:Connect(ok)
		end
		if player.Character then go(player.Character) end
		Modulos.M = player.CharacterAdded:Connect(go)
	end,
	function()
		atualizarIndicador("Mira", false)
		mouse.Icon = ""
		if Modulos.M then Modulos.M:Disconnect() end
		print("✅ MIRA DESLIGADA")
	end)

	-- ==============================================
	-- 🟫 SPEED OLD
	-- ==============================================
	criarIndicador("SpeedOld", 0.12)
	Bot("Speed Old",
	function()
		atualizarIndicador("SpeedOld", true)
		Modulos.SpeedOldAtivo = true
		print("✅ SPEED OLD LIGADO")
		local itens = {WaterBalloon2024=true, SnowballToy2020=true, IceCream=true, Gun=true}
		local norm = 0
		local fast = 9.5
		local function go(c)
			local root = c:WaitForChild("HumanoidRootPart")
			if c:FindFirstChild("GlitchPart") then c.GlitchPart:Destroy() end
			local pt = Instance.new("Part")
			pt.Name = "GlitchPart"
			pt.Size = Vector3.new(2,2,2)
			pt.Transparency = 1
			pt.CanCollide = false
			pt.Parent = c
			local w = Instance.new("Weld")
			w.Part0 = root; w.Part1 = pt
			w.C0 = CFrame.new(0,0,norm)
			w.Parent = pt
			c.ChildAdded:Connect(function(o)
				if o:IsA("Tool") and itens[o.Name] then w.C0 = CFrame.new(0,0,fast); print("✅ SPEED LIGADO: "..o.Name) end
			end)
			c.ChildRemoved:Connect(function(o)
				if o:IsA("Tool") and itens[o.Name] then w.C0 = CFrame.new(0,0,norm); print("✅ SPEED DESLIGADO") end
			end)
		end
		if player.Character then go(player.Character) end
		Modulos.SOC = player.CharacterAdded:Connect(go)
	end,
	function()
		atualizarIndicador("SpeedOld", false)
		Modulos.SpeedOldAtivo = false
		if Modulos.SOC then Modulos.SOC:Disconnect() end
		if player.Character and player.Character:FindFirstChild("GlitchPart") then
			player.Character.GlitchPart:Destroy()
		end
		print("✅ SPEED OLD DESLIGADO")
	end)

	-- ==============================================
	-- 🛡️ ANTI CLICK
	-- ==============================================
	criarIndicador("AntiClick", 0.19)
	Bot("Anti Click",
	function()
		atualizarIndicador("AntiClick", true)
		print("✅ ANTI CLICK LIGADO")
		Modulos.AntiClickAtivo = true
		Modulos.ACLoop = RunService.RenderStepped:Connect(function()
			local c = player.Character; if not c then return end
			local k = c:FindFirstChild("Knife"); if not k then return end
			k.Enabled = false
			for _,p in ipairs(k:GetDescendants()) do if p:IsA("BasePart") then p.CanTouch = false end end
		end)
	end,
	function()
		atualizarIndicador("AntiClick", false)
		if Modulos.ACLoop then Modulos.ACLoop:Disconnect() end
		local c = player.Character; if not c then return end
		local k = c:FindFirstChild("Knife")
		if k then k.Enabled = true; for _,p in ipairs(k:GetDescendants()) do if p:IsA("BasePart") then p.CanTouch = true end end end
		print("✅ ANTI CLICK DESLIGADO")
	end)

	-- ==============================================
	-- ✨ VERTEX
	-- ==============================================
	criarIndicador("Vertex", 0.26)
	local painelVertex = criarPainelzinho("Vertex", 0.72, 0.05, "✨ VERTEX",
	[[Sistema Vertex Ativo!
• Efeitos visuais ativados
• Estilo personalizado
• Desempenho otimizado]])

	Bot("Vertex",
	function()
		atualizarIndicador("Vertex", true)
		painelVertex.Visible = true
		print("✅ VERTEX — APARECEU!")
	end,
	function()
		atualizarIndicador("Vertex", false)
		painelVertex.Visible = false
		print("✅ VERTEX — FECHADO")
	end)

	-- ==============================================
	-- 🐇 ANIM
	-- ==============================================
	criarIndicador("Anim", 0.33)
	local painelAnim = criarPainelzinho("Anim", 0.02, 0.35, "🐇 ANIMAÇÕES",
	[[Painel de Animações Ativo!
• Emotes e gestos
• Movimentos estilizados
• Animações personalizadas]])

	Bot("Anim",
	function()
		atualizarIndicador("Anim", true)
		painelAnim.Visible = true
		print("✅ ANIM — APARECEU!")
	end,
	function()
		atualizarIndicador("Anim", false)
		painelAnim.Visible = false
		print("✅ ANIM — FECHADO")
	end)

	-- ==============================================
	-- 😊 EMOTES
	-- ==============================================
	criarIndicador("Emotes", 0.40)
	Bot("Emotes",
	function()
		atualizarIndicador("Emotes", true)
		_G.EmotesAtivo = true
		print("✅ EMOTES LIGADO")
	end,
	function()
		atualizarIndicador("Emotes", false)
		_G.EmotesAtivo = nil
		print("✅ EMOTES DESLIGADO")
	end)

	-- ==============================================
	-- 🎬 ANIMAÇÃO OLD
	-- ==============================================
	Bot("Animação Old",
	function()
		if not _G.AnimOriginal then _G.AnimOriginal = Workspace.Retargeting end
		Workspace.Retargeting = Enum.AnimatorRetargetingMode.Disabled
		print("✅ ANIMAÇÃO OLD LIGADA")
	end,
	function()
		if _G.AnimOriginal then Workspace.Retargeting = _G.AnimOriginal end
		print("✅ ANIMAÇÃO OLD DESLIGADA")
	end)

	-- ==============================================
	-- 🔄 SHIFT OLD
	-- ==============================================
	Bot("Shift Old",
	function()
		local ligado = Vector3.new(1.2, 0.8, 0)
		local normal = Vector3.new(0, 0, 0)
		local hum, travado
		local function go(c) hum = c:WaitForChild("Humanoid"); hum.CameraOffset = normal end
		if player.Character then go(player.Character) end
		Mod.ShC = player.CharacterAdded:Connect(go)
		Mod.ShL = RunService.RenderStepped:Connect(function()
			if not hum then return end
			local trancado = UserInputService.MouseBehavior == Enum.MouseBehavior.LockCenter
			if trancado and not travado then travado = true; hum.CameraOffset = ligado
			elseif not trancado and travado then travado = false; hum.CameraOffset = normal end
		end)
		print("✅ SHIFT OLD LIGADO")
	end,
	function()
		if Mod.ShL then Mod.ShL:Disconnect() end
		if Mod.ShC then Mod.ShC:Disconnect() end
		if player.Character then
			local h = player.Character:FindFirstChild("Humanoid")
			if h then h.CameraOffset = Vector3.new(0, 0, 0) end
		end
		print("✅ SHIFT OLD DESLIGADO")
	end)

	print("======================================")
	print("✅ PAINEL CNP @ilxlucaa — LIBERADO!")
	print("======================================")
end

-- ==============================================
-- LOGICA DA SENHA
-- ==============================================
local function verificarSenha()
	local digitada = inputSenha.Text
	if digitada == SENHA_CORRETA then
		erroTexto.Visible = false
		abrirPainelPrincipal()
	else
		erroTexto.Visible = true
		erroTexto.Text = "Senha incorreta!"
		inputSenha.Text = ""
	end
end

btnEntrar.Activated:Connect(verificarSenha)
inputSenha.FocusLost:Connect(function(enterPressed)
	if enterPressed then verificarSenha() end
end)

print("✅ Tela de senha carregada — digite: ilxlucaa11")
esse ta mt lindo mas a interface da senha ta vermelha e ta faltando uns scripts e nenhum scripts funciona
