## Remapping Windows Keyboards to OSX: Part 4
### Part 4: Browser Shortcuts

Sometimes you'll want to limit the intervention of Karabiner Elements to certain applications. For example, Windows users will be used to using the `F5` key or a variety of keyboard shortcuts to refresh the pages of web browsers. OSX uses radically different combinations of keys, so complex remapping is required. However, I really only want this behaviour when I'm using a web browser; it's always possible some hitherto-unforeseen scenario will demand the normal function of these key combinations. To do this, I'll use conditions to apply the rule only when using specified applications - my web browsers.

⚠️ **It's important when reassigning combinations of keys to ensure that you understand the behaviour [that OSX might already assign](https://support.apple.com/en-gb/HT201236) to that shortcut.** If you're substituting different behaviour for a key combination that is already an OSX shortcut, you may wish to reassign the OSX shortcut to a different, unassigned combination.

⚠️ **It's important when reassigning combinations of keys to ensure that you understand the behaviour that applications may already assign to that shortcut.** If you're substituting different behaviour for a key combination that is already an application-specific shortcut, you may wish not to include the application in the rule.

---

- [Including Applications in Rules](#including-applications-in-rules)
- [Browser Page Refresh Using `Ctrl` + `r` or `F5`](#browser-page-refresh-using-ctrl--r-or-f5)
- [Browser Page Hard Refresh Using `Ctrl` + `⇧` + `r` or `⇧` + `F5`](#browser-page-hard-refresh-using-ctrl----r-or---f5)

---

### Including Applications in Rules

We've previously discussed using the `frontmost_application_if` condition to [exclude a few programs from a rule]({% post_url 2021-08-23-mapping-keyboards-to-osx-part-three %}#excluding-applications-from-rules). In situations where instead of a rule applying *except* to specific applications a rule should apply *only* to specific applications, Karabiner Elements provides the [`frontmost_application_unless` condition](https://karabiner-elements.pqrs.org/docs/json/complex-modifications-manipulator-definition/conditions/frontmost-application/). A simple example of using `frontmost_application_if` to include browser applications is provided in the [Browser Page Refresh Using `Ctrl` + `r` or `F5`](#copying-with-ctrl--r-or-f5) section below.

---

### Browser Page Refresh Using `Ctrl` + `r` or `F5`

Windows users will have it ingrained in them that the `Ctrl` + `r` key combination or the `F5` key refreshes a webpage, and that the `Ctrl` + `⇧` + `r` key combination or the `Ctrl` and `F5` key combination hard refreshes a webpage. All of my browsers (Safari, Chrome and Firefox) can use the same OSX keyboard shortcut (`⌘` + `r`) to do a refresh, so this rule can be applied to all browsers.

Here are the complex rules I inserted into my `karabiner.json` file:

```
{
  "description": "Assign Ctrl + r to Refresh (Browsers)",
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
  "description": "Assign F5 to Refresh (Browsers)",
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

### Browser Page Hard Refresh Using `Ctrl` + `⇧` + `r` or `⇧` + `F5`

Windows users will also be used hard-refreshing browser pages using the `Ctrl` + `⇧` + `r` or `⇧` + `F5` key combinations. However, unlike my other OSX browsers, which use (`⇧` + `⌘` + `r`) to hard refresh a page, Safari uses a different key combination (`⌘` + `⌥` + `r`). Therefore Safari must have separate rules.

Here are the complex rules I inserted into my `karabiner.json` file:

```
{
  "description": "Assign Ctrl + ⇧ + r to Hard Refresh (Chrome & Firefox Browsers)",
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
  "description": "Assign ⇧ + F5 to Hard Refresh (Chrome & Firefox Browsers)",
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
  "description": "Assign Ctrl + ⇧ + r to Hard Refresh (Safari Browser)",
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
  "description": "Assign ⇧ + F5 to Hard Refresh (Safari Browser)",
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
