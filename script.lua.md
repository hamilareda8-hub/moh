\-- SERVICES

**local** Players = **game**:GetService("Players")

**local** RunService = **game**:GetService("RunService")

**local** UserInputService = **game**:GetService("UserInputService")



**local** player = Players.LocalPlayer

**local** camera = **workspace**.CurrentCamera



\-- CONFIGURATION

**local** ESP_ENABLED = **false**

**local** AIM_ENABLED = **false**

**local** FOV_ENABLED = **true**

**local** FOV_RADIUS = 220

**local** AIM_PART = "Head"

**local** SPEED_ENABLED = **false**

**local** NORMAL_SPEED = 16

**local** FAST_SPEED = 32



\-- GUI

**local** gui = Instance.new("ScreenGui")

gui.Name = "KAWTHER LOVE YOU BABYY 💗"

gui.ResetOnSpawn = **false**

gui.Parent = player:WaitForChild("PlayerGui")



\-- PANNEAU

**local** panel = Instance.new("Frame")

panel.Size = UDim2.fromOffset(250, 390)

panel.Position = UDim2.new(0, 30, 0.35, 0)

panel.BackgroundColor3 = Color3.fromRGB(255, 255, 238)

panel.BorderSizePixel = 0

panel.Parent = gui



**local** panelCorner = Instance.new("UICorner")

panelCorner.CornerRadius = UDim.new(0, 16)

panelCorner.Parent = panel



**local** title = Instance.new("TextLabel")

title.Size = UDim2.new(1, 0, 0, 35)

title.BackgroundColor3 = Color3.fromRGB(48, 48, 62)

title.Text = "KAWTHER LOVE YOU BABYY 💗"

title.TextColor3 = Color3.new(1, 1, 1)

title.Font = **Enum**.Font.GothamBold

title.TextSize = 15

title.Parent = panel



\-- PANNEAU DEPLAÇABLE

**local** dragging = **false**

**local** dragStart

**local** startPosition



title.InputBegan:Connect(**function**(input)

    **if** input.UserInputType == **Enum**.UserInputType.MouseButton1

    **or** input.UserInputType == **Enum**.UserInputType.Touch **then**

        dragging = **true**

        dragStart = input.Position

        startPosition = panel.Position

    **end**

**end**)



UserInputService.InputChanged:Connect(**function**(input)

    **if** dragging **and** (

        input.UserInputType == **Enum**.UserInputType.MouseMovement

        **or** input.UserInputType == **Enum**.UserInputType.Touch

    ) **then**

        **local** delta = input.Position - dragStart

        panel.Position = UDim2.new(

            startPosition.X.Scale,

            startPosition.X.Offset + delta.X,

            startPosition.Y.Scale,

            startPosition.Y.Offset + delta.Y

        )

    **end**

**end**)



UserInputService.InputEnded:Connect(**function**(input)

    **if** input.UserInputType == **Enum**.UserInputType.MouseButton1

    **or** input.UserInputType == **Enum**.UserInputType.Touch **then**

        dragging = **false**

    **end**

**end**)



\-- BOUTONS

**local** **function** createButton(text, y, callback)

    **local** button = Instance.new("TextButton")

    button.Size = UDim2.new(1, -20, 0, 38)

    button.Position = UDim2.fromOffset(10, y)

    button.BackgroundColor3 = Color3.fromRGB(65, 65, 80)

    button.TextColor3 = Color3.new(1, 1, 1)

    button.Font = **Enum**.Font.Gotham

    button.TextSize = 13

    button.Text = text

    button.Parent = panel



    **local** corner = Instance.new("UICorner")

    corner.CornerRadius = UDim.new(0, 6)

    corner.Parent = button



    button.Activated:Connect(**function**()

        callback(button)

    **end**)



    **return** button

**end**



