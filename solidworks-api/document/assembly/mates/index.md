---
layout: article
title: Automating mates in assemblies using SOLIDWORKS API
caption: Mates
description: Collection of articles and code examples for mating components in the assembly
lang: en
image: /solidworks-api/document/assembly/mates/assembly-mating.png
labels: [assembly, mate]
order: 1
---
{% include img.html src="assembly-mating.png" height=300 alt="Mating assembly components via API" align="center" %}

Assembly components can be mated programmatically via [IAssemblyDoc::AddMate3](http://help.solidworks.com/2012/english/api/sldworksapi/SOLIDWORKS.Interop.sldworks~SOLIDWORKS.Interop.sldworks.IAssemblyDoc~AddMate3.html) SOLIDWORKS API method (or newer versions of this method).

It is required to mark the selected objects with different marks for the different mate types. Please refer the SOLIDWORKS API help documentation for the values of marks for a specific mate or use SOLIDWORKS macro recorder to capture the correct mark.