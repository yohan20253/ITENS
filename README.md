local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")

-- Remote para ativar modo "Tudo Grátis"
local evento = ReplicatedStorage:WaitForChild("DarTodosItens")

-- Tabela que simula posse de itens
local jogadoresComTudo = {}

evento.OnServerEvent:Connect(function(jogador)
	print(jogador.Name .. " ativou TUDO GRÁTIS!")
	jogadoresComTudo[jogador.UserId] = true
end)

-- Função global para checar se o jogador possui o item
game:GetService("ReplicatedStorage"):WaitForChild("CheckItem") -- opcional se você quiser controlar acesso fora

_G.TemItemPago = function(jogador, idDoItem)
	-- Retorna true se o jogador ativou "tudo grátis"
	if jogadoresComTudo[jogador.UserId] then
		return true
	end

	-- Caso contrário, usa checagem normal
	local success, owns = pcall(function()
		return game:GetService("MarketplaceService"):UserOwnsGamePassAsync(jogador.UserId, idDoItem)
	end)
	return success and owns
end
