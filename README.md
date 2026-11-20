
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

Bedrock X-Ray Scanner — Android
Current mobile release: 0.2.2
Bedrock X-Ray Scanner is a native Android mod for LeviLauncher. It is distributed as a `.levipack` and uses LeviLauncher Android's Preloader API. This is a separate Android project; it is not the Windows LeviLamina DLL build.
Version 0.2.2 adds the Android world scanner and 3D ore-outline renderer. The user has confirmed that the 0.2.2 package loads and displays outlines on the tested phone.
Features
A launcher-managed floating X-RAY touch button.
A Bedrock X-Ray module in the LeviLauncher Mod Menu.
Incremental scanning of blocks around the local player.
Ore-colored 3D outlines rendered in the world, including through terrain.
Connected outlines: adjacent ore blocks are drawn as one outer silhouette, without the internal seams between touching blocks.
Settings for scan radius, vertical range, scan interval, marker limit, and connected outlines.
Persistent settings saved in the mod's configuration directory.
Reads client-loaded blocks only. It does not request chunks, generate terrain, or send packets to discover ores.
Ores and outline colors
Ore	Outline color
Diamond	Cyan `#32D9FF`
Emerald	Green `#35E77B`
Gold	Yellow `#FFD34F`
Nether gold	Amber `#FFBE45`
Iron	Light grey `#D4D9E2`
Copper	Orange `#FF925C`
Redstone	Red `#FF4D5D`
Lapis lazuli	Blue `#527BFF`
Coal	Grey `#B5B9C2`
Nether quartz	Warm white `#FFF0D5`
Ancient debris	Brown `#B8795E`
Deepslate and lit ore variants are recognized through their block-name suffixes where the game uses the same ore suffix.
Compatibility
Component	Target
Launcher	LeviLauncher Android 1.7.1
Preloader ABI	`cc08b40a31471d3e2dedb267f5f17b28da2b0604`
Minecraft Bedrock Android	1.26.60.4
Android native ABI	`arm64-v8a` (64-bit ARM)
NDK used for the build	28.2.13676358 (r28c)
CMake Android platform	`android-28`
The mod is not tied to a Samsung model or a specific phone brand. It is intended for compatible `arm64-v8a` Android devices running the supported LeviLauncher and Minecraft versions.
This is not a universal binary for every phone or Minecraft version. It does not target 32-bit ARM (`armeabi-v7a`) or x86/x86-64 devices. The game hooks, signatures, and native object offsets are validated for Minecraft Android 1.26.60.4. A Minecraft update can change them, so do not assume this package is compatible with a different game build until it has been revalidated.
The 0.2.2 outline render was visually confirmed by the user on one ARM64 Android device. That confirms the tested setup; it does not claim that every Android model, GPU driver, or game build has been tested.
Install the `.levipack`
Download or build `bedrock_xray_scanner_android-0.2.2-arm64-v8a.levipack`.
Copy the `.levipack` to the phone, such as the Downloads folder.
Open LeviLauncher Android and select the Minecraft instance you use.
Open Manage under Mods.
Choose Add Mod and select the `.levipack` file.
Disable or remove older Bedrock X-Ray Scanner packages before enabling 0.2.2. Avoid running multiple versions of the same mod at once.
Enable Bedrock X-Ray Scanner, launch Minecraft, and enter a world.
Tap the floating X-RAY button or enable the module from the Mod Menu.
Wait for the first incremental scan to complete. Detected ores in the scan area should receive colored 3D outlines.
In-game settings
Open the Bedrock X-Ray Scanner module in the LeviLauncher Mod Menu.
Setting	Range	Default	Description
Horizontal Scan Radius	4–32 blocks	12	Horizontal distance from the player to scan.
Vertical Scan Range	8–64 blocks	32	Blocks above and below the player's current Y level.
Scan Interval	800–10000 ms	1400 ms	Requested delay between scans. Large scans also take time to process incrementally.
Maximum Ore Markers	16–4096	2048	Maximum number of ore markers retained for rendering.
Connected Ore Outlines	On/Off	On	Merges touching ore blocks into connected outer outlines.
Larger radius and vertical settings inspect more blocks and can increase CPU use and scan time, especially on lower-end phones. For smoother performance, start with the default values and increase the range gradually.
Build from source (Windows)
Requirements
Windows 10/11 development PC.
Visual Studio Developer PowerShell.
Android SDK (Android Studio or command-line tools).
Android NDK 28.2.13676358 (r28c).
CMake 3.22 or newer.
Ninja and Git available on `PATH`.
Internet access for the first configure/build so CMake can fetch the pinned Preloader and dependencies.
Build command
Open Visual Studio Developer PowerShell in the project root:
```powershell
Set-ExecutionPolicy -Scope Process Bypass -Force
$env:ANDROID_HOME = "$env:LOCALAPPDATA\Android\Sdk"
.\scripts\package.ps1 -Abi arm64-v8a -Clean
```
If the NDK is installed in another directory, pass it explicitly:
```powershell
.\scripts\package.ps1 -Abi arm64-v8a -NdkHome "C:\Android\Sdk\ndk\28.2.13676358" -Clean
```
The `ExecutionPolicy` change applies only to the current PowerShell process. The script generates the config files, configures the Android build, compiles the native library, and packages the `.levipack`.
Output
```text
build-arm64-v8a\bedrock_xray_scanner_android-0.2.2-arm64-v8a.levipack
```
The `.levipack` is a ZIP package containing the mod directory, manifest, `libbedrock_xray_scanner_android.so`, and configuration files.
Troubleshooting
The mod does not appear in the Mod Menu
Confirm the `.levipack` was imported into the active Minecraft instance.
Check that older copies of the mod are disabled or removed.
Confirm the launcher is using a compatible Preloader build.
The floating button appears, but scanning does not start
Confirm Minecraft is version 1.26.60.4.
Enter a world and enable X-RAY after the world has loaded.
Check that the game hooks resolved successfully in the log.
Remember that only client-loaded blocks are scanned; the mod does not load distant chunks for you.
No outlines are visible
Confirm X-RAY is enabled and wait for a scan to finish.
Temporarily lower the radius and vertical range to the defaults.
Try the test in an area with known ore blocks inside the scan range.
Capture the `BedrockXRay` log and check scan-hit, selected-material, render-status, and mesh-submission messages. A mesh-call counter is diagnostic information, not proof by itself that pixels reached the display.
Capture a diagnostic log
With USB debugging enabled and the phone authorized, use ADB. If `adb` is not on `PATH`, set `$adbPath` to your Android SDK's `platform-tools\adb.exe`:
```powershell
$adbPath = "$env:LOCALAPPDATA\Android\Sdk\platform-tools\adb.exe"
& $adbPath logcat -c
# Launch Minecraft, enter a world, enable X-RAY, and wait about 10 seconds.
& $adbPath logcat -d -s "BedrockXRay:I" "*:S" > .\xray-debug.txt
```
Upload `xray-debug.txt` with any issue report. If the game crashes, disable/remove the mod and include the new native crash log as well.
Updating Minecraft
Do not assume the mod will continue to work after a Minecraft update. The Android backend resolves native signatures at runtime, but those signatures and the object layouts they lead to are specific to the supported game binary. Before adding another Minecraft version to the manifest, re-check the matching `libminecraftpe.so`, revalidate the signatures and offsets, rebuild, and test the scanner and renderer in-game.
Technical notes
Native entry point: `PL_REGISTER_MOD`.
Game integration: Android Preloader signature resolution and native hooks for player tick and world rendering.
Scanning: incremental block lookup from the active dimension's `BlockSource` on the game thread.
Geometry: ore-colored line batches; connected mode removes internal edges between adjacent ore blocks.
Rendering: Tessellator and MeshHelpers line submission using the preferred `name_tag_with_backface` material with fallback materials if it cannot be resolved.
Config: `config/config.json` and `config/config.schema.json`.
The Windows LeviLamina scanner source and its Windows-only game classes are not used by this Android build.

<img width="2340" height="1080" alt="Screenshot_20261119_184830_LeviLauncher" src="https://github.com/user-attachments/assets/2ea3e6c1-a9dd-4717-b2d8-69fc4e6d2b7b" />
