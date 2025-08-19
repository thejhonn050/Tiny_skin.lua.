local Players = game:GetService("Players")
local function applyTiny(char)
    local hum = char:WaitForChild("Humanoid")

    -- Escalas para rigs R15 (visual, no altera colisiones en otros clientes)
    local function setScale(name, val)
        local s = hum:FindFirstChild(name)
        if s and s:IsA("NumberValue") then s.Value = val end
    end
    setScale("BodyHeightScale", 0.45)
    setScale("BodyWidthScale",  0.25)
    setScale("BodyDepthScale",  0.25)
    setScale("HeadScale",       0.35)

    -- Look tipo "stickman"
    for _, d in ipairs(char:GetDescendants()) do
        if d:IsA("BasePart") and d.Name ~= "HumanoidRootPart" then
            d.Color = Color3.fromRGB(0,0,0)
            d.Material = Enum.Material.SmoothPlastic
        end
    end
end

-- Funciona como LocalScript (executor o Studio) y también en pruebas de servidor de Studio
local lp = Players.LocalPlayer
if lp then
    if lp.Character then applyTiny(lp.Character) end
    lp.CharacterAdded:Connect(applyTiny)
else
    Players.PlayerAdded:Connect(function(p)
        p.CharacterAdded:Connect(applyTiny)
    end)
end
