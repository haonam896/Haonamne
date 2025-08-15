local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local player = Players.LocalPlayer
local Tool = script.Parent
local SwingRE = Tool:WaitForChild("Swing")

local COOLDOWN = 0.5
local isCooling = false
local humanoid
local track
local equipped = false

local swingAnim = Instance.new("Animation")
swingAnim.AnimationId = "rbxassetid://507777826"

local function onEquipped()
	equipped = true
	local char = player.Character
	if not char then return end
	humanoid = char:FindFirstChildOfClass("Humanoid")
	if humanoid then
		track = track or humanoid:LoadAnimation(swingAnim)
	end
end

local function onUnequipped()
	equipped = false
end

local function swing()
	if isCooling then return end
	isCooling = true
	if track then track:Play(0.05, 1, 1) end
	SwingRE:FireServer()
	task.delay(COOLDOWN, function()
		isCooling = false
	end)
end

Tool.Equipped:Connect(onEquipped)
Tool.Unequipped:Connect(onUnequipped)
Tool.Activated:Connect(swing)

UIS.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.KeyCode == Enum.KeyCode.F then
		local char = player.Character
		if not char then return end
		local hum = char:FindFirstChildOfClass("Humanoid")
		if not hum then return end
		if equipped then
			Tool.Parent = player.Backpack
			equipped = false
		else
			if Tool.Parent == player.Backpack then
				hum:EquipTool(Tool)
			end
		end
	end
end)
