# MacroDroid macro: Restart the phone every Friday at 4:00 AM

A MacroDroid macro has three parts: a **trigger**, **actions** and **constraints**.
This macro restarts the phone once a week, early on Friday morning.

```
Macro name : Weekly Friday Restart
Trigger    : Day/Time Trigger → 04:00, Friday only
Actions    : 1. Notification "Restarting in 1 minute"   (optional)
             2. Wait 60 seconds                          (optional)
             3. Restart the device  (pick ONE option from section 3 below)
Constraints: Battery level > 20%                         (optional)
```

---

## 1. Create the macro

1. Open MacroDroid → **Add Macro** (the `+` button).
2. Name it **Weekly Friday Restart**.

## 2. Add the trigger

1. Tap **Triggers +** → **Date/Time** → **Day/Time Trigger**.
2. Set the time to **04:00**.
3. Untick every day except **Friday**.
4. Tap **OK**.

## 3. Add the restart action

Android only lets privileged apps reboot the phone. Choose the option that matches your phone.

### Option A: Rooted phone (most reliable)

**Actions +** → **Device Actions** → **Reboot / Power Off** → choose **Reboot**.

You can also use **Actions +** → **Applications** → **Shell Script**, tick **Root**, and enter:

```sh
reboot
```

### Option B: No root, with Shizuku or ADB permissions

If you have set up Shizuku, or granted MacroDroid elevated permissions over ADB, use
**Actions +** → **Applications** → **Shell Script** and choose the Shizuku/ADB mode:

```sh
svc power reboot
```

(If that is rejected, try `cmd power reboot` or `reboot`.)

### Option C: No root (uses Accessibility to tap the power menu)

This works on most stock phones. MacroDroid opens the power menu and taps "Restart" itself.

1. **Actions +** → **Device Actions** → **UI Interaction** → **Global Action** → **Power Dialog**
   (on some versions this appears as *"Show power menu"*).
2. **Actions +** → **Logic/Flow** (or **MacroDroid Specific**) → **Wait Before Next Action** → **2 seconds**.
3. **Actions +** → **Device Actions** → **UI Interaction** → **Click** → **Text content** → type `Restart`
   (use the exact word your phone shows, e.g. `Reboot` on some Samsung or Xiaomi phones).
4. Samsung and some other phones ask you to confirm with a second tap. If yours does, add another
   **Wait 2 seconds** and a second **UI Interaction → Click → Text content → `Restart`**.

You must enable the **MacroDroid UI Interaction** accessibility service when MacroDroid asks for it.

> Option C needs the screen to be usable. If the phone has a PIN, pattern or password lock, the
> power menu may still work from the lock screen, but some phones block it when locked. Test it with
> the phone locked before you rely on it.

## 4. Optional extras

- **Warning first:** before the restart action, add **Notification → Display Notification**
  ("Phone will restart in 1 minute") and **Wait Before Next Action → 60 seconds**.
- **Skip on low battery:** **Constraints +** → **Battery/Power** → **Battery Level** → *Greater than 20%*.
- **Wake the screen first (for Option C):** add **Screen → Screen On** as the first action.

## 5. Save and test

1. Tap the ✔ (save) button and make sure the macro is **enabled**.
2. To test now, long-press the macro → **Test Actions** (save your work first, because the phone will restart).
3. Settings → Apps → MacroDroid → **Battery** → set to **Unrestricted**, so Android does not stop
   MacroDroid overnight and miss the 4:00 AM trigger.

---

### Summary of the finished macro

| Part       | Setting                                                                  |
|------------|--------------------------------------------------------------------------|
| Trigger    | Day/Time Trigger, 04:00, Friday                                          |
| Action 1   | (optional) Screen On                                                     |
| Action 2   | Reboot: root `reboot`, Shizuku `svc power reboot`, or UI Interaction → Power Dialog → Click "Restart" |
| Constraint | (optional) Battery level > 20%                                           |