**local** **function** updateButton(button, name, enabled)

    button.Text = name .. ": " .. (enabled **and** "ON" **or** "OFF")

    button.BackgroundColor3 = enabled

        **and** Color3.fromRGB(45, 135, 85)

        **or** Color3.fromRGB(65, 65, 80)

**end**-- SPEED ⚡

createButton("⚡ Speed: OFF", 229, **function**(button)

    SPEED_ENABLED = **not** SPEED_ENABLED



    updateButton(button, "⚡ Speed", SPEED_ENABLED)



    **local** character = player.Character

    **local** humanoid = character

        **and** character:FindFirstChildOfClass("Humanoid")



    **if** humanoid **then**

        humanoid.WalkSpeed = SPEED_ENABLED

            **and** FAST_SPEED

            **or** NORMAL_SPEED

    **end**

**end**)



\-- ESP

createButton("ESP: OFF", 45, **function**(button)

    ESP_ENABLED = **not** ESP_ENABLED

    updateButton(button, "ESP", ESP_ENABLED)

**end**)



\-- AIM ASSIST

createButton("Aim Assist: OFF", 91, **function**(button)

    AIM_ENABLED = **not** AIM_ENABLED

    updateButton(button, "Aim Assist", AIM_ENABLED)

**end**)



\-- FOV

createButton("FOV Circle: ON", 137, **function**(button)

    FOV_ENABLED = **not** FOV_ENABLED

    updateButton(button, "FOV Circle", FOV_ENABLED)

**end**)



\-- AIM PART

createButton("Aim Part: Head", 183, **function**(button)

    AIM_PART = AIM_PART == "Head" **and** "Torso" **or** "Head"

    button.Text = "Aim Part: " .. AIM_PART

**end**)



\-- CERCLE FOV

**local** fovCircle = Instance.new("Frame")

fovCircle.Name = "FOVCircle"

fovCircle.Size = UDim2.fromOffset(FOV_RADIUS \* 2, FOV_RADIUS \* 2)

fovCircle.AnchorPoint = Vector2.new(0.5, 0.5)

fovCircle.BackgroundTransparency = 1

fovCircle.Visible = FOV_ENABLED

fovCircle.Parent = gui



**local** circleCorner = Instance.new("UICorner")

circleCorner.CornerRadius = UDim.new(1, 0)

circleCorner.Parent = fovCircle



**local** circleStroke = Instance.new("UIStroke")

circleStroke.Color = Color3.fromRGB(80, 200, 255)

circleStroke.Thickness = 2

circleStroke.Parent = fovCircle



\-- ESP : SURBRILLANCE

**local** highlights = {}



**local** **function** removeESP(targetPlayer)

    **if** highlights[targetPlayer] **then**

        highlights[targetPlayer]:Destroy()

        highlights[targetPlayer] = **nil**

    **end**

**end**



**local** **function** updateESP()

    **for** \_, targetPlayer **in** **ipairs**(Players:GetPlayers()) **do**

        **if** targetPlayer \~= player **then**

            **local** character = targetPlayer.Character



            **if** ESP_ENABLED **and** character **then**

                **local** highlight = highlights[targetPlayer]



                **if** **not** highlight **or** highlight.Parent \~= character **then**

                    removeESP(targetPlayer)



                    highlight = Instance.new("Highlight")

                    highlight.Name = "TrainingESP"

                    highlight.FillColor = Color3.fromRGB(255, 90, 90)

                    highlight.OutlineColor = Color3.fromRGB(255, 255, 255)

                    highlight.FillTransparency = 0.65

                    highlight.DepthMode = **Enum**.HighlightDepthMode.AlwaysOnTop

                    highlight.Parent = character



                    highlights[targetPlayer] = highlight

                **end**

            **else**

                removeESP(targetPlayer)

            **end**

        **end**

    **end**

**end**



**local** **function** getAimPart(character)

    **if** AIM_PART == "Head" **then**

        **return** character:FindFirstChild("Head")

    **end**



    **return** character:FindFirstChild("UpperTorso")

        **or** character:FindFirstChild("Torso")

        **or** character:FindFirstChild("HumanoidRootPart")

