# BM5E Detailed Reference

This reference covers detailed behavior, configuration keys, formulas, and integration hooks. Start with the [user guide](../README.md) for everyday setup and use.

Requires Foundry VTT 14.367+ (v14), D&D5e 6.0.2+, and libWrapper. Verified with Foundry 14.369 and D&D5e 6.0.6.

## Table of Contents

- [Module Settings](#module-settings): chat icon double-clicks, damage automation, mutable damage hooks, chat popouts, and status HUD sorting.
- [Chat Roll Controls](#chat-roll-controls): compact-card labels and in-place rerolls.
- [Damage Automations](#damage-automations): Arcane Ward, Death Ward, Undying Sentinel, Undead Fortitude, Elemental Adept, and Massive Damage.
- [Damage Application Restore](#damage-application-restore) and [Effect Application Controls](#effect-application-controls): Apply, Restore, and Remove controls and player permissions.
- [Midi-QOL Compatibility](#midi-qol-compatibility): healing, damage triggers, and reversals.
- [Prone Mechanics](#prone-mechanics), [Grapple Mechanics](#grapple-mechanics), and [Falling Mechanics](#falling-mechanics): token-icon controls and condition automation.
- [Activation Triggers](#activation-triggers): incoming and outgoing damage, healing, attack, movement, and in-range triggered activities.
- [Templates and Emanations](#templates-and-emanations): appearance, placement overrides, concentration, and targeting previews.
- [Contested Checks](#contested-checks): two-participant Group check requests, tie policies, and loser effects.
- [Overtime Effects](#overtime-effects): automatic combat and encounter activity triggers from Active Effects.
- [Enchantments](#enchantments): chat-card target and item selection for dnd5e enchantment activities.

## Module Settings

Settings are grouped into **Chat & Rolls**, **Token HUD**, **Damage Mechanics**, **Conditions & Effects**, and **Troubleshooting** menus. Existing saved preferences are retained. These menus save changes immediately; close them with the window's X. Token HUD changes update an open HUD without saving the main Game Settings form.

All non-world settings are saved per user and follow that user across browsers, including **Compact combined cards**, other Chat & Rolls preferences, auto-popout defaults, and Token HUD preferences. **Hide chat-card tags for** and **Enable template targeting** remain GM-controlled world settings, as do damage automation, condition mechanics, and troubleshooting options. Existing client-scoped Compact combined cards preferences migrate to the user on first load.

Personal settings do not require a reload. Label, reroll-control, and result-display changes refresh existing chat cards and open message popouts immediately. Automation and pop-out preferences apply to subsequent uses; document permissions still govern rolls and activity use. Individual chat-card expand/collapse choices remain browser-local.

### Module Support

The **Module Support** settings menu opens BM5E documentation, issue tracker, Discord, Ko-Fi, and Patreon links.

### Chat Icon Double Clicks

BM5E adds double-click behavior to chat card document icons.

- Double-click an actor name, item icon, activity icon, or effect icon in chat to open that document sheet. Double-click a target pill to open the target actor's sheet when permitted.
- Item icons in Triggered and Overtime activity lists support double-clicks to open or close the item sheet, subject to the existing setting and visibility rules. Adding separate activity icons is deferred pending native system support. Clicking the item/activity text still uses the activity.
- Effect tray handling is restricted to the effect icon rather than the full effect row.
- If a target actor cannot be viewed, its token image opens instead. Other documents use their image when the sheet is unavailable.
- Double-clicking an already open sheet or image popout closes it.
- Buttons, links, inputs, selects, text areas, and damage multiplier buttons are ignored.
- Single clicks are delayed briefly so normal dnd5e click behavior still works when no double-click follows.
- **Enable chat document double-clicks** is a per-user setting and is enabled by default.

### Chat Roll Controls

BM5E extends existing compact D&D5e chat messages without creating replacement roll messages.

Attack and Damage results, including their expandable rolls and application trays, appear inside the originating usage card. Their separate roll messages are hidden from the chat log. This combined layout is bypassed while Midi-QOL is active.

- Click an Attack, Save, or Check dice control to switch the existing roll between Advantage, Normal, and Disadvantage. Advantage keeps the higher stored d20, Disadvantage keeps the lower, and Normal displays only the first die. Missing extra dice are rolled once; stored results survive reloads and can be reused when switching back. Modifiers and critical/fumble thresholds are preserved.
- **Shift-click** a dice control to roll fresh dice using its current Advantage, Normal, or Disadvantage mode, or its current Normal/Critical damage mode. Fresh d20 rolls replace the stored pool; later mode switches use the new results. The normal activity action creates a separate roll.
- Damage and Healing rolls can be rerolled as Normal or Critical.
- Damage rerolls include every damage part produced by the activity.
- Compact save summaries provide a separate reroll control for each target.
- Dice controls update the existing message and retain its initial total. Damage rerolls increment the reroll count; d20 mode changes reuse stored dice and reevaluate success or failure. Attack mode changes also refresh existing damage multipliers per target: hits use 1 and misses use 0.
- Rerolled results refresh linked cards and open popouts on other connected clients, including save/check outcomes and damage previews. This refresh does not undo previously applied damage or effects.
- Damage rerolls add or remove the Critical property pill to match the selected mode.
- Concentration rerolls retain the concentration DC rather than adopting the originating spell's save DC.
- Reroll controls are available only when the current user can update the roll message.
- Compact chat actions use D&D5e's native labels and separate action/resource rows, with equal button widths across rows when **Show roll results on message buttons** is enabled. Disabling it preserves native button sizing, colors, and tooltips.
- With labels enabled, these buttons share the available row width; Template and Resource controls remain compact icon buttons.
- **Hide chat-card tags for** is a world setting, disabled by default. GMs can select **GMs**, **Players**, or both under **Chat & Rolls** to hide item and activity tags, such as weapon type, range, and proficiency. Leave the selection empty to show tags to everyone. Changes apply immediately.
- **Compact combined cards** is a per-user preference, off by default, and applies immediately. Linked attack and damage results share a row. Target pills show hit/miss and expected damage, with multipliers just below. The checkmark beside damage applies it; after a recorded application, it opens **Apply again / Restore** when **Enable damage restoration** is enabled. The amount is separated from the target name for readability.
- Click the chevron on an attack's dotted divider to fold it into a small **Attack 1**, **Attack 2**, etc. row; click the row to expand it. Automatic folding follows **Collapse Trays in Chat**, including collapse after application except in **Never Collapse** mode. Manual choices are remembered on this client.
- Hold **Shift** to show a trash control beside an attack's chevron, whether expanded or collapsed. It deletes that attack and its paired damage together when you have permission to delete both. Separate damage entries retain their own delete control. Deleting a row does not restore damage already applied; use Restore first if needed.
- Clicking the top **Damage** button again creates a separate damage entry without replacing an existing attack/damage pair. Separate damage uses the same right-hand result column and Apply/Restore checkmark. Use the damage row's reroll control to replace that roll. Ambiguous or shared damage stays separate from individual attacks.
- Damage-history tooltips use the corresponding attack numbers and show **-** where damage has not been rolled. Separate damage entries follow afterward. Reroll menus outline the current advantage or critical mode.
- When **Midi-QOL** is active, BM5E bypasses its combined and compact attack-card layout so Midi-QOL can manage its own cards.
- **Show reroll controls** independently enables clickable dice controls and reroll menus for messages your user can update. Changing it refreshes existing cards and popouts without reloading.
- **Show roll results on message buttons** controls Attack/Damage roll-history tooltips and green Save/Check indicators after a visible roll. Resources also turn green while consumed (the system displays Refund), and Template turns green after successful placement from that card. Cancelling placement does not mark it complete; deleting the placed region does not clear the placement history.
- Attack controls indicate hits, misses, critical hits, and fumbles, including after rerolls. Attack and Damage tooltips show numbered totals and outcomes or Normal/Critical damage modes.
- Open combined-card popouts refresh after attack and damage changes, including remote rerolls, while preserving native application trays and weapon mastery links.
- Reroll dice show Automated Conditions 5e's roll attribution tooltip when permitted by its client preference and world visibility filter.

### Auto Roll Damage

**Auto Roll Damage** offers **Disabled** (the default), **When the attack hits**, and **Always**. The hit option opens the damage dialog after a successful or critical attack; Always also opens it after a miss. Existing enabled preferences migrate to **When the attack hits**. Rerolls open any missing damage for eligible attacks using their original targets, without duplicating existing linked damage. Midi-QOL bypasses this automation.

In either enabled mode, save activities open the damage dialog when the activity is used, before saves resolve. Damage activities retain their normal system behavior.

Clicking Attack or Damage again creates another roll and application tray without a Reroll/Replace dialog. Use the dice-icon controls to reroll a specific existing roll. Button tooltips use `Attack 1: 15 (Hit)` and `Damage 1: 7 (Normal)` entries; critical hits and fumbles retain their respective outcomes.

For non-attack damage activities, BM5E can roll damage after the activity use. Pure damage activities are left to dnd5e's default damage roll behavior.

### Mutable Damage Calculation

Enable **Mutable Damage Calculation** to provide mutable `dnd5e.preCalculateDamage` and `dnd5e.calculateDamage` hook context required by automations such as Arcane Ward, Death Ward, Undying Sentinel, and Undead Fortitude. libWrapper is a required BM5E dependency.

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

### Damage Automations

The **Mutable Damage Automations** multiselect is available when **Mutable Damage Calculation** is enabled. Arcane Ward absorbs eligible damage before zero-HP protections run. BM5E resolves enabled zero-HP protections in this order: Death Ward, Resilience, Undying Sentinel, then Undead Fortitude. Only one protection resolves for each damage application.

#### Arcane Ward

- Requires an owned item with the `arcane-ward` identifier; its uses track current ward HP, with a maximum of twice Wizard level plus Intelligence modifier.
- In 2014, level 1 or higher Abjuration spells create or replenish the ward, including rituals and slotless casts. The ward absorbs damage before the Wizard's defenses apply to overflow.
- In 2024, an Abjuration spell must consume a spell slot. Creation fills the ward; later casts restore twice the slot level. An activity on the ward that consumes a spell slot can also restore twice that slot level. Resistance and vulnerability apply before absorption, with immunity applied to overflow.
- A Long Rest empties and deactivates the ward.
- Hover a damage target to see ward absorption, remaining damage, and ward HP without changing native token highlighting. Previews refresh after damage rerolls; recorded applications retain their snapshots after reload.
- Restoring an application also restores ward HP spent by it, including applications fully absorbed by the ward.

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

#### Resilience

Enable **Mutable Damage Calculation**. On the source effect, add a change with type **Bugbear's Mechanics 5e** and choose `flags.bm5e.resilience` from the attribute key suggestions. Open its value editor to choose the saving throw ability, edit the DC or formula, and configure exclusions. Apply saves the generated JSON to that change. Native emanations can distribute this effect without additional BM5E setup.

Resilience can also use `flags.bm5e.resilience` directly on an active effect or the recipient actor; it does not require an item identifier or an entry in Mutable Damage Automations. For example, with a macro where `effect` is its ActiveEffect document:

```js
await effect.setFlag('bm5e', 'resilience', {
  ability: 'con',
  dc: '5 + @bm5e.damageDealt.total',
  excludeCritical: false,
  excludeInstantDeath: false,
  excludedDamageTypes: []
});
```

- For an effect-based configuration, disable or remove the effect to stop granting Resilience. If configured directly on an actor, remove the flag with `actor.unsetFlag('bm5e', 'resilience')`.
- Damage must reduce positive HP to 0. BM5E pauses application while the recipient rolls the save. Success leaves 1 HP; failure, cancellation, or an invalid configuration's DC applies damage normally. The effect is not consumed.
- `@bm5e.damageDealt.total` is the resolved damage total after defenses and damage reductions, before temporary HP absorption and before capping damage at the actor's remaining HP. The DC accepts a number or a deterministic formula using recipient roll data and the existing BM5E damage context, including per-type values such as `@bm5e.damageDealt.fire`. `damageApplied` is unavailable because the save resolves before application.
- Critical hits and instant-death damage are eligible unless explicitly excluded. `excludedDamageTypes` skips an application containing positive damage of any listed type, such as `radiant`.
- If several configurations qualify, the first active effect's eligible configuration is used, followed by the actor flag as a fallback. A failed save does not try another protection. Each recipient resolves independently.
- Save rerolls update the message but do not retroactively revise damage already applied. Resilience uses the existing mutable damage path, including Midi-QOL finalization; runtime testing with the installed Midi-QOL version is recommended.

For continuous distribution, configure the source activity's template as an **Emanation** and add the system's **Apply Active Effect** behavior, selecting the configured effect. Use the activity to place its token-attached Region. The native behavior applies the effect on entry and removes its own effect on exit, using native disposition, size, and creature-type filters. Foundry handles Region membership and elevation. BM5E adds no range checks or recipient filters. The recipient effect retains its Resilience flag because the native behavior copies the source effect data. A benefit that persists after leaving the area should instead use an ordinary timed effect.

#### Standard Damage Automations

The **Standard Damage Automations** multiselect works without enabling **Mutable Damage Calculation**. libWrapper remains a required module dependency.

The Elemental Adept integration applies when the caster owns an item with the `elemental-adept` identifier and its name ends with the damage type, such as `Elemental Adept (Fire)`.

- Applies only to spells.
- Ignores matching damage resistance, but not immunity.
- Does not implement the rule that treats matching damage-die results of 1 as 2. That behavior can be configured with Automated Conditions 5e.

The Massive Damage integration marks all three death saving throw failures when damage reduces an actor to 0 HP and the remaining damage equals or exceeds its maximum HP. It runs only when no enabled zero-HP protection leaves the actor at 1 HP.

### Auto Popout

When **Enable use message pop-outs** is enabled, BM5E can pop out chat cards for repeated-use or multi-roll workflows where the original card is likely to be reused.

Open **Auto pop-out use messages** from **Chat & Rolls** to configure item, activity, actor, and compendium UUID defaults. Each entry can restrict matching by item and activity type. Actor UUID, ID, name, and compendium source entries can match activities on that actor.

The same menu configures Extra Attack feature defaults. Matching features cause melee and ranged weapon attacks to pop out. An activity's **Use message pop-out** Behavior checkbox is an explicit override: enabled always pops out, disabled never pops out, and an untouched checkbox uses the configured defaults.

This behavior is skipped when Ready Set Roll is active.

### Effect Application Outcomes

When **Deselect effect targets by roll outcome** is enabled, BM5E removes missed attack targets and successful save targets from the originating message's Effects tray.

After an attack, missed targets receive a zero multiplier in the Damage tray; hits retain the normal multiplier.

### Damage Application Restore

When **Enable damage restoration** is enabled, BM5E records native damage applications and adds RESTORE/RESTORED controls to damage trays. The label changes from APPLY to APPLIED when damage is recorded and returns to APPLY after restoration.

Initially, only Apply is shown. Once an application is recorded, Apply/Applied and Restore/Restored share one row. Restoring an Arcane Ward application also restores the ward HP spent by it.

Players can apply and restore damage only for actors they own, subject to the system's Allow Player Damage Tray setting. The entire tray is hidden when no owned target is available. An active GM records applications on messages the player cannot update; Restore lists only permitted records. Restore appears only after an application has been recorded.

### Effect Application Controls

The usage card's Effects tray changes APPLY to APPLIED when effects are applied. REMOVE deletes newly applied effects or restores modified effects, then changes to REMOVED. Players can remove effects recorded for their owned actors when Allow Player Effects Tray is enabled. An active GM saves effect application and removal records on chat messages the player cannot update; other recipients' records are preserved. Changes to Allow Player Damage Tray or Allow Player Effects Tray refresh existing chat cards immediately.

The tray initially shows only Apply. After an application is recorded, Apply/Applied and Remove/Removed share one row, matching the damage tray.

### Midi-QOL Compatibility

These integration points were checked against the installed Midi-QOL 14.6.4 source and dnd5e 6.0.6. Source inspection and mocked tests do not establish compatibility with every Midi-QOL configuration or workflow.

- **Card ownership:** BM5E bypasses its combined/compact attack layout and automatic damage rolling whenever Midi-QOL is active. No Midi setting currently hands its workflow to BM5E's combined cards. BM5E's native-tray Restore and Remove controls are not replacements for Midi's damage-card controls.
- **Mutable damage calculation:** when enabled, BM5E has a Midi-specific calculation path and finalizes its context through `midi-qol.dnd5eCalculateDamage`. This includes healing handling; it is not a blanket guarantee for every damage automation or third-party calculation override.
- **Damage/healing activation triggers:** for Midi applications carrying `options.midi`, BM5E derives the application amount from the HP and temporary-HP updates. Damage can include temporary HP lost; temporary HP gained alone is not damage. Outgoing Damages triggers also depend on resolving the originating source actor/message.
- **Damage-card reversals:** BM5E observes clicks on Midi's `reverse-*` and `all-reverse` buttons, reads `flags.midi-qol.undoDamage`, and recognizes the matching HP restoration during damage application. That restoration does not fire Damaged, Healed, or outgoing Damages triggers. Reapplying damage or healing can fire the appropriate event again. This handling does not roll back previously triggered activities or effects and does not cover arbitrary undo macros or other reversal paths.
- **Overtime final results:** wrappers around `rollAbilityCheck`, `rollSavingThrow`, `rollSkill`, and `rollToolCheck` wait for Midi's wrapped operation to return, then process the final result once for a recognized generated or standalone BM5E overtime activity. This path runs only on the active GM client. It includes optional bonuses and rerolls completed inside that operation; it does not reconcile later chat-message rerolls, restore deleted overtime effects, or forward player-client final results to the GM.

For an example of the overtime integration, a successful **Escape Restraint** check can remove **Ensnaring Strike** after Midi's optional roll changes have finished. This is separate from BM5E's chat reroll display updates and does not imply retroactive outcome adjudication.

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

### Prone Mechanics

When **Implement Prone Mechanics** is enabled, applying Prone selects Crawl for the actor's tokens. Removing Prone restores each token's previous movement action when available, or selects its fastest available movement type. Clicking an active Prone icon on a token stands it up and records half its walking speed in the current combat turn's movement history. The creature must have enough movement remaining. Set the actor flag `flags.bm5e.standingCost` to override that cost in grid spaces; for example, `2` costs 10 feet on a 5-foot grid.

Prone movement updates skip tokens already crawling and guard overlapping updates so duplicate processing does not overwrite the saved movement action. With multiple enabled Active Effects granting Prone, movement is restored after the last is removed. Restoration avoids unchanged updates when no saved movement flag needs clearing.

### Grapple Mechanics

When **Implement Grapple Mechanics** is enabled, actor owners and the active GM can click a token's Grappled icon to start an escape. The rolling user chooses **Acrobatics | Athletics**; closing the dialog with X cancels. Players roll their own escape locally. When the GM starts an escape for an actor with an active player owner, BM5E sends that owner a chat request directly; otherwise, the GM rolls locally. Spending the action remains manual, and the active GM must be connected to resolve escapes.

Under the 2014 rules, escape requires beating the grappler's contested Athletics check. The active GM sends the opposing roll to the grappler's designated active player owner, or rolls locally if none is active. Under the 2024 rules, escape uses the DC saved when the originating Save or Check activity applied Grappled. Without a saved DC, BM5E checks the source activity's Save or Check DC, then the grappler's Strength DC. If no DC can be resolved, including for manually assigned Grappled statuses, BM5E queries the active GM with a DC-only dialog. The GM's DC is saved on the effect; afterward, the player chooses their escape skill and whether to roll. Cancelling the GM prompt stops the attempt. GM requests for multiple Grappled effects produce a separate request for each effect.

The GM's DC prompt has **Set** beside the number field, which saves and closes without requesting an escape. **Request Escape** saves a valid DC and continues the attempt, sending a player-owned actor's roll request to its active player owner. Missing or invalid DCs leave the dialog open. The window's X closes without requesting an escape. The active GM can right-click the token's Grappled icon to reopen this dialog with the current DC; if there are multiple grapples, select the effect to edit first. The GM does not choose the player's skill.

Escape rolls retain the effect UUID and an attempt ID, directly or through their system request. The active GM reevaluates failed attempts when either roll is rerolled and removes the associated effect on success. A deleted effect is not recreated by a later reroll, and previously applied HP or effects are not reversed. Activity-applied Grappled effects retain their source actor, tokens, icon path, range, and DC. Under the 2024 rules, the effect also ends when the grappler becomes Incapacitated or either token moves beyond that range.

### Falling Mechanics

GMs can right-click a token's Falling icon to land harmlessly at the system's supporting elevation and level, without a damage card or animation. Left-click retains the system's normal falling action. Enable or disable this behavior with **Harmless Falling on Right-click** in Conditions & Effects.

**Re-evaluate Falling when the active scene loads** optionally reevaluates shared Falling status for linked actors on the active scene when the active GM loads it. It uses the system's last-moved token on that scene, or the first linked token when needed. It does not reconcile multiple linked tokens on the same scene beyond the system's behavior. If D&D5e's **Disable Falling** is enabled, scene activation performs no checks, status changes, or last-moved token updates. The setting is disabled by default.

### Overtime Debug Logging

**Overtime Debug Logging** enables BM5E overtime debug logs. It can be combined with channel flags through `globalThis.bm5e.debug`.

## Activation Triggers

The **BM5E Triggers** activity activation type runs an activity in response to incoming or outgoing damage, healing, attacks, movement, or tokens within an emanation.

Open the trigger editor from the activity's Activation Cost controls, then configure:

- **Event**: Damaged, Damages, Healed, Heals, Use, Save succeeds, Save fails, Attacked, About to be hit, Attacks, Movement, or In Range. Forced Movement is available only while BAA5E is active.
- **Use**: after the actor completes an activity use, offer other matching BM5E Triggers activities on that actor. Optional **Source Activities**, **Source Activity Types**, and **Source Item Types** filters restrict which uses qualify. Target filters and automatic targeting apply per target on the usage message; uses without targets have no opponent. Triggered activities cannot start another Use chain.
- **Save succeeds / Save fails**: observe native ability saving throws with a known DC. **Saving throw made by** selects this actor's own saves or a target saving against this actor's activity. Source activity/type/item and opponent filters apply. Requires an active GM; Midi-QOL, concentration, and death saves are excluded. A response is offered once per activity, save message, and outcome; rerolls and Legendary Resistance can offer the other outcome. Existing applied effects are not reversed by later roll changes.
- **Apply Effect** (save triggers): enable it and drop an Active Effect or paste its UUID, including an AE compendium UUID. After the response activity is used, the GM asynchronously resolves the effect and copies it to the saving actor with a fresh duration start. It does not require that actor's token to be rendered. Enable **Auto Roll** to use the response automatically, or use its trigger card manually. Normal activity consumption still applies. **Automatically target** controls the response activity's ordinary targets; Apply Effect always applies to the saving actor. Consumed resources do not repeat the application, and already-applied or stale responses cannot be activated again for the same outcome.

For temporary immunity after a successful save, create a timed immunity AE with a `system.traits.ci.custom` change naming the protected effect. Add a Utility activity on the feature's actor, set its activation to BM5E Triggers, select **Save succeeds**, watch targets saving against this actor's activity, and filter **Source Activities** to the original save activity. Enable **Apply Effect** and link the immunity AE. Enable **Auto Roll** and **Bypass use dialog** if the response should run automatically. The D&D5e system handles suppression of matching effects; this does not prevent later save prompts or other damage.

- **Heals**: outgoing healing triggers on the healer and can automatically target the recipient. **Healed** triggers on the recipient. Healing Types applies to both; Damages remains outgoing damage only.
- **Offer in chat**: available for **About to be hit** triggers. Enabled by default, including existing activities. Disable it to hide the response button; manual activity use remains available.
- **About to be hit**: with **Offer in chat** enabled, offers a manual AC reaction on a native attack card for each eligible owned target that was hit. Offers last one minute from the attack roll and end when another attack is rolled, whichever happens first. Pending reaction dialogs close at that point, and expired uses are blocked before resource consumption. This event always counts as a reaction. Configure the activity's **AC bonus for this attack**, a nonnegative deterministic number or formula using that activity's roll data. This is a minimum increase for the triggering attack; any prepared AC increase from the applied effects counts toward it rather than stacking with it. Set it to `0` to rely entirely on the effects' AC increase. It uses the normal casting/configuration dialog and consumption, and records Reaction Used in combat. It does not automatically roll the reaction or redirect its target to the attacker.
- **Actor HP** and **Opponent HP**: independent optional comparisons of current HP to maximum HP, excluding temporary HP. Actor means the triggered activity owner; opponent means each creature involved in the event. Movement has no opponent. Existing HP filters retain their original subject.
- **Opponent Sizes** and **Opponent Types**: optional per-opponent filters. Only matching opponents are included when an event involves several creatures.
- **Attack Outcome**: All, Hit, or Miss for Attacks, evaluated per target; unknown outcomes match only All.
- **Damage Types** or **Healing Types**: optional type filters for the corresponding events, including damage-type filtering on attack events.
- **Exclude Damage Types**: optional veto for Damaged, Damages, Attacked, and Attacks. Any excluded type present suppresses the entire trigger, even when another type matches Damage Types. Damage events use resolved damage types; attack events use the activity's configured damage types. Disabling this filter preserves its choices. Healing events are unaffected.
- **Source Item Types**: an optional filter for the item that produced a damage, healing, or attack event.
- **Automatically target the other creature**: receiver events target the source creature, source events target the affected creature, and Movement or In Range targets the triggering token.
- **Bypass use dialog**: skips the activity use dialog.
- **Automatically roll activity**: immediately uses the activity after creating its Triggered chat message. When disabled, the message waits for user input.

The editor groups Event under **Trigger**, activity options under **Behaviours**, and optional filters under **Toggle Filters**. Filter and reduction toggles retain their configured values when disabled.

All enabled filters must match (**AND**). Multiple choices within one filter match any selected value (**OR**). For example, selecting Tiny and Small sizes plus Opponent HP at or below 50% triggers only for a Tiny or Small opponent whose HP is at or below 50%. Disabled filters do not restrict triggers, and their saved choices return when re-enabled.

For a Shield-style reaction, set the Item to Reaction, its casting activity to BM5E Triggers, the event to **About to be hit**, and the AC bonus to `5`. Keep native spell-slot/resource consumption on the casting activity. After an attack hits, the defender's owner or a GM can click the shield button on its attack card (including an embedded attack summary). After successful casting, BM5E automatically applies all effects listed on that cast's usage message to the defender through native effect preparation. That retains native scaling, duration, origin, and existing-effect refresh behavior. The defender's resulting AC increase counts toward the configured attack bonus, preventing a Shield effect and a configured `5` from becoming `+10`. The resulting bonus updates only that defender's recorded AC on that attack; native hit/miss evaluation and linked card/application outcomes refresh. It preserves other targets, attack totals, cover already included in the recorded AC, and natural critical hits. The automatically applied effects persist according to their configured duration and also cover subsequent attacks. Applications are recorded for the casting card's existing Remove Applied Effects control. Only effects applicable to that cast are applied; additional protections such as against Magic Missile depend on the configured effects.

These buttons follow the native Legendary Resistance pattern: eligibility is checked on display and revalidated on use, the button is disabled during its action, and the result is persisted on the relevant message. A defender player keeps their normal casting dialog; an active GM validates changes to another user's attack message. Cancellations do not spend a reaction or change the attack. Competing requests for one defender and updates by different defenders are serialized. If the attack is rerolled or its target changes while the reaction is being cast, BM5E leaves the changed attack alone; inspect the reaction's casting card/resources rather than assuming its completed use was undone. React before applying damage: existing HP loss is not reversed. This initial integration supports native D&D5e single-roll attack messages and is disabled with Midi-QOL.

**In Range** runs the configured activity for creatures in a native Region. Add the In Range area behavior to the activity that places the template, and select its **Triggered Activity** from the same Item. Select source or in-range token turn starts and ends, token movement, or Region movement, set the triggers allowed for each token per turn, filter by creature size or type, and optionally include the source token. **On Creation** batches eligible creatures into one trigger and activity use while recording each creature's allowance. Source turn events process eligible Region members; token turn and movement events process only their triggering creature. All events share each creature's per-turn allowance. The template defines the area, and the triggered activity's Affects choice supplies the ally or enemy filter. Triggered spell activities retain the original cast's level. **Token Moves In/Out/Within** use native `TOKEN_MOVE_IN`/`TOKEN_MOVE_OUT`/`TOKEN_MOVE_WITHIN` events. Native Within includes movement toward an exit and excludes initial entry. **Region Moves Onto Token/Away From Token** use native `REGION_BOUNDARY` events and containment changes when the Region is translated, including attached emanation movement. Creation, resizing, Level/elevation changes, visibility changes, and wall-constraint refreshes do not count as Region movement. Native Apply Active Effect behavior remains independent of BM5E trigger limits.

**Triggers per Token/Turn** is shared across the selected timings during each combat turn, including encounters not linked to a scene. Creating a trigger offer uses that allowance even if the activity is not used. Outside combat, this per-turn limit does not apply.

**Total triggers before removal** deletes the emanation after the configured number of trigger offers across all tokens and turns, including outside combat. Set it to **0** for unlimited triggers. Each offer counts once, including a batched On Creation offer; each creature in that batch still uses its own per-turn allowance. Filtered or unavailable activities do not count. The total persists on the emanation, and processing stops when the limit is reached.

When **Delete after triggers** removes the Region at its limit, the final response keeps a snapshot of its source activity/token, casting level, targets, and event in the trigger message. That response can complete once after cleanup, including after reload. Cancelling its activity dialog permits retry; a completed use is persisted and cannot consume resources again. Competing users are coordinated through the active GM. Older offers from an otherwise deleted Region remain unavailable.

See [Templates and Emanations](#templates-and-emanations) for appearance, placement, and Region lifecycle settings.

Trigger offers and automatic selections check configured consumption using native preparation on an item clone. Unavailable activities are omitted without spending resources. The system validates consumption again when the activity is used.

Triggers respond to native D&D5e damage application and direct Actor HP updates without firing twice for the same change. Matching activities appear in an identifiable Triggered chat message both during and outside combat.

Movement triggers expose the completed movement distance as `@bm5e.distanceTraveled`, the scene's grid distance as `@bm5e.gridDistance`, and the floored number of grid intervals moved as `@bm5e.gridDistanceTraveled`. Distance values use the scene's units and include all completed movement segments. For example, `(@bm5e.gridDistanceTraveled)d4` rolls one d4 per complete grid interval moved.

Forced Movement additionally requires the movement update option `{ bm5eMovementSource: 'forced' }`. Ordinary movement does not fire it. Without active BAA5E, the event is hidden for new configurations, existing Forced Movement selections are disabled and preserved, and no Forced Movement trigger fires.

### Damage Trigger Values

Damage triggers expose the following roll-data paths for each opponent. Use the exact paths, including the `@bm5e.` prefix, in formulas:

- `@bm5e.damageRolled.total`: rolled damage before target defenses.
- `@bm5e.damageIncoming.total`: incoming calculated damage before trigger reductions.
- `@bm5e.damageDealt.total`: final calculated damage after reductions.
- `@bm5e.damageApplied.total`: actual HP and temporary HP lost, capped by what the target had.
- `@bm5e.damageApplied.hpLost`: actual regular HP lost.
- `@bm5e.damageApplied.tempHpLost`: actual temporary HP lost.

The first three maps also expose damage-type values, for example `@bm5e.damageRolled.fire`, `@bm5e.damageIncoming.fire`, and `@bm5e.damageDealt.fire`. Applied damage exposes the HP breakdown above rather than damage-type values. Applied losses are available after damage application, not while calculating an incoming reduction. Trigger and usage messages retain their damage context after reload. Opponent filters are evaluated separately for each creature involved.

Per-damage-type formula examples:

- `floor(@bm5e.damageIncoming.fire / 2)`: half the incoming Fire damage, rounded down. In **Reduce Incoming Damage**, select only Fire so the reduction applies to that type.
- `@bm5e.damageDealt.necrotic + @bm5e.damageDealt.cold`: the final calculated Necrotic and Cold damage combined, for a triggered activity's formula.

Damage-type keys use system identifiers such as `fire`, `cold`, and `necrotic`. Final calculated damage can exceed the HP actually lost; per-type values do not provide a breakdown of applied HP loss.

**Reduce Incoming Damage** on a Damaged trigger supports formulas such as `min(10, ceil(@bm5e.damageIncoming.total / 2))` for Ghostly Command. This requires Mutable Damage Calculation. With Auto Roll disabled, you are prompted before using the activity; declining or having insufficient configured resources leaves damage unchanged. Healing does not trigger the reduction. Selected damage types share one reduction budget; no types selected means all damage types.

**Resist Incoming Damage** uses the same pre-application casting flow and requires Mutable Damage Calculation. After a successful activity use, BM5E recalculates the original damage with resistance added only to that calculation's copied traits. With the Damage Types filter enabled, only matching selected types gain resistance; otherwise, all incoming damage types gain it. Existing resistance does not stack, and native immunity, vulnerability, rounding, and resistance bypasses remain in control. Declining or cancelling the activity grants no resistance.

This option does not create a lasting Active Effect. Apply any associated resistance effect through the native activity Effects tray for later damage. For resistance alone, leave **Reduce Incoming Damage** disabled; enabling both also applies the numeric reduction after resistance. In combat, BM5E applies Reaction Used automatically; it expires at the Actor's next turn start.

The **Ghostly Command** example is available in the **BM5E Overtime Test Items** compendium. Its description covers BM5E configuration only. The Weapon and Spell source filter also accepts non-attack weapon damage; reaction costs and timing are not enforced automatically.

For **About to be hit**, **Show in chat** places the reaction button directly on the eligible attack card. Disabling it offers the reaction in a separate native turn/rest activity message whispered to the defender's owners. Both chat paths use the same casting, reaction consumption, AC update, and stale-offer guards. Using the activity directly from its sheet does not update the recorded attack result.

## Updating Items from Compendiums

**Update from Compendium (BM5e)** appears on editable sourced items in actor inventories and the world Items directory. The source is read from `_stats.compendiumSource`, with legacy `flags.core.sourceId` support. Updating replaces the item's name, image, system rules, activities, effects and flags, including local customizations. Item identity, ownership, world folder, sort order, quantity, equipment, attunement, preparation, container, item spent uses, class levels, spent hit dice, applied advancement values with matching IDs and types and spent uses for activities with matching IDs are retained.

Updates also automatically retain spell preparation method, source item, component supply, identification state, container currency, current equipment HP and assigned crew where the source supports those fields. Other source rules (including component cost, maximum HP and maximum crew) still update.

For GMs, **Update Sourced Items (BM5e)** is also available on an Item compendium in the Compendium directory and on an individual Item inside it. The pack action updates existing world and actor items sourced from that pack; the entry action updates existing copies of that source item. These actions do not import missing items or modify compendium contents. One confirmation shows the number of matched items, followed by a completion summary.

Updates reset local customizations to the source version. Optional preservation choices retain the existing name (`preserve.name`), image (`preserve.img`) and description (`preserve.description`). Item/activity flags, activities and other effect rules reset. The preservation checkbox (`preserve.effectChanges`) retains existing Active Effect world/module flag data (including flag-only Resilience configuration) and change rows targeting `flags.world.*` or `flags.<moduleId>.*`, excluding `flags.dnd5e.*` and `flags.core.*`. An additional checkbox (`preserve.activeModulesOnly`) limits module rows to currently enabled modules; world rows always qualify. Effect enabled/disabled status does not determine eligibility. Both options default to false and work with `confirm: false`. Selecting `activeModulesOnly` also enables preservation on its own. The `ac5e` namespace is associated with the `automated-conditions-5e` module ID.

Effects match by embedded ID. Matching effects receive the source configuration and source changes, then qualifying local rows override any source rows with the same attribute key. Other source rows remain. Unmatched local effects survive only if they contain qualifying flag data or rows and retain their effect context with all other change rows removed. No name matching is attempted. Review unmatched effects if source IDs changed. Bulk choices apply to every matched item and any individual fallback updates. Existing `dnd5e.advancementOrigin` and `dnd5e.advancementRoot` links are always retained independently of these options.

The API is available after `ready`:

```js
// All editable sourced items, including game.items and actor inventories.
const report = await bm5e.updateItemsFromCompendium();

// Only copies of one compendium item (legacy compendium UUIDs also work).
await bm5e.updateItemsFromCompendium({
  sourceUuid: "Compendium.my-module.my-items.Item.SOURCE_ITEM_ID"
});

// Only items sourced from a particular Item pack.
await bm5e.updateItemsFromCompendium({ compendium: "my-module.my-items" });

// World Items directory only; skip the confirmation for a scripted update.
await bm5e.updateItemsFromCompendium({
  compendium: "my-module.my-items",
  actorItems: false,
  confirm: false
});

// Actor items only.
await bm5e.updateItemsFromCompendium({ worldItems: false });

// Allow partial completion by retrying unconfirmed items individually if the batch fails.
await bm5e.updateItemsFromCompendium({
  compendium: "my-module.my-items",
  fallbackToIndividualUpdates: true
});

// One world or embedded Item document.
await bm5e.updateItemFromCompendium(item, { confirm: false });

// Preserve local customizations; these options also preselect the dialog checkboxes.
await bm5e.updateItemsFromCompendium({
  compendium: "my-module.my-items",
  preserve: { effectChanges: true, activeModulesOnly: true }
});
```

`worldItems`, `actorItems` and `confirm` default to `true`. Actor scope includes world actor inventories and unlinked token actor inventories across scenes; each item UUID is updated once. Source UUID and compendium filters combine when both are supplied. Invalid filters reject before any updates. Missing sources, type mismatches and preparation errors are reported before submitting the remaining valid updates. Writes use `foundry.documents.modifyBatch` with update operations grouped by parent, retaining existing item IDs. Batch write failures do not automatically trigger individual retries. `fallbackToIndividualUpdates` defaults to `false`; enabling it retries only items not confirmed updated, permits partial completion and reports each result separately. Only items the calling user can update are included; this API does not elevate permissions.

The batch report contains `matched`, `cancelled` and `updated`, `skipped` and `failed` arrays. Array entries contain the target item `uuid` and a `status`; failures may include an `error`. A single-item call returns one such entry. Item data is prepared with `game.items.fromCompendium(source, { clearFolder: true })`; source compendium folders and ownership are not applied to the existing item.

## Templates and Emanations

### Appearance

Items with a configured template show **Configure Appearance** below **Multiple**. Activities inherit the Item's settings unless their native **Override Targeting** option is enabled; an overriding activity can choose its own Custom, Palette, or Default mode. These settings affect future Regions, including automatically attached emanations. Existing Regions are unchanged.

- **Custom** opens an appearance editor with Appearance and Template Options tabs. Changes save automatically and select Custom. Closing without changes preserves the current mode. Clicking Custom again focuses the existing editor. No Region is created by the editor.
- **Palette** saves a snapshot of the current Region Palette appearance. Click it again to capture later Palette changes. It preserves the placed activity's name, locking, shapes, attachments, elevation, wall restrictions, and behaviors.
- **Default** uses normal dnd5e placement defaults while preserving saved Custom and Palette settings for later use.

On Foundry 14.370+, the GM's world setting **Default template Grid Aware** can enable or disable grid-aware shapes for new templates and emanations using **Default** appearance. **System Default** preserves native placement behavior. Custom and Palette modes are unaffected; existing Regions are unchanged. Target previews use the same preference.

The selected mode is highlighted. Custom includes color, visibility, Hidden, Locked, highlight mode, measurements, wall restrictions, and grid-aware shapes. **Hidden also deactivates behaviors**, as stated in the native form. Foundry provides no per-Region opacity field.

### Placement overrides

Enable **Override Placement** in Custom to save Levels and bottom/top elevation, including whether the top is inclusive. Without an override, normal placement supplies these values. Attached emanations retain their source token's Level; a saved elevation override can still apply. Wall-restricted Regions require a single Level.

Saved Level references use IDs in the original scene and unique matching Level names in another scene. If no matching Level can be resolved, normal placement remains in effect. Palette mode does not capture placement overrides.

### Self emanations

With Midi-QOL inactive and a usage token available, Self-range emanations use Foundry's `createTokenEmanation` API for the token's footprint, attachment, elevation, and Levels. This applies to all behaviors, including native Apply Active Effect. **Stationary** emanations use the full footprint without attaching; other emanations attach and move with the source token. Activities without a token retain normal placement.

Using the casting activity again offers **Turn Off** or **Re-Cast**. Turn Off removes that activity's automatically created emanations on the usage token without consuming resources or creating another use card. Re-Cast uses normal configuration and consumption, replacing the previous emanations only after new creation succeeds. Cancelling preserves the existing emanation. Automatically triggered In Range uses do not recreate it.

Concentration-linked Regions end with their cast's concentration. Removing an emanation also ends matching persistent Sequencer animations associated with its source Item and token, when Sequencer is active. Midi-QOL placement remains unchanged.

### Targeting preview

**Enable template targeting** is a world setting in the main BM5E module settings, enabled by default. Disable it when another module handles template targeting. Changes apply without a reload; disabling it during placement stops the preview and restores the previous selection. It does not disable In Range triggers or native Region effect behaviors.

While positioning an activity template, BM5E previews eligible targets using the activity's target type and native Region containment, including wall restrictions, Levels, and elevation. Placing selects the final targets and updates the casting card; cancelling restores the previous selection. Automatically placed emanations select targets on creation.

Moving an already placed Region does not change selected targets. In Range triggers and continuous native effect application operate independently of that selection.

When Midi-QOL is active, BM5E defers only if its effective **Auto target on template draw** option is enabled. Midi stores the global option as `ConfigSettings.autoTarget`; `none` disables it. An activity's `midiProperties.autoTargetAction` can override that global choice, with `default` inheriting it. If Midi targeting is disabled for the activity, BM5E can target when its own world setting is enabled.

## Contested Checks

Create a **Contested Check** activity and configure the initiator and opponent checks on its Effect tab. Each side can use an ability, a skill/tool, or a choice of skills/tools. Choose whether ties have no winner or favor either side. Add the effects to apply to the loser using the normal activity effect editor.

Target exactly one other creature and use the activity with an active GM connected. Normal activity resources are consumed once. BM5E posts one request card using the system's Group skill-check interface, with one row for each participant. Each creature's owner or the GM rolls its check; multiple configured skills/tools prompt that participant to choose. The card compares totals without imposing a DC and offers **Apply effects to loser** to the initiator's owner and the GM once both results are visible.

Use **Remove applied contest effects** to remove created effects or restore effects refreshed by this contest. Reroll the existing check messages to change the comparison; rerolls never move or reapply effects automatically. Remove an existing application before explicitly applying effects to a new loser. Cancelled rolls remain pending, deleted roll messages allow a new attempt, and the request's accepted results survive reloads. This initial implementation supports two participants and requires an active GM for coordination.

## Overtime Effects

BM5E overtime effects create activities that trigger automatically during combat or encounter timing.

Create an Applied Active Effect on an Activity, then add a BM5E change with this key:

```text
flags.bm5e.overtime
```

Selecting the **BM5E** change type focuses the attribute key and offers overtime and resilience suggestions. Choose `flags.bm5e.overtime` for overtime; existing keys and values are preserved when changing types.

D&D5e displays this key as **Overtime**. The BM5E change-summary icon opens the corresponding overtime editor directly.

### Activity Source

The value determines what activity BM5E should roll:

- `action=pseudo` creates a generated activity from the serialized fields.
- `action=link` with `link=ACTIVITY_UUID` reuses an existing activity. For an activity on the same source item, use `link=.Activity.ACTIVITY_ID`. This resolves against the source item after compendium import and uses its owning actor for the DC. Links to other items can retain their absolute UUIDs.
- Created editor activities are stored on the actor's hidden overtime item.
- An item-owned activity with the **BM5E Overtime** activation cost is a standalone overtime activity. Its editor stores configuration directly on that activity.

Updating an overtime effect without changing its configuration preserves its success/failure progress, trigger count, and outside-combat interval timer.

Supported pseudo activity types:

- `pseudoType=save`
- `pseudoType=check`
- `pseudoType=damage`
- `pseudoType=heal`

Compatibility keys such as `flags.bm5e.overtime.check` are still parsed, but new effects should use `flags.bm5e.overtime`.

### Timing

Use `turn=VALUE`.

Supported values:

- `none` (No combat trigger; use for interval-only effects)
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

`outOfCombat=true` adds outside-combat use without clearing `turn`. For example, `turn=turnStart; outOfCombat=true` keeps the turn-start offer. Damage and healing with a configured `interval` and `intervalUnit` can additionally run as world time advances outside combat.

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
- **Apply Effect** is available in every outcome selector (success/failure, each result, consecutive results, and trigger outcomes). Select it, then drop an Active Effect or paste its UUID. The referenced effect is copied to the affected actor with its configured duration starting at application. It can be combined with statuses and Remove Overtime. For example: `onEachSuccess=applyEffect:Item.example.ActiveEffect.immunity`. Each application creates a new effect, matching existing linked-effect outcomes; it does not toggle an existing immunity off.
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

Linked damage and healing parts are copied with the source cast's scaling already applied, so moving the activity onto the target's overtime item does not lose spell-slot scaling. Generated Damage and Heal activities set `activity.flags.bm5e.disallowRecipientBonuses = true` and retain source physical damage properties, including `mgc`. BM5E excludes native recipient bonuses to damage or healing performed while preserving damage-roll hooks and cancellation. Bonus providers such as AC5E must explicitly check this flag and skip their recipient bonus processing; the presence of other BM5E flags is not an opt-out. Saves, checks, and attacks retain their normal roll paths. Damage application still uses the target's defenses. Healing-received adjustments remain part of normal application.

For AC5E compatibility, check `config.subject?.flags?.bm5e?.disallowRecipientBonuses === true` before evaluating the rolling activity's outgoing damage/healing flags and conditions. Heal activities use the system's DamageRoll/damage-hook pipeline too. Skip AC5E bonus evaluation for flagged rolls, including dialog reevaluation, without returning `false` (which cancels the roll). Do not skip checks, saves, or attacks merely because they have `flags.bm5e`; their recipient modifiers are intentional. This flag does not disable consumption, activity use, target-side defenses, or effects applied to received healing. AC5E requires a separate compatibility patch to honor this contract.

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

Linked activity (replace `ACTIVITY_UUID` with the UUID of the activity to use):

```text
action=link; link=ACTIVITY_UUID; turn=turnStart; onFinalTrigger=removeOvertime; triggerTarget=3
```

At the start and end of a creature's turn, if one or more overtime effects are present on the actor, BM5E creates a chat message showing the available overtime rolls. If Midi-QOL is active and Activation Cost Automation is set to Auto Trigger, these overtime effects are rolled automatically.

## Enchantments

BM5E extends dnd5e enchantment activity cards so the user can choose a target and item directly from the chat card.

- Enable these additions with **Enable enchantment handling** in **Conditions & Effects**. This setting gates compact target selection, ownership controls, preferred-item matching, sequential recipient pickers, grouped GM requests, application limits, and Item (Actor) labels.
- Targeted tokens are shown on the enchantment card. Compact cards use an icon-style Targeted/Selected toggle and compact target rows.
- Enchantment trays are visible to GMs, source actor owners, and owners of targets recorded on the message. Players who only own a recorded target see only their own recipients. Message authorship or locally selected tokens do not grant access.
- Without recorded targets, only GMs and source actor owners see the tray; they can use the currently selected token as the recipient.
- Clicking the enchantment drop area processes all checked recipients sequentially, with one compatible-item picker open at a time. Each picker names its actor; Apply continues, Skip omits that actor, and Cancel Remaining (or closing the picker) stops the batch.
- **Enable item material properties** is an optional world setting under **Conditions & Effects**, disabled by default and requiring a reload. It adds **Wooden** (`wooden`), **Metal** (`metal`), **Glass** (`glass`), and **Crystal** (`crystal`) to weapon and equipment properties; assign them manually. Scripts can check `item.system.properties.has("metal")` or `item.system.properties.has("wooden")`. In an Enchant activity's **Restrictions** tab, select a property such as **Metal** and disable magical items when appropriate. With BM5E enchantment handling enabled, property restrictions are also available for **Any** item type, allowing the same activity to affect both weapons and equipment. Identifiers alone do not determine material.
- Self-enchanting activities show **(Self-enchant)** beside the activity type in the chat-card header. Their BM5e Behavior options retain **Allow enchantment stacks** and **Use message pop-out**; item limits, preferred identifiers, and apply-all are hidden and ignored. Their saved values return when self-enchantment is turned off.
- Enchantment row undo controls support **click** to remove one enchantment, **Shift + click** to remove every enchantment shown in the tray, and **Ctrl + Shift + click** to remove all shown enchantments on that row's item. Bulk removal includes earlier stacks and requires ownership of all affected items.
- **Maximum enchanted items per use** limits how many enchantment applications a single use can make across all items and recipients, including repeated applications to the same item. The tray continues to show active enchantments from all uses of the activity; only applications from the current use count toward its per-use limit. It defaults to **0** (unlimited), while **Maximum enchanted items** keeps its separate overall limit across uses. For a Rust Monster, use `armor*, shield*`, enable **Allow enchantment stacks**, set the per-use limit to **1**, and set the overall maximum higher or to **0**.
- **Preferred item identifiers or tags** on the Enchant activity accepts a comma-separated list. Use `tag:true-strike` to match that exact tag in either `item.flags.bm5e.tags` or `item.flags.blt5e.tags`, stored as arrays of strings (for example, `["true-strike"]`). Add or remove BM5e tags below Item Identifier using the input and Add button. BLT5e tags appear there as read-only entries. Tags are case-sensitive and can be mixed with identifiers; an item may match any entry. Tag preferences limit the picker to compatible matches. Use `*` for partial matches: `shield*` matches identifiers containing `shield`, such as `shield`, `shield-wooden`, or `tower-shield`. Entries without `*` match exactly. A unique compatible match on the chosen actor is selected automatically. With tags or wildcards, the picker shows only matching items; plain identifiers alone put matches first. Enable **Apply to all matching items** to enchant every compatible match without opening the picker, subject to the existing recipient and item limits. Blank identifiers or no matches open the usual picker to choose one item, even when **Apply to all matching items** is enabled. The same preferences apply to GM requests. Dragging another compatible item onto the card remains allowed; self-enchantments retain their normal destination.
- The activity target count limits distinct recipients per use; Maximum Enchanted Items still limits distinct items across the activity (default 1, so increase it or use 0 for multiple recipients). Applied entries show Item (Actor) and remain tracked by native enchantment effects after reload.
- If the user does not own the target actor, BM5E whispers a GM request with an **Apply Enchantment** button.
- A single GM request keeps the original activity, recipient actors, source message, and enchantment profile, and processes its recipients sequentially. Cancelling preserves the remaining recipients in the request. Older single-recipient requests remain supported.
- BM5E passes the source chat message and concentration effect through to dnd5e's enchantment application.
- By default, a matching enchantment from the same activity replaces the previous matching enchantment on that item.
- **Allow Enchantment Stacks** permits repeated enchantments from the same activity on one item.
- **Maximum Enchanted Items** limits how many different items an activity can enchant at once. Its default is `1`, and clearing the field restores that default. Use `0` for no limit.

### Magical overtime activities

`magical=true` toggles the generated activity's magical state relative to its linked Item's magical property. The overtime editor exposes this as a checkbox. Automated Conditions 5e can read that state through its `prepareEvaluationState` hook.
