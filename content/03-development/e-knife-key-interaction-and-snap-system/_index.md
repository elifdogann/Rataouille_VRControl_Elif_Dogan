---
title: "Knife-Key Interaction & Snap System"
weight: 4
resources:
  - src: dev_page_5.png
    params:
      cover: true
---

## Interaction Design Goal

The parkour environment includes an object interaction sequence: the player must use a knife as an indirect manipulation tool to transport a key to a chest slot, unlock the chest, and trigger a book reveal. This is described academically as tool-based indirect manipulation — the knife is the tool, the key is the indirectly controlled object, and snapping provides system-assisted precision.

<div class="video-block">
  <video controls playsinline class="demo-video">
    <source src="grab_knife.mp4" type="video/mp4">
  </video>
</div>

## Interaction Sequence

<img src="4-_milestone_1.png" alt="Refactor" style="width: 200%; max-width: 1200px; display: block; margin: 1.25rem auto 2.5rem auto;" />

## Knife Grab — First Problems
The initial knife grab script used a button-based grab state and updated knife transform to match controller position and rotation. Two failure modes appeared:
•	The button field in the Unity Inspector was left as None — the script received no valid input and grab never activated
•	Controller side, button type (Axis1D vs Button), and object references all had to be verified independently

A second issue emerged when multiple grab scripts were active on the same object simultaneously. Transform-follow logic and physics-based grab logic competed, producing erratic behavior. The fix: one and only one attachment strategy per object.

## Physics-Based Key Attachment — Attempt and Failure

The first key attachment approach used ConfigurableJoint with spring, damper, maximum distance, grab threshold, and release threshold parameters. The intended effect was a key hanging from the knife like a fish on a hook — physically reactive and visually convincing.

Problems with this approach:
•	Key pushed away from the knife instead of attaching — the system was in collision-response mode, not attachment mode
•	Multiple attachment scripts accidentally active at once caused undefined behavior
•	ConfigurableJoint + Rigidbody constraints + trigger timing created cascading instability
•	A compile error appeared: an external script attempted to read a grab state property that was not exposed as a public readable field from the knife component
•	Chest-area colliders triggered the placement check prematurely before the key reached the slot

## Snap-Based Solution
The key handling system was redesigned around snapping. When knife tip and key met proximity/alignment conditions, the key was parented to the knife tip. LateUpdate-based transform synchronization ensured the key visually kept up with fast VR controller motion without lag artifacts.

<img src="4_milestone_2.png" alt="Refactor" style="width: 200%; max-width: 1200px; display: block; margin: 1.25rem auto 2.5rem auto;" />

## Chest Slot Validation

Trigger-based collision checks were replaced with position and rotation tolerance validation. The chest slot script compared the transported key's world position and rotation against target values within defined thresholds. Once validated:
•	Audio confirmation played
•	Chest was removed from the scene (SetActive false or Destroy)
•	Knife was optionally removed
•	A book was spawned at a dedicated empty-transform spawn point
•	Optional particle effect was triggered

<div class="video-block">
  <video controls playsinline class="demo-video">
    <source src="open_chest.mp4" type="video/mp4">
  </video>
</div>

A critical implementation note: the book spawn point must be a dedicated empty Transform — using any other object reference (such as the chest itself) caused the spawn to fail after the chest was removed.
