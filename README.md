# UE-Clang-Tidy
Making Clang-Tidy work with UE source code.


# Progress so far

We have found:

`.\Engine\Binaries\DotNET\UnrealBuildTool\UnrealBuildTool.exe -mode=GenerateClangDatabase UnrealGame Win64 Development`

Which will generate a compile_command.json database that can be passed into Clang-tidy. However, these compile commands don't actually seem to usable for all C++ source files to actually compile the files. For example, compilation may halt with 

> error: 'HAL/IConsoleManager.h' file not found

We were able to make the compile_commands include header files by adding `InputFiles.AddRange(InputFileCollection.HeaderFiles);` to `GenerateClangDatabase.cs` and rebuilding UBT. However, even this larger `compile_commands.json` with the headers seems to not be enough for clang-tidy to proceed past the `file not found` errors.

# Running Clang-tidy

`clang-tidy -p ".\compile_commands.json"  Engine\Plugins\Media\PixelStreaming\Source\PixelStreaming\Private\Streamer.cpp --`

(this assumes a `.clang-tidy` file in the root of your project).
