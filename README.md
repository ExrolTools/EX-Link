# EX-Link
EX-Link is a production ready, networked UI generation framework built specifically for Somnium Space. It handles local, global, and master locked UI synchronization, VR interactions, and automated late joiner state reconstruction.

In short: EX-Link allows you to manipulate Game Objects, Components, and Animator Parameters states via UI Toggles, Buttons or Sliders, and builds automatically UI Menu and interactable UI elements for you, which is ready to use in Somnium Space Worlds. You can create Local, Global All or Global Master (Synced or mixed) UI Menu easily. 
All this is done with a drag and drop and a few clicks. No UI building needed, no networking knowledge needed, no scripting needed. Drag, drop, assign and you are done.

## Requirements

* Unity 6.3 (6000.3.5f2)
* Unity TextMeshPro (TMP)
* Somnium ProSDK V3 (Previous versions NOT supported)


## Installation

Installation Video Link: https://youtu.be/jJzBffOsgA0

## Getting started with EX-Link

Once you place the EX-Link prefab into your scene, the very first step is to position it where you want the menu to appear in your world.

After that, open the **Adjust UI Visuals** tab.
This will temporarily spawn a dummy Toggle, Slider, and Button for preview purposes.

From there, you can fully customize the global appearance of your UI using the provided color palettes and visual settings. These settings affect every toggle, slider, and button globally by default.

If you want specific elements to have unique visuals (for example one toggle green, another red, another blue), you can do so individually after creating them by using the **Icons & Color Override** section inside each specific element. This allows you to override colors only for that particular toggle, slider, or button.

You can also assign custom icons individually to any toggle, slider, or button.
Keep in mind that all images must be imported as **Sprites** beforehand.

While still inside **Adjust UI Visuals**, you can also:

* Change the background sprite
* Adjust background color and opacity
* Enable or disable outlines
* Customize outline color and transparency
* Change the UI title
* Swap fonts using TMP font assets

To preview different UI states while editing visuals, use the included preview testing options. These allow you to:

* Preview Toggle ON/OFF states
* Test Button press animations
* Adjust Slider values live

---

# Creating Toggles, Sliders & Buttons

Once visuals are ready, move to the **Create Toggles & Logic** tab.

Here you can create:

* Toggles
* Sliders
* Buttons

All three follow the same workflow:

1. Create the element
2. Click **“Add Action”**
3. Drag and drop a GameObject into the action field
4. EX-Link will automatically detect supported components on that object
5. Select the desired action from the dropdown menu

Example:

* `GameObject -> Toggle`
* `AudioSource -> Volume`
* `Light -> Intensity`

You can assign multiple actions to a single toggle, slider, or button.

---

# Networking & Sync Types

Each element supports different sync modes.

## Local Only

The interaction affects only the local player.

## Network Synced

The interaction is synchronized across players.

## Master Only

Only the world master can trigger the interaction globally.

If you want every player to interact with an element and affect everyone else, use:

* `Network Synced`
* Leave `Master Only` disabled

If you want only the master to control it globally:

* Enable `Master Only`

This system works for toggles, sliders, and buttons.

---

# Toggle Options

## Default ON

Determines whether the toggle starts enabled or disabled when the world loads.

The connected object/component will automatically match this default state.

---

# Slider Options

## Start Position

Controls the starting slider value (`0 - 1` by default).

Useful for:

* Animator float control
* Audio volume
* Video players
* Shader properties

## Custom Limits

Allows you to override the default slider range.

Example:

* `0.002 -> 0.06`
* `-10 -> 10`

## Whole Number

Available when Custom Limits is enabled.

Allows the slider to use integer values only.

Useful for:

* Light intensity
* Object scaling
* Counters

---

# Managing Elements

Every toggle, slider, and button can be:

* Renamed
* Duplicated
* Deleted
* Reordered
* Expanded with multiple actions

Actions themselves can also be removed individually at any time.

---

# UI Layout & Organization

Once your logic setup is complete, you can adjust:

## Max Items Per Column

(Default: `5`)

After the limit is reached, EX-Link automatically creates a new column.

You can freely increase or decrease this number depending on your layout preferences.

## UI Scale

Safely scales the entire menu.

Important:
Do NOT scale the prefab GameObject directly.
Always use the built-in UI Scale option instead.

---

# UI Element Arrangement

The **UI Elements Arrangement** tab allows you to reorder all created elements.

This is useful for organizing layouts such as:

* Sliders in one column
* Toggles in another
* Buttons grouped separately

This helps avoid messy or difficult-to-read menus.

---

# IMPORTANT — Backup UI System

After baking the UI:

DO NOT delete the Backup UI.

The Backup UI is required if you want to continue editing the menu later.

## Understanding the System

### Baked UI

The finalized version used inside your uploaded Somnium world.

### Backup UI

The editable working version used inside Unity.

If you need to make changes later:

1. Delete the baked UI
2. Re-enable the backup UI
3. Continue editing safely
4. Bake again

The Backup UI must always remain inside the scene.

Important:
Do NOT convert the Backup UI into a prefab asset inside the Assets folder.

Doing so will break scene references and you will lose all hooked-up actions and object connections.


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

## Customizations

Users can easily edit the following elements within the EX-Link UI:

* Font
* UI Title
* UI Background (Color & Alpha)
* UI Background Outline (Color Alpha)
* UI Background Image
* Toggle Backgound
* Toggle ON state Color
* Toggle OFF state Color
* Toggle ON Text Color
* Toggle OFF Text Color
* Button Background Color
* Button Color
* Button Pressed Color
* Button Text Color
* Button Text Pressed Color
* Slider Track Color
* Slider Fill Color
* Slider Text Color
* Icon for Toggles
* Icon for Buttons
* Icon for Sliders (Left and Right icons can be included)

 You can further edit sprites of the Toggle, Sliders and Buttons prefabs located at EX-Link -> Components if you know what you are doing. However doing so will likely break the visuals of the UI elements.
