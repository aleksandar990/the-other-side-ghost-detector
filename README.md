# The Other Side — Ghost Detector

An unofficial Windows companion for **The Other Side**. Identify the ghost on a supported game build, follow match changes, and view its evidence, behavior, and hunt reference in one place.

**v1.1.0 · Public test release · Windows x64**

> Compatibility is currently limited to one verified game executable. See [Supported game build](#supported-game-build) before downloading.

## Features

- **Automatic game discovery** from running processes, Steam libraries, Downloads, and Desktop.
- **Verified ghost identification** through a read-only runtime connection on the supported build.
- **Automatic match monitoring** after connecting, with unknown results shown when identity cannot be verified.
- **Ghost reference details** covering evidence, behavior, speed, detection range, holy water effectiveness, and hunt cooldown.
- **Optional evidence mode** for narrowing candidates from gameplay observations. Predictions are labeled separately from confirmed identities.
- **Connection diagnostics**, local logs, and manual executable selection.
- **Subtle interface animations** that respect Windows animation and High Contrast settings.
- **Local privacy controls** that mask Windows account names in displayed game paths, errors, and logs.

## Requirements

- Windows x64.
- [.NET 8 Desktop Runtime, Windows x64](https://dotnet.microsoft.com/en-us/download/dotnet/8.0).
- The supported copy of The Other Side, installed separately.

The downloadable app is framework-dependent. Install the **Desktop Runtime**; the base .NET runtime alone is insufficient for WPF.

## Installation

1. Open this repository’s **Releases** page.
2. Download `GhostDetector-v1.1.0-win-x64.zip`.
3. Extract **all files** into a folder.
4. Run `GhostDetector.exe`.

Keep the executable, DLLs, runtime configuration, and JSON reference files together. The source ZIP is for building the project, not for running the app directly.

The public test build is unsigned.

## Using the app

1. Launch The Other Side.
2. Open Ghost Detector. It searches for the installed game automatically.
3. Press **INJECT** to connect.
4. Enter an Identify contract. A verified result appears when the supported reader can retrieve the current identity.
5. Use **Find ghost** to reconnect and refresh manually. After connection, new contracts are monitored automatically.
6. Open **Ghost dossier** for the full reference, or **Connection** for diagnostics.

Despite its label, **INJECT starts a read-only connection; it does not inject code**.

If automatic discovery fails, choose the executable in **Connection**. You can select `TheOtherSide.exe` or `TheOtherSide-Win64-Shipping.exe`; the launcher resolves to the shipping executable when it is present. Discovery is bounded and does not search every folder on every drive.

### Reading results

| Result | Meaning |
| --- | --- |
| Verified identity | The supported runtime reader retrieved the current ghost. |
| Unknown / waiting | A current identity is unavailable or has not been verified. |
| Possible ghosts | Evidence-based candidates, not confirmed identities. |
| Last detected result | A previous result retained for reference, not a current live reading. |
| Reference details | Community guide facts, not measurements from your match. |

The app does not submit your answer. Use the in-game phone’s **Identifier** app to select the ghost before ending the contract.

## Supported game build

The runtime reader validates the actual shipping executable against this SHA-256:

```text
20c4fcb0cba38320de5c9ae0b4f6f67bf500f24eef670f2e2d73188596f61e65
```

A different executable is rejected before the integration profile is applied. Finding a game installation does not establish that its build is supported. A game update may require a newly investigated and verified profile.

Live identity was verified on the inspected build. Compatibility with other builds and every possible match transition has not been established. Community reference details may also differ between versions.

## Privacy

- No login, API key, or bundled `game-path.txt` is required.
- The game path is saved locally in `%LOCALAPPDATA%/OtherSideGhostDetector/settings.json`.
- Logs are stored in `%LOCALAPPDATA%/OtherSideGhostDetector/Logs`.
- Displayed game paths and diagnostic text replace Windows account directories with `%USERPROFILE%`.
- Settings and logs stay on the local machine. The app does not upload them.

Public packages exclude local settings, logs, debug symbols, captures, research notes, and private development previews. Review logs and screenshots before sharing them yourself.

## Troubleshooting

| Problem | What to try |
| --- | --- |
| Game not found | Launch the game, press INJECT again, or choose the executable manually. |
| Unsupported build | Compare the executable hash with the supported fingerprint above. A different build needs an updated integration profile. |
| Access denied | Review Connection diagnostics. Windows may prevent the app from reading the game process. |
| Unknown during a match | Let loading finish, check diagnostics, then try Find ghost. The app will not substitute a guessed result. |
| No animations | Enable Animation effects in Windows Accessibility settings. High Contrast also disables app motion. |
| Missing runtime | Install the Windows x64 .NET 8 Desktop Runtime. |

Use **Open logs** in Connection for detailed local diagnostics. The application does not bypass game protections or modify game memory.

## Build from source

Install the [.NET 8 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/8.0), then open PowerShell in the repository root:

```powershell
dotnet restore GhostDetector/GhostDetector.csproj
./build.ps1 -Publish
```

The Windows build is written to `artifacts/public-build`.

Run backend checks separately:

```powershell
dotnet run --project Tests/Tests.csproj -c Release
```

Check WPF animation behavior using a hidden app window:

```powershell
./artifacts/public-build/GhostDetector.exe --motion-check artifacts/motion-check
```

This diagnostic exercises animation clocks without capturing the desktop or changing Windows settings. See [VALIDATION.md](VALIDATION.md) for the release’s validation results and limitations.

## Packaging a release

With Python 3 installed:

```powershell
./build.ps1 -Publish
python tools/package_release.py
```

The script creates these files in `release-output`:

| File | Purpose |
| --- | --- |
| `GhostDetector-v1.1.0-github-source.zip` | Clean repository source. Extract its contents into your repository. |
| `GhostDetector-v1.1.0-win-x64.zip` | Downloadable application for GitHub Releases. |
| `SHA256SUMS.txt` | SHA-256 checksums for both ZIPs. |
| `RELEASE-VALIDATION.json` | Package contents and privacy-audit results. |

The packaging script uses explicit file allowlists and scans for developer profile information. It requires a framework-dependent publish. Do not upload the entire development workspace. Your GitHub profile, commit-author identity, and existing Git history are separate from these package checks.

## License and credits

Project code is available under the [MIT License](LICENSE).

Ghost reference facts come from the community [P.M.S. Tracker](https://p8riot.github.io/tos-ghost-tracker/Index.html). Source and retrieval information are retained in the JSON reference files. Bundled fonts retain their SIL Open Font License notices; see [NOTICE.md](NOTICE.md).

This project is unofficial and is not affiliated with the game developer. No game executable or game assets are included.
