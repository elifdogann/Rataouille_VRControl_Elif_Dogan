---
title: "Defining the Core Control Philosophy"
weight: 1
resources:
  - src: dev_page_1.png
    params:
      cover: true
---

The first developmental axis of the project was to treat movement not as a purely mechanical
response to player input but as a design problem that defined who or what was actually being
controlled inside the scene. For that reason, the project did not begin by writing a simple avatar
controller. Instead, it first examined whether the visible human, the camera, and the controlling
influence should be interpreted as the same entity or as layered roles inside one mediated
interaction model.
At this stage a deliberate interpretation layer was placed between input and movement. High-level
meanings such as Move, Turn, Jump, and Action were not treated as raw transform changes but as
gameplay intentions that could later be translated by different systems. This allowed the project to
grow without locking itself into one device-specific movement script. Controller-driven pull
locomotion, proxy influence, and the rat-human relationship all became possible because the project
had already separated signal from action.
This definition of control changed the nature of the entire prototype. The player was no longer
framed as a floating observer who simply drives a camera through space. Instead, the player
became part of a relationship between a visible human body and an external source of influence.

That framing gave later visual, mechanical, and spatial decisions a common direction.
As a result, the core principle of the project became clear very early: reading input and applying
movement are not the same operation, and the interpretive layer between them defines the identity
of the experience. The first milestone therefore functioned not only as a conceptual starting point
but as the technical and experiential foundation of the whole project.
