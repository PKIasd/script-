--[[
  Adiciona ao seu GojoHub uma opção de "Inf Jump".
  - Botão "Ativar Inf Jump" aparece no Hub.
  - Enquanto ativo, você pode pular infinitamente (inclusive no ar).
  - Para desativar, clique novamente.
  - Seguro: só afeta seu personagem.
--]]

-- (demais funções e variáveis do seu Hub permanecem iguais...)

-- Adicione ao final dos botões (ajuste a posição se necessário):
local botaoInfJump = Instance.new("TextButton")
botaoInfJump.Text = "Ativar Inf Jump"
botaoInfJump.Size = UDim2.new(1, -20, 0, 40)
botaoInfJump.Position = UDim2.new(0, 10, 0, 510)
botaoInfJump.BackgroundColor3 = Color3.fromRGB(255, 220, 100)
botaoInfJump.TextColor3 = Color3.new(0.3,0.2,0)
botaoInfJump.Font = Enum.Font.SourceSansBold
botaoInfJump.TextSize = 18
botaoInfJump.Parent = frame

-- Controle do Inf Jump
local infJumpAtivo = false
local infJumpConn = nil
local UserInputService = game:GetService("UserInputService")

local function setInfJump(enabled)
    infJumpAtivo = enabled
    if infJumpAtivo then
        botaoInfJump.Text = "Desativar Inf Jump"
        -- Conectar evento de Input
        infJumpConn = UserInputService.JumpRequest:Connect(function()
            local char = player.Character
            if char and char:FindFirstChildOfClass("Humanoid") then
                char:FindFirstChildOfClass("Humanoid"):ChangeState(Enum.HumanoidStateType.Jumping)
            end
        end)
    else
        botaoInfJump.Text = "Ativar Inf Jump"
        if infJumpConn then
            infJumpConn:Disconnect()
            infJumpConn = nil
        end
    end
end

botaoInfJump.MouseButton1Click:Connect(function()
    setInfJump(not infJumpAtivo)
end)

-- Segurança extra: desativa inf jump ao morrer
player.CharacterAdded:Connect(function()
    setInfJump(false)
end)

-- (restante do seu script permanece igual)
