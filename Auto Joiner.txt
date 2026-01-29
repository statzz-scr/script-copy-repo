print ("leaked by @bu8f on discord")

-- Loading UI and Tween Animations Guide
-- Extracted from auto joiner.lua for reuse in other scripts

-- Services
local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- Tween configurations for smooth animations
local buttonTweenInfo = TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.InOut)
local tweenInfo = TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.InOut)
local loadingTweenInfo = TweenInfo.new(1.5, Enum.EasingStyle.Quad, Enum.EasingDirection.InOut)

-- Loading UI Variables
local LoadingUI = nil
local LoadingUIElements = {}

-- Function to create loading UI
local function createLoadingUI()
    local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")
    
    -- Main ScreenGui
    local loadingScreenGui = Instance.new("ScreenGui")
    loadingScreenGui.IgnoreGuiInset = true
    loadingScreenGui.ScreenInsets = Enum.ScreenInsets.DeviceSafeInsets
    loadingScreenGui.Name = "LoadingUI"
    loadingScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    loadingScreenGui.Parent = PlayerGui

    -- Main Frame
    local mainFrame = Instance.new("Frame")
    mainFrame.BorderSizePixel = 0
    mainFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    mainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
    mainFrame.Size = UDim2.new(1.455, 0, 1.388, 0)
    mainFrame.Position = UDim2.new(0.2725, 0, 0.306, 0)
    mainFrame.BorderColor3 = Color3.fromRGB(0, 0, 0)
    mainFrame.Draggable = true
    mainFrame.Parent = loadingScreenGui

    -- Status Container (background for progress bar)
    local statusContainer = Instance.new("Frame")
    statusContainer.BackgroundColor3 = Color3.fromRGB(48, 48, 49)
    statusContainer.AnchorPoint = Vector2.new(0.5, 0.5)
    statusContainer.Size = UDim2.new(0.28245, 0, 0.02301, 0)
    statusContainer.Position = UDim2.new(0.65608, 0, 0.76207, 0)
    statusContainer.Name = "StatusContainer"
    statusContainer.Parent = mainFrame

    -- Status Container Corner
    local statusCorner = Instance.new("UICorner")
    statusCorner.CornerRadius = UDim.new(1, 0)
    statusCorner.Parent = statusContainer

    -- Status Bar (animated progress bar)
    local statusBar = Instance.new("Frame")
    statusBar.BackgroundColor3 = Color3.fromRGB(250, 249, 255)
    statusBar.AnchorPoint = Vector2.new(0.5, 0.5)
    statusBar.Size = UDim2.new(0.01163, 0, 0.02301, 0)
    statusBar.Position = UDim2.new(0.52012, 0, 0.76207, 0)
    statusBar.Name = "StatusBar"
    statusBar.Parent = mainFrame

    -- Status Bar Corner
    local statusBarCorner = Instance.new("UICorner")
    statusBarCorner.CornerRadius = UDim.new(1, 0)
    statusBarCorner.Parent = statusBar

    -- Status Bar Gradient
    local statusBarGradient = Instance.new("UIGradient")
    statusBarGradient.Rotation = 90
    statusBarGradient.Color = ColorSequence.new{
        ColorSequenceKeypoint.new(0.000, Color3.fromRGB(116, 246, 108)),
        ColorSequenceKeypoint.new(1.000, Color3.fromRGB(116, 246, 108))
    }
    statusBarGradient.Parent = statusBar

    -- Title Text
    local titleText = Instance.new("TextLabel")
    titleText.TextWrapped = true
    titleText.TextTransparency = 0.36
    titleText.TextScaled = true
    titleText.FontFace = Font.new("rbxasset://fonts/families/HighwayGothic.json", Enum.FontWeight.Regular, Enum.FontStyle.Normal)
    titleText.TextColor3 = Color3.fromRGB(255, 255, 255)
    titleText.BackgroundTransparency = 1
    titleText.AnchorPoint = Vector2.new(0.5, 0.5)
    titleText.Size = UDim2.new(0.19605, 0, 0.07749, 0)
    titleText.Text = "Puzzle Auto Joiner"
    titleText.Position = UDim2.new(0.65497, 0, 0.59194, 0)
    titleText.Parent = mainFrame

    -- Discord Link Text
    local discordText = Instance.new("TextLabel")
    discordText.TextWrapped = true
    discordText.TextTransparency = 0.56
    discordText.TextScaled = true
    discordText.FontFace = Font.new("rbxasset://fonts/families/GothamSSm.json", Enum.FontWeight.Bold, Enum.FontStyle.Normal)
    discordText.TextColor3 = Color3.fromRGB(255, 255, 255)
    discordText.BackgroundTransparency = 1
    discordText.AnchorPoint = Vector2.new(0.5, 0.5)
    discordText.Size = UDim2.new(0.27414, 0, 0.11988, 0)
    discordText.Text = "discord.gg/brainrotfinder"
    discordText.Position = UDim2.new(0.6547, 0, 0.65067, 0)
    discordText.Parent = mainFrame

    -- Teleporting Status Text
    local teleportingStatus = Instance.new("TextLabel")
    teleportingStatus.TextWrapped = true
    teleportingStatus.TextTransparency = 0.56
    teleportingStatus.TextScaled = true
    teleportingStatus.FontFace = Font.new("rbxasset://fonts/families/GothamSSm.json", Enum.FontWeight.Regular, Enum.FontStyle.Normal)
    teleportingStatus.TextColor3 = Color3.fromRGB(255, 255, 255)
    teleportingStatus.BackgroundTransparency = 1
    teleportingStatus.AnchorPoint = Vector2.new(0.5, 0.5)
    teleportingStatus.Size = UDim2.new(0.04375, 0, 0.01453, 0)
    teleportingStatus.Text = "Teleporting"
    teleportingStatus.Name = "TeleportingStatus"
    teleportingStatus.Position = UDim2.new(0.65525, 0, 0.72514, 0)
    teleportingStatus.Parent = mainFrame

    -- Brainrot Details Text
    local brainrotDetails = Instance.new("TextLabel")
    brainrotDetails.TextWrapped = true
    brainrotDetails.TextTransparency = 0.56
    brainrotDetails.TextScaled = true
    brainrotDetails.FontFace = Font.new("rbxasset://fonts/families/Inconsolata.json", Enum.FontWeight.Regular, Enum.FontStyle.Normal)
    brainrotDetails.TextColor3 = Color3.fromRGB(255, 255, 255)
    brainrotDetails.BackgroundTransparency = 1
    brainrotDetails.AnchorPoint = Vector2.new(0.5, 0.5)
    brainrotDetails.Size = UDim2.new(0.10689, 0, 0.02785, 0)
    brainrotDetails.Text = "[Noobini Pizzanini (3M)]"
    brainrotDetails.Name = "BrainrotDetails"
    brainrotDetails.Position = UDim2.new(0.65636, 0, 0.79839, 0)
    brainrotDetails.Parent = mainFrame

    -- Stroke Frame (border)
    local strokeFrame = Instance.new("Frame")
    strokeFrame.BorderSizePixel = 0
    strokeFrame.BackgroundColor3 = Color3.fromRGB(157, 157, 157)
    strokeFrame.AnchorPoint = Vector2.new(0.5, 0.5)
    strokeFrame.Size = UDim2.new(0.93929, 0, 0.91237, 0)
    strokeFrame.Position = UDim2.new(0.49732, 0, 0.49844, 0)
    strokeFrame.BorderColor3 = Color3.fromRGB(0, 0, 0)
    strokeFrame.Name = "StrokeFrame"
    strokeFrame.BackgroundTransparency = 1
    strokeFrame.Parent = loadingScreenGui

    -- Stroke Frame Border
    local strokeFrameBorder = Instance.new("UIStroke")
    strokeFrameBorder.Color = Color3.fromRGB(255, 255, 255)
    strokeFrameBorder.Parent = strokeFrame

    -- Aspect Ratio Constraint
    local aspectRatioConstraint = Instance.new("UIAspectRatioConstraint")
    aspectRatioConstraint.AspectRatio = 2.08571
    aspectRatioConstraint.Parent = loadingScreenGui

    -- Return UI elements for easy access
    return {
        screenGui = loadingScreenGui,
        mainFrame = mainFrame,
        statusContainer = statusContainer,
        statusBar = statusBar,
        titleText = titleText,
        discordText = discordText,
        teleportingStatus = teleportingStatus,
        brainrotDetails = brainrotDetails,
        strokeFrame = strokeFrame
    }
