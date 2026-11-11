# 📜 WoW Forever Macro Compendium

Practical class, profession and utility macros for **World of Warcraft: Forever**. Choose macros that add useful targeting, modifiers or control; put ordinary single-action spells directly on your action bar.

> 🧪 **Testing status:** Reviewed against the official class articles and development notes linked below. Not all macros here have been tested in-game. Spell availability depends on level, talents, race, pet and the installed build. Check each macro with your character before relying on it.

<a id="contents"></a>

## 🧭 Contents

- [Setup and macro rules](#setup-and-macro-rules)
- [Communication and Lua](#communication-and-lua)
- [Druid](#druid)
- [Hunter](#hunter)
- [Mage](#mage)
- [Paladin](#paladin)
- [Priest](#priest)
- [Rogue](#rogue)
- [Shaman](#shaman)
- [Warlock](#warlock)
- [Warrior](#warrior)
- [Universal](#universal)
- [Professions and camping](#professions-and-camping)
- [Sources](#sources)
- [Buy me a coffee](#buy-me-a-coffee)

<br>

---

<br>

<a id="setup-and-macro-rules"></a>

## 🛠️ Setup and macro rules

Open `/macro`, create a character-specific macro, select the question-mark icon and paste one code block. Keep each macro within **255 characters**, including line breaks; the examples below are checked against that conservative limit. Use spell names from your localized spellbook. Unranked spells avoid pinning an obsolete rank. Talent spells must be learned.

- One press normally executes at most one ability on the global cooldown (GCD). Separate `/cast` lines do not wait. Off-GCD actions and item cooldown rules still apply.
- `/castsequence` needs repeated presses, advances only when its current action succeeds, and can stall. `reset=5` counts inactivity, not a timer that casts the next spell.
- `[@mouseover,help,nodead][help,nodead][@player]` means friendly living mouseover, friendly living target, then self. `[]` uses normal targeting/auto-self-cast settings; it is not an explicit self target.
- `[@focus,harm,nodead][harm,nodead]` means hostile living focus, then target. Focus is set with `/focus`; it is not automatically your tank.
- Use `mod:shift`, `mod:ctrl`, or `mod:alt` for deliberate choices. A conflicting modified key binding can intercept the press.
- `!` prevents toggling an applicable toggle off. It does not bypass a cooldown, force a proc or confirm a cast.
- `/stopcasting` interrupts your current cast/channel. Use it deliberately for urgent interrupts, not on every heal.
- `/cancelaura` removes a cancellable aura, not arbitrary harmful debuffs. `/cancelform` can expose a druid in caster form.
- `/startattack` may break crowd control or stealth. It is intentionally omitted from control/opening macros.
- Warrior stances normally use 1 Battle, 2 Defensive, 3 Berserker. Verify druid form indices on your character before using form conditions.
- `[@cursor]` specifies ground placement for supported spells; it is not a general `cursor` condition.

**Commands:** `/cast`, `/use`, `/stopcasting`, `/stopattack`, `/startattack`, `/focus`, `/clearfocus`, `/petattack`, `/petpassive`, `/petfollow`, `/petmoveto`, `/equipslot`, `/dismount`, `/cancelaura`, `/stopmacro`.

<br>

### Conditions in square brackets

Conditions filter an action; they do not wait for it to become usable. Unit selectors choose whom it affects. These examples use standard [macro conditional syntax](https://us.forums.blizzard.com/en/wow/t/macros-essential-information/21139).

| Syntax | Purpose |
|---|---|
| `@mouseover`, `@focus`, `@player`, `@pet` | Select mouseover, focus, yourself or your pet. |
| `help` / `harm` | Require an ally / enemy. |
| `exists` | Require a unit. |
| `dead` / `nodead` | Require a dead / living unit. |
| `combat` / `nocombat` | Check your combat state. |
| `mod:shift`, `mod:ctrl`, `mod:alt` | Check held modifier keys. |
| `nomod` | No modifier held. |
| `stance:1` / `form:3` | Check stance / form index. |
| `stealth` / `nostealth` | Check stealth state. |
| `pet` / `nopet` | Check pet presence. |
| `channeling` / `nochanneling` | Check whether you are channeling. |
| `worn:Bows` / `equipped:Bows` | Check equipped item type; synonymous keywords. |
| `group:party` / `group:raid` | Check group type. |

**Reading a line:** commas require all tests; adjacent brackets offer alternatives; a semicolon introduces another action; `[]` accepts normal targeting. A matching option that fails range or cooldown checks does not fall through.

**Avoid redundant checks:** `help`, `harm` and `dead` each imply `exists`; adding `exists` to the same bracket is unnecessary. `@target` is also optional because the current target is the default. Keep `nodead` when you need a living unit: `help` and `harm` do not replace it. Without it, a dead mouseover can match and block the target/self fallback. `nodead` alone does not establish that a unit exists, so pet checks such as `[@pet,exists,nodead]` remain useful.

`!` precedes applicable toggle spells; `#showtooltip` controls button display. Neither is a bracket condition. Keep condition keywords in English; localize spell names and equipment-type values. Confirm your character's form indices.

<br>

**Equipment slots:** **13** first trinket, **14** second trinket, **16** main hand, **17** off hand. Equipping a trinket is not activating it; equipment restrictions and equip cooldowns still apply.



---
<br>
<br>


<a id="communication-and-lua"></a>

## 💬 Communication and Lua

Modern API namespaces, protected/secret values, encounter restrictions and chat throttles must be checked against the current client. Addon messaging belongs in an addon using its supported API, not this copy-ready library.

Use `%t` in slash-command chat for the current target. Do not rely on `%f` or `%m`, or expect `%t` embedded in a Lua string to perform slash-command substitution. `/run` cannot bypass secure casting/targeting restrictions. Raid warnings require appropriate group privileges. Whisper recipients should be entered explicitly; a creature focus is not a whisper recipient.

<br>

### Separate party intent callout

This announces your **current target**, not your mouseover or focus. Press only when useful. A `/cast` followed by chat sends the chat even if the cast fails. Use callouts to communicate intent rather than confirm success.

```text
/p Preparing crowd control on %t - please avoid damage.
```



---

<br>
<br>

<a id="druid"></a>

## 🌿 Druid

**WoW Forever changes:** Use Faerie Fire in caster or feral forms. Revive provides out-of-combat resurrection. Use your learned Bear/Dire Bear name in form templates. Shifting and a follow-up ability can need another press after the GCD. See [official Druid changes](https://news.blizzard.com/en-us/article/24301515/world-of-warcraft-forever-class-deep-dives-hunter-and-druid).

Focus: form setup, crowd control and healing without changing targets. Put straightforward abilities such as Barkskin directly on your action bar.

<br>

### Bash / bear setup

Replace Dire Bear Form with Bear Form if that is what you know.

```text
#showtooltip Bash
/cast [noform:1] Dire Bear Form
/cast [form:1] Bash
```

<br>

### Cat Form

Repeated presses preserve the active form or attack instead of toggling it off.

```text
#showtooltip Cat Form
/cast !Cat Form
```

<br>

### Feral Charge — Bear / Cat

Requires the talent. Use from Bear/Dire Bear or Cat Form: hostile living mouseover → current enemy, preserving your chosen form. Verify Bear is form 1 and Cat is form 3. The Cat version does not interrupt or immobilize; see [Blizzard's Druid changes](https://news.blizzard.com/en-us/article/24301515/world-of-warcraft-forever-class-deep-dives-hunter-and-druid). Use the Feral Charge name from your spellbook if your client labels the form variants separately.

```text
#showtooltip Feral Charge
/cast [form:1/3,@mouseover,harm,nodead][form:1/3,harm,nodead] Feral Charge
```

<br>

### Growl

Taunts a hostile mouseover, then your current enemy, while in Bear Form.

```text
#showtooltip Growl
/cast [form:1,@mouseover,harm,nodead][form:1,harm,nodead] Growl
```

<br>

### Entangling Roots

Hostile mouseover → hostile focus → current enemy. Your selected target stays unchanged.

```text
#showtooltip Entangling Roots
/cast [@mouseover,harm,nodead][@focus,harm,nodead][harm,nodead] Entangling Roots
```

<br>

### Hibernate

Hostile mouseover → hostile focus → current enemy. Your selected target stays unchanged.

```text
#showtooltip Hibernate
/cast [@mouseover,harm,nodead][@focus,harm,nodead][harm,nodead] Hibernate
```

<br>

### Innervate

Friendly mouseover → friendly target → self, without changing your selected target.

```text
#showtooltip Innervate
/cast [@mouseover,help,nodead][help,nodead][@player] Innervate
```

<br>

### Healing Touch

Friendly mouseover → friendly target → self. Useful for a direct heal without changing your selected target.

```text
#showtooltip Healing Touch
/cast [@mouseover,help,nodead][help,nodead][@player] Healing Touch
```

<br>

### Rejuvenation

Friendly mouseover → friendly target → self, without changing your selected target.

```text
#showtooltip Rejuvenation
/cast [@mouseover,help,nodead][help,nodead][@player] Rejuvenation
```

<br>

### Regrowth

Friendly mouseover → friendly target → self, without changing your selected target.

```text
#showtooltip Regrowth
/cast [@mouseover,help,nodead][help,nodead][@player] Regrowth
```

<br>

### Mark / Gift of the Wild / Thorns

**Normal:** Mark of the Wild. **Shift:** Gift of the Wild. **Ctrl:** Thorns. Choose a friendly target or use your normal self-cast behavior. Learn the selected spell and carry its reagents; this does not automatically leave a form or cycle through buffs.

```text
#showtooltip
/cast [mod:ctrl] Thorns; [mod:shift] Gift of the Wild; Mark of the Wild
```

<br>

### Pounce / Rake

Use in Cat Form with Prowl for the stealth opener. Do not add `/startattack` before it. Shadowmeld alone does not grant Pounce or Ravage; Blizzard corrected that interaction in the [November 6 hotfixes](https://news.blizzard.com/en-us/article/24315281/wow-forever-hotfixes-november-6-2026).

```text
#showtooltip
/cast [stealth] Pounce; Rake
```

<br>

### Ravage / Shred

In Cat Form: Ravage while stealthed with Prowl, otherwise Shred. Position requirements still apply.

```text
#showtooltip
/cast [stealth] Ravage; Shred
```

<br>

### Shred / Shifting Power

**Normal:** start attacking a living enemy and use Shred. **Shift:** use your learned Shifting Power talent. Use in Cat Form; Shred's position and resource requirements still apply. Shift does not start auto-attack, but it does not stop an attack already running.

```text
#showtooltip
/startattack [nomod:shift,harm,nodead]
/cast [mod:shift] Shifting Power; [harm,nodead] Shred
```

<br>

### Prowl / Cat setup

Verify Cat is form 3. May need another press after shifting.

```text
#showtooltip Prowl
/cast [noform:3] Cat Form
/cast [form:3,nocombat] !Prowl
```

<br>

### Dash / Cat setup

Enter Cat Form, then use Dash. Another press may be needed after shifting.

```text
#showtooltip Dash
/cast [noform:3] Cat Form
/cast [form:3] Dash
```

<br>

### Rebirth / Revive

Combat selects Rebirth; otherwise Revive. A living or hostile target is not selected. Rebirth still needs its reagent/cooldown.

```text
#showtooltip
/cast [combat,@mouseover,help,dead][combat,help,dead] Rebirth; [@mouseover,help,dead][help,dead] Revive
```

<br>

### Travel forms

Only learned forms; no automatic fallback if the selected form is unavailable.

```text
#showtooltip
/cast [swimming] !Aquatic Form; [outdoors] !Travel Form; !Cat Form
```



---

<br>
<br>

<a id="hunter"></a>

## 🏹 Hunter

**WoW Forever changes:** Traps are placed at your own location. Use control and traps separately. Talent-dependent abilities such as Bestial Wrath and Scatter Shot require the appropriate build.

Focus: Auto Shot, pet handling and crowd control. Put Bestial Wrath and Rapid Fire directly on your action bar. Use [shared pet positioning](#pet-positioning) to park or recall your pet. Place Freezing Trap at your feet on a separate action from Scatter Shot.

<br>

### Aimed Shot

Starts Auto Shot without toggling it off on repeated presses. Requires a valid enemy and normal range/movement conditions.

```text
#showtooltip Aimed Shot
/cast !Auto Shot
/cast [harm,nodead] Aimed Shot
```

<br>

### Arcane Shot

Starts Auto Shot without toggling it off on repeated presses. Requires a valid enemy and normal range/movement conditions.

```text
#showtooltip Arcane Shot
/cast !Auto Shot
/cast [harm,nodead] Arcane Shot
```

<br>

### Serpent Sting

Starts Auto Shot without toggling it off on repeated presses. Requires a valid enemy and normal range/movement conditions.

```text
#showtooltip Serpent Sting
/cast !Auto Shot
/cast [harm,nodead] Serpent Sting
```

<br>

### Auto Shot

Starts Auto Shot without toggling it off on repeated presses. Requires a valid enemy and normal range/movement conditions.

```text
#showtooltip Auto Shot
/cast !Auto Shot
```

<br>

### Hunter's Mark / pet attack

Starts the pet immediately; use plain Hunter's Mark when marking before a pull.

```text
#showtooltip Hunter's Mark
/petattack [harm,nodead]
/cast [harm,nodead] Hunter's Mark
```

<br>

### Feign Death

Stops your cast and attacks, recalls your pet, then attempts Feign Death.

```text
#showtooltip Feign Death
/stopcasting
/stopattack
/petpassive
/petfollow
/cast Feign Death
```

<br>

### Scatter Shot

Pet recall and stopping attacks reduce accidental breaks; existing damage-over-time effects still matter.

```text
#showtooltip Scatter Shot
/stopcasting
/stopattack
/petpassive
/petfollow
/cast [harm,nodead] Scatter Shot
```

<br>

### Freezing Trap / pet recall

Stops your attacks and recalls your pet before placing the trap at your feet. Existing damage-over-time effects can still break the freeze. Your pet stays Passive until you change its behavior or give another attack command.

```text
#showtooltip Freezing Trap
/stopcasting
/stopattack
/petpassive
/petfollow
/cast Freezing Trap
```

<br>

### Aspects: Hawk / Cheetah / Monkey

**Normal:** Hawk. **Shift:** Cheetah. **Ctrl:** Monkey. Requires the chosen aspect; repeated presses preserve it.

```text
#showtooltip
/cast [mod:ctrl] !Aspect of the Monkey; [mod:shift] !Aspect of the Cheetah; !Aspect of the Hawk
```

<br>

### Cancel travel aspects

Remove your own Cheetah or Pack before taking hits, without casting another aspect. This does not remove an existing daze or another Hunter's Pack.

```text
/cancelaura Aspect of the Cheetah
/cancelaura Aspect of the Pack
```

<br>

### Revive / Call / Mend Pet

Shift forces Revive Pet when a dead/despawned pet is not exposed as a pet unit.

```text
#showtooltip
/cast [mod:shift][@pet,dead] Revive Pet; [nopet] Call Pet; [@pet,exists,nodead] Mend Pet
```

<br>

### Pet attack / recall

Normal press sends your pet to a hostile living mouseover, otherwise your current enemy. Shift recalls even when an enemy remains selected. For ground placement, use [shared pet positioning](#pet-positioning).

```text
#showtooltip
/petattack [nomod:shift,@mouseover,harm,nodead][nomod:shift,harm,nodead]
/petpassive [mod:shift]
/petfollow [mod:shift]
```

<br>

### Feed Pet — item template

Replace YOUR PET FOOD with an appropriate food from your bags. Verify Feed Pet exists and the pet diet accepts it.

```text
#showtooltip Feed Pet
/cast [nocombat,@pet,exists,nodead] Feed Pet
/use [nocombat,@pet,exists,nodead] YOUR PET FOOD
```

<br>

### Melee choices

Shift chooses Mongoose Bite; normal press chooses Raptor Strike. Does not bypass their requirements.

```text
#showtooltip
/cast [mod:shift] Mongoose Bite; Raptor Strike
/startattack [harm,nodead]
```

---

<br>
<br>
<br>

<a id="mage"></a>

## ❄️ Mage

**WoW Forever changes:** Arcane Blast requires its talent. Use deliberate downranking for utility rather than damage; see [Mage changes](https://news.blizzard.com/en-us/article/24302097/world-of-warcraft-forever-class-deep-dives-mage-and-shaman).

Focus: interrupts, crowd control and preserving channels. Blizzard and Arcane Explosion can use their normal spell buttons.

<br>

### Arcane Blast

Does not interrupt an existing channel when pressed again.

```text
#showtooltip Arcane Blast
/cast [nochanneling] Arcane Blast
```

<br>

### Conjure Food / Water

**Normal:** conjure water. **Shift:** conjure food.

```text
#showtooltip
/cast [mod:shift] Conjure Food; Conjure Water
```

<br>

### Counterspell

Targets a hostile mouseover, then focus, then your current target.

```text
#showtooltip Counterspell
/stopcasting
/cast [@mouseover,harm,nodead][@focus,harm,nodead][harm,nodead] Counterspell
```

<br>

### Evocation

Repeated presses preserve an existing channel. Move to cancel intentionally.

```text
#showtooltip Evocation
/cast [nochanneling] Evocation
```

<br>

### Ice Block

Normal press casts; Shift cancels without immediately recasting.

```text
#showtooltip Ice Block
/stopcasting
/cancelaura [mod:shift] Ice Block
/stopmacro [mod:shift]
/cast !Ice Block
```

<br>

### Polymorph

Hostile mouseover → hostile focus → current enemy. Your selected target stays unchanged.

```text
#showtooltip Polymorph
/cast [@mouseover,harm,nodead][@focus,harm,nodead][harm,nodead] Polymorph
```

<br>

### Frostbolt / Rank 1 slow

**Normal:** highest learned Frostbolt rank. **Alt:** Rank 1 for its shorter cast. Use your localized rank text. Frost Blast was removed in the [November 2 development notes](https://us.forums.blizzard.com/en/wow/t/2360696).

```text
#showtooltip
/cast [mod:alt] Frostbolt(Rank 1); Frostbolt
```

<br>

### Remove Lesser Curse

Friendly mouseover → friendly target → self, without changing your selected target.

```text
#showtooltip Remove Lesser Curse
/cast [@mouseover,help,nodead][help,nodead][@player] Remove Lesser Curse
```


---

<br>
<br>

<a id="paladin"></a>

## 🔨 Paladin

**WoW Forever changes:** **Seals are no longer consumed by Judgements.** Holy Strike deals melee Holy damage. Judgement with Seal of Fury active taunts the enemy. These mechanics are documented in the [Paladin deep dive](https://news.blizzard.com/en-us/article/24302099/world-of-warcraft-forever-class-deep-dives-paladin).

Focus: support targeting, deliberate bubble cancellation and seal control.

<br>

### Divine Shield

Normal press preserves the bubble; Shift cancels it deliberately.

```text
#showtooltip Divine Shield
/stopcasting
/cancelaura [mod:shift] Divine Shield
/stopmacro [mod:shift]
/cast Divine Shield
```

<br>

### Cleanse

Friendly mouseover → friendly target → self, without changing your selected target.

```text
#showtooltip Cleanse
/cast [@mouseover,help,nodead][help,nodead][@player] Cleanse
```

<br>

### Blessing of Freedom

Friendly mouseover → friendly target → self, without changing your selected target.

```text
#showtooltip Blessing of Freedom
/cast [@mouseover,help,nodead][help,nodead][@player] Blessing of Freedom
```

<br>

### Blessing of Protection

Choose the recipient deliberately: protection can make enemies switch to another target.

```text
#showtooltip Blessing of Protection
/cast [@mouseover,help,nodead][help,nodead][@player] Blessing of Protection
```

<br>

### Holy Light

Friendly mouseover → friendly target → self, without changing your selected target.

```text
#showtooltip Holy Light
/cast [@mouseover,help,nodead][help,nodead][@player] Holy Light
```

<br>

### Flash of Light

Friendly mouseover → friendly target → self, without changing your selected target.

```text
#showtooltip Flash of Light
/cast [@mouseover,help,nodead][help,nodead][@player] Flash of Light
```

<br>

### Lay on Hands

Friendly mouseover → friendly target → self, without changing your selected target.

```text
#showtooltip Lay on Hands
/cast [@mouseover,help,nodead][help,nodead][@player] Lay on Hands
```

<br>

### Holy Strike

Starts melee auto-attacks even when the ability cannot fire yet. Choose a valid enemy first.

```text
#showtooltip Holy Strike
/startattack [harm,nodead]
/cast [harm,nodead] Holy Strike
```

<br>

### Light's Vigil: focus support

Requires the Holy talent. Cast on a living friendly focus, otherwise a living friendly target. No enemy fallback: [Blizzard made Light's Vigil friendly-only](https://us.forums.blizzard.com/en/wow/t/2360696).

```text
#showtooltip Light's Vigil
/cast [@focus,help,nodead][help,nodead] Light's Vigil
```

<br>

### Holy Shock

Heal a friendly mouseover or target; otherwise damage your enemy target. Requires its talent.

```text
#showtooltip Holy Shock
/cast [@mouseover,help,nodead][help,nodead][harm,nodead] Holy Shock
```

<br>

### Seal / Judgement

Shift applies the seal. Normal press judges the currently active seal without consuming it.

```text
#showtooltip
/cast [mod:shift] Seal of Righteousness; [harm,nodead] Judgement
```

<br>

### Seal of Fury taunt setup

First apply Seal of Fury with Shift. After the GCD, release Shift and judge the enemy. With a different seal active this does NOT taunt. No automatic buff detection.

```text
#showtooltip
/cast [mod:shift] Seal of Fury; [@mouseover,harm,nodead][harm,nodead] Judgement
```

<br>

### Divine Favor / Holy Light

Requires Divine Favor. If cooldown/GCD rules prevent the heal on the same press, press again.

```text
#showtooltip Holy Light
/cast Divine Favor
/cast [@mouseover,help,nodead][help,nodead][@player] Holy Light
```



---

<br>
<br>

<a id="priest"></a>

## ✨ Priest

**WoW Forever changes:** Binding Heal and Prayer of Mending require their talents. Shadow Word: Death is an enemy-targeted ability. See [official Priest changes](https://news.blizzard.com/en-us/article/24301514/world-of-warcraft-forever-class-deep-dives-priest-and-warrior).

Focus: mouseover support, enemy utility and channel protection. The healing macros share a familiar target priority so you can use the spells you know.

<br>

### Binding Heal

Friendly mouseover → friendly target → self, without changing your selected target.

```text
#showtooltip Binding Heal
/cast [@mouseover,help,nodead][help,nodead][@player] Binding Heal
```

<br>

### Flash Heal

Friendly mouseover → friendly target → self, without changing your selected target.

```text
#showtooltip Flash Heal
/cast [@mouseover,help,nodead][help,nodead][@player] Flash Heal
```

<br>

### Greater Heal

Friendly mouseover → friendly target → self, without changing your selected target.

```text
#showtooltip Greater Heal
/cast [@mouseover,help,nodead][help,nodead][@player] Greater Heal
```

<br>

### Power Word: Shield

Friendly mouseover → friendly target → self, without changing your selected target.

```text
#showtooltip Power Word: Shield
/cast [@mouseover,help,nodead][help,nodead][@player] Power Word: Shield
```

<br>

### Prayer of Mending

Friendly mouseover → friendly target → self, without changing your selected target.

```text
#showtooltip Prayer of Mending
/cast [@mouseover,help,nodead][help,nodead][@player] Prayer of Mending
```

<br>

### Renew

Friendly mouseover → friendly target → self, without changing your selected target.

```text
#showtooltip Renew
/cast [@mouseover,help,nodead][help,nodead][@player] Renew
```

<br>

### Dispel Magic

Friendly mouseover → friendly focus → friendly target → self. This version is for defensive dispels.

```text
#showtooltip Dispel Magic
/cast [@mouseover,help,nodead][@focus,help,nodead][help,nodead][@player] Dispel Magic
```

<br>

### Power Infusion

Requires the Power Infusion talent.

```text
#showtooltip Power Infusion
/cast [@mouseover,help,nodead][help,nodead][@player] Power Infusion
```

<br>

### Prayer of Healing

Targets the chosen ally's party; it is not automatically a whole-raid heal.

```text
#showtooltip Prayer of Healing
/cast [@mouseover,help,nodead][help,nodead][@player] Prayer of Healing
```

<br>

### Mind Flay

Does not interrupt an existing channel when pressed again.

```text
#showtooltip Mind Flay
/cast [nochanneling] Mind Flay
```

<br>

### Shadowform

Repeated presses preserve the active form or attack instead of toggling it off.

```text
#showtooltip Shadowform
/cast !Shadowform
```

<br>

### Shadow Word: Death

Never target yourself. The backlash can hurt or kill you; it does not guarantee that you will break incoming crowd control.

```text
#showtooltip Shadow Word: Death
/cast [@mouseover,harm,nodead][@focus,harm,nodead][harm,nodead] Shadow Word: Death
```

<br>

### Shadow Word: Pain

Hostile mouseover → hostile focus → current enemy. Your selected target stays unchanged.

```text
#showtooltip Shadow Word: Pain
/cast [@mouseover,harm,nodead][@focus,harm,nodead][harm,nodead] Shadow Word: Pain
```

<br>

### Silence

Hostile mouseover → hostile focus → current enemy. Your selected target stays unchanged.

```text
#showtooltip Silence
/cast [@mouseover,harm,nodead][@focus,harm,nodead][harm,nodead] Silence
```

<br>

### Wand

Repeated presses preserve the active form or attack instead of toggling it off.

```text
#showtooltip Shoot
/cast !Shoot
```

<br>

### Fear Ward

Friendly mouseover → friendly target → self, without changing your selected target.

```text
#showtooltip Fear Ward
/cast [@mouseover,help,nodead][help,nodead][@player] Fear Ward
```

<br>

### Penance

Requires Penance. Friendly mouseover wins; otherwise heals a friendly target or damages an enemy.

```text
#showtooltip Penance
/cast [nochanneling,@mouseover,help,nodead][nochanneling,help,nodead][nochanneling,harm,nodead] Penance
```


---

<br>
<br>

<a id="rogue"></a>

## 🗡️ Rogue

**WoW Forever changes:** Ordinary Stealth requires you to be out of combat. Choose talent cooldowns that match your build.

Focus: stealth-safe attacks, control and poison application. Use normal buttons for Adrenaline Rush and Blade Flurry.

<br>

### Backstab

Starts melee auto-attacks even when the ability cannot fire yet. Choose a valid enemy first.

```text
#showtooltip Backstab
/startattack [nostealth,harm,nodead]
/cast Backstab
```

<br>

### Sinister Strike

Starts melee auto-attacks even when the ability cannot fire yet. Choose a valid enemy first.

```text
#showtooltip Sinister Strike
/startattack [nostealth,harm,nodead]
/cast Sinister Strike
```

<br>

### Blind

Hostile mouseover → hostile focus → current enemy. Your selected target stays unchanged.

```text
#showtooltip Blind
/cast [@mouseover,harm,nodead][@focus,harm,nodead][harm,nodead] Blind
```

<br>

### Stealth / Cheap Shot

Enter stealth out of combat, then press again as needed. Not an in-combat stealth bypass.

```text
#showtooltip Cheap Shot
/dismount
/cast [nocombat,nostealth] !Stealth
/cast [stealth,harm,nodead] Cheap Shot
```

<br>

### Vanish

Use an opener separately once stealth is available; no guaranteed automatic escape or success announcement.

```text
#showtooltip Vanish
/stopattack
/cast Vanish
```

<br>

### Kick

Ignores friendly mouseover and focus units when choosing an enemy.

```text
#showtooltip Kick
/cast [@mouseover,harm,nodead][@focus,harm,nodead][harm,nodead] Kick
```

<br>

### Pick Pocket

Choose a pickpocketable target manually; preserves it rather than clearing it.

```text
#showtooltip Pick Pocket
/cast [stealth,harm,nodead] Pick Pocket
```

<br>

### Poison application — item template

Replace YOUR POISON with the exact item and rank in your bags. Normal press main hand; Shift off hand. Confirm any replacement dialog manually.

```text
#showtooltip
/use YOUR POISON
/use [mod:shift] 17; 16
```

<br>

### Riposte / Sinister Strike — optional alternative

Use this instead of the plain Sinister Strike macro if you have Riposte. Shift chooses Riposte; an unavailable Riposte does not fall through to Sinister Strike.

```text
#showtooltip
/startattack [nostealth,harm,nodead]
/cast [mod:shift] Riposte; Sinister Strike
```

<br>

### Sap

Hostile mouseover → hostile focus → current enemy. Your selected target stays unchanged.

```text
#showtooltip Sap
/cast [@mouseover,harm,nodead][@focus,harm,nodead][harm,nodead] Sap
```

<br>

### Stealth toggle

Normal press enters/preserves Stealth; Shift leaves it.

```text
#showtooltip Stealth
/cancelaura [mod:shift] Stealth
/stopmacro [mod:shift]
/cast [nocombat] !Stealth
```

<br>

### Mouseover Gouge

Control a hostile mouseover, otherwise your current enemy. Facing and range requirements still apply.

```text
#showtooltip Gouge
/cast [@mouseover,harm,nodead][harm,nodead] Gouge
```


---

<br>
<br>

<a id="shaman"></a>

## ⚡ Shaman

**WoW Forever changes:** Call spells summon configured totem sets, while Totemic Recall removes active totems. See the [Shaman deep dive](https://news.blizzard.com/en-us/article/24302097/world-of-warcraft-forever-class-deep-dives-mage-and-shaman).

Focus: totem-set selection, interrupts and mouseover healing. Put direct damage spells and Totemic Recall on normal buttons; recall removes your active totems.

<br>

### Totem sets

**Normal:** Elements. **Shift:** Spirits. **Ctrl:** Ancestors. Configure each unlocked set in the totem UI first. Let the cast finish. An unlearned set does not fall back to another one.

```text
#showtooltip
/cast [mod:ctrl] Call of the Ancestors; [mod:shift] Call of the Spirits; Call of the Elements
```

<br>

### Chain Heal

Friendly mouseover → friendly target → self, without changing your selected target.

```text
#showtooltip Chain Heal
/cast [@mouseover,help,nodead][help,nodead][@player] Chain Heal
```

<br>

### Healing Wave

Friendly mouseover → friendly target → self, without changing your selected target.

```text
#showtooltip Healing Wave
/cast [@mouseover,help,nodead][help,nodead][@player] Healing Wave
```

<br>

### Lesser Healing Wave

Friendly mouseover → friendly target → self, without changing your selected target.

```text
#showtooltip Lesser Healing Wave
/cast [@mouseover,help,nodead][help,nodead][@player] Lesser Healing Wave
```

<br>

### Earth Shock interrupt

Check your learned Earth Shock rank's tooltip for its interrupt effect and shared shock cooldown.

```text
#showtooltip Earth Shock
/stopcasting
/cast [@mouseover,harm,nodead][@focus,harm,nodead][harm,nodead] Earth Shock
```

<br>

### Nature's Swiftness / Healing Wave

Requires the talent; another press may be needed if the current GCD prevents the heal.

```text
#showtooltip Healing Wave
/stopcasting
/cast Nature's Swiftness
/cast [@mouseover,help,nodead][help,nodead][@player] Healing Wave
```

<br>

### Purge

Dispels a hostile mouseover, otherwise your current enemy.

```text
#showtooltip Purge
/cast [@mouseover,harm,nodead][harm,nodead] Purge
```

<br>

### Riptide

Requires the Restoration talent. Riptide provides healing over time, without an initial heal, under the [November 2 changes](https://us.forums.blizzard.com/en/wow/t/2360696).

```text
#showtooltip Riptide
/cast [@mouseover,help,nodead][help,nodead][@player] Riptide
```



---

<br>
<br>

<a id="warlock"></a>

## 🔥 Warlock

**WoW Forever changes:** Banes and utility Curses are separate choices; use the learned Bane names. See [Warlock changes](https://news.blizzard.com/en-gb/article/24310968/world-of-warcraft-forever-class-deep-dives-rogue-and-warlock).

Focus: crowd control, pet utility and applying damage without changing targets. Use normal buttons for Rain of Fire and Voidwalker Sacrifice; use [shared pet positioning](#pet-positioning) to park your demon or recall it.

<br>

### Banish

Hostile mouseover → hostile focus → current enemy. Your selected target stays unchanged.

```text
#showtooltip Banish
/cast [@mouseover,harm,nodead][@focus,harm,nodead][harm,nodead] Banish
```

<br>

### Corruption

Hostile mouseover → hostile focus → current enemy. Your selected target stays unchanged.

```text
#showtooltip Corruption
/cast [@mouseover,harm,nodead][@focus,harm,nodead][harm,nodead] Corruption
```

<br>

### Fear

Hostile mouseover → hostile focus → current enemy. Your selected target stays unchanged.

```text
#showtooltip Fear
/cast [@mouseover,harm,nodead][@focus,harm,nodead][harm,nodead] Fear
```

<br>

### Drain Life

Does not interrupt an existing channel when pressed again.

```text
#showtooltip Drain Life
/cast [nochanneling] Drain Life
```

<br>

### Fel Domination / summon

Requires the talent and summoning resources. Replace Voidwalker with a learned demon if desired; another press may be necessary.

```text
#showtooltip Summon Voidwalker
/cast Fel Domination
/cast Summon Voidwalker
```

<br>

### Spell Lock

Requires an active Felhunter with Spell Lock learned. Disable its autocast in the pet UI if saving it for manual interrupts.

```text
#showtooltip Spell Lock
/cast [@mouseover,harm,nodead][@focus,harm,nodead][harm,nodead] Spell Lock
```

<br>

### Shadow Bolt

Cast at a hostile mouseover without changing targets; otherwise use your current enemy.

```text
#showtooltip Shadow Bolt
/cast [@mouseover,harm,nodead][harm,nodead] Shadow Bolt
```

<br>

### Soulstone — item template

Create the stone separately; replace the placeholder with the actual usable Soulstone item name.

```text
#showtooltip
/use [@mouseover,help,nodead][help,nodead][@player] YOUR SOULSTONE ITEM
```

<br>

### Seduction

Requires a Succubus with Seduction. Do not order it to attack the target you intend to control.

```text
#showtooltip Seduction
/petpassive
/petfollow
/cast [@mouseover,harm,nodead][@focus,harm,nodead][harm,nodead] Seduction
```

<br>

### Bane of Agony

Apply to a hostile mouseover, otherwise your current enemy.

```text
#showtooltip Bane of Agony
/cast [@mouseover,harm,nodead][harm,nodead] Bane of Agony
```

<br>

### Curse of the Elements

Bane and utility Curse are separate choices; do not automatically alternate them with a castsequence.

```text
#showtooltip Curse of the Elements
/cast [@mouseover,harm,nodead][harm,nodead] Curse of the Elements
```


---

<br>
<br>

<a id="warrior"></a>

## ⚔️ Warrior

**WoW Forever changes:** Stance changes, weapon swaps, Rage, range and GCDs can require additional presses. Use your own equipment names.

Focus: stance preparation, starting melee attacks and tank utility. Use normal buttons for shouts, Bloodrage, Thunder Clap and Victory Rush. Stance changes may require another press and can affect your Rage.

<br>

### Bloodthirst

Starts melee auto-attacks even when the ability cannot fire yet. Choose a valid enemy first.

```text
#showtooltip Bloodthirst
/startattack [harm,nodead]
/cast Bloodthirst
```

<br>

### Cleave

Starts melee auto-attacks even when the ability cannot fire yet. Choose a valid enemy first.

```text
#showtooltip Cleave
/startattack [harm,nodead]
/cast Cleave
```

<br>

### Heroic Strike

Starts melee auto-attacks even when the ability cannot fire yet. Choose a valid enemy first.

```text
#showtooltip Heroic Strike
/startattack [harm,nodead]
/cast Heroic Strike
```

<br>

### Mortal Strike

Starts melee auto-attacks even when the ability cannot fire yet. Choose a valid enemy first.

```text
#showtooltip Mortal Strike
/startattack [harm,nodead]
/cast Mortal Strike
```

<br>

### Sunder Armor

Starts melee auto-attacks even when the ability cannot fire yet. Choose a valid enemy first.

```text
#showtooltip Sunder Armor
/startattack [harm,nodead]
/cast Sunder Armor
```

<br>

### Battle / Defensive Stance toggle

Switches from Battle to Defensive Stance; from any other stance it selects Battle Stance. A stance change can affect Rage and does not cast a follow-up attack.

```text
#showtooltip
/cast [stance:1] Defensive Stance; Battle Stance
```

<br>

### Charge

Requires the usual combat, range and activation conditions.

```text
#showtooltip Charge
/cast [nostance:1] Battle Stance
/cast Charge
```

<br>

### Overpower

Prepares the required stance, then attempts the ability. Press again after the stance change if needed.

```text
#showtooltip Overpower
/cast [nostance:1] Battle Stance
/cast Overpower
```

<br>

### Disarm

Prepares the required stance, then attempts the ability. Press again after the stance change if needed.

```text
#showtooltip Disarm
/cast [nostance:2] Defensive Stance
/cast Disarm
```

<br>

### Intercept

Prepares the required stance, then attempts the ability. Press again after the stance change if needed.

```text
#showtooltip Intercept
/cast [nostance:3] Berserker Stance
/cast Intercept
```

<br>

### Berserker Rage

Prepares the required stance, then attempts the ability. Press again after the stance change if needed.

```text
#showtooltip Berserker Rage
/cast [nostance:3] Berserker Stance
/cast Berserker Rage
```

<br>

### Mocking Blow

Prepares the required stance, then attempts the ability. Press again after the stance change if needed.

```text
#showtooltip Mocking Blow
/cast [nostance:1] Battle Stance
/cast Mocking Blow
```

<br>

### Revenge

Prepares the required stance, then attempts the ability. Press again after the stance change if needed.

```text
#showtooltip Revenge
/cast [nostance:2] Defensive Stance
/cast Revenge
```

<br>

### Whirlwind

Prepares the required stance, then attempts the ability. Press again after the stance change if needed.

```text
#showtooltip Whirlwind
/cast [nostance:3] Berserker Stance
/cast Whirlwind
```

<br>

### Sweeping Strikes

Prepares the required stance, then attempts the ability. Press again after the stance change if needed.

```text
#showtooltip Sweeping Strikes
/cast [nostance:1] Battle Stance
/cast Sweeping Strikes
```

<br>

### Retaliation

Prepares the required stance, then attempts the ability. Press again after the stance change if needed.

```text
#showtooltip Retaliation
/cast [nostance:1] Battle Stance
/cast Retaliation
```

<br>

### Hamstring

Leave Defensive Stance for Berserker Stance when needed, then slow your current enemy.

```text
#showtooltip Hamstring
/cast [stance:2] Berserker Stance
/cast [harm,nodead] Hamstring
```

<br>

### Execute

Leave Defensive Stance for Berserker Stance when needed. Execute still requires its normal activation conditions.

```text
#showtooltip Execute
/cast [stance:2] Berserker Stance
/cast [harm,nodead] Execute
```

<br>

### Rend

Switch from Berserker to Battle Stance when needed, then apply Rend.

```text
#showtooltip Rend
/cast [stance:3] Battle Stance
/cast [harm,nodead] Rend
```

<br>

### Pummel / stance setup

A stance change can delay the interrupt. Prepare the stance ahead of time when possible.

```text
#showtooltip Pummel
/cast [nostance:3] Berserker Stance
/cast [stance:3,@focus,harm,nodead][stance:3,harm,nodead] Pummel
```

<br>

### Sword and shield setup — template

Replace both placeholders. Equip the one-handed weapon first. Then use the shield ability after setup is complete.

```text
/equipslot 16 YOUR ONE-HANDED WEAPON
/equipslot 17 YOUR SHIELD
/cast Defensive Stance
```

<br>

### Two-handed weapon — template

Replace the placeholder with an item you own and can equip.

```text
/equipslot 16 YOUR TWO-HANDED WEAPON
```

<br>

### Shield Block / Shield Wall

**Normal:** Shield Block. **Shift:** Shield Wall. Requires Defensive Stance and an equipped shield. Keep Shield Wall on its own key instead if you prefer a dedicated emergency button.

```text
#showtooltip
/cast [stance:2,mod:shift] Shield Wall; [stance:2] Shield Block
```

<br>

### Shield Bash

Requires a shield. Prepare it before an interrupt is needed. Pummel is the alternative for a Berserker Stance setup.

```text
#showtooltip Shield Bash
/cast [stance:1/2,@mouseover,harm,nodead][stance:1/2,harm,nodead] Shield Bash
```

<br>

### Intimidating Shout

Select the intended target first; the macro does not acquire a nearby enemy automatically.

```text
#showtooltip Intimidating Shout
/stopattack
/cast [harm,nodead] Intimidating Shout
```

<br>

### Mouseover Taunt

In Defensive Stance, taunt a hostile mouseover or your current enemy.

```text
#showtooltip Taunt
/cast [stance:2,@mouseover,harm,nodead][stance:2,harm,nodead] Taunt
```


---

<br>
<br>

<a id="universal"></a>

## 🧰 Universal

Templates using a class spell work only for that class. Replace item/player placeholders before use. Trinket swaps are out-of-combat preparation, not automatic PvE/PvP detection.

<br>

### Ranged weapon: automatic attack selection

No need to replace your ranged ability when you change weapon types: this button selects the matching attack. Choose the question-mark macro icon so `#showtooltip` updates the icon and tooltip with your equipped weapon type. Great for **⚔️ Warriors** and **🗡️ Rogues**!

Requires the corresponding weapon skill, an equipped ranged weapon and ammunition where applicable. Spell names and equipment-type names must match your client language.

```text
#showtooltip
/cast [worn:Bows] Shoot Bow; [worn:Crossbows] Shoot Crossbow; [worn:Guns] Shoot Gun; [worn:Thrown] Throw
```

<br>

### Set / clear focus

Normal press sets mouseover or target; Shift only clears.

```text
/clearfocus [mod:shift]
/stopmacro [mod:shift]
/focus [@mouseover,exists][exists]
```

<br>

### Assist focus

Set your tank or caller as focus first. This changes your selected target.

```text
/assist [@focus,exists]
```

<br>

<a id="pet-positioning"></a>

### Pet positioning / recall — all pet classes

For Hunters, Warlocks and other characters with a controllable pet. **Normal:** put the pet on Passive and open a ground reticle; click where it should move. **Shift:** recall it instead. Useful for moving a pet away from patrols or hazards without sending it to attack. Temporary guardians without pet commands cannot use this.

```text
/stopmacro [nopet][@pet,dead]
/petpassive
/petfollow [mod:shift]
/stopmacro [mod:shift]
/petmoveto
```

The pet remains Passive; give an explicit attack command or restore your preferred pet behavior afterward. Click the destination yourself. See [Gladius Gaming's Forever pet-control examples](https://mobalytics.gg/wow-forever/classes/forever-solo-hunter-gladius-gaming).

<br>

### Emergency pet recall

For pet classes; does not cancel existing damage-over-time effects.

```text
/petpassive
/petfollow
```

<br>

### Deliberate trinket choice

Normal press uses slot 13; Shift uses slot 14. Shared cooldowns still apply.

```text
#showtooltip
/use [mod:shift] 14; 13
```

<br>

### Trinket equipment — template

Replace both placeholders with trinkets you own. Equip them out of combat.

```text
/stopmacro [combat]
/equipslot 13 YOUR FIRST TRINKET
/equipslot 14 YOUR SECOND TRINKET
```

<br>

### Stop casting / attacking

A deliberate stop button; pets require the recall macro above.

```text
/stopcasting
/stopattack
```

<br>

### Food / drink — item templates

**Normal:** eat. **Shift:** drink. Replace both names with supplies in your bags. This chooses one item per press; use each separately if you need both.

```text
#showtooltip
/stopmacro [combat]
/use [mod:shift] YOUR DRINK; YOUR FOOD
```

<br>

### Mouseover bandage / Shift self — item template

Replace YOUR BANDAGE with one you can use. **Shift** forces self; otherwise mouseover → friendly target → self. Damage interrupts bandaging.

```text
#showtooltip
/use [mod:shift,@player][@mouseover,help,nodead][help,nodead][@player] YOUR BANDAGE
```

<br>

### Ready check

For group leaders or assistants preparing a pull. This asks the group whether they are ready; it does not start combat.

```text
/readycheck
```

<br>

### Follow focus

Set a friendly player as focus first. Following does not avoid terrain or hazards and is not unattended travel.

```text
/follow [@focus,help,nodead]
```

<br>

### Cancel Blessing of Protection

Cancels the protection on yourself so you can resume restricted attacks. Use deliberately: you immediately lose its protection.

```text
/cancelaura Blessing of Protection
```

<br>

---

<br>

<a id="professions-and-camping"></a>

## ⛺ Professions and camping

Use macros for equipment changes, lure application and deliberate tracking choices. Open crafting professions from their normal buttons, and drag usable camp-feature items directly onto your bar. Replace uppercase placeholders with your actual item names and recheck the 255-character limit after substitution.

These fishing templates use a pole in main-hand slot 16. Test equipping, restoring your off-hand and applying a lure before relying on them. They do not automate catches, gathering, crafting or resting.

<br>

### Fishing: equip pole / restore combat gear

**Normal:** equip your fishing pole and cast Fishing. **Shift:** stop fishing and restore a saved equipment set named `Combat`. Create that set with your normal weapons and off-hand before using the macro; only include other slots if you want those restored too.

Replace `YOUR FISHING POLE` with its exact bag name. Repeated normal presses preserve an active Fishing channel. If equipping delays the cast, press again. Click the bobber yourself; Shift restores gear only when out of combat.

```text
#showtooltip Fishing
/stopmacro [combat]
/stopcasting [mod:shift]
/equipset [mod:shift] Combat
/stopmacro [mod:shift][channeling]
/equipslot 16 YOUR FISHING POLE
/cast Fishing
```

<br>

### Herb / mineral tracking

**Normal:** Find Herbs. **Shift:** Find Minerals. Useful if you have both gathering professions; otherwise use your profession's normal tracking button. This selects tracking explicitly, without relying on cast-sequence state.

```text
#showtooltip
/cast [mod:shift] Find Minerals; Find Herbs
```

<br>

### Fishing lure: equip pole and apply

Replace both placeholders with your pole and lure item names. Use between catches: finish any cast first; the macro stops while you are channeling, equips the pole, then applies the lure to main-hand slot **16**. If the equipment change has not completed, press again.

`Fishing Pole` is the English equipment type, not the name of your specific pole. Adjust it for your client language. Let the lure application finish, confirm any replacement prompt yourself, then use the Fishing macro. It does not detect whether a lure is already active.

```text
#showtooltip
/stopmacro [combat][channeling]
/equipslot 16 YOUR FISHING POLE
/stopmacro [noequipped:Fishing Pole]
/use YOUR FISHING LURE
/use 16
```

<br>

### Camping basics

A Basic Campfire supports three camp features, with one contribution per player. Sit or craft nearby for at least one minute to receive feature benefits. Features share a one-hour cooldown and can overlap with class buffs, so coordinate before placing one. [Blizzard camping guide](https://news.blizzard.com/en-us/article/24301516/gather-around-the-campfire-in-world-of-warcraft-forever).

<br>

### Campfire / rest

**Normal:** build your learned Basic Campfire. After placement finishes, **Shift:** sit beside it. Replace the fire name if you use a learned upgrade. This does not wait a minute or collect buffs automatically; finish any targeting step manually.

```text
#showtooltip Basic Campfire
/stopmacro [combat]
/cast [nomod:shift] Basic Campfire
/stopmacro [nomod:shift]
/sit
```

<br>

**Camp items:** Drag the item onto your bar and use it beside an existing campfire; meet its profession requirements and complete placement manually. Useful choices include a **Reagent Bot** for reagent purchases, a **Camp Tent** for rested experience and a **Mana Well** for mana regeneration; check buff overlap and the item tooltip. [Blizzard camping guide](https://news.blizzard.com/en-us/article/24301516/gather-around-the-campfire-in-world-of-warcraft-forever).

**Macro reference:** [Forever player fishing example on Blizzard forums](https://us.forums.blizzard.com/en/wow/t/wow-forever-controller-alpha-feedback-leveling-dungeons-suggestions-macros/2388507) uses equipment sets and the `Fishing Pole` equipment condition. This is a player report, not official macro certification; the templates above still need in-game testing.

**Profession update:** Tanning Rack now requires Leatherworking 125, according to the [November 6 hotfixes](https://news.blizzard.com/en-us/article/24315281/wow-forever-hotfixes-november-6-2026).




---

<br>
<br>
<br>

<a id="sources"></a>

## 📚 Sources

These sources document game behavior; they do not certify every macro here.

<br>

### Live updates and profession guidance

- [Forever hotfixes — latest dated entry November 6](https://news.blizzard.com/en-us/article/24315281/wow-forever-hotfixes-november-6-2026): includes the Druid stealth-opener fix, Arcane Blast duration correction, Mage Study/Research correction and Tanning Rack requirement change.
- [Camping and profession camp features — October 21](https://news.blizzard.com/en-us/article/24301516/gather-around-the-campfire-in-world-of-warcraft-forever): camp setup, resting, feature limits and buff overlap.

<br>


### Additional macro reading

- [Epiccarry Forever Hunter macros](https://epiccarry.com/blogs/wow-forever-best-hunter-macros/) — reviewed for pet positioning, mouseover attacks, trap control and aspect shortcuts. Adapted selectively; existing shot, feeding and revival macros already cover much of the guide.
- [Gladius Gaming's Forever Hunter guide](https://mobalytics.gg/wow-forever/classes/forever-solo-hunter-gladius-gaming) — player-authored examples of `/petmoveto` and pet-safe control.
- [Mmonster Forever macro guide](https://mmonster.co/wow-forever/guides/macros) — compared for missing shortcuts and condition-table organization. Third-party guidance, not Blizzard certification. Existing healing, control and pet macros cover most examples; modifiers avoid redundant buff sequences and automatic trinket use.

<br>

### Class design references

- [Hunter and Druid](https://news.blizzard.com/en-us/article/24301515/world-of-warcraft-forever-class-deep-dives-hunter-and-druid)
- [Priest and Warrior](https://news.blizzard.com/en-us/article/24301514/world-of-warcraft-forever-class-deep-dives-priest-and-warrior)
- [Mage and Shaman](https://news.blizzard.com/en-us/article/24302097/world-of-warcraft-forever-class-deep-dives-mage-and-shaman)
- [Paladin](https://news.blizzard.com/en-us/article/24302099/world-of-warcraft-forever-class-deep-dives-paladin)
- [Rogue and Warlock](https://news.blizzard.com/en-gb/article/24310968/world-of-warcraft-forever-class-deep-dives-rogue-and-warlock)
- [Official development notes, updated November 2](https://us.forums.blizzard.com/en/wow/t/2360696)
- [Macro syntax reference by its author on Blizzard forums](https://us.forums.blizzard.com/en/wow/t/macros-essential-information/21139) — community-authored, general WoW guidance, not a Forever-specific certification.

---

<br>
<br><br>
<br>

<a id="buy-me-a-coffee"></a>

## ☕ Buy me a coffee

If you like my content and would like to show your appreciation, feel free to [buy me a coffee](https://buymeacoffee.com/eightyfive).
