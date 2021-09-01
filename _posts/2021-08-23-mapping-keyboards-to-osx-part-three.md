## Remapping Windows Keyboards to OSX: Part 3
### Part 3: Windows Key Functions

Karabiner Elements' Preferences pane has a 'Complex modifications' tab capable of implementing complex rules of key reassignment. This is useful if you'd like to execute complex key reassignment (e.g. [partially reassigning keys]({% post_url 2021-08-16-mapping-keyboards-to-osx-part-two %}#partial-reassignment) or [partially disabling keys]({% post_url 2021-08-16-mapping-keyboards-to-osx-part-two %}#partial-disabling)), but mimicking some of the most common Windows key behaviour requires special combinations of rules.

Some commonly desired rules are provided below.

---

- [Page Refresh Using the F5 Key](#page-refresh-using-the-f5-key)
- [Print Screen Key](#print-screen-key)
- [Home and End Keys](#home-and-end-keys)

---

### Page Refresh Using the F5 Key

Windows users will have it ingrained in them that the `F5` key refreshes a webpage.

---

### Print Screen Key

Apple's Print Screen function is useful, but requires a three-key (`Shift` + `Command` + `3`) combination to execute the same functionality as a Windows keyboard's single `Print Screen` button. Mapping the Mac key combinations to the `Print Screen` key is as simple as adding another complex rule to the `karabiner.json` file; if you're not sure how to do that, revisit the [Manual Reassignment]({% post_url 2021-08-16-mapping-keyboards-to-osx-part-two %}#manual-reassignment)] section of Part 2 of this topic. There are three options for printing a Mac's screen:

|                         | Mac Key Combination                     | Desired Key Combination      |
|-------------------------|-----------------------------------------|------------------------------|
| Print the entire screen | `Shift` + `Command` + `3`               | `Shift` + `Print Screen`     |
| Print a selected area   | `Shift` + `Command` + `4`               | `Print Screen`               |
| Print a selected window | `Shift` + `Command` + `4`, then `Space` | `Print Screen`, then `Space` |

Here's the complex rule I inserted into my `karabiner.json` file:

```
{
  "description": "Remap print_screen",
  "manipulators": [
    {
      "from": {
        "key_code": "print_screen",
        "modifiers": {
          "mandatory": [
            "shift"
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
        "key_code": "print_screen",
      },
      "to": {
        "key_code": "4",
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

### Home and End Keys

The Apple application of the `Home` and `End` keys will be befuddling to most Windows users. If you'd like to revert to `Home` sending you to the beginning of a line rather than the beginning of a document, and `End` sending you to the end of a line rather than the end of the document, and would like `Shift` to select the content between the cursor position and that point, you'll need to institute new complex rules. The existing and desired combinations are as follows:

| Command                                      | Mac Key Combination                 | Desired Key Combination      |
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
