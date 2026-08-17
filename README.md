local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local player = Players.LocalPlayer

getgenv().KickPower = 3
getgenv().ReachDist = 12
getgenv().SmoothStick = false
getgenv().DesarmeActive = false
getgenv().StrongKick = false
getgenv().BugBallActive = false

-- FPS Booster Leve e Otimizado para Mobile (Zera o lag)
task.spawn(function()
    pcall(function()
        if setfpscap then setfpscap(999) end
        local lighting = game:GetService("Lighting")
        lighting.GlobalShadows = false
        lighting.FogEnd = 9e9
        settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
    end)
end)

local CoreGui = game:GetService("CoreGui")
if CoreGui:FindFirstChild("WTYStoreUltimate") then CoreGui.WTYStoreUltimate:Destroy() end

local ScreenGui = Instance.new("ScreenGui", CoreGui)
ScreenGui.Name = "WTYStoreUltimate"
ScreenGui.ResetOnSpawn = false

-- Função de Arrastar Suave
local function MakeDraggable(frame)
    local dragging, dragStart, startPos
    frame.InputBegan:Connect(function(input) 
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then 
            dragging = true; dragStart = input.Position; startPos = frame.Position 
        end 
    end)
    UserInputService.InputChanged:Connect(function(input)
        if (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) and dragging then
            local delta = input.Position - dragStart
            frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
    UserInputService.InputEnded:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = false end end)
end

-- Botão Flutuante Estilizado
local ToggleBtn = Instance.new("TextButton", ScreenGui)
ToggleBtn.Size = UDim2.new(0, 65, 0, 65)
ToggleBtn.Position = UDim2.new(0.03, 0, 0.4, 0)
ToggleBtn.BackgroundColor3 = Color3.fromRGB(15, 15, 20)
ToggleBtn.Text = "⚡WTY"
ToggleBtn.TextColor3 = Color3.fromRGB(255, 40, 40)
ToggleBtn.TextSize = 18
ToggleBtn.Font = Enum.Font.GothamBlack
Instance.new("UICorner", ToggleBtn).CornerRadius = UDim.new(1, 0)
local ToggleStroke = Instance.new("UIStroke", ToggleBtn)
ToggleStroke.Color = Color3.fromRGB(255, 30, 30)
ToggleStroke.Thickness = 2.5
MakeDraggable(ToggleBtn)

-- Painel Principal Formatado e Elegante
local Main = Instance.new("Frame", ScreenGui)
Main.Size = UDim2.new(0, 340, 0, 530)
Main.Position = UDim2.new(0.5, -170, 0.15, 0)
Main.BackgroundColor3 = Color3.fromRGB(10, 10, 14)
Main.Visible = false
MakeDraggable(Main)

Instance.new("UICorner", Main).CornerRadius = UDim.new(0, 16)
local MainStroke = Instance.new("UIStroke", Main)
MainStroke.Color = Color3.fromRGB(255, 35, 35)
MainStroke.Thickness = 2.5

local BgGrad = Instance.new("UIGradient", Main)
BgGrad.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(22, 22, 30)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(8, 8, 12))
}
BgGrad.Rotation = 45

ToggleBtn.MouseButton1Click:Connect(function() Main.Visible = not Main.Visible end)

-- Cabeçalho do Painel
local Header = Instance.new("TextLabel", Main)
Header.Size = UDim2.new(1, 0, 0, 48)
Header.BackgroundColor3 = Color3.fromRGB(16, 16, 22)
Header.Text = "  WTY STORE - SUPREME V11 (ULTRA LIGHT)"
Header.TextColor3 = Color3.fromRGB(255, 255, 255)
Header.Font = Enum.Font.GothamBlack
Header.TextSize = 13
Header.TextXAlignment = Enum.TextXAlignment.Left
Instance.new("UICorner", Header).CornerRadius = UDim.new(0, 16)

local Scroll = Instance.new("ScrollingFrame", Main)
Scroll.Size = UDim2.new(1, -20, 1, -60)
Scroll.Position = UDim2.new(0, 10, 0, 54)
Scroll.BackgroundTransparency = 1
Scroll.ScrollBarThickness = 3
Instance.new("UIListLayout", Scroll).Padding = UDim.new(0, 8)

