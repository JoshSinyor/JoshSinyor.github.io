## Remapping Windows Keyboards to OSX: Part 3
### Part 3: Windows Shortcuts

Karabiner Elements' Preferences pane has a 'Complex modifications' tab capable of implementing complex rules of key reassignment. This is useful if you'd like to execute complex key reassignment (e.g. [partially reassigning keys]({% post_url 2021-08-16-mapping-keyboards-to-osx-part-two %}#partial-reassignment) or [partially disabling keys]({% post_url 2021-08-16-mapping-keyboards-to-osx-part-two %}#partial-disabling)), but mimicking some of the most common Windows key behaviour requires special combinations of rules.

Some commonly desired rules are provided below.

---

- [Saving with `Control` + `S`](#saving-with-control--s)
- [Browser Page Refresh Using `F5`](#browser-page-refresh-using-f5)
- [Browser Page Refresh Using `Control` + `R`](#browser-page-refresh-using-control--r)
- [Print Screen Key](#print-screen-key)
- [Home and End Keys](#home-and-end-keys)

---

### Saving with `Control` + `S`

One of the most commonly-used Windows shortcuts is Save, using the `Control` and `S` shortcut. The `Control` and `S` combination is not actually assigned on Mac, so there's no obvious downside to assigning this shortcut to Save. Here's the complex rule I inserted into my `karabiner.json` file:

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

### Browser Page Refresh Using `F5`

Windows users will have it ingrained in them that the `F5` key refreshes a webpage, and that `Control` and `F5` hard refreshes a webpage. On a MacBook the `F5` and `F6` keys respectively decrease and increase the brightness of the keyboard backlighting, a feature my Windows keyboard does not even have. By default Karabiner Elements assigns a function I rarely use (Dictation) to the `F5` key and nothing at all to the `F6` key. I'll remove the Dictation shortcut, and reassign `F5` to refreshing duties.

I really only want this behaviour when I'm using a web browser; it's always possible there will be a hitherto-unforeseen scenario where I do actually want the default `F5` and `F6` behaviour.

---

### Browser Page Refresh Using `Control` + `R`

Windows users will also be used to refreshing browser webpages using `Control` + `R`, and hard-refreshing browser pages using `Control` + `Shift` + `R`. Once again, I really only want this behaviour when I'm using a web browser. Here's the complex rule I inserted into my `karabiner.json` file:

```
{
  "description": "Assign control + r to Refresh",
  "conditions": [
    {
      "bundle_identifiers": [
        "^com\\.apple\\.Safari2$",
        "^com\\.google\\.Chrome$",
        "^org\\.mozilla\\.Firefox$",
        "^org\\.torproject\\.Torbrowser$"
      ],
      "type": "frontmost_application_if"
    }
  ],
  "manipulators": [
    {
      "from": {
        "key_code": "r",
        "modifiers": {
          "mandatory": [
            "control"
          ]
        }
      },
      "to": {
        "key_code": "r",
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

Apple's Print Screen function is useful, but requires a three-key (`Shift` + `Command` + `3`) combination to execute the same functionality as a Windows keyboard's single `Print Screen` button. Mapping the Mac key combinations to the `Print Screen` key is as simple as adding another complex rule to the `karabiner.json` file; if you're not sure how to do that, revisit the [Manual Reassignment]({% post_url 2021-08-16-mapping-keyboards-to-osx-part-two %}#manual-reassignment) section of Part 2 of this topic.

There are six options for printing a Mac's screen; as you can see below, it takes a scarcely believable five keys to execute the fairly common command of printing a single window to clipboard. To execute some operations in Windows you must use a shortcut (`Windows Key` + `Shift` + `S`) to open Snip & Sketch, a program which has a native equivalent in OSX.

| Command                               | Mac Key Combination                                     | Windows Key Combination        |
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

The Apple application of the `Home` and `End` keys will be befuddling to most Windows users. If you'd like to revert to `Home` sending you to the beginning of a line rather than the beginning of a document, and `End` sending you to the end of a line rather than the end of the document, and would like `Shift` to select the content between the cursor position and that point, you'll need to institute new complex rules. The existing and desired combinations are as follows:

| Command                                      | Mac Key Combination                 | Windows Key Combination      |
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

That about sums up my use of Karabiner Elements to crosslink my external keyboard to my MacBook Air. I hope you've found this a useful aggregate of disparate and poorly-documented information - good luck!
