# Cybershoes Quest SDK Developer Handbook for Unity
*Tested in Unity 2020.1.6f1*  
If your game is using the Oculus Utilities Plugin check out the example project at: https://github.com/CybershoesVR/Quest_SDK_Unity_Example

**Abstract:**  
*Prerequisites: Unity Input System, Unity XR Plugin Management.*  
Cybershoes example project usable in Quest development.  
Shoe input appears as gamepad input.  
The example illustrates how to get gamepad input and suggests solutions for a seated play mode.  
Testing gamepad functionality can either be done using the shoes, or if they're not available using a bluetooth gamepad (e.g., an Xbox One gamepad).

*Please note that shoe movement is always relative to the rotation of the hmd.*  
*Walking forward while looking to the side should result in strafing.*  
*All directions of movement should have the same movement speed multiplier.*  

Link to the hardware installation guide: bit.ly/302QyC0

Cybershoes Functionality Scripts:
* CybershoesInput.cs
* CybershoesHeightScaler.cs

Cybershoes Example Script:
* CybershoesManager.cs
* StaticHeightScaling.cs

Cybershoes Example Scene:
* CybershoesSampleScene.unity

## Cybershoes SDK Scripts Details
### CybershoesInput.cs
**Public function GetRotatedShoeVector():**  
Takes the forward rotation of your hmd, e.g., yourXrCamera.transform.localRotation and input from a gamepad.  
The shoes are treated as a Bluetooth gamepad by their driver software.  
Bluetooth package loss is mitigated by GetRotatedShoeVector to return the most accurate current shoe rotation.  
  
### CybershoesHeightScaler.cs
**Public function InitHeightScaler():**  
Takes the tracking device posing as the center eye component and a float that signifies how tall your player should be in a standing position in meters, e.g., yourCenterEyeDevice and 1.75f.  
Both parameters are then assigned to member variables of the HeightScaler script. Initialize the height scaler at the moment of calibrating the player height.  
You should adjust the size of your player character model, if you have one, to the target standing height of the player.  

**Public function CalculateOffset():**  
Returns the difference of your target player standing height (simulated) and the current seated user height (real).  
The goal of CalculateOffset is that the returned value may then be used to position the player camera as if the user were standing instead of being seated.  
Moving the camera leads to the chaperone being moved in the y-direction as well which leads to users not being able to pick up items off the ground.  
CalculateOffset incorporates a mitigation tactic that looks whether a user is bending down towards the ground and moves the player position in the y-direction accordingly.  
This is incorporated into the return value of CalculateOffset.  

## How to use the Example Project
The example project uses the XR-camera and input systems included with current versions of Unity.  
It is, however, possible to implement the Cybershoes specific functionality shown in this project in other camera/input setups.  
Prerequisites to implement your own Cybershoes functionality would be to have a way of getting gamepad input and being able to move the player camera freely.  
Some setups might require workarounds to be able to move the player camera in the y-direction freely.  

Open the project using UnityHub and then open the CybershoesSampleScene in Unity.  
Open the Build Settings, set the platform to Android and select ASTC in the Texture Compression menu point. Make sure that the CybershoesSampleScene is listed under Scenes In Build.  
Connect your Quest with activated developer mode to your PC and click Build And Run.  

The scene provides three cubes, all with a size of 1 meters. Two of them are stacked to create a construct that is 2 meters tall.  
  
### CybershoesManager.cs
This script implements the height scaling functionality and handles input taken from the shoes.  

The IntitHeightScaler function is invoked using a helper function in Start. This function is called with a delay of one second. The delay is necessary to initialize the height scaler successfully.   
InitHeightScaler is called using the center eye anchor of your XR-camera with a target player standing height of 1.75m.  
The CalculateOffset function of the CybershoesHeightScaler script is used in Update to apply the offset to the player camera.  
In order for height scaling to apply the correct offset each frame, the player camera gets moved into the negative y-direction of the offset added in the frame before the current one.  
Then the offset calculated in the current frame is used to move the player camera in the positive y-direction (upwards).  
To showcase the functionality of the height scaler, it is activated after 1 second, then deactivated after 30 seconds and then activated again after 40 seconds.  
DeactivateCybershoesHeightScaler shows an example of turning height scaling off.  
This is done by calling InitHeightScaler with a newly created input device and 0 to set the member variables in the CybershoesHeightScaler script to values that return 0 as the offset.    

**Private function GetCybershoesInput():**  
Example implementation of constructing a three-dimensional vector by calling the GetRotatedShoeVector function.  
GetRotatedShoeVector takes a 2D vector detailing the current state of the primary thumbstick of a gamepad and returns an adjusted two-dimensional vector of the x and y movement of the shoes.  
The 2D x value is used for the 3D x value, while the 2D y value is used for the 3D z value.  
This three-dimensional vector is then used to move the player in Update including Time.deltaTime to make movement framerate independent.  
Additionally the current rotation of the hmd is factored into the movement vector, restricted to the rotation around the y-axis.  
The example implementation uses Unity's InputSystem to query the current state of the gamepad thumbstick.
The InputSystem first had to be installed from Window -> Package Manager  
and the active input handling in Edit -> Project Settings -> Player -> Other Settings to both.  
You are, however, free to use your own solution of getting the thumbstick state and passing it to the GetRotatedShoeVector function.  

### StaticHeightScaling.cs
This script poses as an alternative to height scaling. Activate this script and deactivate the CybershoesManager script to see the effect.  
It changes the height of the camera using the transform.localPosition component to a static value on a press of the A-button on the right touch controller.  
Pressing B on the right touch controller reverts the camera height to its original value.  
We recommend using an offset-value of 0.3 or 0.4 units, which will put your player height at approximately 1.7 to 1.8 meters, assuming that the user is sitting on a Cyberchair.  
If you use player models, be aware that you have to scale them to the new height.  

CybershoesManager.cs, CybershoesHeightScaler.cs and StaticHeightScaling.cs are located on the empty Game Object called CybershoesManager.  



