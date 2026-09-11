[README.md](https://github.com/user-attachments/files/32128848/README.md)
# Unity Device Simulator – iPhone 17 Series

Device definitions (`.device`) and graphic overlays to use **iPhone 17**, **iPhone 17 Pro**, and **iPhone 17 Pro Max** in the [Unity Device Simulator](https://docs.unity3d.com/Manual/device-simulator.html), which does not yet officially include them in the `com.unity.device-simulator.devices` package.

## Why this repo

Unity hasn't shipped official device definitions for the iPhone 17 lineup yet (released by Apple in September 2025). This repo fills that gap with ready-to-use files, based on official Apple specifications and **verified with direct measurements on the Xcode simulator**.

## Included devices

| File | Model | Identifier | Resolution | DPI |
|---|---|---|---|---|
| `iPhone17.device` | iPhone 17 | iPhone18,3 | 1206 × 2622 | 460 |
| `iPhone17Pro.device` | iPhone 17 Pro | iPhone18,1 | 1206 × 2622 | 460 |
| `iPhone17ProMax.device` | iPhone 17 Pro Max | iPhone18,2 | 1320 × 2868 | 460 |

Each `.device` file comes with a matching PNG overlay showing the phone's silhouette (rounded bezel + Dynamic Island).

## Data reliability

| Data | Reliability | Source |
|---|---|---|
| Resolution, DPI, identifier | 🟢 Certain | Official Apple technical specifications |
| Safe Area Insets (portrait and landscape) | 🟢 Verified | Measured directly with `GeometryReader` (SwiftUI) on a real Xcode simulator, for all 3 models |
| Dynamic Island cutout position/size | 🟡 Reasoned estimate | Derived from official insets; Apple doesn't publish this as a separate rectangle — SwiftUI/UIKit doesn't expose it even on physical devices |
| Graphic overlay (phone silhouette PNG) | 🟡 Reconstruction | Drawn from scratch based on proportions measured from an Xcode simulator screenshot; not an original Apple asset |

### Measured Safe Area Insets (identical across all 3 models — same Dynamic Island panel)

**Portrait**

| top | bottom | left | right |
|---|---|---|---|
| 62pt | 34pt | 0pt | 0pt |

**Landscape**

| top | bottom | left | right |
|---|---|---|---|
| 0pt | 20pt | 62pt | 62pt |

## Installation

1. Download the `.device` files and their matching `.png` overlays from this repo.
2. Copy them into any folder under `Assets/` in your Unity project (e.g. `Assets/DeviceSimulator/`) — the device file and its overlay must be in the **same folder**.
3. Select each `.png` file in the Project window and, in the Inspector under **Advanced**, enable **Read/Write Enabled** → Apply.
4. Open `Window → General → Device Simulator`.
5. Pick the device from the dropdown — it will now show up in the list.

> Requirements: Unity 6000.x (Device Simulator built into the Editor) or Unity 2019.3+ with the `com.unity.device-simulator` package installed via Package Manager.

## How these were generated

Safe area values were obtained with a small SwiftUI test project, run on the Xcode simulator for each model:

```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        GeometryReader { geo in
            VStack(alignment: .leading, spacing: 12) {
                Text("Screen bounds (points)")
                Text("\(geo.size.width, specifier: "%.1f") x \(geo.size.height, specifier: "%.1f")")
                Text("Safe Area Insets (points)")
                Text("top: \(geo.safeAreaInsets.top, specifier: "%.1f")")
                Text("bottom: \(geo.safeAreaInsets.bottom, specifier: "%.1f")")
                Text("left: \(geo.safeAreaInsets.leading, specifier: "%.1f")")
                Text("right: \(geo.safeAreaInsets.trailing, specifier: "%.1f")")
            }
            .padding()
            .frame(maxWidth: .infinity, maxHeight: .infinity, alignment: .topLeading)
            .ignoresSafeArea()
        }
    }
}
```

## Contributing

If you own a physical device and want to confirm or correct the estimated Dynamic Island cutout value (via `Screen.cutouts` in a real build), feel free to open an issue or a PR — it's the only data point not yet 100% verified.

## License

MIT — see [LICENSE](LICENSE). The graphic overlays are original artwork (geometric shapes generated via code), not Apple assets.

## Disclaimer

This project is not affiliated with Apple Inc. or Unity Technologies. "iPhone" is a registered trademark of Apple Inc. "Unity" is a registered trademark of Unity Technologies.
