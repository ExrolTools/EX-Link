# EX-Link
EX-Link is a production ready, networked UI generation framework built specifically for Somnium Space. It handles local, global, and master locked UI synchronization, VR interactions, and automated late joiner state reconstruction.

In short: EX-Link allows you to manipulate Game Objects, Components, and Animator Parameters states via UI Toggles, Buttons or Sliders, and builds automatically UI Menu and interactable UI elements for you, which is ready to use in Somnium Space Worlds. You can create Local, Global All or Global Master (Synced or mixed) UI Menu easily. 
All this is done with a drag and drop and a few clicks. No UI building needed, no networking knowledge needed, no scripting needed. Drag, drop, assign and you are done.

## Requirements

* Unity 6.3 (6000.3.5f2)
* Unity TextMeshPro (TMP)
* Somnium ProSDK V3 (Previous versions NOT supported)


## Installation

Installation Video Link: 

## UI Element Logic & Network States

### Toggles

* **Behavior:** Forces the target object to match the exact ON/OFF state of the UI.
* **Network Memory:** Late joiners will download the current state of all toggles and update the world accordingly.

### Buttons

* **Behavior:** Inverts the current state of target boolean components natively.
* **Network Memory:** Buttons do not hold memory. Late joiners will not know how many times a button was clicked. (If a visual state must persist for late joiners, use a Toggle, not a Button.)

### Sliders

* **Behavior:** When mapped to boolean components (e.g., Renderer), sliders evaluate float values greater than `0.001f` as `true` to prevent logic breakages.
* **Network Memory:** Late joiners will download the exact float values of all sliders.

### Synchronization Types

* **Local Only:** Executes exclusively on the interacting client.
* **Network Synced (All):** Executes on all clients. Any user can interact.
* **Network Synced (Master):** Executes on all clients. UI interaction is strictly locked to the instance Master.

---

## Action Dictionary

The following actions can be wired to any UI element via the builder.

### GameObject

* Enables or disables the target GameObject hierarchy.
* Modifies the X, Y, and Z local scale of the target. (Requires Slider).

### Physics & Rendering

* Enables/disables MeshRenderers or SkinnedMeshRenderers without disabling the parent GameObject.
* Enables/disables physical collisions. (Box Collider, Sphere Collider, etc.)

### Script Logic

* Enables/disables specific MonoBehaviours or custom scripts.

### Audio (AudioSource)

* Disable or enable Audio Source Component.
* Fires a one-shot audio clip.
* Toggles the mute state of the AudioSource.
* Drives the 0.0 to 1.0 volume parameter. (Requires Slider).

### Lighting (Light)

* Disable or enable Light Components.
* Modifies the emission strength of the light.
* Modifies the distance of the light. (Point Light)

### Visual Effects (ParticleSystem)

* Flips between Play and Stop/Clear based on input state.

### Animation (Animator)

* Flips a true/false parameter in an Animator Controller. Requires string parameter name.
* Fires an instantaneous trigger. Requires string parameter name.
* Drives a float parameter or blend tree. Requires string parameter name. 