local function AddHeaderLabel(txt)
    local lbl = Instance.new("TextLabel", Scroll)
    lbl.Size = UDim2.new(1, 0, 0, 22)
    lbl.BackgroundTransparency = 1
    lbl.Text = "  " .. txt
    lbl.TextColor3 = Color3.fromRGB(255, 85, 85)
    lbl.Font = Enum.Font.GothamBold
    lbl.TextSize = 11
    lbl.TextXAlignment = Enum.TextXAlignment.Left
end

-- Grid Potência do Chute
AddHeaderLabel("POTÊNCIA DO CHUTE (1 A 5)")
local KickGrid = Instance.new("Frame", Scroll)
KickGrid.Size = UDim2.new(1, 0, 0, 36)
KickGrid.BackgroundTransparency = 1
Instance.new("UIGridLayout", KickGrid).CellSize = UDim2.new(0, 60, 0, 34)
for i = 1, 5 do
    local b = Instance.new("TextButton", KickGrid)
    b.Text = tostring(i)
    b.BackgroundColor3 = (i == getgenv().KickPower) and Color3.fromRGB(255, 35, 35) or Color3.fromRGB(18, 18, 24)
    b.TextColor3 = Color3.new(1,1,1)
    b.Font = Enum.Font.GothamBold
    Instance.new("UICorner", b).CornerRadius = UDim.new(0, 8)
    b.MouseButton1Click:Connect(function()
        getgenv().KickPower = i
        for _, c in pairs(KickGrid:GetChildren()) do
            if c:IsA("TextButton") then c.BackgroundColor3 = Color3.fromRGB(18, 18, 24) end
        end
        b.BackgroundColor3 = Color3.fromRGB(255, 35, 35)
    end)
end

-- Grid Reach Direcional
AddHeaderLabel("REACH DIRECIONAL (ANALÓGICO)")
local ReachGrid = Instance.new("Frame", Scroll)
ReachGrid.Size = UDim2.new(1, 0, 0, 36)
ReachGrid.BackgroundTransparency = 1
Instance.new("UIGridLayout", ReachGrid).CellSize = UDim2.new(0, 60, 0, 34)
for _, r in pairs({5, 10, 15, 20, 30}) do
    local b = Instance.new("TextButton", ReachGrid)
    b.Text = tostring(r)
    b.BackgroundColor3 = (r == getgenv().ReachDist) and Color3.fromRGB(255, 35, 35) or Color3.fromRGB(18, 18, 24)
    b.TextColor3 = Color3.new(1,1,1)
    b.Font = Enum.Font.GothamBold
    Instance.new("UICorner", b).CornerRadius = UDim.new(0, 8)
    b.MouseButton1Click:Connect(function()
        getgenv().ReachDist = r
        for _, c in pairs(ReachGrid:GetChildren()) do
            if c:IsA("TextButton") then c.BackgroundColor3 = Color3.fromRGB(18, 18, 24) end
        end
        b.BackgroundColor3 = Color3.fromRGB(255, 35, 35)
    end)
end

local function AddToggle(text, varName)
    local btn = Instance.new("TextButton", Scroll)
    btn.Size = UDim2.new(1, 0, 0, 40)
    btn.BackgroundColor3 = Color3.fromRGB(16, 16, 22)
    btn.Text = "   [OFF] " .. text
    btn.TextColor3 = Color3.fromRGB(180, 180, 190)
    btn.Font = Enum.Font.GothamMedium
    btn.TextSize = 12
    btn.TextXAlignment = Enum.TextXAlignment.Left
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 8)
    
    local active = false
    btn.MouseButton1Click:Connect(function()
        active = not active
        getgenv()[varName] = active
        btn.Text = (active and "   [ON] " or "   [OFF] ") .. text
        btn.BackgroundColor3 = active and Color3.fromRGB(220, 30, 40) or Color3.fromRGB(16, 16, 22)
        btn.TextColor3 = active and Color3.new(1,1,1) or Color3.fromRGB(180, 180, 190)
    end)
end

AddToggle("Desarme Vantagem (Frente e Lados + Reach)", "DesarmeActive")
AddToggle("Bola Chiclete (Suave / Sem Lag)", "SmoothStick")
AddToggle("Bug Ball (Congela a Bola Exato 3s)", "BugBallActive")
AddToggle("Chute Direcional (Atravessa Apenas Players)", "StrongKick")

