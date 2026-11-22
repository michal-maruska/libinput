libinput (fork)
================

This is a fork of [libinput](https://gitlab.freedesktop.org/libinput/libinput),
the input stack library used by display servers and other applications.

Purpose
-------

This fork exists to test
[fork-plugin-core](https://github.com/michal-maruska/fork-plugin-core)
under Weston. In the future it may be tested under other Wayland
environments as well.

Status: not currently in use, but it has been tested successfully.

What this fork adds
-------------------

The changes on top of `debian-unstable` (see `git log debian-unstable..HEAD`)
add a plugin hook to libinput's keyboard event pipeline:

- A separate `libinput_setup_fork` to load a "fork" plugin, based on
  Weston's `weston_load_module`. It must be invoked after logging is
  configured (i.e. as a second step after the udev context).
- A plugin API: `services` (log, vlog, `post_event`, rewrite) and
  `libinput_keyboard_plugin`, plus a one-time `libinput_register_fork()`
  to register the event-processing API.
- Plugins can carry their own `user_data`, which is passed to all API calls.
- Once registered, keyboard events are passed through
  `keyboard_pipeline->accept_event()` and can call back into the plugin.
- Debian packaging/CI tweaks: native source format, symbols updates,
  and CI builds for arm64 and `unstable/*` tags.

Documentation
-------------

All documentation lives in
[fork-plugin-core](https://github.com/michal-maruska/fork-plugin-core),
with some small notes in
[weston](https://github.com/michal-maruska/weston).

Original sources
----------------

The upstream libinput sources are available at:
https://gitlab.freedesktop.org/libinput/libinput
