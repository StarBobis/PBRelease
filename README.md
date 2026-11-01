# ProjectBunny - Binary Releases

This repository hosts **binary releases only**. ProjectBunny's source code is
kept private; the packaged zip is the only artifact that leaves the development
machine.

## Download

Get the newest `ProjectBunny-vX.Y.Z-d3d12-x64.zip` from the
[Releases](../../releases/latest) page.

## Zip contents

| File | Role |
| --- | --- |
| `d3d12.dll` | DX12 hooking / shader-manipulation layer |
| `dxcompiler.dll`, `dxil.dll` | Microsoft DirectXShaderCompiler runtime pair (SM6) |
| `d3dx.ini` | Default configuration |
| `LICENSE.txt` | ProjectBunny license |

## Important: keep the DXC pair together

`d3d12.dll` probes **its own directory first** for `dxcompiler.dll` and loads
`dxil.dll` from that same directory. The pair is a runtime dependency, not an
optional extra: `[CustomShaderX]` mod elements (the SSMT UI panel among them)
are compiled to SM6 while the game runs.

- Keep `dxcompiler.dll` and `dxil.dll` beside `d3d12.dll`.
- Do not let a foreign `dxcompiler.dll` from the DLL search path (for example a
  self-contained Vulkan SDK build) be picked up instead; it can fail-fast on the
  second in-process compile.
- If the pair is missing, the compile degrades to a logged failure and custom
  elements are skipped. Nothing crashes.

## Configuration

`d3dx.ini` ships as the **XXMI Launcher** variant: `[Loader]` names the target
executable, whitelists `XXMI Launcher.exe` as the loader, sets
`module = d3d12.dll` and enables `require_admin`. Edit that section if you
launch through a different loader.

> **No loader is bundled.** The zip contains no `Run.exe`: your launcher
> performs the injection and reads `d3dx.ini` itself.

## Versioning

Release tags are `vX.Y.Z` and are independent of the upstream 3Dmigoto version
carried by `CMakeLists.txt` (currently 1.3.16).

## Notes

- x64 only; 32-bit builds are not maintained.
- Third-party binaries keep their upstream licenses. The DXC pair ships with a
  `dxc-LICENSE.TXT` notice when available.