**end**



\-- WALL CHECK

**local** **function** hasLineOfSight(character, targetPart)

    camera = **workspace**.CurrentCamera



    **local** origin = camera.CFrame.Position

    **local** direction = targetPart.Position - origin



    **local** params = RaycastParams.new()

    params.FilterType = **Enum**.RaycastFilterType.Exclude

    params.FilterDescendantsInstances = {

        player.Character,

        camera

    }

    params.IgnoreWater = **true**



    **local** result = **workspace**:Raycast(origin, direction, params)



    **return** result \~= **nil**

        **and** result.Instance:IsDescendantOf(character)

**end**



\-- CIBLE LA PLUS PROCHE DU CENTRE

**local** **function** findTarget()

    **local** closestPlayer = **nil**

    **local** closestPart = **nil**

    **local** closestDistance = FOV_RADIUS

    **local** screenCenter = camera.ViewportSize / 2



    **for** \_, targetPlayer **in** **ipairs**(Players:GetPlayers()) **do**

        **if** targetPlayer \~= player **then**

            **local** character = targetPlayer.Character

            **local** humanoid = character

                **and** character:FindFirstChildOfClass("Humanoid")

            **local** part = character **and** getAimPart(character)



            **if** humanoid **and** humanoid.Health > 0 **and** part **then**

                **local** screenPosition, onScreen =

                    camera:WorldToViewportPoint(part.Position)



                **if** onScreen

                **and** screenPosition.Z > 0

                **and** hasLineOfSight(character, part) **then**



                    **local** point = Vector2.new(

                        screenPosition.X,

                        screenPosition.Y

                    )



                    **local** distance = (point - screenCenter).Magnitude



                    **if** distance < closestDistance **then**

                        closestDistance = distance

                        closestPlayer = targetPlayer

                        closestPart = part

                    **end**

                **end**

            **end**

        **end**

    **end**



    **return** closestPlayer, closestPart

**end**



\-- BOUCLE PRINCIPALE

RunService.RenderStepped:Connect(**function**()

    camera = **workspace**.CurrentCamera

    **if** **not** camera **then**

        **return**

    **end**



    **local** center = camera.ViewportSize / 2

    fovCircle.Position = UDim2.fromOffset(center.X, center.Y)

    fovCircle.Visible = FOV_ENABLED



    updateESP()



    **if** AIM_ENABLED **then**

        **local** targetPlayer, targetPart = findTarget()



        **if** targetPlayer **and** targetPart **then**

            camera.CFrame = CFrame.lookAt(

                camera.CFrame.Position,

                targetPart.Position

            )

        **end**

    **end**

**end**)



Players.PlayerRemoving:Connect(**function**(targetPlayer)

    removeESP(targetPlayer)

**end**)



\-- BOUTON OUVRIR / FERMER 💗

**local** toggleButton = Instance.new("TextButton")

toggleButton.Name = "TogglePanel"

toggleButton.Size = UDim2.fromOffset(120, 38)

toggleButton.Position = UDim2.new(1, -140, 1, -60)

toggleButton.BackgroundColor3 = Color3.fromRGB(48, 48, 62)

toggleButton.TextColor3 = Color3.new(1, 1, 1)

toggleButton.Text = "💗 FERMER"

toggleButton.TextSize = 14

toggleButton.Font = **Enum**.Font.GothamBold

toggleButton.Parent = gui



**local** corner = Instance.new("UICorner")

corner.CornerRadius = UDim.new(0, 8)

corner.Parent = toggleButton



toggleButton.Activated:Connect(**function**()

    panel.Visible = **not** panel.Visible



    **if** panel.Visible **then**

        toggleButton.Text = "💗 FERMER"

    **else**

        toggleButton.Text = "💗 OUVRIR"

    **end**

**end**)