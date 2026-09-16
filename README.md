# Bugbear's Mechanics for 5e

Bugbear's Mechanics is a Foundry VTT module which introduces some highly opinionated system mechanics.

The current release requires Foundry VTT 14 and D&D5e 6.0.2 or newer.

[Showcase video](https://github.com/user-attachments/assets/7d135cea-8e2b-4143-96b3-af81690d9e64)

## Table of Contents

- [Module Settings](#module-settings): chat icon double-clicks, damage automation, mutable damage hooks, chat popouts, and status HUD sorting.
- [Chat Roll Controls](#chat-roll-controls): compact-card labels and in-place rerolls.
- [Integrated Damage Features](#integrated-damage-features): Death Ward, Undying Sentinel, Undead Fortitude, and Elemental Adept.
- [Activation Triggers](#activation-triggers): damage and healing triggered activities.
- [Overtime Effects](#overtime-effects): automatic combat and encounter activity triggers from Active Effects.
- [Enchantments](#enchantments): chat-card target and item selection for dnd5e enchantment activities.

## Module Settings

### Module Support

The **Module Support** settings menu opens BM5E documentation, issue tracker, Discord, Ko-Fi, and Patreon links.

### Chat Icon Double Clicks

BM5E adds double-click behavior to chat card document icons.

- Double-click an actor name, target pill, item icon, activity icon, or effect icon in chat to open that document sheet.
- Effect tray handling is restricted to the effect icon rather than the full effect row.
- If the document is not visible to the user but has an image, BM5E opens an image popout instead.
- Double-clicking an already open sheet or image popout closes it.
- Buttons, links, inputs, selects, text areas, and damage multiplier buttons are ignored.
- Single clicks are delayed briefly so normal dnd5e click behavior still works when no double-click follows.
- **Enable chat document double-clicks** is a per-user setting and is enabled by default.

### Chat Roll Controls

BM5E extends existing compact D&D5e chat messages without creating replacement roll messages.

- Attack, Save, and Check rolls can be rerolled with Advantage, Normal, or Disadvantage.
- Damage and Healing rolls can be rerolled as Normal or Critical.
- Damage rerolls include every damage part produced by the activity.
- Compact save summaries provide a separate reroll control for each target.
- Rerolls update the existing message, retain the initial total, count rerolls, and reevaluate the target DC and success or failure.
- Damage rerolls add or remove the Critical property pill to match the selected mode.
- Reroll controls are available only when the current user can update the roll message.
- Compact Attack, Damage, Save, and Check controls receive visible text labels.
- Attack controls indicate hits and misses, while Advantage and Critical options use the system success color.

### Auto Roll Damage

When **Auto Roll Damage** is enabled and Midi-QOL is not active, BM5E listens for attack rolls. If the attack succeeds or crits, BM5E clicks the originating card's damage button automatically.

Manual damage clicks are also intercepted. If the card already has a damage message, BM5E asks whether to reroll damage or replace the latest damage message. Damage and attack buttons are updated with their latest roll totals and reuse state.

For non-attack damage activities, BM5E can roll damage after the activity use. Pure damage activities are left to dnd5e's default damage roll behavior.

### Mutable Damage Calculation

Enable **Mutable Damage Calculation** only while libWrapper is active. The setting reloads the world and provides mutable `dnd5e.preCalculateDamage` and `dnd5e.calculateDamage` hook context required by automations such as Death Ward, Undying Sentinel, and Undead Fortitude.

The post-calculation context includes `damageAfterTempHP`, `wouldDropToZero`, `remainingDamage`, `killedOutright`, and `dropToOneHP()`.

```js
Hooks.on('dnd5e.calculateDamage', (_actor, _damages, _options, context) => {
	if (!context?.killedOutright) context?.dropToOneHP?.();
});
```

Pre-calculation hooks can mutate damage traits and modifications for one calculation:

```js
Hooks.on('dnd5e.preCalculateDamage', (actor, _damages, _options, context) => {
	if (!actor.appliedEffects.some((effect) => effect.name === 'Elemental Ward')) return;
	context.modifications.fire = (context.modifications.fire ?? 0) - 2;
	context.traits.dr.value.add('cold');
});
```

### Integrated Damage Features

The **Integrated Damage Features** multiselect is available when **Mutable Damage Calculation** is enabled. BM5E resolves enabled lethal-damage protections in this order: Death Ward, Undying Sentinel, then Undead Fortitude. Only one protection resolves for each damage application.

#### Death Ward

- Recognizes an Active Effect named `Death Ward` or `Protection from Death`.
- Also recognizes an effect whose origin is an item with the `death-ward` identifier.
- Deletes the consumed effect and leaves the actor at 1 HP.

#### Undying Sentinel

- Requires an owned item with the `undying-sentinel` identifier.
- Triggers only when the item and its selected activity have sufficient uses available.
- Leaves the actor at 1 HP, targets the actor's token, and uses the item's Heal activity when available.
- If no uses are available, BM5E continues to the next eligible protection without attempting to use the item.

#### Undead Fortitude

- Requires an owned item with the `undead-fortitude` identifier.
- Does not trigger for Radiant damage or damage from a Critical Hit.
- Rolls a configurable Constitution saving throw against DC 5 plus the damage taken.
- On a success, the actor drops to 1 HP instead of 0 HP.
- Rerolling the save updates its chat message but does not retroactively revise damage already applied.

#### Integrated Damage Modifiers

The independent **Integrated Damage Modifiers** multiselect does not require **Mutable Damage Calculation** or libWrapper.

The Elemental Adept integration applies when the caster owns an item with the `elemental-adept` identifier and its name ends with the damage type, such as `Elemental Adept (Fire)`.

- Applies only to spells.
- Ignores matching damage resistance, but not immunity.
- Does not implement the rule that treats matching damage-die results of 1 as 2. That behavior can be configured with Automated Conditions 5e.

### Auto Popout

When **Enable use message pop-outs** is enabled, BM5E can pop out chat cards for repeated-use or multi-roll workflows where the original card is likely to be reused.

Use the **Auto pop-out use message** settings menu to configure item, activity, actor, and compendium UUID defaults. Each entry can restrict matching by item and activity type. Actor UUID, ID, name, and compendium source entries can match activities on that actor.

The same menu configures Extra Attack feature defaults. Matching features cause melee and ranged weapon attacks to pop out. An activity's **Use message pop-out** Behavior checkbox is an explicit override: enabled always pops out, disabled never pops out, and an untouched checkbox uses the configured defaults.

This behavior is skipped when Ready Set Roll is active.

### Effect Application Outcomes

When **Deselect effect targets by roll outcome** is enabled, BM5E removes missed attack targets and successful save targets from the originating message's Effects tray.

### Damage Application Restore

When **Enable damage restoration** is enabled, BM5E records native damage applications and adds RESTORE/RESTORED controls to damage trays. APPLY remains available for repeated applications.

### Status HUD Sorting

When **Enable Status HUD Sorting** is enabled, BM5E replaces the token status palette layout with a labeled, sorted grid.

- Status effects are labeled under their icons.
- Cover, bonus action used, and reaction used are pinned first.
- Other statuses sort alphabetically by label.
- Unknown or custom active effects are kept visible and sorted after known statuses.
- Exhaustion uses dnd5e's exhaustion image for the actor's current exhaustion level.
- Concentration and exhaustion keep their dnd5e management behavior.

**Status Effects sorting** controls whether the grid fills by rows or by columns. **Number of columns** controls the grid width. **HUD scale** scales the palette and adjusts against canvas zoom. **Enable status filter** adds a search box, Escape clears or closes the palette, Enter applies the first visible match, and **Clear effects** removes current actor statuses and custom HUD effects.

The status palette is constrained to the visible viewport and gains vertical scrolling when its configured rows, columns, scale, or current canvas zoom would otherwise place it off-screen.

### Overtime Debug Logging

**Overtime Debug Logging** enables BM5E overtime debug logs. It can be combined with channel flags through `globalThis.bm5e.debug`.

## Activation Triggers

The **BM5E Triggers** activity activation type runs an activity after its actor is damaged or healed.

Open the trigger editor from the activity's Activation Cost controls, then configure:

- **Event**: Damaged or Healed.
- **Only when HP is**: an optional resulting-HP percentage comparison.
- **Damage Types** or **Healing Types**: an optional type filter for the selected event.

Triggers respond to native D&D5e damage application and direct Actor HP updates without firing twice for the same change. During combat, matching activities appear in an identifiable turn-style chat message for the actor's owners. Outside combat, matching activities execute directly.

## Overtime Effects

BM5E overtime effects create activities that trigger automatically during combat or encounter timing.

Create an Applied Active Effect on an Activity, then add a BM5E change with this key:

```text
flags.bm5e.overtime
```

When the change type is **BM5E**, the editor uses that key automatically.

D&D5e displays this key as **Overtime**. Selecting another change type clears an automatically populated overtime key, and the BM5E change-summary icon opens the corresponding overtime editor directly.

### Activity Source

The value determines what activity BM5E should roll:

- `action=pseudo` creates a generated activity from the serialized fields.
- `action=link` with `link=ACTIVITY_UUID` reuses an existing activity.
- Created editor activities are stored on the actor's hidden overtime item.
- An item-owned activity with the **BM5E Overtime** activation cost is a standalone overtime activity. Its editor stores configuration directly on that activity.

Supported pseudo activity types:

- `pseudoType=save`
- `pseudoType=check`
- `pseudoType=damage`
- `pseudoType=heal`

Compatibility keys such as `flags.bm5e.overtime.check` are still parsed, but new effects should use `flags.bm5e.overtime`.

### Timing

Use `turn=VALUE`.

Supported values:

- `turnStart`
- `turnEnd`
- `sourceTurnStart`
- `sourceTurnEnd`
- `eachTurnStart`
- `eachTurnEnd`
- `roundStart`
- `roundEnd`
- `encounter`
- `encounterEnd`

Aliases include `start`, `end`, `sourceStart`, `sourceEnd`, `eachStart`, `eachEnd`, `combatStart`, and `combatEnd`.

### Roll Fields

- `name=TEXT` overrides the generated activity name.
- `ability=str` sets the primary ability.
- `abilities=str|dex` allows multiple save or check abilities.
- `skills=ath|acr` allows associated skills for checks.
- `tools=...` allows associated tools for checks.
- `dc=15` sets the save or check DC.
- `damage=2d6` sets damage.
- `damageType=fire` sets the damage type.
- `critical=true` enables critical damage handling.
- `criticalDamage=FORMULA` sets critical damage.
- `heal=1d8` sets healing.
- `healType=healing` or `healType=temphp` sets healing type.
- `onSave=half`, `onSave=full`, or `onSave=none` controls save damage.

### Outcome Fields

Outcomes can apply status effects, apply linked activity effects, or remove the overtime effect.

- `onEachSuccess=prone` applies on every success.
- `onEachFailure=poisoned` applies on every failure.
- `overallSuccessTarget=2` counts total successes.
- `overallFailureTarget=2` counts total failures.
- `onSuccess=removeOvertime` applies when the overall success target is met.
- `onFailure=prone` applies when the overall failure target is met.
- `consecutiveSuccessTarget=2` counts consecutive successes.
- `consecutiveFailureTarget=2` counts consecutive failures.
- `onConsecutiveSuccess=removeOvertime` applies when the consecutive success target is met.
- `onConsecutiveFailure=prone` applies when the consecutive failure target is met.
- `triggerTarget=3` counts non-save/check triggers.
- `onEachTrigger=prone` applies on every trigger.
- `onFinalTrigger=removeOvertime` applies when the trigger target is met.
- `persistent=true` keeps pending overtime resolutions from resolving at combat end.

`remove`, `delete`, and `removeOvertime` remove the overtime effect. Status IDs such as `prone` apply that status with `active: true`. `exhaustion` adds one exhaustion level, up to the D&D5e configured maximum. Linked activity effect IDs apply the matching embedded activity effect to the actor.

### Linked Save and Check DCs

Linked Save and Check activities can use **Preserve source DC**. When enabled, BM5E resolves the source activity's ability, spellcasting, class-source spellcasting, formula, bonus, and scaling data before creating the target overtime activity, then stores the resulting DC as a custom value. Disable it to resolve the DC from the target actor instead.

Compatibility aliases for previous BM5E versions include `overallFailures`, `failures`, `saveFailures`, `saveFails`, `overallSuccesses`, `successes`, `saveSuccesses`, `saveSuccess`, `consecutiveFailures`, `successiveFailures`, `successiveFails`, `consecutiveSuccesses`, `successiveSuccesses`, `successiveSaves`, `onFail`, `onSaveFailure`, `onSaveFail`, `onSaveSuccess`, `onEachFail`, and `triggers`.

### Examples

Check with consecutive outcomes:

```text
action=pseudo; pseudoType=check; turn=turnStart; abilities=str; skills=ath|acr; dc=15; onConsecutiveSuccess=removeOvertime; onConsecutiveFailure=prone; consecutiveSuccessTarget=2; consecutiveFailureTarget=2
```

Save with each-failure and overall success:

```text
action=pseudo; pseudoType=save; turn=turnEnd; abilities=con|wis; dc=13; onEachFailure=poisoned; overallSuccessTarget=1; onSuccess=removeOvertime
```

Linked activity:

```text
action=link; link=Scene.9gP78CMgo7ZmATt2.Token.3TjlspjtW1JPoYk1.Actor.KBYJmwlbPBToyRY9.Item.DgH1PglrwHrwf9zv.Activity.erT6fai51NJq3AQG; turn=turnStart; onFinalTrigger=removeOvertime; triggerTarget=3
```

At the start and end of a creature's turn, if one or more overtime effects are present on the actor, BM5E creates a chat message showing the available overtime rolls. If Midi-QOL is active and Activation Cost Automation is set to Auto Trigger, these overtime effects are rolled automatically.

## Enchantments

BM5E extends dnd5e enchantment activity cards so the user can choose a target and item directly from the chat card.

- Enable this behavior with the **Enable enchantment handling** world setting.
- Targeted tokens are shown on the enchantment card.
- If no target list is available, BM5E can use the currently selected token.
- Clicking the enchantment drop area opens an item picker for compatible items on the chosen actor.
- If the user does not own the target actor, BM5E whispers a GM request with an **Apply Enchantment** button.
- The GM request keeps the original activity, target actor, source message, and enchantment profile.
- BM5E passes the source chat message and concentration effect through to dnd5e's enchantment application.
- By default, a matching enchantment from the same activity replaces the previous matching enchantment on that item.
- **Allow Enchantment Stacks** permits repeated enchantments from the same activity on one item.
- **Maximum Enchanted Items** limits how many different items an activity can enchant at once. Use `0` for no limit.
