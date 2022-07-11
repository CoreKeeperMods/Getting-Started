# First Steps
The best way to get started with modding Core Keeper is to follow the guides from a few crucial libraries and tools.
This section provides a background to the libraries/tools and links to the guides. 

# Libraries

## BepInEx
BepInEx is a plugin framework for Unity and .NET framework games. We use BepInEx to load the mods into the game. 
It provides a good guide to get started with BepInEx and by the end of the guide you should have made your very first working mod!

Be sure to select the IL2CPP code examples when you're creating your mod!

**Link:** [https://docs.bepinex.dev/master/articles/user_guide/installation/index.html](https://docs.bepinex.dev/master/articles/user_guide/installation/index.html)

**Download the latest IL2CPP build here**: [https://builds.bepinex.dev/projects/bepinex_be](https://builds.bepinex.dev/projects/bepinex_be)

## Harmony
Harmony is a library for patching, replacing and decorating existing classes and methods in the game. 
The patching is done in runtime, which means you don't have to change the existing code!
We use harmony to get a hold of manager instances and other important game related classes and values.

It is recommended to read through the Introduction, basics, patching and annotations chapters

**Link:** [https://harmony.pardeike.net/articles/intro.html](https://harmony.pardeike.net/articles/intro.html)

# Tools

## dnSpy
dnSpy is a way to get a look into the code of the game. Because Core Keeper is made with the IL2CPP backend, it is recommended to first run BepInEx to generate the assemblies. The assemblies can be found in the `Core Keeper\BepInEx\unhollowed` folder. 

Another way to generate assemblies is to use IL2CPPDumper. 
The executable can be found at https://github.com/Perfare/Il2CppDumper and needs to be run through the command line.
A separate page will go into more detail on how to generate the assemblies.

**Important!** If you look for a dnSpy tutorial, most will tell you to look for Assembly-CSharp.dll. This is **NOT** the case with Core Keeper and the relevant DLLs all begin with `Pug`

**Link:** [https://github.com/dnSpy/dnSpy](https://github.com/dnSpy/dnSpy)

## UnityExplorer
UnityExplorer is a BepInEx plugin that lets you see the game structure in the game itself. It's a good tool to the games structure and get into the nitty gritty of the code.

**Link:** [https://github.com/sinai-dev/UnityExplorer](https://github.com/sinai-dev/UnityExplorer)