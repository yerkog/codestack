---
layout: article
title: Call functions of SOLIDWORKS add-in via Running Objects Table (ROT)
caption: Via Running Objects Table (ROT)
description: Calling SOLIDWORKS add-in functions (add-in API) by registering an API object in the Running Object Table (ROT)
lang: en
image: /solidworks-api/getting-started/inter-process-communication/invoke-add-in-functions/via-rot/stand-alone-references.png
labels: [add-in api,stand-alone,rot]
order: 2
---
This articles provides detailed step-by-step instructions of how to call the functions of the SOLIDWORKS add-in from stand-alone applications or scripts via Running Object Table (ROT). This could be considered as enabling the Application Programming Interface (API) in the add-in itself.

This approach allows to connect to API Object by extracting it from the Running Object Table (ROT) by process id. One of the main benefits of this approach is an ability to control add-in API without directly using any SOLIDWORKS API commands or even adding SOLIDWORKS type libraries or interops.

In this example SOLIDWORKS add-in allows to count faces of the selected body. It provides a menu for user to click.

{% include img.html src="add-in-menu-command.png" width=350 alt="Add-in menu with commands" align="center" %}

The result is displayed in the message box.

{% include img.html src="add-in-command-result-message.png" width=450 alt="Add-in command result displayed in the message box" align="center" %}

Add-in also provides an API object for 3rd parties to call. API extends the functionality of the UI and allows to pass the parameter to filter the minimum area of the faces.

Both API method and User Interface command handler are calling the same function.

This is a solution tree of the add-in and API object. The GeometryHelperApiObject compiles to dll and contains the definitions of add-in interfaces. It doesn't contain any implementation and is not referencing any SOLIDWORKS interops. This dll is referenced in the SOLIDWORKS add-in project which implements the API object and will be also referenced by all third party applications to access the API. So none of the third party applications need to reference the main add-in dll which enables the level of abstractions.

{% include img.html src="solution-tree.png" width=450 alt="Visual Studio Solution tree for project" align="center" %}

Explore the source code and explanation below for more details of how to implement this framework.

## GeometryHelperApiObject Project

This project contains the definitions (signatures) of API methods and interfaces which should be exposed by the add-in.

### GeometryHelperApiObjectFactory.cs

This is a helper COM object which will simplify the access to the instance of the add-in API object. It is responsible for retrieving the instance of API object from the process by its ID.

{% include_relative api-object-interface/GeometryHelperApiObjectFactory.cs.codesnippet %}

### IGeometryHelperApiObject.cs

This is an interface of the API exposed to 3rd party applications. In this example the function will return the number of faces based on the area filter.

{% include_relative api-object-interface/IGeometryHelperApiObject.cs.codesnippet %}

## GeometryHelperAddIn Project

This project is a SOLIDWORKS add-in. In this example it is developed using the [SwEx.AddIn Framework]({{ "/labs/solidworks/swex/add-in/" | relative_url }}), but any other frameworks or SDK would be supported.

### GeometryHelperService.cs

Helper class within add-in which is invoking SOLIDWORKS API for calculating the number of faces from the selected body based on the minimum area.

{% include_relative rot-api-object/GeometryHelperService.cs.codesnippet %}

Add-in must implement the functionality of API object. There are 2 general approaches for this.

* By directly implementing the functionality in the inherited class as shown below

### GeometryHelperApiObject.cs

API object implementation is using the SOLIDWORKS API directly within its class to handle the request and provide response to API call.

{% include_relative rot-api-object/GeometryHelperApiObject.cs.codesnippet %}

### MainAddIn.cs

This is the implementation of main SOLIDWORKS add-in class. The service to handle the functionality is passed directly to API object implementation and called from there to handle the API call.

{% include_relative rot-api-object/MainAddIn.cs.codesnippet %}

* By implementing the proxy API object. This approach may be considered more beneficial and secure as it doesn't expose any internal objects in its structure. All of the requests are handled outside of the proxy class.

### GeometryHelperApiObjectProxy.cs

Proxy object doesn't contain any references to any objects of the add-in. Instead it will generate the request event, handled and processed by the add-in.

{% include_relative rot-api-object/GeometryHelperApiObjectProxy.cs.codesnippet %}

### MainAddIn.cs with proxy API object

Handling of the event in the add-in and providing results.

{% include_relative rot-api-object/MainAddInProxyApiObject.cs.codesnippet %}

### RotHelper.cs

In order for the API object to be available it is required to register it in the Running Objects Table (ROT). This helper class allows to register the object by name.

{% include_relative rot-api-object/RotHelper.cs.codesnippet %}

## Calling the API from stand-alone applications

Now the add-in API can be called form any COM-compatible programming language.

### VBA Macro

Example of calling the API from the Excel VBA macro.

{% include img.html src="excel-vba-macro.png" width=450 alt="Macro to call SOLIDWORKS add-in API from Excel" align="center" %}

Add the reference to Type Library to enable [early binding]({{ "/visual-basic/variables/declaration#early-binding-and-late-binding" | relative_url }}) for the calls.

{% include img.html src="macro-browse-type-library.png" width=450 alt="Browsing type library for COM applications" align="center" %}

Macro creates and instance of Factory object by its prog id and retrieves the instance of the API object by process id.

{% include_relative excel-macro.vba.codesnippet %}

### C# Console Application

Example of calling the API from the C# Console application.

As shown below it is only required to add the reference to the API Object dll which contain the definitions of interfaces. There is no need to add any other references (including add-in dll or any SOLIDWORKS interops)

{% include img.html src="stand-alone-references.png" width=450 alt="References to stand-alone application" align="center" %}

The result of the API call is printed into the console window.

{% include img.html src="command-line-result.png" width=450 alt="Command result displayed in the console" align="center" %}

{% include_relative stand-alone/Program.cs.codesnippet %}

> The most common reason of object cannot be retrieved from the Running Object Table (i.e. null is returned) is difference in accounts levels running SOLIDWORKS and the stand-alone application. For example SOLIDWORKS is run as administrator while the stand-alone is not or vice-versa. This is a Windows limitation and it is required to run both applications on the same permissions level to enable the communication.

Download the source code at [GitHub](https://github.com/codestackdev/solidworks-api-examples/tree/master/swex/add-in/geometry-helper-api-rot)