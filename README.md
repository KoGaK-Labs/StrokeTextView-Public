# API Reference v1.0

The [`StrokeTextView`](https://stroketextview.netlify.app/) exposes a clean and intuitive API. All property setters are optimized to automatically recalculate the view's bounding box (adding padding extra) and trigger hardware-accelerated redraws (`invalidate()`) only when necessary.
[site here](https://stroketextview.netlify.app/)

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
| **`layoutGravity`** | `Int` | `Gravity.NO_GRAVITY` | Controls the alignment of the text within the View's bounds (e.g., `Gravity.CENTER`, `Gravity.START`). |
| **`allowUnsafePropsValues`** | `Boolean` | `false` | The "Escape Hatch". When set to `true`, it disables the internal safety clamps (such as the `50f` limit on `strokeWidth`). <br><br>**Use with caution:** *Unleashing extreme stroke widths on fonts with small internal counters may produce visual overlapping artifacts due to native Canvas Path constraints.* |
