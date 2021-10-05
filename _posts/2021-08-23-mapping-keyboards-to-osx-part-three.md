## Remapping Windows Keyboards to OSX: Part 3
### Part 3: Windows Shortcuts

Karabiner Elements' Preferences pane has a 'Complex modifications' tab capable of implementing complex rules of key reassignment. This is useful if you'd like to execute complex key reassignment (e.g. [partially reassigning keys]({% post_url 2021-08-16-mapping-keyboards-to-osx-part-two %}#partial-reassignment) or [partially disabling keys]({% post_url 2021-08-16-mapping-keyboards-to-osx-part-two %}#partial-disabling)), but mimicking some of the most common Windows key behaviour requires special combinations of rules.

⚠️ **It's important when reassigning combinations of keys to ensure that you understand the behaviour [that OSX might already assign](https://support.apple.com/en-gb/HT201236) to that shortcut.** If you're substituting different behaviour for a key combination that is already an OSX shortcut, you may wish to reassign the OSX shortcut to a different, unassigned combination.

⚠️ **It's important when reassigning combinations of keys to ensure that you understand the behaviour that applications may already assign to that shortcut.** If you're substituting different behaviour for a key combination that is already an application-specific shortcut, you may wish to exclude the application from the rule.

Some commonly desired rules are provided below.

---

- [Excluding Applications from Rules](#excluding-applications-from-rules)
- [Saving with `Ctrl` + `s`](#saving-with-ctrl--s)
- [Copying with `Ctrl` + `c`](#copying-with-ctrl--c)
- [Cutting with `Ctrl` + `x`](#cutting-with-ctrl--x)
- [Pasting with `Ctrl` + `v`](#pasting-with-ctrl--v)
- [Undoing with `Ctrl` + `z`](#undoing-with-ctrl--z)
- [Redoing with `Ctrl` + `y`](#redoing-with-ctrl--y)
- [Finding with `Ctrl` + `f`](#finding-with-ctrl--f)
- [Selecting All with `Ctrl` + `a`](#selecting-all-with-ctrl--a)
- [Creating with `Ctrl` + `n`](#creating-with-ctrl--n)
- [Print Screen Key](#print-screen-key)
- [Home and End Keys](#home-and-end-keys)
- [Lock Screen with `Ctrl` + `l`](#lock-screen-with-ctrl--l)
- [Reassigning Keys 4: Browser Shortcuts](#reassigning-keys-4-browser-shortcuts)

---

### Excluding Applications from Rules

A number of the keyboard shortcuts discussed in this part of this series are unassigned by OSX and will be useful across almost all applications, but would wreak havoc on a small number of applications. For example, while unassigned by OSX and with widespread utility across almost all applications, the assignment of Save functionality to `Ctrl` + `s` wreaks havoc on terminal programs, which use the shortcut to cancel input, shut down the execution of scripts and so on. To exclude applications from Karabiner Elements' rules, Karabiner Elements provides the [`frontmost_application_if` condition](https://karabiner-elements.pqrs.org/docs/json/complex-modifications-manipulator-definition/conditions/frontmost-application/). A simple example of using `frontmost_application_if` to exclude terminal applications is provided in the [Copying with `Ctrl` + `c`](#copying-with-ctrl--c) section below.

⚠️ **Karabiner Elements cannot distinguish between individual parts of the frontmost application. To exclude an internal terminal window within an application from a rule using `frontmost_application_if`, the entire application must be excluded.**

Some individual applications permit custom keybinding, which can be used instead of Karabiner Elements to address specific parts of the application. For example, Atom can be configured an internal terminal window (e.g. with the platformio-ide-terminal package). Karabiner Elements cannot distinguish between entries made in Atom's text editor and entries made in the platformio-ide-terminal, so the entirety of Atom must be excluded from the `Ctrl` + `c` and `Ctrl` + `v` complex rules to prevent Karabiner Elements from interfering with platformio-ide-terminal. However, [Atom permits custom keybinds](https://flight-manual.atom.io/behind-atom/sections/keymaps-in-depth/). Atom keybinds can be limited in scope, e.g. to the text editor but not to a terminal window. To add the Copy and Paste keybinds to the text editor but not the terminal window, I added these keybinds to Atom's `Keymap.cson`:

```
'atom-text-editor':
  'ctrl-a': 'core:'
  'ctrl-c': 'core:copy'
  'ctrl-v': 'core:paste'
```

I then prefixed the [Copy](#copying-with-ctrl--c) and [Paste](#pasting-with-ctrl--v) rules I implemented in Karabiner Elements with conditions which excluded Atom:

```
{
  "conditions": [
        {
          "bundle_identifiers": [
            "^com\\.github\\.atom$",
          ],
          "type": "frontmost_application_unless"
        }
      ],
}
```

Examples of how conditions are integrated into rules are provided in the Copy and Paste sections below.

---

### Saving with `Ctrl` + `s`

One of the most commonly-used Windows shortcuts is Save, using the `Ctrl` and `s` shortcut. The `Ctrl` and `s` combination is not actually assigned on OSX, so there's no obvious side-effect to assigning this shortcut to Save. Here's the complex rule I inserted into my `karabiner.json` file:

```
{
  "description": "Assign Ctrl + s to Save",
  "manipulators": [
    {
      "from": {
        "key_code": "s",
        "modifiers": {
          "mandatory": [
            "control"
          ]
        }
      },
      "to": {
        "key_code": "s",
        "modifiers": [
          "command"
        ]
      },
      "type": "basic"
    }
  ]
}
```

---

### Copying with `Ctrl` + `c`

Another one of the most commonly-used Windows shortcuts is Copy, using the `Ctrl` and `c` shortcut.

⚠️ **While the `Ctrl` + `c` combination is not assigned by OSX, it is assigned by some common programs, especially terminal applications.** These applications must be excluded from the operation of this rule; see the [Excluding Applications from Rules](#excluding-applications-from-rules) section above.

Here's the complex rule I inserted into my `karabiner.json` file:

```
{
  "description": "Assign Ctrl + c to Copy",
  "manipulators": [
  "conditions": [
        {
          "bundle_identifiers": [
            "^com\\.apple\\.Terminal$",
            "^com\\.github\\.atom$",
            "^com\\.googlecode\\.iterm2$"
          ],
          "type": "frontmost_application_unless"
        }
      ],
    {
      "from": {
        "key_code": "c",
        "modifiers": {
          "mandatory": [
            "control"
          ]
        }
      },
      "to": {
        "key_code": "c",
        "modifiers": [
          "command"
        ]
      },
      "type": "basic"
    }
  ]
}
```

---

### Cutting with `Ctrl` + `x`

A commonly-used Windows shortcut is Cut, using the `Ctrl` and `x` shortcut. As before, the `Ctrl` and `x` combination is not actually assigned on OSX, so there's no obvious side-effect to assigning this shortcut to Cut.

⚠️ OSX uses the `⌘` + `x` key combination for the Cut command. However, **the Cut command does not work for files in Finder**.

Here's the complex rule I inserted into my `karabiner.json` file:

```
{
  "description": "Assign Ctrl + X to Cut",
  "manipulators": [
    {
      "from": {
        "key_code": "x",
        "modifiers": {
          "mandatory": [
            "control"
          ]
        }
      },
      "to": {
        "key_code": "x",
        "modifiers": [
          "command"
        ]
      },
      "type": "basic"
    }
  ]
}
```

---

### Pasting with `Ctrl` + `v`

Another one of the most commonly-used Windows shortcuts is Paste, using the `Ctrl` and `v` shortcut.

⚠️ **While the `Ctrl` + `v` combination is not assigned by OSX, it is assigned by some common programs, especially terminal applications and applications with an internal terminal.** These applications must be excluded from the operation of this rule; see the [Excluding Applications from Rules](#excluding-applications-from-rules) section above.

Here's the complex rule I inserted into my `karabiner.json` file:

```
{
  "description": "Assign Ctrl + V to Paste",
  "manipulators": [
  "conditions": [
      {
        "bundle_identifiers": [
          "^com\\.apple\\.Terminal$",
          "^com\\.github\\.atom$",
          "^com\\.googlecode\\.iterm2$"
        ],
        "type": "frontmost_application_unless"
      }
    ],
    {
      "from": {
        "key_code": "v",
        "modifiers": {
          "mandatory": [
            "control"
          ]
        }
      },
      "to": {
        "key_code": "v",
        "modifiers": [
          "command"
        ]
      },
      "type": "basic"
    }
  ]
}
```
---

### Undoing with `Ctrl` + `z`

A commonly-used Windows shortcut is Undo, using the `Ctrl` and `z` shortcut. As before, the `Ctrl` and `z` combination is not actually assigned on OSX, so there's no obvious side-effect to assigning this shortcut to Find. Here's the complex rule I inserted into my `karabiner.json` file:

```
{
  "description": "Assign Ctrl + Z to Undo",
  "manipulators": [
    {
      "from": {
        "key_code": "z",
        "modifiers": {
          "mandatory": [
            "control"
          ]
        }
      },
      "to": {
        "key_code": "z",
        "modifiers": [
          "command"
        ]
      },
      "type": "basic"
    }
  ]
}
```

---

### Redoing with `Ctrl` + `y`

A commonly-used Windows shortcut is Redo, using the `Ctrl` and `y` shortcut. As before, the `Ctrl` and `y` combination is not actually assigned on OSX, so there's no obvious side-effect to assigning this shortcut to Find. Here's the complex rule I inserted into my `karabiner.json` file:

```
{
  "description": "Assign Ctrl + Y to Redo",
  "manipulators": [
    {
      "from": {
        "key_code": "y",
        "modifiers": {
          "mandatory": [
            "control"
          ]
        }
      },
      "to": {
        "key_code": "x",
        "modifiers": [
          "command",
          "shift"
        ]
      },
      "type": "basic"
    }
  ]
}
```

---

### Finding with `Ctrl` + `f`

A commonly-used Windows shortcut is Find, using the `Ctrl` and `f` shortcut. As before, the `Ctrl` and `f` combination is not actually assigned on OSX, so there's no obvious side-effect to assigning this shortcut to Find. Here's the complex rule I inserted into my `karabiner.json` file:

```
{
  "description": "Assign Ctrl + f to Find",
  "manipulators": [
    {
      "from": {
        "key_code": "f",
        "modifiers": {
          "mandatory": [
            "control"
          ]
        }
      },
      "to": {
        "key_code": "f",
        "modifiers": [
          "command"
        ]
      },
      "type": "basic"
    }
  ]
}
```

---

### Selecting All with `Ctrl` + `a`

A commonly-used Windows shortcut is Select All, using the `Ctrl` and `a` shortcut. The `Ctrl` and `a` combination is not actually assigned on OSX, so there's no obvious side-effect to assigning this shortcut to Select All. Here's the complex rule I inserted into my `karabiner.json` file:

```
{
  "description": "Assign Ctrl + a to Select All",
  "manipulators": [
    {
      "from": {
        "key_code": "a",
        "modifiers": {
          "mandatory": [
            "control"
          ]
        }
      },
      "to": {
        "key_code": "a",
        "modifiers": [
          "command"
        ]
      },
      "type": "basic"
    }
  ]
}
```

---

### Creating with `Ctrl` + `n`

A commonly-used Windows shortcut is New, using the `Ctrl` and `n` shortcut. As before, the `Ctrl` and `n` combination is not actually assigned on OSX, so there's no obvious side-effect to assigning this shortcut to New. Here's the complex rule I inserted into my `karabiner.json` file:

```
{
  "description": "Assign Ctrl + n to New",
  "manipulators": [
    {
      "from": {
        "key_code": "n",
        "modifiers": {
          "mandatory": [
            "control"
          ]
        }
      },
      "to": {
        "key_code": "n",
        "modifiers": [
          "command"
        ]
      },
      "type": "basic"
    }
  ]
}
```

---

### Print Screen Key

OSX's Print Screen function is useful, but requires a three-key (`⇧` + `⌘` + `3`) combination to execute the same functionality as a Windows keyboard's single `Print Screen` button. Mapping the OSX key combinations to the `Print Screen` key is as simple as adding another complex rule to the `karabiner.json` file; if you're not sure how to do that, revisit the [Manual Reassignment]({% post_url 2021-08-16-mapping-keyboards-to-osx-part-two %}#manual-reassignment) section of Part 2 of this topic.

There are six options for printing a Mac's screen; as you can see below, it takes a scarcely believable five keys to execute the fairly common command of printing a single window to clipboard. To execute some operations in Windows you must use a shortcut (`⊞` + `⇧` + `s`) to open Snip & Sketch, a program which has a native equivalent in OSX.

| Command                               | OSX Key Combination                        | Windows Key Combination |
|---------------------------------------|--------------------------------------------|-------------------------|
| Print the entire screen to clipboard. | `Ctrl` + `⇧` + `⌘` + `3`                   | `Print Screen`          |
| Print the entire screen to file.      | `⇧` + `⌘` + `3`                            | `⊞` + `Print Screen`    |
| Print a selected area to clipboard.   | `Ctrl` + `⇧` + `⌘` + `4`                   | `⊞` + `⇧` + `s`         |
| Print a selected area to file.        | `⇧` + `⌘` + `4`                            | `⊞` + `⇧` + `s`         |
| Print a selected window to clipboard. | `Ctrl` + `⇧` + `⌘` + `4`, then `Space Bar` | `Alt` + `Print Screen`  |
| Print a selected window to file.      | `⇧` + `⌘` + `4`, then `Space Bar`          | `⊞` + `⇧` + `s`         |

Here's the complex rule I inserted into my `karabiner.json` file:

```
{
  "description": "Remap print_screen",
  "manipulators": [
    {
      "from": {
        "key_code": "print_screen"
      },
      "to": {
        "key_code": "3",
        "modifiers": [
          "control",
          "shift",
          "command"
        ]
      },
      "type": "basic"
    },
    {
      "from": {
        "key_code": "print_screen",
        "modifiers": {
          "mandatory": [
            "left_option"
          ]
        }
      },
      "to": {
        "key_code": "3",
        "modifiers": [
          "shift",
          "command"
        ]
      },
      "type": "basic"
    },
    {
      "from": {
        "key_code": "s",
        "modifiers": {
          "mandatory": [
            "left_option",
            "shift"
          ]
        }
      },
      "to": {
        "key_code": "5",
        "modifiers": [
          "shift",
          "command"
        ]
      },
      "type": "basic"
    },
    {
      "from": {
        "key_code": "print_screen",
        "modifiers": {
          "mandatory": [
            "command"
          ]
        }
      },
      "to": {
        "key_code": "4",
        "modifiers": [
          "control",
          "shift",
          "command"
        ]
      },
      "type": "basic"
    }
  ]
}
```

---

### Home and End Keys

The OSX application of the `Home` and `End` keys will be befuddling to most Windows users. If you'd like to revert to `Home` sending you to the beginning of a line rather than the beginning of a document, and `End` sending you to the end of a line rather than the end of the document, and would like `⇧` to select the content between the cursor position and that point, you'll need to institute new complex rules. The existing and desired combinations are as follows:

| Command                                      | OSX Key Combination         | Windows Key Combination |
|----------------------------------------------|-----------------------------|-------------------------|
| Move cursor to beginning of line.            | `⌘` + `Left Arrow`          | `Home`                  |
| Move cursor to end of line.                  | `⌘` + `Right Arrow`         | `End`                   |
| Select from cursor to beginning of line.     | `⇧` + `⌘` + `Left Arrow`    | `⇧` + `Home`            |
| Select from cursor to end of line.           | `⇧` + `⌘` + `Right Arrow`   | `⇧` + `End`             |
| Move cursor to beginning of document.        | `Ctrl` + `Up Arrow`         | `Ctrl` + `Home`         |
| Move cursor to end of document.              | `Ctrl` + `Down Arrow`       | `Ctrl` + `End`          |
| Select from cursor to beginning of document. | `⇧` + `Ctrl` + `Up Arrow`   | `⇧` + `Ctrl` + `Home`   |
| Select from cursor to end of document.       | `⇧` + `Ctrl` + `Down Arrow` | `⇧` + `Ctrl` + `End`    |

```
{
  "description": "Remap home and end",
  "manipulators": [
    {
      "from": {
        "key_code": "home"
      },
      "to": {
        "key_code": "left_arrow",
        "modifiers": [
          "command"
        ]
      },
      "type": "basic"
    },
    {
      "from": {
        "key_code": "home",
        "modifiers": {
          "mandatory": [
            "shift"
          ]
        }
      },
      "to": {
        "key_code": "left_arrow",
        "modifiers": [
          "command",
          "shift"
        ]
      },
      "type": "basic"
    },
    {
      "from": {
        "key_code": "home",
        "modifiers": {
          "mandatory": [
            "control"
          ]
        }
      },
      "to": {
        "key_code": "up_arrow",
        "modifiers": [
          "command"
        ]
      },
      "type": "basic"
    },
    {
      "from": {
        "key_code": "home",
        "modifiers": {
          "mandatory": [
            "shift",
            "control"
          ]
        }
      },
      "to": {
        "key_code": "up_arrow",
        "modifiers": [
          "shift",
          "command"
        ]
      },
      "type": "basic"
    },
    {
      "from": {
        "key_code": "end"
      },
      "to": {
        "key_code": "right_arrow",
        "modifiers": [
          "command"
        ]
      },
      "type": "basic"
    },
    {
      "from": {
        "key_code": "end",
        "modifiers": {
          "mandatory": [
            "shift"
          ]
        }
      },
      "to": {
        "key_code": "right_arrow",
        "modifiers": [
          "command",
          "shift"
        ]
      },
      "type": "basic"
    },
    {
      "from": {
        "key_code": "end",
        "modifiers": {
          "mandatory": [
            "control"
          ]
        }
      },
      "to": {
        "key_code": "down_arrow",
        "modifiers": [
          "command"
        ]
      },
      "type": "basic"
    },
    {
      "from": {
        "key_code": "end",
        "modifiers": {
          "mandatory": [
            "shift",
            "control"
          ]
        }
      },
      "to": {
        "key_code": "down_arrow",
        "modifiers": [
          "shift",
          "command"
        ]
      },
      "type": "basic"
    }
  ]
}
```
---

### Lock Screen with `Ctrl` + `l`

A oxymoronically simple complex rule will replicate the Windows shortcut (`⊞` + `l`) used to lock your computer. Here's the complex rule I inserted into my `karabiner.json` file:

```
{
  "description": "Assign ⊞ + l to Lock Screen",
  "manipulators": [
    {
      "from": {
        "key_code": "l",
        "modifiers": {
          "mandatory": [
            "option"
          ]
        }
      },
      "to": {
        "key_code": "q",
        "modifiers": [
          "control",
          "command"
        ]
      },
      "type": "basic"
    }
  ]
}
```

---

### Reassigning Keys 4: Browser Shortcuts

There are situations where rather than applying reassignments by exclusion (that is, *unless* you're using a particular application), you'll want reassignment to apply by inclusion (that is, when you *are* using a particular application). A good example of this can be found in browser-specific shortcuts, where you'll want to apply reassignments to a handful of included applications, rather than to all applications and then having to exclude all applications that aren't browsers. A demonstration of reassignments of this type will be addressed in [Part 4]({% post_url 2021-10-01-mapping-keyboards-to-osx-part-four %}) of this series.
