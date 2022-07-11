This page is intended to serve as a reference while no other tutorials have been made.
It is basically a pastebin.

# Getting the main manager instance
Getting the main manager instance opens a lot of doors to the rest of the game's functionality!
Almost everything related to the game is stored in this object instance. 

Basic patch:

```c#
 [HarmonyPatch(typeof(Manager))]
 internal class ManagerPatch
 {   
     [HarmonyPatch("Awake")]
     [HarmonyPostfix]
     static void ManagerUpdatePatch(Manager __instance) {
         // Your code here, use __instance to get access to the Manager Instance
     }
}
```


# Ensuring the game is in its main loop

Use this boolean to see if the game is in its main loop: 
`Manager.currentSceneHandler.isInGame`

# Listening to Input
The normal Unity Input system works so it's possible to assign functionalities to keys that haven't been defined by the game itself. 
A `Input.GetKeyDown()` in an `Update` method should be enough. 

Core Keeper also has its own InputManager which handles some actions inside the game. Be sure not unintentionally block vanilla actions through your own code. 

# Changing a recipe
It is possible to change the recipe of an item by changing the `requiredObjectsToCraft` value.

```c#
foreach (var objectType in PugDatabase.objectsByType) {
    if (ot.Value.objectID == ObjectID.CopperMiningPick) { // ObjectID can be anything!
        List<CraftingObject> newList = new List<CraftingObject>(); // Create a new list of Crafting Objects

        CraftingObject newCO = new CraftingObject(); Create a new Crafting Object
        newCO.objectID = ObjectID.Wood; // Set the ObjectID to wood (can be anything)
        newCO.amount = 1; // Set the required amount
        newList.Add(newCO); // Add the requirement to the recipe

        ot.Value.requiredObjectsToCraft = newList; // Set the new recipe
    }
}
```

# Running MonoBehaviour classes through IL2CPP

BepInEx allows to run normal MonoBehaviour classes through the IL2CPP BasePlugin.

The way it is set up is through an `AddComponent<T>()` statement in the `Load()` method. `<T>` is your custom `MonoBehaviour` class.

This is handy for when you need to run code every frame without patching into an existing class' lifecycle. 

```c#
public class Plugin : BasePlugin {
    public override void Load() {
        AddComponent<CustomMono>();
    }
}

public class CustomMono : MonoBehaviour {
    public CustomMono(IntPtr handle) : base(handle) { }

    private void Update() {
        CoreLib.Logger.LogInfo("Every update"); // This will run on every frame!
    }
}
```

