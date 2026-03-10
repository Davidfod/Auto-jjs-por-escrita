local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local TextChatService = game:GetService("TextChatService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

-- Configurações de Spam
local rodando = false
local numeros = {"UM!", "DOIS!", "TRÊS!", "QUATRO!", "CINCO!", "SEIS!", "SETE!", "OITO!", "NOVE!", "DEZ!", "ONZE!", "DOZE!", "TREZE!", "QUATORZE!", "QUINZE!", "DEZESSEIS!", "DEZESSETE!", "DEZOITO!", "DEZENOVE!", "VINTE!", "VINTE E UM!", "VINTE E DOIS!", "VINTE E TRÊS!", "VINTE E QUATRO!", "VINTE E CINCO!", "VINTE E SEIS!", "VINTE E SETE!", "VINTE E OITO!", "VINTE E NOVE!", "TRINTA!", "TRINTA E UM!", "TRINTA E DOIS!", "TRINTA E TRÊS!", "TRINTA E QUATRO!", "TRINTA E CINCO!", "TRINTA E SEIS!", "TRINTA E SETE!", "TRINTA E OITO!", "TRINTA E NOVE!", "QUARENTA!", "QUARENTA E UM!", "QUARENTA E DOIS!", "QUARENTA E TRÊS!", "QUARENTA E QUATRO!", "QUARENTA E CINCO!", "QUARENTA E SEIS!", "QUARENTA E SETE!", "QUARENTA E OITO!", "QUARENTA E NOVE!", "CINQUENTA!", "CINQUENTA E UM!", "CINQUENTA E DOIS!", "CINQUENTA E TRÊS!", "CINQUENTA E QUATRO!", "CINQUENTA E CINCO!", "CINQUENTA E SEIS!", "CINQUENTA E SETE!", "CINQUENTA E OITO!", "CINQUENTA E NOVE!", "SESSENTA!", "SESSENTA E UM!", "SESSENTA E DOIS!", "SESSENTA E TRÊS!", "SESSENTA E QUATRO!", "SESSENTA E CINCO!", "SESSENTA E SEIS!", "SESSENTA E SETE!", "SESSENTA E OITO!", "SESSENTA E NOVE!", "SETENTA!", "SETENTA E UM!", "SETENTA E DOIS!", "SETENTA E TRÊS!", "SETENTA E QUATRO!", "SETENTA E CINCO!", "SETENTA E SEIS!", "SETENTA E SETE!", "SETENTA E OITO!", "SETENTA E NOVE!", "OITENTA!", "OITENTA E UM!", "OITENTA E DOIS!", "OITENTA E TRÊS!", "OITENTA E QUATRO!", "OITENTA E CINCO!", "OITENTA E SEIS!", "OITENTA E SETE!", "OITENTA E OITO!", "OITENTA E NOVE!", "NOVENTA!", "NOVENTA E UM!", "NOVENTA E DOIS!", "NOVENTA E TRÊS!", "NOVENTA E QUATRO!", "NOVENTA E CINCO!", "NOVENTA E SEIS!", "NOVENTA E SETE!", "NOVENTA E OITO!", "NOVENTA E NOVE!", "CEM!"}

-- Criação da Interface
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local TopBar = Instance.new("Frame")
local Title = Instance.new("TextLabel")
local CloseBtn = Instance.new("TextButton")
local MiniBtn = Instance.new("TextButton")
local Content = Instance.new("Frame")
local StartBtn = Instance.new("TextButton")
local StopBtn = Instance.new("TextButton")

-- Propriedades Básicas
ScreenGui.Name = "SpamControl"
ScreenGui.Parent = game.CoreGui
ScreenGui.ResetOnSpawn = false

MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MainFrame.BorderSizePixel = 0
MainFrame.Position = UDim2.new(0.5, -100, 0.5, -75)
MainFrame.Size = UDim2.new(0, 200, 0, 150)
MainFrame.ClipsDescendants = true

TopBar.Name = "TopBar"
TopBar.Parent = MainFrame
TopBar.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
TopBar.Size = UDim2.new(1, 0, 0, 30)

Title.Parent = TopBar
Title.BackgroundTransparency = 1
Title.Size = UDim2.new(1, -60, 1, 0)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.Text = "Spam Count"
Title.TextColor3 = Color3.new(1, 1, 1)
Title.TextXAlignment = Enum.TextXAlignment.Left

CloseBtn.Parent = TopBar
CloseBtn.Position = UDim2.new(1, -30, 0, 0)
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Text = "X"
CloseBtn.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
CloseBtn.TextColor3 = Color3.new(1, 1, 1)

MiniBtn.Parent = TopBar
MiniBtn.Position = UDim2.new(1, -60, 0, 0)
MiniBtn.Size = UDim2.new(0, 30, 0, 30)
MiniBtn.Text = "-"
MiniBtn.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
MiniBtn.TextColor3 = Color3.new(1, 1, 1)

Content.Name = "Content"
Content.Parent = MainFrame
Content.Position = UDim2.new(0, 0, 0, 30)
Content.Size = UDim2.new(1, 0, 1, -30)
Content.BackgroundTransparency = 1

StartBtn.Parent = Content
StartBtn.Position = UDim2.new(0.1, 0, 0.1, 0)
StartBtn.Size = UDim2.new(0.8, 0, 0.35, 0)
StartBtn.Text = "ATIVAR"
StartBtn.BackgroundColor3 = Color3.fromRGB(50, 150, 50)
StartBtn.TextColor3 = Color3.new(1, 1, 1)

StopBtn.Parent = Content
StopBtn.Position = UDim2.new(0.1, 0, 0.55, 0)
StopBtn.Size = UDim2.new(0.8, 0, 0.35, 0)
StopBtn.Text = "DESATIVAR"
StopBtn.BackgroundColor3 = Color3.fromRGB(150, 50, 50)
StopBtn.TextColor3 = Color3.new(1, 1, 1)

-- Sistema de Mensagens
local function enviar(txt)
    if TextChatService.ChatVersion == Enum.ChatVersion.TextChatService then
        local channel = TextChatService.TextChannels.RBXGeneral
        channel:SendAsync(txt)
    else
        ReplicatedStorage.DefaultChatSystemChatEvents.SayMessageRequest:FireServer(txt, "All")
    end
end

-- Lógica dos Botões
StartBtn.MouseButton1Click:Connect(function()
    if rodando then return end
    rodando = true
    for _, msg in pairs(numeros) do
        if not rodando then break end
        enviar(msg)
        task.wait(0.15) -- Pequeno delay para evitar mute
    end
    rodando = false
end)

StopBtn.MouseButton1Click:Connect(function()
    rodando = false
end)

CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

local minimizado = false
MiniBtn.MouseButton1Click:Connect(function()
    minimizado = not minimizado
    if minimizado then
        MainFrame:TweenSize(UDim2.new(0, 200, 0, 30), "Out", "Quad", 0.3, true)
        Content.Visible = false
    else
        MainFrame:TweenSize(UDim2.new(0, 200, 0, 150), "Out", "Quad", 0.3, true)
        Content.Visible = true
    end
end)

-- Sistema de Arrastar (Draggable)
local dragging, dragInput, dragStart, startPos
TopBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = MainFrame.Position
    end
end)

TopBar.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement then
        dragInput = input
    end
end)

RunService.RenderStepped:Connect(function()
    if dragging and dragInput then
        local delta = dragInput.Position - dragStart
        MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)
