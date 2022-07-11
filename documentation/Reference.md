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

Also [CoreLib](https://github.com/Jrprogrammer/CoreLib) provides an API to add new Rewired keybinds. These keybinds will be rebindable by the user.

In your plugin `Load()` method call:
```c#
RewiredKeybinds.AddKeybind("KeyBindUniqueId", "My Key Bind", KeyboardKeyCode.K);
```
Then in your MonoBehavior `Update()` method you can check the button:
```c#
public class UpdateMono : MonoBehaviour
{
    private Player player;

    private void Awake()
    {
        RewiredKeybinds.rewiredStart += OnRewiredStart;
    }

    private void OnRewiredStart()
    {
        player = ReInput.players.GetPlayer(0);
    }

    private void Update()
    {
        if (player != null)
        {
            if (player.GetButtonDown("KeyBindUniqueId"))
            {
                // Do something
            }
        }
    }
}
```

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

