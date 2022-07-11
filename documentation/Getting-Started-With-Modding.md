# First Steps
The best way to get started with modding Core Keeper is to follow the guides from a few crucial libraries and tools.
This section provides a background to the libraries/tools and links to the guides. 

# Libraries

## BepInEx
BepInEx is a plugin framework for Unity and .NET framework games. We use BepInEx to load the mods into the game. 
It provides a good guide to get started with BepInEx and by the end of the guide you should have made your very first working mod!

Be sure to select the IL2CPP code examples when you're creating your mod!

**Link:** https://docs.bepinex.dev/master/articles/user_guide/installation/index.html

**Download the latest IL2CPP build here**: https://builds.bepinex.dev/projects/bepinex_be

## Harmony
Harmony is a library for patching, replacing and decorating existing classes and methods in the game. 
The patching is done in runtime, which means you don't have to change the existing code!
We use harmony to run our code before or after game methods. This allows us to modify game behavior.

It is recommended to read through the Introduction, basics, patching and annotations chapters. Also please note that transpilers are not apllicable with Il2Cpp

**Link:** https://harmony.pardeike.net/articles/intro.html

# Tools

## DnSpy
DnSpy is a tool that allows to read .NET assemblies code with ease. Unfortunately Core Keeper is made with the IL2CPP backend. This means we can't use DnSpy itself to view the source code. However it can be useful to view asseblies generated with Cpp2Il.

**Important!** If you look for a dnSpy tutorial, most will tell you to look for Assembly-CSharp.dll. This is **NOT** the case with Core Keeper and the relevant DLLs all begin with `Pug` (`Pug.Core`, `Pug.Other` and `PugWorldGen`)

**Link:** https://github.com/dnSpy/dnSpy

## Cpp2Il
Cpp2Il is a tool that attempts to reconstruct game assemblies, such that DnSpy would be able to read them. The tool is currently WIP and does not provide full and accurate output, however it is still useful.

**Link:** https://github.com/SamboyCoding/Cpp2IL

## Ghidra
Ghidra is a free and open source reverse engineering tool developed by the National Security Agency (NSA) of the United States. This tool allows us to read the `GameAssembly.dll` directly. This assembly contains compiled machine game code. To make it easier to find relevant code we can add Il2Cpp metadata. To do so we can use [Il2CppInspector](https://github.com/djkaty/Il2CppInspector) or [Il2CppDumper](https://github.com/Perfare/Il2CppDumper)

**Link:** https://github.com/NationalSecurityAgency/ghidra/releases

## UnityExplorer
UnityExplorer is a BepInEx plugin that lets you inspect game objects and classes at the runtime. The main use for this tool is to figure out how to change something or check if your code worked correctly. It also has a C# REPL console which allows to execute code right in the game.

**Link:** https://github.com/sinai-dev/UnityExplorer