end

-- Function to show loading UI
local function showLoadingUI(brainrotName, generation)
    if LoadingUI then
        LoadingUI:Destroy()
    end
    
    LoadingUIElements = createLoadingUI()
    LoadingUI = LoadingUIElements.screenGui
    
    -- Update brainrot details with generation
    LoadingUIElements.brainrotDetails.Text = "[" .. brainrotName .. " (" .. generation .. ")]"
    
    -- Reset status
    LoadingUIElements.teleportingStatus.Text = "Teleporting"
    
    -- Reset loading bar
    LoadingUIElements.statusBar.Size = UDim2.new(0.01163, 0, 0.02301, 0)
    LoadingUIElements.statusBar.Position = UDim2.new(0.52012, 0, 0.76207, 0)
    
    -- Animate loading bar - stretch to fill entire status container
    local statusContainerSize = LoadingUIElements.statusContainer.Size.X.Scale
    local statusContainerPosition = LoadingUIElements.statusContainer.Position.X.Scale
    
    local tween = TweenService:Create(LoadingUIElements.statusBar, loadingTweenInfo, {
        Size = UDim2.new(statusContainerSize, 0, 0.02301, 0),
        Position = UDim2.new(statusContainerPosition, 0, 0.76207, 0)
    })
    tween:Play()
    
    -- Animate teleporting status dots
    local dotCount = 0
    local maxDots = 3
    local dotConnection
    local lastDotUpdate = 0
    
    local function updateDots()
        local currentTime = tick()
        if currentTime - lastDotUpdate >= 0.3 then -- Update every 0.3 seconds
            dotCount = dotCount + 1
            if dotCount > maxDots then
                dotCount = 1
            end
            
            local dots = string.rep(".", dotCount)
            LoadingUIElements.teleportingStatus.Text = "Teleporting" .. dots
            lastDotUpdate = currentTime
        end
    end
    
    dotConnection = game:GetService("RunService").Heartbeat:Connect(function()
        updateDots()
    end)
    
    -- Store connection for cleanup
    LoadingUIElements.dotConnection = dotConnection
    
    -- Auto-hide loading UI after 2 seconds
    task.spawn(function()
        task.wait(2)
        hideLoadingUI()
    end)
