---
layout: article
title: Create body rotation animation using SOLIDWORKS API
caption: Create Body Rotation Animation
description: VBA example to create a rotation animation of a selected body around Y axis using SOLIDWORKS API and temp bodies
lang: en
image: /solidworks-api/geometry/body-rotate-animation/body-rotate.gif
labels: [animation,rotate,temp body]
---
{% include img.html src="body-rotate.gif" alt="Body rotation animation" align="center" %}

This VBA example demonstrates how to create a rotation animation of a selected body in part document using SOLIDWORKS API.

There will be no additional features created in the Feature Manager tree. This macro **is not** using the SOLIDWORKS motion study. Body is rotated around Y axis at origin. Animation is created using the temp bodies and original body or feature manager tree is not affected.

Select body from the Feature Manager tree and run the macro.

{% include img.html src="feature-tree-body-selected.png" width=250 alt="Body selected in the feature manager tree" align="center" %}

Preview of the body is created and rotated until selection is cleared. When macro stops the original body is reverted to the original state.

{% include_relative Macro.vba.codesnippet %}