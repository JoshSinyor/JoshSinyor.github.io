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

1. Printing the entire screen (`Shift` + `Command` + `3`)
2. Printing a selected area (`Shift` + `Command` + `4`)
3. Printing an individual window (`Shift` + `Command` + `4` + `Space`)

This requires reassigning these key combinations to the `Print Screen` button on the external keyboard. I chose to combine them, in order, as:

1. `Shift` + `Print Screen`
2. `Print Screen`
3. `Print Screen` + `Space`

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

---

That about sums up my use of Karabiner Elements to crosslink my external keyboard to my MacBook Air. I hope you've found this a useful aggregate of disparate and poorly-documented information - good luck!
