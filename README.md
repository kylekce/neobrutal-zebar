# 🟦 Neobrutal [Zebar](https://github.com/glzr-io/zebar) - Personal Fork

A personal modified version of the Neobrutal Zebar configuration for Zebar, built in Svelte with Tailwind. Since the original creator did an amazing job, I wanted to continue developing it with new Zebar features and ideas!

> [!NOTE]
> 
> This is a personal fork of [adriankarlen/neobrutal-zebar](https://github.com/adriankarlen/neobrutal-zebar), which has been archived. This version includes my own modifications and improvements to the original design. I plan to continue developing this project with new features, improvements, and support for the latest Zebar capabilities.

## ✨ Features

- Process icons for current workspace, with current focus indicator.
- System information meters, with shutdown and restart buttons.
- Media display for browser and music player.
- Animations like process focus, workspace switching, and marquee scrolling.
- Configuration using CSS-variables.

> [!IMPORTANT]
>
> This configuration is in early development! Contributions are very welcome, especially:
> - **Process icon mappings** - Help expand the icon map for better app recognition
> - **Media display improvements** - Better browser and music player compatibility  
> - **New Zebar features** - Integration of latest Zebar capabilities
> - **Theme contributions** - New color schemes and visual styles
> - **Bug reports and feature requests** - Help make this better for everyone!
> 
> The original creator did incredible work, and I want to keep that spirit alive. If you have any issues, ideas, or want to contribute, please open an issue or PR on the GitHub repository!

## 🖼️ Showcase

### My Personal Setup

![image](misc/own-setup.png)

### Original Examples

![image](misc/fullscreen.png)
![image](misc/catppuccin.png)

## 🚀 Getting started

> [!NOTE]
> If you are using window scaling in windows, you might need to edit
> `bar.zebar.json` to get a correct height (default is `60px`).

### Installation

#### Prerequisites

- **[Zebar](https://github.com/glzr-io/zebar)** - The desktop widget framework this configuration is built for
- **[GlazeWM](https://github.com/glzr-io/glazewm)** - The tiling window manager (required for workspace and window management features)
- **Package manager/runtime** of your choice (`npm` (recommended), `pnpm`, `yarn`, etc). This fork uses
  `npm` for package management.

#### Instructions

1. Download or clone this repo into your zebar configuration directory
2. Edit your Zebar `settings.json` to point to the neobrutal directory + `/build/index.html`. Example: `neobrutal-zebar/build/index.html`
3. Navigate to the root dir of neobrutal-zebar.
4. Run a install command for your selected package manager.
   ```bash
   npm install
   ```
5. Make any edits you want (see [Themes](#Themes) and [Recipies](#Recipies) )
6. Build the project
   ```bash
   npm run build
   ```
7. Launch Zebar

In `config.css` there exists a lot of configuration available to tweak.

## ⚙️ Additional Configuration

> [!IMPORTANT]
> **Power Controls Setup**: The shutdown and restart buttons require additional configuration in your GlazeWM settings to work properly.

Add the following to your GlazeWM `config.yaml` file:

```yaml
general:
  # ...existing config...
  shell_exec:
    # Allow shell execution for power controls
    enabled: true
```

Or alternatively, you can add custom keybindings to your GlazeWM config:

```yaml
keybindings:
  # ...existing keybindings...
  
  # Power controls
  - command: "shell-exec shutdown /s /t 0"
    bindings: ["Alt+F4"]
  - command: "shell-exec shutdown /r /t 0"  
    bindings: ["Alt+Shift+F4"]
```

Without this configuration, the power buttons in the bar will not function.

## 🎨 Themes

Neobrutal currently ships with 4 different themes, Rosé Pine, Catppuccin, Nord
and Material. Below follows some examples.

### 🌷Rosé Pine (default)

<details>
<summary>Example config</summary>

##### config.css

```css
/* colors */
--text: var(--rp-text);
--bg: var(--rp-overlay);
--border: var(--rp-highlight-low);
--shadow: var(--rp-highlight-low);
--icon: var(--rp-love);
--memory: var(--rp-iris);
--cpu: var(--rp-rose);
--cpu-high-usage: var(--rp-love);
--battery-good: var(--rp-pine);
--battery-mid: var(--rp-gold);
--battery-low: var(--rp-love);
--focused-process: var(--rp-text);
--process: var(--rp-muted);
--displayed: var(--rp-text);
--ws-1: var(--rp-gold);
--ws-2: var(--rp-love);
--ws-3: var(--rp-pine);
--ws-4: var(--rp-foam);
--ws-5: var(--rp-iris);
--tiling-direction: var(--rp-rose);
--not-playing: var(--rp-love);
--now-playing: var(--rp-pine);
--network: var(--rp-text);
--weather: var(--rp-text);
```

</details>

### 😸 Catppuccin

Utilizes [Catppuccin Palette](https://github.com/catppuccin/palette/blob/main/docs/css.md), configure css variables accoriding to your liking.

<details>
<summary>Config show in one of the showcase above</summary>

##### config.css

```css
/* border */
--radius: 9999px;

/* shadow */
--shadow-size-bar: 0;
--shadow-size-button: 0;

/* colors */
--text: var(--ctp-mocha-text);
--bg: var(--ctp-mocha-surface0);
--border: var(--ctp-mocha-crust);
--border-button: var(--ctp-mocha-crust);
--shadow: var(--ctp-mocha-mantle);
--icon: var(--ctp-mocha-red);
--memory: var(--ctp-mocha-mauve);
--cpu: var(--ctp-mocha-pink);
--cpu-high-usage: var(--ctp-mocha-red);
--battery-good: var(--ctp-mocha-green);
--battery-mid: var(--ctp-mocha-peach);
--battery-low: var(--ctp-mocha-red);
--focused-process: var(--ctp-mocha-text);
--process: var(--ctp-mocha-surface2);
--displayed: var(--ctp-mocha-text);
--ws-1: var(--ctp-mocha-peach);
--ws-2: var(--ctp-mocha-red);
--ws-3: var(--ctp-mocha-green);
--ws-4: var(--ctp-mocha-blue);
--ws-5: var(--ctp-mocha-mauve);
--tiling-direction: var(--ctp-mocha-lavender);
--not-playing: var(--ctp-mocha-red);
--now-playing: var(--ctp-mocha-green);
--network: var(--ctp-mocha-text);
--weather: var(--ctp-mocha-text);
```

##### src/components/LeftGroup.svelte

```svelte
<!-- replace this line -->
<Button class="text-zb-icon" iconClass="heart-filled" />
<!-- with this line -->
<Button class="text-zb-icon" iconClass="cat" />
```

</details>

### ❄️ Nord

Utilizes [Nord Theme](https://www.nordtheme.com/) color palette, providing a cool arctic aesthetic.

<details>
<summary>Example Nord config</summary>

##### config.css

```css
/* colors */
--text: hsl(var(--nord6));
--bg: hsl(var(--nord0));
--border: hsl(var(--nord3));
--shadow: hsl(var(--nord1));
--icon: hsl(var(--nord7));
--memory: hsl(var(--nord15));
--cpu: hsl(var(--nord8));
--cpu-high-usage: hsl(var(--nord11));
--battery-good: hsl(var(--nord14));
--battery-mid: hsl(var(--nord13));
--battery-low: hsl(var(--nord11));
--focused-process: hsl(var(--nord6));
--process: hsl(var(--nord3));
--displayed: hsl(var(--nord6));
--ws-1: hsl(var(--nord13));
--ws-2: hsl(var(--nord11));
--ws-3: hsl(var(--nord14));
--ws-4: hsl(var(--nord7));
--ws-5: hsl(var(--nord15));
--tiling-direction: hsl(var(--nord8));
--not-playing: hsl(var(--nord11));
--now-playing: hsl(var(--nord14));
--network: hsl(var(--nord6));
--weather: hsl(var(--nord6));
```

</details>

### 🎨 Material Design

Based on [Material Design Color System](https://m2.material.io/design/color/), offering vibrant and accessible colors.

<details>
<summary>Example Material config</summary>

##### config.css

```css
/* colors */
--text: hsl(var(--md-grey-900));
--bg: hsl(var(--md-grey-50));
--border: hsl(var(--md-grey-300));
--shadow: hsl(var(--md-grey-200));
--icon: hsl(var(--md-blue-600));
--memory: hsl(var(--md-purple-500));
--cpu: hsl(var(--md-pink-500));
--cpu-high-usage: hsl(var(--md-red-500));
--battery-good: hsl(var(--md-green-500));
--battery-mid: hsl(var(--md-orange-500));
--battery-low: hsl(var(--md-red-500));
--focused-process: hsl(var(--md-grey-900));
--process: hsl(var(--md-grey-400));
--displayed: hsl(var(--md-grey-900));
--ws-1: hsl(var(--md-orange-500));
--ws-2: hsl(var(--md-red-500));
--ws-3: hsl(var(--md-green-500));
--ws-4: hsl(var(--md-blue-500));
--ws-5: hsl(var(--md-purple-500));
--tiling-direction: hsl(var(--md-indigo-500));
--not-playing: hsl(var(--md-red-500));
--now-playing: hsl(var(--md-green-500));
--network: hsl(var(--md-grey-900));
--weather: hsl(var(--md-grey-900));
```

</details>

## 🍳 Recipies

<details>
<summary>Soft Brutal</summary>

```css
--radius: 9999px;
```

<img src="misc/brutal-soft.png" />
</details>
<details>
<summary>Round bars without shadow</summary>

```css
--border-size: 1px;
--radius: 9999px;
--shadow-size-bar: 0px;
--shadow-size-button: 0px;
```

<img src="misc/non-brutal.png" />
</details>

## 📜 License

This project is licensed under the MIT License - see the
[LICENSE](LICENSE) file
