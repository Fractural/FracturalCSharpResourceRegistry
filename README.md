> [!Warning]
> This addon is deprecated now that Godot supports adding C# classes through the [`[global]`](https://docs.godotengine.org/en/stable/tutorials/scripting/c_sharp/c_sharp_global_classes.html) attribute.


# FracturalCSharpResourceRegistry

![Deploy](https://github.com/Fractural/FracturalCSharpResourceRegistry/actions/workflows/deploy.yml/badge.svg) ![Unit Tests](https://github.com/Fractural/FracturalCSharpResourceRegistry/actions/workflows/tests.yml/badge.svg)

This is a Godot C# plugin that registers custom C# resources and custom nodes for Godot. This plugin serves as a workaround for the Godot engine's [C# resource missing in context menu issue](https://github.com/godotengine/godot/issues/27470).

Based off of *CustomResourceRegisterPlugin* made by [wmigor](https://github.com/wmigor/godot-mono-custom-resource-register)

## How to Use 

### Adding custom C# resources/nodes:

1. Add the [**RegisteredTypeAttribute**](#registeredtypeattribute) to your resource/node class. The file containing your class must have the same name as the class in order to be detected by the plugin. Make sure to add `using MonoCustomResourceRegistry;` to the top of your file to import this plugin's namespace, which contains `RegisteredTypeAttribute`.
2. Make sure your C# file is under one of the [**Resource Script Directories**](#settings)
3. Rebuild the solution
4. Press the **"CRR"** button to update the registered types

### Deleting custom C# resources/nodes:

1. Delete the C# resource/node script
2. Rebuild the solution
3. Press the **"CRR"** button to update the registered types

Anytime the Plugin registers/unregisters a resource/node, the plugin will print its actions into the **Output** window.

## RegisteredTypeAttribute

```C#
[RegisteredType(string name, string iconPath = "", string baseType = ""))]
```

- **name** - Name of the custom type.
- **iconPath** (Optional) - File path to the icon displayed for the custom type. Leave empty for no custom icon.
- **baseType** (Optional) - Name of the base type. Leave empty for the default base type (**"Node"** for custom nodes and **"Resource"** for custom resources).

Sample usage:
```C#
// Inside a file named CustomNodeDemo.cs
using MonoCustomResourceRegistry;

// Registers a custom type with 
// 	a name of "CustomNodeDemo",
//	an icon located at "res://custom_icon.png",
//	and a base type of "Node2D"
[RegisteredType(nameof(CustomNodeDemo), "res://custom_icon.png", nameof(Node2D))]
public class CustomNodeDemo : Node2D
{
	...
}
```
```C#
// Inside a file named CustomNodeDemo2.cs
using MonoCustomResourceRegistry;

// Registers a custom type with 
// 	a name of "CustomNodeDemo",
//	no icon,
//	and a base type of "Button"
[RegisteredType(nameof(CustomNodeDemo), "", nameof(Button))]
public class CustomNodeDemo2 : Button
{
	...
}
```
```C#
// Inside a file named CustomNodeDemo3.cs
using MonoCustomResourceRegistry;

// Registers a custom type with 
// 	a name of "CustomNodeDemo",
//	no icon,
//	and a default base type of "Resource"
[RegisteredType(nameof(CustomNodeDemo))]
public class CustomNodeDemo3 : Resource
{
	...
}
```

## Settings

This plugin comes with some settings to configure how C# resources are loaded.
The settings can be accessed by going to **Project > ProjectSettings > General > Mono Custom Resource Registry**.

All settings are listed below:

**Class Prefix** - The prefix that is seen before all custom nodes and resources.

**Resource Script Directories** - The paths to the directories where you want to scan for C# resource scripts to register as custom resources. By default, it only contains **"res://"**. 

**Search Type** - The method used to gather custom C# resource scripts.

- **Namespace** - Looks for scripts by using their namespace as a directory. 
	
	For example with the C# script below, the plugin will look under each resource script directory for the script by using the path **"./Some/Long/Namespace/Something.cs"**.

	```C#
	namespace Some.Long.Namespace
	{
		public class Something : Resource
		{
		}
	}
	```

- **Recursive** - Looks for scripts by searching through all "Resource Script Directories" and the directories within them.