-- Botão OTM Otimizado
local OtmBtn = Instance.new("TextButton", Scroll)
OtmBtn.Size = UDim2.new(1, 0, 0, 40)
OtmBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 52)
OtmBtn.Text = "⚙️ Carregar Painel OTM Otimizado"
OtmBtn.TextColor3 = Color3.new(1,1,1)
OtmBtn.Font = Enum.Font.GothamBold
OtmBtn.TextSize = 12
Instance.new("UICorner", OtmBtn).CornerRadius = UDim.new(0, 8)
OtmBtn.MouseButton1Click:Connect(function()
    pcall(function() loadstring(game:HttpGet("https://pastefy.app/KEfkfhsr/raw"))() end)
end)

local bugTimer = 0

-- Loop Ultra Leve (Garante FPS Máximo no Celular)
RunService.RenderStepped:Connect(function()
    pcall(function()
        local char = player.Character
        local hrp = char and char:FindFirstChild("HumanoidRootPart")
        local humanoid = char and char:FindFirstChildOfClass("Humanoid")
        if not hrp then return end

        -- Passa apenas pelos outros jogadores sem pesar o jogo
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= player and p.Character then
                for _, part in pairs(p.Character:GetChildren()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end
        end

        local overlapParams = OverlapParams.new()
        overlapParams.FilterType = Enum.RaycastFilterType.Exclude
        overlapParams.FilterDescendantsInstances = {char}

        local currentReach = math.max(getgenv().ReachDist, 6)
        local nearbyParts = workspace:GetPartBoundsInRadius(hrp.Position, math.max(currentReach, 30), overlapParams)

        for _, v in pairs(nearbyParts) do
            if v:IsA("BasePart") and (v.Name:lower():find("ball") or v.Name:lower():find("soccer")) then
                local dist = (v.Position - hrp.Position).Magnitude

                -- 1. Reach Direcional pelo Analógico
                if getgenv().ReachDist > 0 and dist <= getgenv().ReachDist then
                    if humanoid and humanoid.MoveDirection.Magnitude > 0 then
                        local moveDir = humanoid.MoveDirection
                        v.AssemblyLinearVelocity = Vector3.new(moveDir.X * 30, v.AssemblyLinearVelocity.Y, moveDir.Z * 30)
                    end
                end

                -- 2. Bola Chiclete Super Leve
                if getgenv().SmoothStick and dist <= 3.8 then
                    local targetPos = (hrp.CFrame * CFrame.new(0, -2.2, -1.2)).Position
                    v.CFrame = v.CFrame:Lerp(CFrame.new(Vector3.new(targetPos.X, math.min(v.Position.Y, targetPos.Y + 0.4), targetPos.Z)), 0.3)
                    v.AssemblyLinearVelocity = Vector3.new(0, 0, 0)
                end

                -- 3. Bug Ball (3 segundos exatos)
                if getgenv().BugBallActive and dist <= math.max(currentReach, 5) then
                    bugTimer = bugTimer + 1
                    if bugTimer <= 180 then
                        v.AssemblyLinearVelocity = Vector3.new(0, 0, 0)
                        v.AssemblyAngularVelocity = Vector3.new(0, 0, 0)
                    else
                        bugTimer = 0
                    end
                else
                    bugTimer = 0
                end

                -- 4. Desarme Vantagem
                if getgenv().DesarmeActive and dist <= currentReach then
                    local forwardForce = hrp.CFrame.LookVector * 40
                    local sideForce = hrp.CFrame.RightVector * math.random(-20, 20)
                    v.AssemblyLinearVelocity = forwardForce + sideForce + Vector3.new(0, 2, 0)
                end

                -- 5. Chute Direcional Limpo
                if getgenv().StrongKick and dist <= 4.5 then
                    v.CanCollide = false
                    task.delay(0.12, function()
                        if v and v.Parent then v.CanCollide = true end
                    end)
                    local power = getgenv().KickPower * 80
                    v.AssemblyLinearVelocity = hrp.CFrame.LookVector * power + Vector3.new(0, getgenv().KickPower * 10, 0)
                end
            end
        end
    end)
end)
