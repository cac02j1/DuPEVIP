local Players = game:GetService("Players")
local lp = Players.LocalPlayer
local RunService = game:GetService("RunService")
local fakeMoney = 0

-- GUI chính
local gui = Instance.new("ScreenGui", lp:WaitForChild("PlayerGui"))
gui.Name = "DupeProByChuong"
gui.ResetOnSpawn = false

-- GUI khung chính
local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 250, 0, 160)
frame.Position = UDim2.new(0.5, -125, 0.5, -80)
frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
frame.BorderSizePixel = 0
frame.AnchorPoint = Vector2.new(0.5, 0.5)
frame.Active = true
frame.Draggable = true
frame.Name = "Main"

local uicorner = Instance.new("UICorner", frame)
uicorner.CornerRadius = UDim.new(0, 12)

-- Background
local bgImage = Instance.new("ImageLabel", frame)
bgImage.Name = "Background"
bgImage.Size = UDim2.new(1, 0, 1, 0)
bgImage.Image = "rbxassetid://124570536149875"
bgImage.BackgroundTransparency = 1
bgImage.ImageTransparency = 0.4
bgImage.ZIndex = 0

-- Tiêu đề
local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 30)
title.Text = "Dupe Pro by Chuong"
title.BackgroundTransparency = 1
title.TextColor3 = Color3.fromRGB(255, 255, 0)
title.Font = Enum.Font.GothamBold
title.TextSize = 18
title.ZIndex = 2

-- Load label
local loadLabel = Instance.new("TextLabel", frame)
loadLabel.Position = UDim2.new(0.1, 0, 0.3, 0)
loadLabel.Size = UDim2.new(0.8, 0, 0, 30)
loadLabel.BackgroundTransparency = 1
loadLabel.TextColor3 = Color3.fromRGB(0, 255, 0)
loadLabel.Font = Enum.Font.Code
loadLabel.TextSize = 20
loadLabel.Text = "Load: 0%"
loadLabel.ZIndex = 2

-- Load Button
local loadBtn = Instance.new("TextButton", frame)
loadBtn.Position = UDim2.new(0.1, 0, 0.6, 0)
loadBtn.Size = UDim2.new(0.8, 0, 0, 30)
loadBtn.Text = "Start Loading"
loadBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
loadBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
loadBtn.Font = Enum.Font.GothamBold
loadBtn.TextSize = 14
loadBtn.ZIndex = 2

local uicornerBtn = Instance.new("UICorner", loadBtn)
uicornerBtn.CornerRadius = UDim.new(1, 0)

-- Tiền ảo GUI góc phải
local moneyGui = Instance.new("TextLabel", gui)
moneyGui.Size = UDim2.new(0, 160, 0, 40)
moneyGui.Position = UDim2.new(1, -170, 0, 20)
moneyGui.BackgroundTransparency = 1
moneyGui.TextColor3 = Color3.fromRGB(0, 255, 0)
moneyGui.Font = Enum.Font.GothamBold
moneyGui.TextSize = 20
moneyGui.TextXAlignment = Enum.TextXAlignment.Right
moneyGui.Text = "$0"
moneyGui.Name = "FakeMoney"

-- Trạng thái
local loaded = false
local dupeLoop = nil
local inSellZone = false
local lastSell = 0

-- SellZone detector (phát hiện gần vùng bán)
local function isNearSellZone()
	local hrp = lp.Character and lp.Character:FindFirstChild("HumanoidRootPart")
	if not hrp then return false end

	-- Tìm part có tên kiểu "Sell" hoặc "SellArea"
	for _, v in pairs(workspace:GetDescendants()) do
		if v:IsA("BasePart") and v.Name:lower():find("sell") then
			if (v.Position - hrp.Position).Magnitude <= 10 then
				return true
			end
		end
	end
	return false
end

-- Auto SELL Loop
local function startSelling()
	if dupeLoop then return end
	dupeLoop = RunService.Heartbeat:Connect(function()
		if not loaded then return end

		if isNearSellZone() then
			if tick() - lastSell >= 0.15 then
				lastSell = tick()
				local char = lp.Character
				if char then
					local tool = char:FindFirstChildOfClass("Tool")
					if tool then
						local clone = tool:Clone()
						clone.Parent = lp.Backpack
					end
				end

				-- Tăng tiền ảo
				local plus = math.random(50, 120)
				fakeMoney += plus
				moneyGui.Text = "$" .. tostring(fakeMoney)
				loadLabel.Text = "Đã bán: +$" .. plus
			end
		end
	end)
end

-- Load nút
loadBtn.MouseButton1Click:Connect(function()
	if loaded then return end
	loaded = true
	loadBtn.Text = "Loading..."

	for i = 1, 100 do
		loadLabel.Text = "Load: " .. i .. "%"
		wait(0.02)
	end

	loadBtn.Text = "Loaded!"
	loadLabel.Text = "Đi đến chỗ SELL để bán!"
	startSelling()
end)
