-- Script de farm de pecas para Roblox (Ilha Bela Roleplay)
-- Adicione este script a um Script dentro de um Part no workspace

local part = script.Parent  -- O objeto onde os jogadores devem coletar peças
local cooldown = 10-- Tempo de espera entre cada coleta (reduzido para ser mais rápido)
local itemName = "Peca"  -- Nome do item coletado

-- Criando um sistema de armazenamento de itens
local function givePlayerItem(player)
    local leaderstats = player:FindFirstChild("leaderstats")
    if not leaderstats then
        leaderstats = Instance.new("Folder")
        leaderstats.Name = "leaderstats"
        leaderstats.Parent = player
    end

    local item = leaderstats:FindFirstChild(itemName)
    if not item then
        item = Instance.new("IntValue")
        item.Name = itemName
        item.Value = 0
        item.Parent = leaderstats
    end

    item.Value = item.Value + 1
end

-- Detectando quando um jogador toca na peça
part.Touched:Connect(function(hit)
    local player = game.Players:GetPlayerFromCharacter(hit.Parent)
    if player then
        local lastCollection = player:GetAttribute("LastCollection") or 0
        if tick() - lastCollection >= cooldown then
            givePlayerItem(player)
            player:SetAttribute("LastCollection", tick())
            print(player.Name .. " coletou uma " .. itemName)
        else
            print("Aguarde o cooldown para coletar novamente.")
        end
    end
end)
