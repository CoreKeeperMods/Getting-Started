# How to view game source code
Because this game was made using Il2Cpp backend, tools like DnSpy won't give you access to the source code. There are two ways to view source code: Cpp2Il and Ghidra

## Using Cpp2IL
[Cpp2Il](https://github.com/SamboyCoding/Cpp2IL) can generate .NET assemblies which can be open with DnSpy.

To generate these assemblies you need to use the tool in the command line with following options: `.\Cpp2IL.exe --game-path "<path to your core keeper installation>" --just-give-me-dlls-asap-dammit`

Note that the source code is divided into three assemblies (`Pug.Core`, `Pug.Other` and `PugWorldGen`) and to let Cpp2Il know you want to analyze them you need to use one of these:
- `--run-analysis-for-assembly <assembly name>`. When using it you will need to run the tool three times and combine the output. 
- `--analyze-all`. This option will analyze all the assemblies, but it will take much longer.

After running the tool you will see new folder `cpp2il_out`. You can now use DnSpy to read the source code.

## Using Ghidra / IDA
Using a binary reverse engineering tool we can get more precise output. You can use both Ghidra and IDA for this, but since Ghidra is free this guide will be focused on it. 

Unfortunately using these tools alone would also provide bad results. We need to add Il2Cpp metadata (Which contains information about all types, methods and fields) to the Ghidra. To do so we will use [Il2CppDumper](https://github.com/Perfare/Il2CppDumper), in your command line type:
```
Il2CppDumper.exe <executable-file> <global-metadata>
```
`GameAssembly.dll` is the executable file you need, and you will find global metadata file in `Core Keeper\CoreKeeper_Data\il2cpp_data\Metadata\`.

After the tool is done executing you should see `il2cpp.h`, `dump.cs` and `script.json` files. These files contain metadata information we need. To import it into Ghidra first open the `GameAssembly.dll` with ghidra.

**NOTE:** For best results, choose No when Ghidra asks if you would like to perform auto-analysis when the binary is first loaded. If you receive a `Conflicting data exists at address` error when running the script below, re-load the binary into the project and choose No at the auto-analysis prompt.

**NOTE:** To significantly speed up analysis for ELF files, set the image base to zero (`0x00000000`) in the load options for the binary.

To import metadata into an existing Ghidra project:

1. From the _Code Browser_, choose _File -> Parse C Source..._
2. Create a new profile and add the generated C++ type header file. This is `il2cpp.h`.
3. Ensure the _Parse Options_ are set exactly as follows:

   `-D_GHIDRA_`

4. Click Parse to Program and accept any warnings. This may take a long time to complete.
5. Open the _Script Manager_ and add the output folder of the Il2CppDumper.
6. Click Refresh to make the script appear in _Script Manager_.
7. Right-click the script and choose _Run_. The script is going to ask for `script.json` file, which you can find in the Il2CppDumper output folder. This may take a while to complete.
