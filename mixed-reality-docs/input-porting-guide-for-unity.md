---
title: Input porting guide for Unity
description: 
author: 
ms.author: alexturn
ms.date: 2/28/2018
ms.topic: article
keywords: 
---



# Input porting guide for Unity

You can port your input logic to Windows Mixed Reality using one of two approaches, Unity's general Input.GetButton/GetAxis APIs that span across multiple platforms, or the Windows-specific XR.WSA.Input APIs that offer richer data specifically for motion controllers and HoloLens hands.
> [!NOTE]
> 


This article has been updated for the final shipping Unity 2017.2 API shapes:
* If you are using Unity 5.6, you will see an older version of these APIs under the UnityEngine.VR namespace rather than UnityEngine.XR. Beyond the namespace change, there are other minor breaking API changes between Unity 5.6 and Unity 2017.2 that Unity's script updater will fix for you when moving to 2017.2.
* If you are using an earlier beta build of Unity 2017.2, you will see these APIs under UnityEngine.XR as expected, but you may see some differences from what is described below, as the initial 2017.2 beta builds contain an older version of the API shape.

## General Input.GetButton/GetAxis APIs

Unity currently uses its general Input.GetButton/Input.GetAxis APIs to expose input for [the Oculus SDK](https://docs.unity3d.com/Manual/OculusControllers.html) and [the OpenVR SDK](https://docs.unity3d.com/Manual/OpenVRControllers.html). If your apps are already using these APIs for input, this is the easiest path for supporting motion controllers in Windows Mixed Reality: you should just need to remap buttons and axes in the Input Manager.

For more information, see the [Unity button/axis mapping table](gestures-and-motion-controllers-in-unity.md#unity-buttonaxis-mapping-table) and the [overview of the common Unity APIs](gestures-and-motion-controllers-in-unity.md#common-unity-apis-inputgetbuttongetaxis).

## Windows-specific XR.WSA.Input APIs

If your app already builds custom input logic for each platform, you can choose to use the Windows-specific spatial input APIs under the **UnityEngine.XR.WSA.Input** namespace. This lets you access additional information, such as position accuracy or the source kind, letting you tell hands and controllers apart on HoloLens.

For more information, see the [overview of the UnityEngine.XR.WSA.Input APIs](gestures-and-motion-controllers-in-unity.md#windows-specific-apis-xrwsainput).

## Grip pose vs. pointing pose

Windows Mixed Reality supports motion controllers in a variety of form factors, with each controller's design differing in its relationship between the user's hand position and the natural "forward" direction that apps should use for pointing when rendering the controller.

To better represent these controllers, there are two kinds of poses you can investigate for each interaction source:
* The **grip pose**, representing the location of either the palm of a hand detected by a HoloLens, or the palm holding a motion controller.
* On immersive headsets, this pose is best used to render **the user's hand** or **an object held in the user's hand**, such as a sword or gun.
* The **grip position**: The palm centroid when holding the controller naturally, adjusted left or right to center the position within the grip.
* The **grip orientation's Right axis**: When you completely open your hand to form a flat 5-finger pose, the ray that is normal to your palm (forward from left palm, backward from right palm)
* The **grip orientation's Forward axis**: When you close your hand partially (as if holding the controller), the ray that points "forward" through the tube formed by your non-thumb fingers.
* The **grip orientation's Up axis**: The Up axis implied by the Right and Forward definitions.
* You can access the grip pose through either Unity's cross-vendor input API (**[XR.InputTracking](https://docs.unity3d.com/2017.2/Documentation/ScriptReference/XR.InputTracking.html).GetLocalPosition/Rotation**) or through the Windows-specific API (**sourceState.sourcePose.TryGetPosition/Rotation**, requesting the Grip pose).
* The **pointer pose**, representing the tip of the controller pointing forward.
* This pose is best used to raycast when **pointing at UI** when you are rendering the controller model itself.
* Currently, the pointer pose is available only through the Windows-specific API (**sourceState.sourcePose.TryGetPosition/Rotation**, requesting the Pointer pose).

These pose coordinates are all expressed in Unity world coordinates.

## See also
* [Motion controllers](motion-controllers.md)
* [Gestures and motion controllers in Unity](gestures-and-motion-controllers-in-unity.md)
* [UnityEngine.XR.WSA.Input](https://docs.unity3d.com/2017.2/Documentation/ScriptReference/XR.WSA.Input.InteractionManager.html)
* [UnityEngine.XR.InputTracking](https://docs.unity3d.com/2017.2/Documentation/ScriptReference/XR.InputTracking.html)
* [Porting guides](porting-guides.md)