end

-- Function to hide loading UI
local function hideLoadingUI()
    -- Hide loading UI immediately (no wait)
    if LoadingUIElements and LoadingUIElements.dotConnection then
        LoadingUIElements.dotConnection:Disconnect()
    end
    
    if LoadingUI then
        LoadingUI:Destroy()
        LoadingUI = nil
        LoadingUIElements = {}
    end
end

-- Function to update loading UI status
local function updateLoadingStatus(status)
    if LoadingUIElements and LoadingUIElements.teleportingStatus then
        LoadingUIElements.teleportingStatus.Text = status
    end
end

-- Button Animation Examples
local function animateButton(button, targetSize, targetColor, duration)
    local tweenInfo = TweenInfo.new(duration or 0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.InOut)
    
    -- Animate size
    if targetSize then
        TweenService:Create(button, tweenInfo, {Size = targetSize}):Play()
    end
    
    -- Animate color
    if targetColor then
        TweenService:Create(button, tweenInfo, {BackgroundColor3 = targetColor}):Play()
    end
end

-- Toggle Button Animation Example
local function animateToggleButton(button, frame, isOn)
    local tweenInfo = TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.InOut)
    
    if isOn then
        -- Animate to ON position (right side)
        TweenService:Create(button, tweenInfo, {Position = UDim2.new(0.79, 0, 0.5, 0)}):Play()
        TweenService:Create(frame, tweenInfo, {BackgroundColor3 = Color3.fromRGB(0, 255, 0)}):Play() -- Green
    else
        -- Animate to OFF position (left side)
        TweenService:Create(button, tweenInfo, {Position = UDim2.new(0.21, 0, 0.5, 0)}):Play()
        TweenService:Create(frame, tweenInfo, {BackgroundColor3 = Color3.fromRGB(255, 70, 50)}):Play() -- Red
    end
