## Remapping Windows Keyboards to OSX: Part 1
### Part 1: Identifying Keys

Many people new to programming - including me - learn their lessons on a Mac. MacBooks, with their uniform hardware and UNIX-based operating system, are beloved by coding schools. That said, there's no two ways about it: Apple's keyboards are not the choice of champions. In addition to being costly (even the most basic Magic Keyboard is £99; the full-fat version is an eye-watering £179), like most OEMs Apple's 'boards use membrane switches, giving a mushy press and return. Modern typists after a definitive click, or a particularly obnoxious typewriter-like rattle, have returned to the noisy embrace of the mechanical keyboard.

Maybe you want to use a mechanical keyboard to annoy the neighbours, maybe you want to use some familiar Windows shortcuts, maybe you just don't want to shell out for Apple's seal of approval; either way, you want to use something outside Tim Cook's playground. As you've already discovered this isn't as simple as it should be, but you can be reassured that it's not impossible either.

---

### Table of Contents

- [Keyboard Layouts](#keyboard-layouts)
- [Software](#software)
- [Identifying Keys](#identifying-keys)
- [Reassigning Keys 1: Simple Rules](#reassigning-keys-1-simple-rules)
  * [Switching the Command & Option Keys](#switching-the-command--option-keys)
  * [Disabling Keys](#disabling-keys)
- [Reassigning Keys 2: Complex Rules](#reassigning-keys-2-complex-rules)

---

### Keyboard Layouts

There are a number of different types of Mac keyboard, and limitless configurations of Windows ones. For the purposes of this post, I'm mapping a tenkeyless UK-pattern ISO keyboard to an American-market ANSI MacBook Air keyboard. Here are my relative layouts:

![MacBook Air Keyboard](/images/2021-08-12/wikipedia_kb_mac_us_english.svg)
<p align="center"><i>MacBook Air International English Keyboard</i></p>

![Windows TKL Keyboard](/images/2021-08-12/wikipedia_kb_windows_uk_english.svg)
<p align="center"><i>ISO TKL UK English Keyboard</i></p>

<p align="center"><small><i>Images modified from Wikipedia and licensed under <a href="https://creativecommons.org/licenses/by-sa/3.0/legalcode">CC BY-SA 3.0</a></i></small></p>

Here I've highlighted the keys that don't map correctly when the keyboard is plugged into the Mac.

![Windows TKL Keyboard Highlighted](/images/2021-08-12/wikipedia_kb_windows_uk_english_highlighted.svg)

The red keys don't correspond to the Mac's layout, or don't function at all - the Home and End keys do function in the Mac style, but not in the far more useful Windows style; more on that and other common Windows shortcuts in part 3.

---

### Software

You'll need a copy of [Karabiner-Elements](https://karabiner-elements.pqrs.org/), a free open-source application intended for exactly this purpose. Check that your version of OSX is supported (anything after 10.15.6 should be OK), that you have Administrator-level permissions on your computer, and download the application from their website. It installs like any other app, but requires permissions you'll have to approve manually. Follow the [official instructions](https://karabiner-elements.pqrs.org/docs/getting-started/installation/) carefully to ensure the app has the permissions it needs.

Karabiner-Element installs two distinct programs:
- **Elements**, where keys are reassigned according to rules, and
- **Event Viewer**, where a keylogger shows the assigned names and flags of keys pressed on any connected keyboard.

---

### Identifying Keys

Each key on a keyboard has a specific name. The number keys are "1" through "0", the letters "a" through "z". The modifier, arrow, keypad, international and control and symbol keys are named more specifically. For example, the caps lock key is "caps_lock", the up arrow key "up_arrow", the keypad number 1 key "keypad_1", and the backspace key "delete_or_backspace".

To remap the keys, you need to know their names. Open Event Viewer, and start typing. Here's what happens when I type `hello` on my MacBook's internal keyboard:

![Event Viewer 01](/images/2021-08-12/event_viewer_01.png)

In the first column is the Event Type; the press (`down`) and release (`up`) of each key. In the second column is the Human Interface Device (HID) code of the key; both the internal and external keyboard are prefixed `7`, and the letter h is `11`, the letter e `8` and so on. In the third is the Name of the key; the letter h is `"key_code":"h"`, the letter e `"key_code":"e"` and so on.

Let's revisit some of the buttons that don't match up. Event Viewer identifies these as follows:

![Windows TKL Keyboard Event Viewer](/images/2021-08-12/wikipedia_kb_windows_uk_english_highlighted_event_viewer.svg)

This process can be repeated on the Apple keyboard to establish the names of the keys the mismatched keys *should* correspond to.

---

### Reassigning Keys 1: Simple Rules

Armed with the names of the keys that should be matched up, we can begin remapping the keyboard. The most straightforward remapping - key-for-key - is done through Karabiner-Element's GUI.

#### Switching the Command & Option Keys

The most obvious switch most users will wish to prioritise is reassigning the Command and Option keys, which are inverted on Mac and Windows keyboards.

![Keyboard Option & Command](/images/2021-08-12/wikipedia_kb_command_and_option.svg)

This, and other simple key remapping, is readily achieved in Karabiner Elements. Open the application, and select Preferences. Select the 'Simple modifications' tab. Select the external keyboard from the 'Target Device' drop-down menu. Click the 'Add Item' button, and select the key you want to reassign from the 'From key' drop-down menu. Then select the key you want to reassign it to from the 'To key' drop-down menu, and you're done.

![Karabiner Elements Option & Command](/images/2021-08-12/karabiner_elements_01.png)

You can repeat this for any other keys you want to reassign like-for-like.

#### Disabling Keys

Disabling keys is another simple feature supported by the 'Simple modifications' tab. There are several keys on the typical Windows keyboard that are superfluous on a basic Mac layout. Some you might want to reassign to more complex modes, others conflict with your Windows keyboard muscle memory. I chose to disable the `Scroll Lock` and `Pause` keys, which have extremely limited utility even in Windows use. Again, it's a simple case of selecting the key you wish to reassign, and assigning it to `vk_none (disable this key)`.

---

### Reassigning Keys 2: Complex Rules

The most common situation requiring complex rules are conditional reassignments - e.g. where the combination of a key and a modifier key needs to be reassigned, but the key alone does not. This will be addressed in Part 2 of this series.
