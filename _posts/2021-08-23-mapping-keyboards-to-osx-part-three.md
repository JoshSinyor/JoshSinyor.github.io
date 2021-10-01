## Remapping Windows Keyboards to OSX: Part 3
### Part 3: Windows Shortcuts

Karabiner Elements' Preferences pane has a 'Complex modifications' tab capable of implementing complex rules of key reassignment. This is useful if you'd like to execute complex key reassignment (e.g. [partially reassigning keys]({% post_url 2021-08-16-mapping-keyboards-to-osx-part-two %}#partial-reassignment) or [partially disabling keys]({% post_url 2021-08-16-mapping-keyboards-to-osx-part-two %}#partial-disabling)), but mimicking some of the most common Windows key behaviour requires special combinations of rules.

⚠️ **It's important when reassigning combinations of keys to ensure that you understand the behaviour [that OSX might already assign](https://support.apple.com/en-gb/HT201236) to that shortcut.** If you're substituting different behaviour for a key combination that is already an OSX shortcut you may wish to reassign the OSX shortcut to a different, unassigned combination.

Some commonly desired rules are provided below.

---

- [Saving with `Control` + `S`](#saving-with-control--s)
- [Copying with `Control` + `C`](#copying-with-control--c)
- [Cutting with `Control` + `X`](#cutting-with-control--x)
- [Pasting with `Control` + `V`](#pasting-with-control--v)
- [Finding with `Control` + `F`](#finding-with-control--f)
- [Print Screen Key](#print-screen-key)
- [Home and End Keys](#home-and-end-keys)
- [Lock Computer](#lock-computer)
- [Reassigning Keys 4: Browser Shortcuts](#reassigning-keys-4-browser-shortcuts)

---

### Saving with `Control` + `S`

One of the most commonly-used Windows shortcuts is Save, using the `Control` and `S` shortcut. The `Control` and `S` combination is not actually assigned on OSX, so there's no obvious downside to assigning this shortcut to Save. Here's the complex rule I inserted into my `karabiner.json` file:

```
{
  "description": "Assign control + s to Save",
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

### Copying with `Control` + `C`

Another one of the most commonly-used Windows shortcuts is Copy, using the `Control` and `C` shortcut. As before, the `Control` and `C` combination is not actually assigned on OSX, so there's no obvious downside to assigning this shortcut to Copy.

⚠️ **While the `Control` + `C` combination is not assigned by OSX, it is assigned by some common programs, especially terminal applications.** These applications must be excluded from the operation of this rule.

⚠️ **To disable an application with an internal terminal window (e.g. Atom, with the platformio-ide-terminal package installed) the entire application must be excluded.**

Here's the complex rule I inserted into my `karabiner.json` file:

```
{
  "description": "Assign control + c to Copy",
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

### Cutting with `Control` + `X`

---

### Pasting with `Control` + `V`

Another one of the most commonly-used Windows shortcuts is Paste, using the `Control` and `V` shortcut. As before, the `Control` and `V` combination is not actually assigned on OSX, so there's no obvious downside to assigning this shortcut to Paste.

⚠️ **As before, while the `Control` + `C` combination is not assigned by OSX, it is assigned by some common programs, especially terminal applications and applications with an internal terminal.** These applications must be excluded from the operation of this rule.

Here's the complex rule I inserted into my `karabiner.json` file:

```
{
  "description": "Assign control + v to Paste",
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

### Finding with `Control` + `F`

A commonly-used Windows shortcuts is Find, using the `Control` and `F` shortcut. As before, the `Control` and `F` combination is not actually assigned on OSX, so there's no obvious downside to assigning this shortcut to Paste. Here's the complex rule I inserted into my `karabiner.json` file:

```
{
  "description": "Assign control + f to Find",
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

### Print Screen Key

OSX's Print Screen function is useful, but requires a three-key (`Shift` + `Command` + `3`) combination to execute the same functionality as a Windows keyboard's single `Print Screen` button. Mapping the OSX key combinations to the `Print Screen` key is as simple as adding another complex rule to the `karabiner.json` file; if you're not sure how to do that, revisit the [Manual Reassignment]({% post_url 2021-08-16-mapping-keyboards-to-osx-part-two %}#manual-reassignment) section of Part 2 of this topic.

There are six options for printing a Mac's screen; as you can see below, it takes a scarcely believable five keys to execute the fairly common command of printing a single window to clipboard. To execute some operations in Windows you must use a shortcut (`Windows Key` + `Shift` + `S`) to open Snip & Sketch, a program which has a native equivalent in OSX.

| Command                               | OSX Key Combination                                     | Windows Key Combination        |
|---------------------------------------|---------------------------------------------------------|--------------------------------|
| Print the entire screen to clipboard. | `Control` + `Shift` + `Command` + `3`                   | `Print Screen`                 |
| Print the entire screen to file.      | `Shift` + `Command` + `3`                               | `Windows Key` + `Print Screen` |
| Print a selected area to clipboard.   | `Control` + `Shift` + `Command` + `4`                   | `Windows Key` + `Shift` + `S`  |
| Print a selected area to file.        | `Shift` + `Command` + `4`                               | `Windows Key` + `Shift` + `S`  |
| Print a selected window to clipboard. | `Control` + `Shift` + `Command` + `4`, then `Space Bar` | `Alt` + `Print Screen`         |
| Print a selected window to file.      | `Shift` + `Command` + `4`, then `Space Bar`             | `Windows Key` + `Shift` + `S`  |

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

The OSX application of the `Home` and `End` keys will be befuddling to most Windows users. If you'd like to revert to `Home` sending you to the beginning of a line rather than the beginning of a document, and `End` sending you to the end of a line rather than the end of the document, and would like `Shift` to select the content between the cursor position and that point, you'll need to institute new complex rules. The existing and desired combinations are as follows:

| Command                                      | OSX Key Combination                 | Windows Key Combination      |
|----------------------------------------------|-------------------------------------|------------------------------|
| Move cursor to beginning of line.            | `Command` + `Left Arrow`            | `Home`                       |
| Move cursor to end of line.                  | `Command` + `Right Arrow`           | `End`                        |
| Select from cursor to beginning of line.     | `Shift` + `Command` + `Left Arrow`  | `Shift` + `Home`             |
| Select from cursor to end of line.           | `Shift` + `Command` + `Right Arrow` | `Shift` + `End`              |
| Move cursor to beginning of document.        | `Control` + `Up Arrow`              | `Control` + `Home`           |
| Move cursor to end of document.              | `Control` + `Down Arrow`            | `Control` + `End`            |
| Select from cursor to beginning of document. | `Shift` + `Control` + `Up Arrow`    | `Shift` + `Control` + `Home` |
| Select from cursor to end of document.       | `Shift` + `Control` + `Down Arrow`  | `Shift` + `Control` + `End`  |

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

### Lock Computer

A relatively simple complex rule will replicate the Windows shortcut used to lock your computer.

---

### Reassigning Keys 4: Browser Shortcuts

There are some situations where you'll only want reassignment to apply when you're using certain applications. Windows users will find Commonly desired modifications of this type will be addressed in [Part 4]({% post_url 2021-10-01-mapping-keyboards-to-osx-part-four %}) of this series.
