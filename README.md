# mmpd

mmpd turns a MIDI keyboard or controller hooked up to your computer into a versatile macro pad. The aim is to assign
behavior to keys and controllers, while differentiating based on the application that is currently focused.

Essentially, think of it as an additional keyboard that does custom things based on what application you're working with.

You can also set up "global" actions that work regardless of the application.

Initially written for use on Linux distributions using the X windowing system, it was structured to easily allow
adding implementations for other platforms. At the moment, Linux with X windowing system, Windows, and Mac OS are
supported. The Mac OS implementation is quite slow since it works through shell calls to `osascript`. It's an area that
could do with improvement, perhaps through rust bindings to the CoreGraphics library.

## Current status: tentatively ready for some use

What's implemented so far:

- Detecting focused window (window class, name)
- Connecting to a MIDI input device, receiving and parsing its messages
- Data structures for describing:
  - Scopes (focused window matching)
    - With flexible string matching
  - Actions (to be run in response to MIDI events)
  - Event matchers (describes an event to matched to trigger an event)
    - Midi Event matcher with flexible parameter value matching options
  - Macros (combining scopes, event matchers, and actions into one package)
  - Preconditions (state that must be satisfied in addition to an event matching in
    order to execute a macro)
    - Midi preconditions for note_on, control, program, pitch_bend
- Configuration: YAML parser to intermediary "RawConfig" format, plus a parser
  from RawConfig into the aforementioned data structures
- Command line interfaces covering
  - Picking a config file or loading one from default location
  - list-midi-devices subcommand
  - monitor subcommand (to view incoming events without running macros)
  - (no subcommand) listening for events and running configured macros in response
- Support for Linux (using X server), Windows, and Mac OS

There's documentation on the configuration format in [docs/config.md](https://github.com/michd/midi-macro-pad/blob/main/docs/config.md)
including some future plans.

## To do:

- See [Issues](https://github.com/michd/mmpd/issues)
- Rewrite this readme with full guide on building / installing etc

## Dependencies

### Linux
- [xdotool](https://www.semicomplete.com/projects/xdotool/) (get it through your system's package manager)
  
  xdotool is needed for its library (libxdo).

### Windows

Nothing specific I think, but see [Installing `rustup` on Windows](https://doc.rust-lang.org/stable/book/ch01-01-installation.html#installing-rustup-on-windows).

### Mac OS

Nothing specific. Mac OS-specific parts interface with the system through AppleScript.