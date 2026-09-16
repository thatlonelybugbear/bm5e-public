## v14.602.1
* Updated BM5e for D&D5e v6.0 and dropped v5.x compatibility.
  * Updated chat-message, target, activity, damage, healing, enchantment, and overtime handling for the D&D5e 6 data model.
  * Updated Active Effect changes to use `effect.system.changes`, stable change IDs, and the new `EffectChangeConfig` application.
* Fixed hot-reload registration issues.
* Added roll controls to existing compact system chat messages:
  * Attack, save, and check rolls can be rerolled with Advantage, Normal, or Disadvantage.
  * Damage and healing rolls can be rerolled as Normal or Critical, including every damage part belonging to an activity.
  * Compact save summaries provide a reroll control for each target.
  * Rerolls update the existing message, retain the initial total and target DC, track reroll counts, and refresh success or failure.
* Added labels to compact chat-card Attack, Damage, Save, and Check controls.
* Added hit/miss coloring for attack controls and Advantage/Critical coloring for reroll options.
* Relinked configurable chat document double-click handling for actor names, target pills, item icons, activity icons, and effect icons, with image fallback when the user cannot open the document.
* Added a configurable chat document double-click handling for the chat message flavor text, with image fallback when the user cannot open the document.
* Added the BM5e Triggers activity activation type and editor for Damaged and Healed events, resulting HP percentage conditions, and damage or healing type filters. Triggered activities use identifiable turn-style chat messages in combat and execute directly outside combat.
* Added integrated mutable-damage automations for Death Ward, Undying Sentinel, and Undead Fortitude (select the ones you want in the module settings). When multiple protections are present, BM5e resolves Death Ward, then Undying Sentinel, then Undead Fortitude. Undying Sentinel triggers only while its item has uses available.
* Added an independent Integrated Damage Modifiers multiselect, currently offering Elemental Adept. Matching spell damage ignores resistance when the caster owns an `elemental-adept` item named with its damage type, such as `Elemental Adept (Fire)`.
* Updated the BM5e Active Effect value-editor controls to be compatible with the new system change editor, including direct access from BM5e change summary icons.
* Displayed the `flags.bm5e.overtime` attribute as `Overtime` and cleared that automatically populated key when another change type is selected.
* Matched the damage RESTORE button to the compact system APPLY button.
* Deferred save-based damage multipliers to the native system damage application tray.
* Kept detailed Attack and Damage roll results in button tooltips to avoid crowding compact chat cards.
* Deferred concentration and exhaustion condition management to the system and constrained the Status HUD palette to the viewport while preserving HUD scaling.
* Replaced BM5e prototype bookkeeping properties with private WeakMap state without changing libWrapper registrations or fallback method replacements.

* Undead Fortitude save rerolls update the save message but do not retroactively revise damage that has already been applied.

## v14.533.4
* Activities that trigger when their actor takes damage now create their card correctly during combat.
* The activity BM5E Editor now properly shows only when the activity cost is set to Overtime.
* Updated compatibility for Foundry VTT v14.366.

## v14.533.3
* Overtime activities can be marked magical; linked activities inherit their source Item's magical property and can toggle it per overtime activity.
    * To be properly identified by Automated Conditions 5e, make sure you are on v14.533.15 or later of that module.

## v14.533.2
* Assign Status Effects Sorter scaling is now optional
* Added missing packs

## v14.533.1
* Added configurable use-message pop-out defaults:
    * Configure item, activity, actor, and Extra Attack identifiers from the module settings.
    * Import, export, and restore the default identifier configuration.
    * Enable use-message pop-outs per activity from its Behavior settings.
* Added optional enchantment handling:
    * Enable BM5E enchantment behavior from the module settings.
    * Configure stackable enchantments and the maximum number of different enchanted items per activity.
    * Added safer authorization checks for self-enchant and targeted enchantment cards.
* Expanded overtime support:
    * Activities can use the BM5E Overtime activation cost and save their configuration directly on the activity.
    * Overtime attack activities now resolve success and failure outcomes from hit and miss results.
    * Standalone overtime activities can trigger from combat turn messages.
    * Linked Save and Check activities can preserve their source DC, including ability, spellcasting, class-source, custom-formula, bonus, and scaling data.
    * Overtime outcomes that apply Exhaustion now increase the target's exhaustion level, up to the configured D&D5e maximum.
* Added optional effect-application outcome handling to keep effect targets aligned with attack misses and save outcomes.
* Added optional damage-application restoration with APPLY/APPLIED and RESTORE/RESTORED controls.
* Added an optional temporary libWrapper override for mutable D&D 5e damage-calculation hooks.
* Fixed Status HUD layout when filtering is disabled.
* Added Status HUD scaling with canvas zoom compensation.
* Added a one-time repair for BM5E compendium folder assignments cached by Foundry.
* Updated the in-app README introduction and module-settings documentation.

## v13.533.4
* Expanded BM5e overtime activity generation:
    * Overtime feature items are now labeled per actor/token, for example `Randal Overtime Effects`.
    * Generated overtime activities add the `name=` entry to the activity chat flavor.
    * Multi-ability overtime saves/checks accept `ability=str||dex`, `ability=str || dex`, or `ability=str | dex | con`.
