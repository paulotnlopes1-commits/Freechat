--[=[
 d888b  db    db d888888b      .d888b.      db      db    db  .d8b.  
88' Y8b 88    88   `88'        VP  `8D      88      88    88 d8' `8b 
88      88    88    88            odD'      88      88    88 88ooo88 
88  ooo 88    88    88          .88'        88      88    88 88~~~88 
88. ~8~ 88b  d88   .88.        j88.         88booo. 88b  d88 88   88    @uniquadev
 Y888P  ~Y8888P' Y888888P      888888D      Y88888P ~Y8888P' YP   YP  CONVERTER 
]=]

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")

-- Dicionário de ID Físico para Mensagens (Garante precisão total)
local CodigosMensagem = {
    [5010] = "Oi!",
    [5020] = "Funcionou de boa!",
    [5030] = "Conectado e escutando.",
    [5040] = "Testando o segundo celular...",
}

-- Interface Base (Estilo a anterior que você gostou)
local ScreenGui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
ScreenGui.Name = "UTG_PHYSICS_BRIDGE"
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 420, 0, 320)
MainFrame.Position = UDim2.new(0.5, -210, 0.5, -160)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
MainFrame.BorderSizePixel = 2
MainFrame.BorderColor3 = Color3.fromRGB(255, 0, 0)

Instance.new("UIDragDetector", MainFrame)

local Title = Instance.new("TextLabel", MainFrame)
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Title.Text = "⚡ PONTE FÍSICA ESTÁVEL (MÉTODO 12 CODIFICADO) ⚡"
Title.TextColor3 = Color3.fromRGB(255, 0, 0)
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 14

local ChatLog = Instance.new("ScrollingFrame", MainFrame)
ChatLog.Size = UDim2.new(1, -20, 1, -120)
ChatLog.Position = UDim2.new(0, 10, 0, 40)
ChatLog.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
ChatLog.CanvasSize = UDim2.new(0, 0, 5, 0)
local ListLayout = Instance.new("UIListLayout", ChatLog)

local ChatInput = Instance.new("TextBox", MainFrame)
ChatInput.Size = UDim2.new(1, -20, 0, 30)
ChatInput.Position = UDim2.new(0, 10, 1, -40)
ChatInput.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
ChatInput.TextColor3 = Color3.fromRGB(255, 255, 255)
ChatInput.PlaceholderText = "Digite 'oi' ou 'funciona' para testar..."
ChatInput.Text = ""

-- Botões Rápidos de Teste de Pulso
local FastBtn1 = Instance.new("TextButton", MainFrame)
FastBtn1.Size = UDim2.new(0, 190, 0, 30)
FastBtn1.Position = UDim2.new(0, 10, 1, -80)
FastBtn1.BackgroundColor3 = Color3.fromRGB(0, 120, 0)
FastBtn1.Text = "Enviar 'Oi!' (Cód 5010)"
FastBtn1.TextColor3 = Color3.fromRGB(255, 255, 255)

local FastBtn2 = Instance.new("TextButton", MainFrame)
FastBtn2.Size = UDim2.new(0, 190, 0, 30)
FastBtn2.Position = UDim2.new(0, 220, 1, -80)
FastBtn2.BackgroundColor3 = Color3.fromRGB(0, 100, 150)
FastBtn2.Text = "Enviar 'Funcionou!' (Cód 5020)"
FastBtn2.TextColor3 = Color3.fromRGB(255, 255, 255)

local function AddMessage(sender, text, color)
    local lbl = Instance.new("TextLabel", ChatLog)
    lbl.Size = UDim2.new(1, 0, 0, 20)
    lbl.BackgroundTransparency = 1
    lbl.Text = "[" .. sender .. "]: " .. text
    lbl.TextColor3 = color or Color3.fromRGB(255, 255, 255)
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    ChatLog.CanvasPosition = Vector2.new(0, ChatLog.CanvasSize.Y.Offset)
end

-- Transmissor por Sustentação de Impulso (Mantém o valor ativo tempo suficiente para a rede ler)
local function EnviarCodigoFisico(codigo)
    if not LocalPlayer.Character or not LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then return end
    local root = LocalPlayer.Character.HumanoidRootPart
    
    task.spawn(function()
        -- Mantém a velocidade ativa por 0.5 segundos para garantir que o outro celular capture o número exato
        local t = 0
        while t < 0.5 do
            root.AssemblyLinearVelocity = Vector3.new(0, codigo, 0)
            t = t + RunService.Heartbeat:Wait()
        end
        root.AssemblyLinearVelocity = Vector3.new(0, 0, 0)
    end)
end

-- Receptor por Varredura de Estabilidade
local ultimoCodigoRecebido = 0
local tempoBloqueio = 0

RunService.Heartbeat:Connect(function()
    if tempoBloqueio > 0 then
        tempoBloqueio = tempoBloqueio - RunService.Heartbeat:Wait()
        return
    end

    for _, p in pairs(Players:GetPlayers()) do
        if p.Character and p.Name ~= LocalPlayer.Name then
            local root = p.Character:FindFirstChild("HumanoidRootPart")
            if root then
                local velY = math.round(root.AssemblyLinearVelocity.Y)
                
                -- Se a velocidade capturada for um dos nossos códigos configurados
                if CodigosMensagem[velY] then
                    local msgTraduzida = CodigosMensagem[velY]
                    AddMessage(p.Name, msgTraduzida, Color3.fromRGB(0, 255, 0))
                    
                    -- Anti-spam: Bloqueia leituras repetidas do mesmo sinal por 1 segundo
                    tempoBloqueio = 1.0
                    break
                end
            end
        end
    end
end)

-- Eventos dos Botões Rápidos
FastBtn1.MouseButton1Click:Connect(function()
    AddMessage("VOCÊ", "Enviando código 5010...", Color3.fromRGB(150, 150, 150))
    EnviarCodigoFisico(5010)
end)

FastBtn2.MouseButton1Click:Connect(function()
    AddMessage("VOCÊ", "Enviando código 5020...", Color3.fromRGB(150, 150, 150))
    EnviarCodigoFisico(5020)
end)

-- Evento da Caixa de Texto (Tradução Manual Inteligente)
ChatInput.FocusLost:Connect(function(enterPressed)
    if enterPressed and ChatInput.Text ~= "" then
        local t = string.lower(ChatInput.Text)
        ChatInput.Text = ""
        
        if t == "oi" then
            AddMessage("VOCÊ", "Oi!", Color3.fromRGB(255, 255, 255))
            EnviarCodigoFisico(5010)
        elseif t == "funciona" or t == "funcionou" then
            AddMessage("VOCÊ", "Funcionou de boa!", Color3.fromRGB(255, 255, 255))
            EnviarCodigoFisico(5020)
        else
            AddMessage("SISTEMA", "Palavra inválida para o teste estável. Use 'oi', 'funciona' ou os botões coloridos.", Color3.fromRGB(255, 0, 0))
        end
    end
end)

AddMessage("SISTEMA", "Painel Estável Carregado. Método 2 (Folder) salvo na memória.", Color3.fromRGB(255, 255, 0))