end

-- Smooth Fade Animation
local function fadeIn(element, duration)
    duration = duration or 0.5
    local tweenInfo = TweenInfo.new(duration, Enum.EasingStyle.Quad, Enum.EasingDirection.InOut)
    
    element.Visible = true
    element.BackgroundTransparency = 1
    TweenService:Create(element, tweenInfo, {BackgroundTransparency = 0}):Play()
end

local function fadeOut(element, duration)
    duration = duration or 0.5
    local tweenInfo = TweenInfo.new(duration, Enum.EasingStyle.Quad, Enum.EasingDirection.InOut)
    
    TweenService:Create(element, tweenInfo, {BackgroundTransparency = 1}):Play()
    
    -- Hide after animation completes
    task.spawn(function()
        task.wait(duration)
        element.Visible = false
    end)
end

-- Scale Animation
local function scaleIn(element, duration)
    duration = duration or 0.3
    local tweenInfo = TweenInfo.new(duration, Enum.EasingStyle.Back, Enum.EasingDirection.Out)
    
    element.Size = UDim2.new(0, 0, 0, 0)
    element.Visible = true
    TweenService:Create(element, tweenInfo, {Size = UDim2.new(0.2, 0, 0.1, 0)}):Play()
end

local function scaleOut(element, duration)
    duration = duration or 0.3
    local tweenInfo = TweenInfo.new(duration, Enum.EasingStyle.Back, Enum.EasingDirection.In)
    
    TweenService:Create(element, tweenInfo, {Size = UDim2.new(0, 0, 0, 0)}):Play()
    
    -- Hide after animation completes
    task.spawn(function()
        task.wait(duration)
        element.Visible = false
    end)
end

-- Slide Animation
local function slideIn(element, direction, duration)
    duration = duration or 0.4
    local tweenInfo = TweenInfo.new(duration, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
    
    local startPosition
    local endPosition = element.Position
    
    if direction == "left" then
        startPosition = UDim2.new(-0.3, 0, endPosition.Y.Scale, endPosition.Y.Offset)
    elseif direction == "right" then
        startPosition = UDim2.new(1.3, 0, endPosition.Y.Scale, endPosition.Y.Offset)
    elseif direction == "up" then
        startPosition = UDim2.new(endPosition.X.Scale, endPosition.X.Offset, -0.3, 0)
    elseif direction == "down" then
        startPosition = UDim2.new(endPosition.X.Scale, endPosition.X.Offset, 1.3, 0)
    end
    
    element.Position = startPosition
    element.Visible = true
    TweenService:Create(element, tweenInfo, {Position = endPosition}):Play()
end

-- Usage Examples:
--[[
-- Show loading UI
showLoadingUI("Example Brainrot", "5M")

-- Animate a button
animateButton(someButton, UDim2.new(0.2, 0, 0.1, 0), Color3.fromRGB(0, 255, 0), 0.5)

-- Animate toggle
animateToggleButton(toggleButton, toggleFrame, true) -- true for ON, false for OFF

-- Fade animations
fadeIn(someFrame, 0.5)
fadeOut(someFrame, 0.3)

-- Scale animations
scaleIn(someFrame, 0.4)
scaleOut(someFrame, 0.3)

-- Slide animations
slideIn(someFrame, "left", 0.5)
slideIn(someFrame, "right", 0.4)
slideIn(someFrame, "up", 0.3)
slideIn(someFrame, "down", 0.6)
--]]

-- Export functions for use in other scripts
return {
    showLoadingUI = showLoadingUI,
    hideLoadingUI = hideLoadingUI,
    updateLoadingStatus = updateLoadingStatus,
    animateButton = animateButton,
    animateToggleButton = animateToggleButton,
    fadeIn = fadeIn,
    fadeOut = fadeOut,
    scaleIn = scaleIn,
    scaleOut = scaleOut,
    slideIn = slideIn,
    TweenService = TweenService,
    buttonTweenInfo = buttonTweenInfo,
    tweenInfo = tweenInfo,
    loadingTweenInfo = loadingTweenInfo
}
