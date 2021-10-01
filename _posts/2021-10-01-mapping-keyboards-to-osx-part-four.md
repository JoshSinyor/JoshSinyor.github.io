## Remapping Windows Keyboards to OSX: Part 4
### Part 4: Browser Shortcuts

Sometimes you'll want to limit the intervention of Karabiner Elements to certain applications. For example, Windows users will be used to using the `F5` key or a variety of keyboard shortcuts to refresh the pages of web browsers. OSX uses radically different combinations of keys, so complex remapping is required. However, I really only want this behaviour when I'm using a web browser; it's always possible some hitherto-unforeseen scenario will demand the normal function of these key combinations. To do this, I'll use conditions to apply the rule only when using specified applications - my web browsers.

---



---

### Browser Page Refresh Using `Control` + `R` or `F5`

Windows users will have it ingrained in them that the `Control` + `R` key combination or the `F5` key refreshes a webpage, and that the `Control` + `Shift` + `R` key combination or the `Control` and `F5` key combination hard refreshes a webpage. All of my browsers (Safari, Chrome and Firefox) can use the same OSX keyboard shortcut (`Command` + `R`) to do a refresh, so this rule can be applied to all browsers.

Here are the complex rules I inserted into my `karabiner.json` file:

```
{
  "description": "Assign control + r to Refresh (Browsers)",
  "manipulators": [
    {
      "conditions": [
        {
          "bundle_identifiers": [
            "^com\\.apple\\.Safari$",
            "^com\\.google\\.Chrome$",
            "^org\\.mozilla\\.firefox$"
          ],
          "type": "frontmost_application_if"
        }
      ],
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
},
{
  "description": "Assign f5 to Refresh (Browsers)",
  "manipulators": [
    {
      "conditions": [
        {
          "bundle_identifiers": [
            "^com\\.apple\\.Safari$",
            "^com\\.google\\.Chrome$",
            "^org\\.mozilla\\.firefox$"
          ],
          "type": "frontmost_application_if"
        }
      ],
      "from": {
        "key_code": "f5"
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

### Browser Page Hard Refresh Using `Control` + `Shift` + `R` or `Shift` + `F5`

Windows users will also be used hard-refreshing browser pages using the `Control` + `Shift` + `R` or `Shift` + `F5` key combinations. However, unlike my other OSX browsers, which use (`Shift` + `Command` + `R`) to hard refresh a page, Safari uses a different key combination (`Command` + `Option` + `R`). Therefore Safari must have separate rules.

Here are the complex rules I inserted into my `karabiner.json` file:

```
{
  "description": "Assign control + shift + r to Hard Refresh (Chrome & Firefox Browsers)",
  "manipulators": [
    {
      "conditions": [
        {
          "bundle_identifiers": [
            "^com\\.google\\.Chrome$",
            "^org\\.mozilla\\.firefox$"
          ],
          "type": "frontmost_application_if"
        }
      ],
      "from": {
        "key_code": "r",
        "modifiers": {
          "mandatory": [
            "shift",
            "control"
          ]
        }
      },
      "to": {
        "key_code": "r",
        "modifiers": [
          "shift",
          "command"
        ]
      },
      "type": "basic"
    }
  ]
},
{
  "description": "Assign shift + f5 to Hard Refresh (Chrome & Firefox Browsers)",
  "manipulators": [
    {
      "conditions": [
        {
          "bundle_identifiers": [
            "^com\\.google\\.Chrome$",
            "^org\\.mozilla\\.firefox$"
          ],
          "type": "frontmost_application_if"
        }
      ],
      "from": {
        "key_code": "f5",
        "modifiers": {
          "mandatory": [
            "shift"
          ]
        }
      },
      "to": {
        "key_code": "r",
        "modifiers": [
          "shift",
          "command"
        ]
      },
      "type": "basic"
    }
  ]
},
{
  "description": "Assign control + shift + r to Hard Refresh (Safari Browser)",
  "manipulators": [
    {
      "conditions": [
        {
          "bundle_identifiers": [
            "^com\\.apple\\.Safari$"
          ],
          "type": "frontmost_application_if"
        }
      ],
      "from": {
        "key_code": "r",
        "modifiers": {
          "mandatory": [
            "shift",
            "control"
          ]
        }
      },
      "to": {
        "key_code": "r",
        "modifiers": [
          "option",
          "command"
        ]
      },
      "type": "basic"
    }
  ]
},
{
  "description": "Assign shift + f5 to Hard Refresh (Safari Browser)",
  "manipulators": [
    {
      "conditions": [
        {
          "bundle_identifiers": [
            "^com\\.apple\\.Safari$"
          ],
          "type": "frontmost_application_if"
        }
      ],
      "from": {
        "key_code": "f5",
        "modifiers": {
          "mandatory": [
            "shift"
          ]
        }
      },
      "to": {
        "key_code": "r",
        "modifiers": [
          "option",
          "command"
        ]
      },
      "type": "basic"
    }
  ]
}
```

---

That about sums up my use of Karabiner Elements to crosslink my external keyboard to my MacBook Air. I hope you've found this a useful aggregate of disparate and poorly-documented information - good luck!
