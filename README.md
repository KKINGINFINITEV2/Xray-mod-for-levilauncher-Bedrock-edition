
Bedrock X-Ray Scanner
A native, client-side X-Ray scanner for Minecraft Bedrock Edition (GDK), built for LeviLamina. It scans chunks already loaded by the client and draws ore-specific colored highlights through terrain.
<p align="center">
  <img width="1909" height="997" alt="A4D861E1-CD0D-4066-9E0E-8EE98C25E4FA" src="https://github.com/user-attachments/assets/fa355ce2-01eb-4d30-8077-be8651ab9093" />
</p><p align="center">
  <strong>Minecraft 1.26.60.05 (GDK)</strong> · <strong>LeviLamina 26.60.7</strong> · <strong>Windows x64</strong>
</p>Features
- Through-wall ore highlights — detected target blocks are highlighted even when covered by terrain.
- Ore-specific colors — different target ores use different highlight colors.
- Client-side scanning — inspects chunks that Minecraft has already loaded on the client.
- Configurable scan radius — adjust the horizontal search radius in-game.
- Configurable vertical range — control how far above and below the player to scan.
- Scan limits and interval controls — tune the amount of work performed per scan and the scan cadence.
- In-game commands — enable, disable, inspect, and configure the scanner without editing files.
- Persistent configuration — scanner settings are saved between launches.
- Desktop toggle — press F6 to toggle the overlay.
Scope note: The scanner does not request, generate, or download unexplored chunks. It can only inspect block data available in chunks already loaded by the local game client. It is a client-side visual tool; it does not modify blocks or server data.

Compatibility
Component	Supported target
Minecraft	Bedrock 1.26.60.05 (GDK)
Loader	LeviLamina 26.60.7
Platform	Windows x64
Build system	xmake
Language	C++


The Windows native build is the supported release target documented here. The Android/Preloader work is a separate port and is not covered by this Windows build guide.
Controls
Input	Action
F6	Toggle X-Ray highlights
/xray on	Enable the scanner
/xray off	Disable the scanner
/xray	Toggle the scanner


Commands
Use /xray help in-game to display command help.
Command	Description
/xray	Toggle the scanner
/xray on	Enable highlights
/xray off	Disable highlights
/xray status	Display current scanner settings/state
/xray radius <blocks>	Set horizontal scan radius (supported range: 4–48)
/xray vertical <blocks>	Set vertical scan range
/xray interval <milliseconds>	Set scan interval
/xray limit <count>	Set the maximum number of scan results/markers
/xray help	Display available commands


Example configuration:
/xray on
/xray radius 32
/xray vertical 256
/xray interval 1800
/xray limit 2048
/xray status
The exact scan workload depends on the chosen radius, vertical range, interval, and result limit. Larger settings can increase CPU usage, so increase them gradually.
Installation
1. Install the matching Minecraft Bedrock GDK instance and LeviLamina 26.60.7.
2. Download or build the BedrockXRayScanner native mod package.
3. Close Minecraft before replacing the mod files.
4. Copy the contents of the generated bin/BedrockXRayScanner directory into the instance's mods/BedrockXRayScanner directory.
5. Launch the matching instance. Check the LeviLamina log for BedrockXRayScanner loaded and BedrockXRayScanner enabled.
6. Join a world and press F6, or use /xray on.
Typical LeviLauncher mod path:
%LOCALAPPDATA%\LeviLauncher\Instances\1.26.60.05\mods\BedrockXRayScanner\
The mod folder should contain at least:
BedrockXRayScanner/
├── manifest.json
└── BedrockXRayScanner.dll
Building from source
Requirements
- Windows x64
- Visual Studio with Desktop development with C++
- MSVC x64 build tools and a Windows SDK
- LLVM/Clang (clang-cl)
- xmake
- LeviLamina 26.60.7 dependency (resolved by the project build configuration)
Open Visual Studio Developer PowerShell and run these commands from the project root:
xmake f -c -y -p windows -a x64 -m release --target_type=client
xmake -y
The built mod is generated under:
bin/BedrockXRayScanner/
Before installing a rebuilt DLL, fully close Minecraft so Windows releases the loaded library.
Configuration
The scanner creates and loads its configuration when the mod starts. In-game /xray commands update supported settings without requiring a rebuild. Use /xray status to inspect the current settings and /xray help for available commands.
Troubleshooting
The mod does not appear in the log
- Confirm that the mod folder is inside the active LeviLauncher instance's mods directory.
- Confirm that the instance is using the matching Bedrock and LeviLamina versions.
- Check logs/latest.log for BedrockXRayScanner, Failed to load, or Failed to enable.
LeviLamina rejects the DLL for unified memory allocation
Ensure the project includes the unified memory-operator translation unit required by LeviLamina, and rebuild the DLL before reinstalling it.
No ores are highlighted
- Make sure the scanner is enabled with F6 or /xray on.
- Test near target ores in chunks that have already loaded.
- Check /xray status and increase the radius or vertical range if appropriate.
- Remember that unloaded terrain is not scanned or fetched by this client-side mod.
The DLL cannot be overwritten
Close Minecraft and ensure the game process has exited before copying the rebuilt DLL into the instance.
Showcase
The screenshot above is from the running scanner and demonstrates multiple ore-specific highlight colors in the world.
Development
This project is developed as a standalone LeviLamina native client mod. It does not bundle the rest of another Minecraft client or require a separate client framework.

Contributions, bug reports, and compatibility reports should include the Minecraft version, LeviLamina version, relevant latest.log output, and steps to reproduce the issue.

but im lazy so ill add it later

