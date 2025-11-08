-- LocalScript (poner dentro de DeltaTool)
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

local deltaRequest = ReplicatedStorage:WaitForChild("DeltaRequest")
local deltaResponse = ReplicatedStorage:WaitForChild("DeltaResponse")

local tool = script.Parent
local cooldownGui -- opcional: podrías mostrar cooldown en GUI

-- Cuando el jugador activa la herramienta (clic)
tool.Activated:Connect(function()
    -- Se solicita el efecto al servidor (todas las comprobaciones se hacen en servidor)
    deltaRequest:FireServer()
end)

-- Respuestas del servidor (éxito, error, cooldown)
deltaResponse.OnClientEvent:Connect(function(action, message, remaining)
    -- action: "Success", "Cooldown", "Error"
    -- message: texto para mostrar
    -- remaining: segundos restantes (si aplica)
    if action == "Success" then
        -- Notificación nativa (puede fallar si SetCore no está disponible en contextos limitados)
        pcall(function()
            game:GetService("StarterGui"):SetCore("SendNotification", {
                Title = "Delta",
                Text = message,
                Duration = 3
            })
        end)
    elseif action == "Cooldown" then
        pcall(function()
            game:GetService("StarterGui"):SetCore("SendNotification", {
                Title = "Delta - En enfriamiento",
                Text = ("Espera %d s para usar de nuevo"):format(math.ceil(remaining)),
                Duration = 3
            })
        end)
    else -- "Error"
        pcall(function()
            game:GetService("StarterGui"):SetCore("SendNotification", {
                Title = "Delta - Error",
                Text = message,
                Duration = 3
            })
        end)
    end
end)
