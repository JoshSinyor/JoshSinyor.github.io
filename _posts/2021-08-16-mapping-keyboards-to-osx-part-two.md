## Remapping Windows Keyboards to OSX: Part 2
### Part 2: Complex Rules

Karabiner Elements' Preferences pane has a 'Simple modifications' tab capable of simple key reassignment. This is useful if you'd like to switch keys like-for-like (e.g. [reversing the Command and Option keys](/_posts/2021-08-12-mapping-keyboards-to-osx-part-one#switching-the-command--option-keys), or [disabling a key altogether](/_posts/2021-08-12-mapping-keyboards-to-osx-part-one#disabling-keys))

---

### Table of Contents

---

### Importing Complex Rules

Manual addition of complex rules to Karabiner Elements is surprisingly difficult. Instead of dumping users into the deep end, Karabiner Elements allows them to import rulesets uploaded by other users. Instructions for this [are provided](https://karabiner-elements.pqrs.org/docs/manual/configuration/configure-complex-modifications/).

However, none of these rulesets suit my desired configuration. I'll have to write my own complex rules to match my keys.

---

### Manual Reassignment

To edit keys manually, you must directly manipulate the `karabiner.json` file. The default location is `~/.config/karabiner.json`. A link to the `.config` folder is conveniently provided on the Misc tab of the Preferences pane.

![Config Folder Link](/images/2021-08-16/karabiner_elements_01.png)

Opening the `karabiner.json` file reveals a typical `.json` format.

#### Partial Reassignment

The most common situation requiring complex rules are conditional reassignments - e.g. where the combination of a key and a modifier key needs to be reassigned, but the key alone does not. A good example of this on my keyboard is the `2` key; while this is the same on the Mac keyboard, the `2` + `Shift` combination on my keyboard should correspond to `"`, but instead corresponds to `@` on the Mac keyboard. I need to reassign the `2` key only when it is flagged with `Shift`.
