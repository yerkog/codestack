---
layout: sw-tool
title: Macro to move suppressed mates into feature folder using SOLIDWORKS API
caption: Move Suppressed Mates Into A Folder
description: VBA macro to move suppressed mates in assembly into feature folder using SOLIDWORKS API
lang: en
image: /solidworks-api/document/assembly/mates/move-suppressed-to-folder/move-mates-to-folder.png
labels: [mates,suppressed,move,folder]
categories: sw-tools
group: Assembly
---
{% include img.html src="suppressed-solidworks-mates.png" width=250 alt="Suppressed mates moved to the folder" align="center" %}

This VBA macro allows to move all suppressed mates to a nominated feature manager folder using SOLIDWORKS API. Macro will create folder if it doesn't exist or move to already existing one.

Macro will also move all unsuppressed mates of the folder if exist.

To configure the folder name, change the value of the *FOLDER_NAME* variable:

~~~ vb
Const FOLDER_NAME As String = "<Folder Name>"
~~~

{% include_relative Macro.vba.codesnippet %}