# paf0186/halloy

Halloy 2026.9 with two local changes, on branch `dictation-and-zoom`:

- **macOS dictation works.** winit comes from
  [paf0186/winit@macos-dictation](https://github.com/paf0186/winit/tree/macos-dictation),
  which is iced's winit plus a fix for rust-windowing/winit#4666. The same fix is
  under review upstream in rust-windowing/winit#4748 (master) and #4749 (0.30.x).
  The change is macOS-only; other platforms build unchanged code.
- **Zoom shortcuts.** `zoom_in`, `zoom_out` and `zoom_reset` (⌘/ctrl `=`, `-`, `0`)
  scale the whole UI in ×1.1 steps, on top of `scale_factor`. The level is saved
  in the data directory as `zoom` and restored on start.

`contrib/patrick/` holds Patrick's config (`config.toml`) and the `clear-dark` /
`clear-light` themes.

## Building

Requires a recent stable Rust: iced declares 1.93, but 1.93 fails on if-let guards;
1.99.1 is known to work (`rustup toolchain install stable`).

```sh
git clone -b dictation-and-zoom https://github.com/paf0186/halloy.git
cd halloy
cargo build --release
```

The binary is `target/release/halloy` (`halloy.exe` on Windows).

### Windows

1. Install Git, rustup (stable, MSVC toolchain), and the Visual Studio Build Tools
   with the "Desktop development with C++" workload.
2. Build as above. Run `target\release\halloy.exe`, or copy it somewhere permanent
   and add a Start menu shortcut.
3. Config goes in `%AppData%\halloy\`. Copy `contrib\patrick\config.toml` to
   `%AppData%\halloy\config.toml` and `contrib\patrick\themes\*` to
   `%AppData%\halloy\themes\`.
   Give each machine its own fallback nick (`alt_nicks = ["patrick-win"]` under
   `[servers.mulberry]`), so a second machine still connects while the first holds `patrick`.
4. Fonts: install Inter (https://rsms.me/inter/) and JetBrains Mono
   (https://www.jetbrains.com/lp/mono/), or change `[font]` in the config.
5. The config expects an ssh tunnel on `localhost:16668` to the IRC server's port 6667.
   Windows' built-in OpenSSH can run it (`ssh -N -L 16668:127.0.0.1:6667 <user>@<host>`),
   kept alive by a Task Scheduler task that starts at logon and restarts on failure.
