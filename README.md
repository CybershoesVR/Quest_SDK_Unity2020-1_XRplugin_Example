# XR Toolkit. Cybershoes is added in:
## Action Based Continuous Move Provider
Recommended: Make a new Action Based Continuous Move Provider if you want to handle Cybershoes separately.
It is irrelevant whether your other Continuous Move Provider is device or action based.
Don't use the CybershoesManager.cs

Goto Left Hand Move Action  
Uncheck User Reference  
In Action click on "+"   
Then doubleclick "Path"  
Select  "Gamepad" > LeftStick  

![bindings gamepad00](https://user-images.githubusercontent.com/42228867/143268832-7e3691be-63b4-4e6c-abf8-4b473457c0b2.JPG)
![bindings gamepad1](https://user-images.githubusercontent.com/42228867/143266016-cf910a1e-8a90-4a97-95e0-1bbdf94b18bc.JPG)
![bindings gamepad2](https://user-images.githubusercontent.com/42228867/143266022-199e618b-b00b-42b4-a631-bb822c3659da.JPG)
Done

Extra Option: The Path "Gamepad" > LeftStickPress yields a single event when the Cybershoes are moved very fast. This can be used to trigger a Sprint Mode - comparable to how you'd use the Oculus Touch Joystick Button Press to start Sprint Mode. Cybershoes end the SprintMode when their speed drops below 0.5. Therefore your gamelogic should end SprintMode when Cybershoes Speed drops below 0.5.

CybershoesManager.cs is not neded. To Do: fix this line  
`cybershoesScaler = GetComponent<CybershoesHeightScaler>();`

