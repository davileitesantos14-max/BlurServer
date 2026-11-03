-- Script (Server) - Coloque dentro da Part
local part = script.Parent
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")

local remote = ReplicatedStorage:WaitForChild("BlurRemote")
local COOLDOWN = 5 -- segundos

local playersOnCooldown = {}

part.Touched:Connect(function(hit)
	local character = hit.Parent
	local humanoid = character:FindFirstChildOfClass("Humanoid")
	if not humanoid then return end

	local player = Players:GetPlayerFromCharacter(character)
	if not player then return end

	-- Verifica cooldown
	if playersOnCooldown[player.UserId] then
		return
	end

	-- Coloca em cooldown
	playersOnCooldown[player.UserId] = true

	-- Manda o cliente alternar o blur
	remote:FireClient(player)

	-- Remove o cooldown depois de 5 segundos
	task.delay(COOLDOWN, function()
		playersOnCooldown[player.UserId] = nil
	end)
end)
