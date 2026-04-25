# API Reference v1.0
![StrokeTextView](https://stroketextview.netlify.app/src/assets/images/stroke_text_view.webp)
The [`StrokeTextView`](https://stroketextview.netlify.app/) exposes a clean and intuitive API. All property setters are optimized to automatically recalculate the view's bounding box (adding padding extra) and trigger hardware-accelerated redraws (`invalidate()`) only when necessary.

## Demo app
<table align="center"><tr><td align="center"><b>Stroke Axis</b><br><br><video src="https://github.com/user-attachments/assets/ee77c239-fa74-4cb0-8f93-334e2c1023d0" width="160" controls autoplay loop muted></video></td><td align="center"><b>Shadow Axis</b><br><br><video src="https://github.com/user-attachments/assets/a939b622-b3f8-40e8-b86b-fd835241ccfe" width="160" controls autoplay loop muted></video></td><td align="center"><b> Native Shadows</b><br><br><video src="https://github.com/user-attachments/assets/db50d852-3b4e-4e85-9ef0-14a3a7b910d7" width="160" controls autoplay loop muted></video></td><td align="center"><b>Customizations</b><br><br><video src="https://github.com/user-attachments/assets/7fb5e105-8d41-4d4c-aa05-e79f14be6365" width="160" controls autoplay loop muted></video></td></tr></table>

## 🎮 Showcase (Game Mobile)

<table align="center">
  <tr>
    <td align="center">
      <a href="https://stroketextview.netlify.app/" target="_blank">
        <img src="https://github.com/user-attachments/assets/352a3d6b-2e38-449a-8df3-3e4593564578" width="160" alt="Watch Showcase">
      </a>
    </td>
  </tr>
</table>
*Click the image above to watch the StrokeTextView rendering native 60FPS shadows and outlines in a real production environment on our Landing Page.*

## 🚀 Quick Start / Installation

Installing the **StrokeTextView** component is straightforward and takes less than a minute.

**Step 1: Add the `.aar` file to your project**
1. Switch your Android Studio project view from "Android" to "Project".
2. Navigate to `app/libs/` (create the `libs` folder if it doesn't exist).
3. Copy and paste the `StrokeTextView.aar` file into this folder.

**Step 2: Update your `build.gradle`**
Open your app-level `build.gradle` (or `build.gradle.kts`) and add the following line inside the `dependencies {}` block:

```gradle
dependencies {
    // KoGaK Labs - StrokeTextView Component

    // Option A: Direct file reference
    implementation(files("libs/StrokeTextView.aar"))
    
    // Option B: Wildcard (Includes all .aar files)
    // implementation(fileTree("libs") { include("*.aar") })
    
    // ... your other dependencies
}
```

**Step 3: Sync Project**
Click on "Sync Now" in the yellow banner at the top of Android Studio.
That's it! The StrokeTextView is now ready to be used in your XML layouts or Kotlin/Compose code with Zero Runtime Overhead.

## 🖌️ Stroke Properties

| Property | Type | Default | Description |
| :--- | :---: | :---: | :--- |
| **`strokeColor`** | `Int` | `Color.BLACK` | Defines the color of the text stroke. |
| **`strokeWidth`** | `Float` | `0f` | Defines the thickness of the stroke. For optimal visual quality and to prevent path self-intersection artifacts, this value is safely capped at `50f` by default. *(See `allowUnsafePropsValues` to bypass this limit).* |
| **`strokeDx`** | `Float` | `0f` | **Range:** `-200.0` to `200.0`. The horizontal offset of the stroke. Positive values move the stroke to the right, negative to the left. The view's padding expands automatically to prevent clipping. |
| **`strokeDy`** | `Float` | `0f` | **Range:** `-200.0` to `200.0`. The vertical offset of the stroke. Positive values move the stroke downwards, negative upwards. |

---

## 🌑 Custom Shadow Properties

*High-performance drop shadows rendered directly via Canvas, avoiding the overhead of Android's native blur algorithms.*

| Property | Type | Default | Description |
| :--- | :---: | :---: | :--- |
| **`customShadowColor`** | `Int` | `0` *(Transparent)* | The color of the high-performance custom shadow. Set to any valid color int to enable. |
| **`customShadowDx`** | `Float` | `0f` | **Range:** `-200.0` to `200.0`. The horizontal offset of the custom shadow. |
| **`customShadowDy`** | `Float` | `0f` | **Range:** `-200.0` to `200.0`. The vertical offset of the custom shadow. |

---

## ⚠️ Native Shadow

| Property | Type | Default | Description |
| :--- | :---: | :---: | :--- |
| **`nativeShadowEnabled`** | `Boolean` | `false` | Toggles Android's native shadow rendering layer. <br><br>**Warning:** *Enabling native shadows on very large texts or complex paths may cause CPU overhead or frame drops on older devices. Use `customShadowColor` for a 60fps zero-overhead alternative.* |

---

## ⚙️ Layout & Advanced Behavior

| Property | Type | Default | Description |
| :--- | :---: | :---: | :--- |
| **`gravity`** | `Int` | `Gravity.TOP \| Gravity.START` | Controls the alignment of the text **inside** the View's own bounds (e.g., `Gravity.CENTER`, `Gravity.END`). |
| **`layoutGravity`** | `Int` | `Gravity.NO_GRAVITY` | Controls the alignment of the entire View in relation to its **Parent View** (e.g., centering the component inside a `FrameLayout`). |
| **`allowUnsafePropsValues`** | `Boolean` | `false` | The "Escape Hatch". When set to `true`, it disables the internal safety clamps (such as the `50f` limit on `strokeWidth`). <br><br>**Use with caution:** *Unleashing extreme stroke widths on fonts with small internal counters may produce visual overlapping artifacts due to native Canvas Path constraints.* |


## 💻 Programmatic Usage (Dynamic & Custom Views)

XML is entirely optional. `StrokeTextView` is designed to be fully instantiated and managed programmatically. 

If you are using it inside a Custom View without XML, or generating UI dynamically, the component calculates its own bounds automatically. Every time you set a property (like `strokeWidth` or `customShadowDx`), it automatically recalculates the required extra padding to prevent path clipping and triggers hardware-accelerated redraws (`invalidate() / requestLayout()`) only when necessary.

**Example: Instantiating directly in Kotlin**

```kotlin
// 1. Instantiate passing the Context
val dynamicStrokeText = StrokeTextView(context).apply {
    // Standard Text Properties
    text = "Zero Overhead"
    setTextColor(Color.WHITE)
    textSize = 48f // Ensure you use SP to PX conversion if needed
    
    // Stroke Properties
    strokeColor = Color.GREEN
    strokeWidth = 14f // Safely capped at 50f unless allowUnsafePropsValues is true
    
    // Shadow Properties
    customShadowColor = Color.DKGRAY
    customShadowDx = 10f
    customShadowDy = 10f
}

// 3. Add to your Custom View or Layout
addView(dynamicStrokeText)
````

## 📄 License & Usage

**StrokeTextView** is a commercial, proprietary UI component developed by **KoGaK Labs™**. 

⚠️ **Note:** This public repository serves solely as the official documentation hub and issue tracker. The actual source code and the `.aar` library are **not** open-source.

To use this component in your personal or commercial Android projects (and monetize them), you must purchase a Single-Seat License.

🛒 **[Purchase the StrokeTextView Component Here](COLOQUE_SEU_LINK_DO_GUMROAD_AQUI)**

*Upon purchase, your download will include the `.aar` file and the full `LICENSE.txt` agreement granting you the rights to use it without restrictions in your compiled applications.*