* Added separate overtime result counters for total and consecutive roll outcomes:
    * Total removals: `overallSuccesses=3; onSuccess=remove` or `overallFailures=3; onFailure=remove`.
    * Consecutive removals: `consecutiveSuccesses=3; onSuccess=remove` or `consecutiveFailures=3; onFailure=remove`.
    * Per-roll status actions: `onEachSuccess=prone` or `onEachFailure=prone`.
    * Compatibility aliases remain supported: `saveSuccesses`, `saveSuccess`, `successes`, `saveFailures`, `saveFails`, `failures`, `successiveSuccesses`, `successiveSaves`, `successiveFailures`, and `successiveFails`.
* Overtime chat cards now select the associated token before non-midi-qol rolls so follow-up roll and damage actions use the same token; this initial implementation intentionally leaves that token selected.
* Added Status HUD scaling with canvas zoom compensation.
* Added a Module Support settings menu with README, issue tracker, Discord, Ko-Fi, and Patreon links.
* Improved enchantment chat card rendering after reload by using the D&D5e chat render hook surface.
* Hardened enchantment target selection so clicks use the target tray from the rendered card that was clicked.

## v13.533.3
* Removed leftover overtime debug `console.log` calls from activity creation and roll handling.
* Fixed token HUD exhaustion interaction handling to prevent duplicate processing from rapid/secondary click events.
* Added `auxclick` handling for exhaustion status in the token HUD to avoid unintended right-button aux interactions.

## v13.533.2.1
* Fix for selected effects not being visibly marked.

## v13.533.2
* Reworked CSS when the Status Effect Sorter is enabled, to match the Foundry UI layout more closely.

## v13.533.1.1
* Added pt_BR translation by [Kharmans](https://github.com/Kharmans) 🤗

## v13.533.1
* Restrict hit detection to specific elements for double clicking on Chat images mechanics.
* Compatibility bump for v5.3.3

## v13.530.1
* Compatibility bump for D&D5e v5.3.0

## v13.525.3
* Status HUD filter keyboard flow improvements:
    * Opening the Assign Status panel now brings focus to the filter field for quicker keyboard use.
    * Press Enter to activate the first visible matching status effect and keep typing in the filter field.
    * Press Escape to clear the filter, and press Escape again on an empty filter to close the Assign Status panel.
    * While filtering, left/right clicking a status now restores filter focus after the HUD updates so keyboard flow continues smoothly.
    * The status filter now retains focus after toggling a status on/off, allowing for quick successive toggling without needing to re-focus the filter.
* Fixed dnd5e Exhaustion icon refresh when lowering Exhaustion back to 0 from the HUD.
* Improved Status HUD sorting consistency so status ordering is stable and predictable.
* Fixed an issue where the first Status HUD open after world load could show incorrect grid sizing until a setting was changed.
* Improved Assign Status Effects compatibility with modules that inject additional temporary/custom effects like CPR:
    * Added effects now follow the same click behavior as regular status rows (icon or label area).
    * Clear effects now also removes compatible module-added effects alongside regular statuses in one update.
* Improved Auto Popout item detection:
    * Matching is now more resilient across item names/identifiers (including slug-style names like `eldritch-blast`).
    * Detection now respects the expected item type (for example spells vs features) for more reliable behavior.
* Hardened overtime and damage-application handling to reduce edge-case failures during roll/message lifecycle updates (more to come in the next patch).

## v13.525.2.2
* Quick fix for damage rolls and shortcut keys.

## v13.525.2.1
* Quick fix for toggling status effects via the Clear all button of the sorter.

## v13.525.2
* Make sure that using keyboard shortcuts for attack roll advantageModes do not pass themselves on damage rolls when auto rolling damage.
* Adds an active GM warning notification when both Bugbear's Assign Status Effects Sorted and Bugbear's Mechanics 5e are enabled at the same time. Please use the BM5e status HUD sorter option instead.
  
## v13.525.1
* Assign Status Effects Sorter
    * Per user settings
    * Sorted statuses left to right (row) or top to bottom (column)
    * Status search field
    * Clear all statuses button
* Auto roll damage
    * For attack activities, auto-roll damage only applies on hit/crit (based on the attack result)
* Auto pop out chat cards for quick reuse of attack/damage buttons for a built-in list of items/features. Currently:
    * Extra attack feature (when the actor has an item with identifier `extra-attack`)
    * Eldritch Blast (character level 5+)
    * Guardian of Faith spell
    * Magic Missile spell
    * Scorching Ray
    * Spirit Guardians spell
    * Spiritual Weapon attacks
* Attack buttons show the attack total, outcome (critical/fumble/hit/miss), and reuse count when reusing the same chat card.
* Damage buttons show the damage total and the number of times the damage button has been reused (or replaced) from the same chat card.
* Double clicking in the chat log on Actor/Effect/Item icons will:
    * Open the document if you have permission.
    * Otherwise open the document's image.
    * Double clicking again will close the relevant opened sheet/popout.
* Verified for D&D5e 5.2.5.

## v13.524.2.1
* Reinstate damage application tray functionality for saves

## v13.524.2
* Refactor user permissions for overtimes

## v13.524.1
* Fix for damage overtimes not working
* System v5.2.4 compatibility bump

## v13.523.1
* First pass on Overtime effects
* System v5.2.3 compatibility bump

## v13.521.2
* Remove forgotten notification message.

## v13.521.1
* Initial release
* The system's damage application tray for save related damage rolls, will now always respect the Damage on Save options of the activity, selecting the correct multiplier based on whether the save is a success or not.
