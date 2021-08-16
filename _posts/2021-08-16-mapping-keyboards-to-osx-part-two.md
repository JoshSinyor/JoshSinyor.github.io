## Remapping Windows Keyboards to OSX: Part 2
### Part 2: Complex Rules

Karabiner Elements' Preferences pane has a 'Simple modifications' tab capable of simple key reassignment. This is useful if you'd like to switch keys like-for-like (e.g. [reversing the Command and Option keys](/_posts/2021-08-12-mapping-keyboards-to-osx-part-one#switching-the-command--option-keys), or [disabling a key altogether](/_posts/2021-08-12-mapping-keyboards-to-osx-part-one#disabling-keys)) but incapable of dealing with more complex situations, like combinations of keystrokes, or partial reassingment of keys.

---

### Table of Contents

- [Importing Complex Rules](#importing-complex-rules)
- [Manual Reassignment](#manual-reassignment)
  * [Partial Reassignment](#partial-reassignment)
  * [Partial Disabling](#partial-disabling)
  * [Multiple Complex Rules](#multiple-complex-rules)
- [Reassigning Keys 3: Windows Key Functions](#reassigning-keys-3-windows-key-functions)

---

### Importing Complex Rules

Manual addition of complex rules to Karabiner Elements is surprisingly difficult. Instead of dumping users into the deep end, Karabiner Elements allows them to import rulesets uploaded by other users. Instructions for this [are provided](https://karabiner-elements.pqrs.org/docs/manual/configuration/configure-complex-modifications/).

However, none of these rulesets suit my desired configuration. I'll have to write my own complex rules to match my keys.

---

### Manual Reassignment

To edit keys manually, you must directly manipulate the `karabiner.json` file. The default location is `~/.config/karabiner.json`. A link to the `.config` folder is conveniently provided on the Misc tab of the Preferences pane.

![Config Folder Link](/images/2021-08-16/karabiner_elements_01.png)

Opening the `karabiner.json` file reveals a typical `.json` format.

![Karabiner JSON File Overview](/images/2021-08-16/karabiner_json_01.png)

Manually adding complex rules is as simple as adding the desired reassignment to the `karabiner.json` file.

#### Partial Reassignment

The most common situation requiring complex rules are conditional reassignments - e.g. where the combination of a key and a modifier key needs to be reassigned, but the key alone does not. A good example of this on my keyboard is the `2` key; while this is the same on the Mac keyboard, the `2` + `Shift` combination on my keyboard should correspond to `"`, but instead corresponds to `@` on the Mac keyboard. Karabiner Elements' Simple modifications aren't suitable - I don't want to reassign the `2` key to a different key. I need to reassign the `2` key only when it is pressed in conjunction with a `shift` key.

Here's an example of how it works:

```
{
  "description": "Remap 2",
  "manipulators": [
    {
      "from": {
      "key_code": "2",
      "modifiers": {
        "mandatory": [
          "shift"
        ]
      }
      },
      "to": {
        "key_code": "quote",
        "modifiers": [
          "shift"
        ]
      },
      "type": "basic"
    }
  ]
}
```
Here, the external keyboard's `2` key (`"key_code":"2"`, as per the previous post), with the mandatory modification of either `shift` key, is reassigned to the Mac's `quote` key (`"key_code":"quote"`), with the modification of either `shift` key. Pressing the `2` and either `shift` keys now returns the same outcome as pressing the `quote` and either `shift` keys on the Mac's keyboard.

Custom complex rules are inserted into the `karabiner.json` file in the `"complex_modifications": {rules": []}` section.

![Karabiner JSON File Rules](/images/2021-08-16/karabiner_json_02.png)

You can view the new complex rule in the Rules tab of the Complex modifications tab of the Karabiner Elements Preferences pane.

![Complex Rule Tab](/images/2021-08-16/karabiner_elements_02.png)

#### Partial Disabling

The `command` key modifier provides alternate output for many of the Mac keyboard's keys. Some of these are unexpected or unhelpful. Disabling them using complex rules is similar to using simple rules - the reassignment of some combination of keys to `vk_none`. For example, to assign `2` with the modification of either `control` key (`™` by default) to `vk_none`:

```
{
  "description": "Remap 2",
  "manipulators": [
      {
          "from": {
              "key_code": "2",
              "modifiers": {
                  "mandatory": [
                      "option"
                  ]
              }
          },
          "to": {
              "key_code": "vk_none"
          },
          "type": "basic"
      }
  ]
}
```

#### Multiple Complex Rules

Multiple complex rules can be stacked on a single key. For example, to simultaneously apply both of two rules listed above for the `2` keys, simply insert a comma between them:

```
{
  "description": "Remap 2",
  "manipulators": [
      {
          "from": {
              "key_code": "2",
              "modifiers": {
                  "mandatory": [
                      "shift"
                  ]
              }
          },
          "to": {
              "key_code": "quote",
              "modifiers": [
                  "shift"
              ]
          },
          "type": "basic"
      },
      {
          "from": {
              "key_code": "2",
              "modifiers": {
                  "mandatory": [
                      "option"
                  ]
              }
          },
          "to": {
              "key_code": "vk_none"
          },
          "type": "basic"
      }
  ]
},
```

---

### Reassigning Keys 3: Windows Key Functions

There are some key combinations from Windows that will be so ingrained in your muscle memory - or so conspicuously lacking from Mac's default function - that you'll want to add them over and above the basic reassignment. For example, the default behaviour of the `Home` and `End` keys - especially when it comes to highlighting text using a `shift` modifier (default behaviour: skipping to the beginning and end of files, rather than the beginning or end of the line) feels inadequate after using the default Windows option. The three-key combination required to execute a Print Screen command seems wildly excessive when a typical Windows keyboard will have a dedicated button. Commonly desired modifications of this type will be addressed in [Part 3 of this series](/_posts/2021-08-16-mapping-keyboards-to-osx-part-two).
