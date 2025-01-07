-- Carregar a biblioteca de UI
local Lib = loadstring(game:HttpGet("https://raw.githubusercontent.com/7yhx/kwargs_Ui_Library/main/source.lua"))()

-- Criar a interface
local UI = Lib:Create{
    Theme = "Dark", -- ou qualquer outro tema
    Size = UDim2.new(0, 555, 0, 400) -- tamanho padrão
}

-- Criar a aba principal
local Main = UI:Tab{
    Name = "Inicia"
}

-- Criar divisores
local Divider = Main:Divider{
    Name = "Inicia Shit"
}

local QuitDivider = Main:Divider{
    Name = "Sair"
}

-- Funções do script com botões, toggles e mais
local KillAll = Divider:Button{
    Name = "Kill all",
    Description = "Kills all the players in the game!",
    Callback = function()
        for _, player in pairs(game.Players:GetPlayers()) do
            -- Simula a morte de todos os jogadores
            player:LoadCharacter()
            print(player.Name .. " killed.")
        end
    end
}

local LoopKillAll = Divider:Toggle{
    Name = "Loop kill all",
    Description = "Loop kills everyone in the game.",
    Callback = function(State)
        if State then
            while true do
                for _, player in pairs(game.Players:GetPlayers()) do
                    player:LoadCharacter() -- Mata o jogador
                end
                wait(2) -- Espera 2 segundos antes de matar novamente
            end
        end
    end
}

local OtherToggleStyle = Divider:Toggle{
    Name = "2nd style of toggle",
    Style = 2 -- outro estilo de toggle
}

-- Dropdown para selecionar jogadores
local Players = Divider:Dropdown{
    Name = "Player list",
    Options = {"Player1", "Player2", "Player3", "Player4", "Player5"},
    Callback = function(Value)
        print("Você selecionou: " .. Value)
    end
}

-- ColorPicker para escolher a cor do ESP
Divider:ColorPicker{
    Name = "ESP color",
    Default = Color3.fromRGB(0, 255, 255), -- cor padrão
    Callback = function(Value)
        print("Cor escolhida: ", Value)
    end
}

-- Caixa de texto para nome de veículo ou carro
Divider:Box{
    Name = "Car name",
    ClearText = true, -- se o campo de texto limpa quando ganha o foco
    Callback = function(Value)
        print("Carro selecionado: " .. Value)
    end
}

-- Dropdown de teletransporte
Divider:SearchDropdown{
    Name = "Teleports",
    Options = {"Pleasant Park", "Loot Lake", "Tomato Town", "Wailing Woods", "Anarchy Acres", "Retail Row"},
    ClearText = false, -- padrão
    Callback = function(Value)
        print("Você escolheu: " .. Value)
    end
}

-- Função de encerramento da UI
local Quit = QuitDivider:Button{
    Name = "Closes the ui library.",
    Callback = function()
        UI:Quit{
            Message = "Sair... Até logo!", -- mensagem ao fechar
            Length = 1 -- tempo para a mensagem desaparecer
        }
    end
}

-- Nova funcionalidade: Limpar o mapa
local ClearMap = Divider:Button{
    Name = "Clear Map",
    Description = "Remove todos os objetos do mapa!",
    Callback = function()
        for _, object in pairs(workspace:GetChildren()) do
            if object:IsA("BasePart") then
                object:Destroy()
            end
        end
        print("Mapa limpo!")
    end
}

-- Nova funcionalidade: Adicionar uma moeda
local AddCoin = Divider:Button{
    Name = "Add Coin",
    Description = "Adiciona uma moeda ao mapa!",
    Callback = function()
        local coin = Instance.new("Part")
        coin.Size = Vector3.new(2, 2, 2)
        coin.Shape = Enum.PartType.Ball
        coin.Position = game.Workspace.CurrentCamera.CFrame.Position + Vector3.new(0, 10, 0) -- spawn no ar
        coin.Anchored = true
        coin.Color = Color3.fromRGB(255, 223, 0) -- cor de moeda
        coin.Parent = workspace
        print("Moeda adicionada ao mapa!")
    end
}

-- Nova funcionalidade: Controlar velocidade (Speed)
local SpeedSlider = Divider:Slider{
    Name = "Speed",
    Min = 16, -- velocidade mínima (padrão do Roblox)
    Max = 100, -- velocidade máxima
    Default = 16, -- valor inicial (padrão do Roblox)
    Callback = function(Value)
        local player = game.Players.LocalPlayer
        local character = player.Character or player.CharacterAdded:Wait()
        local humanoid = character:WaitForChild("Humanoid")
        
        humanoid.WalkSpeed = Value -- Ajusta a velocidade do jogador
        print("Velocidade ajustada para: " .. Value)
    end
}
