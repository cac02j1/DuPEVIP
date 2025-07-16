local Players = game:GetService("Players")
local lp = Players.LocalPlayer

-- GUI chính
local gui = Instance.new("ScreenGui", lp:WaitForChild("PlayerGui"))
gui.Name = "DupeProByChuong"
gui.ResetOnSpawn = false

-- Main Frame
local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 250, 0, 160)
frame.Position = UDim2.new(0.5, -125, 0.5, -80)
frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
frame.BorderSizePixel = 0
frame.AnchorPoint = Vector2.new(0.5, 0.5)
frame.Active = true
frame.Draggable = true
frame.Name = "Main"
frame.ClipsDescendants = true

local uicorner = Instance.new("UICorner", frame)
uicorner.CornerRadius = UDim.new(0, 12)

-- Nền background
local bgImage = Instance.new("ImageLabel", frame)
bgImage.Name = "Background"
bgImage.Size = UDim2.new(1, 0, 1, 0)
bgImage.Position = UDim2.new(0, 0, 0, 0)
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

-- Label loading
local loadLabel = Instance.new("TextLabel", frame)
loadLabel.Position = UDim2.new(0.1, 0, 0.3, 0)
loadLabel.Size = UDim2.new(0.8, 0, 0, 30)
loadLabel.BackgroundTransparency = 1
loadLabel.TextColor3 = Color3.fromRGB(0, 255, 0)
loadLabel.Font = Enum.Font.Code
loadLabel.TextSize = 20
loadLabel.Text = "Load: 0%"
loadLabel.ZIndex = 2

-- Nút Load
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

-- Nút Toggle (Viên thuốc)
local toggle = Instance.new("TextButton", frame)
toggle.Position = UDim2.new(0.5, -40, 0.85, 0)
toggle.Size = UDim2.new(0, 80, 0, 25)
toggle.Text = "OFF"
toggle.BackgroundColor3 = Color3.fromRGB(120, 0, 0)
toggle.TextColor3 = Color3.fromRGB(255, 255, 255)
toggle.Font = Enum.Font.Gotham
toggle.TextSize = 14
toggle.Visible = false
toggle.ZIndex = 2

local uicornerToggle = Instance.new("UICorner", toggle)
uicornerToggle.CornerRadius = UDim.new(1, 0)

-- Label Tiền Ảo ở góc
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
local toggled = false
local dupeLoop
local fakeMoney = 0

-- Khi bấm nút load
loadBtn.MouseButton1Click:Connect(function()
	if loaded then return end
	loaded = true
	loadBtn.Text = "Loading..."

	for i = 1, 100 do
		loadLabel.Text = "Load: " .. i .. "%"
		wait(0.03)
	end

	loadBtn.Text = "Loaded!"
	toggle.Visible = true
end)

-- Toggle Dupe + Bán Ảo
toggle.MouseButton1Click:Connect(function()
	toggled = not toggled
	toggle.Text = toggled and "ON" or "OFF"
	toggle.BackgroundColor3 = toggled and Color3.fromRGB(0, 200, 0) or Color3.fromRGB(120, 0, 0)

	if toggled then
		dupeLoop = task.spawn(function()
			while toggled do
				local char = lp.Character
				if char then
					local tool = char:FindFirstChildOfClass("Tool")
					if tool then
						-- Clone tool
						local clone = tool:Clone()
						clone.Parent = lp.Backpack

						-- Giả lập bán
						local plus = math.random(50, 100)
						fakeMoney += plus
						moneyGui.Text = "$" .. fakeMoney

						-- Hiệu ứng rung nhỏ (tuỳ chọn)
						moneyGui.Position = UDim2.new(1, -170 + math.random(-2, 2), 0, 20 + math.random(-1, 1))
					end
				end
				wait(0.1)
			end
		end)
	else
		if dupeLoop then
			task.cancel(dupeLoop)
		end
	end
end)
