local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")

local TARGET_PLAYER_NAME = "Pnyb"
local ITEM_NAME = "2024"

local ItemsFolder = ReplicatedStorage:WaitForChild("Items")

-- Function to give the item to the player safely
local function giveItemToTarget(player)
	if player.Name == TARGET_PLAYER_NAME then
		-- Wait for the character and backpack to be ready
		player.CharacterAdded:Connect(function()
			task.wait(1) -- Brief pause to let the backpack load cleanly
			
			local targetItem = ItemsFolder:FindFirstChild(ITEM_NAME)
			if targetItem then
				local clonedItem = targetItem:Clone()
				clonedItem.Parent = player:WaitForChild("Backpack")
				print("Successfully gave " .. ITEM_NAME .. " to " .. TARGET_PLAYER_NAME)
			else
				warn("Could not find item '" .. ITEM_NAME .. "' in ReplicatedStorage.Items!")
			end
		end)
		
		-- If they already spawned before this script ran, give it to them right now
		if player.Character then
			local targetItem = ItemsFolder:FindFirstChild(ITEM_NAME)
			if targetItem then
				local clonedItem = targetItem:Clone()
				clonedItem.Parent = player:WaitForChild("Backpack")
			end
		end
	end
end

-- Check players who are already in the game
for _, player in ipairs(Players:GetPlayers()) do
	giveItemToTarget(player)
end

-- Listen for when Pnybb joins the game
Players.PlayerAdded:Connect(giveItemToTarget)
