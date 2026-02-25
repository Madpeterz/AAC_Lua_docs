# AAC API Reference

## Table of Contents
- [Globals](#globals)
- [Logging](#logging)
- [File Handling](#file-handling)
- [Interface](#interface)
- [Unit](#unit)
- [Team](#team)
- [Bag](#bag)
- [Cursor](#cursor)
- [Time](#time)
- [Quest](#quest)
- [Input](#input)
- [Player](#player)
- [Events & Timers](#events--timers)
- [Ability](#ability)
- [Item](#item)
- [Skill](#skill)
- [Store](#store)
- [Bank](#bank)
- [Equipment](#equipment)
- [Siege Weapon](#siege-weapon)
- [Item Enchant](#item-enchant)
- [Map](#map)
- [Zone](#zone)

## Globals

These are all provided in the environment you'll be using.

### ADDON

- `GetContent(UIC)` - Use this to get any of the windows defined in UIC
- `RegisterContentWidget(UIC, widget)` - Register a content widget
- `ShowContent(UIC, show)` - Show or hide content
- `RegisterContentTriggerFunc(UIC, ToggleFunction)` - Calls ToggleFunction with true/false when ShowContent is called

### UIC

Window identifiers:

```lua
UIC = {
    DEV_WINDOW,
    ABILITY_CHANGE,
    CHARACTER_INFO,
    AUTH_MSG_WND,
    BUBBLE_ACTION_BAR,
    BAG,
    DEATH_AND_RESURRECTION_WND,
    OPTION_FRAME,
    SYSTEM_CONFIG_FRAME,
    GAME_EXIT_FRAME,
    SLAVE_EQUIPMENT,
    PLAYER_UNITFRAME,
    TARGET_UNITFRAME,
    RAID_COMMAND_MESSAGE,
    COMBAT_TEXT_FRAME,
    TARGET_OF_TARGET_FRAME,
    WATCH_TARGET_FRAME,
    RAID_MANAGER,
    COMMUNITY_WINDOW,
    ENCHANT_WINDOW
}
```

### ALIGN

Alignment constants:

```lua
ALIGN = {
    LEFT,
    RIGHT,
    CENTER,
    TOP,
    BOTTOM,
    BOTTOM_RIGHT,
    BOTTOM_LEFT,
    TOP_LEFT,
    TOP_RIGHT
}
```

### SLOT_STYLE

Available slot styles including:
- `DEFAULT`
- `BAG_DEFAULT`
- `SOCKET`
- `BAG_NOT_ALL_TAB`
- `BUFF`
- `EQUIP_ITEM`
- `PET_ITEM`
- `SLAVE_EQUIP` (contains ship equipment slots)

### Character Equipment Slots

```lua
EQUIP_SLOT = {
    HEAD,
    NECK,
    CHEST,
    WAIST,
    LEGS,
    HANDS,
    FEET,
    ARMS,
    BACK,
    EAR_1,
    EAR_2,
    FINGER_1,
    FINGER_2,
    UNDERSHIRT,
    UNDERPANTS,
    MAINHAND,
    OFFHAND,
    RANGED,
    MUSICAL,
    BACKPACK,
    COSPLAY
}
```

### Global Functions

- `ConvertColor(color)` - Takes a single color as a value from 1 to 255, returns as a percentage decimal
- `ApplyTextColor(widget, color)` - Applies a color to the widget. Use FONT_COLOR values
- `ApplyButtonSkin(widget, skin)` - Applies a skin to a button. Skins are available in BUTTON_BASIC but can also be customized
- `CreateItemIconButton(id, parent)` - Creates an item icon button
- `ParseCombatMessage(combatEvent, ...)` - Parses combat messages
- `GetTextureInfo(path, key)` - Gets texture information

### X2Util

Utility library in the game's client:

- `GetMyMoneyString()` - Get player's money as a string
- `ConvertWorldToScreen(x, y, z)` - Takes x, y, z and returns screenX, screenY and distance. Negative distance means the coordinate is off screen

### X2Chat

Chat library functions:

- `ExpressEmotion(emotion)` - Express an emotion
- `DispatchChatMessage(channel, message)` - Dispatch a chat message

### Helper Libraries

- **W_MONEY** - Money window helpers
  - `CreateMoneyEditsWindow(id, parent, titleText)`
  - `CreateTwoLineMoneyWindow(id, parent)`
  - `CreateDefaultMoneyWindow(id, parent, width, height)`
  - `CreateTitleMoneyWindow(id, parent, titleText, direction, layoutType)`

- **W_CTRL** - Control widget helpers
  - `CreateComboBox(parent)`
  - `CreateEdit(id, parent)`
  - `CreditMultiLineEdit(id, parent)`
  - `CreateLabel(id, parent)`
  - `CreateList(id, parent)`
  - `CreateListCtrl(id, parent)`
  - `CreatePageControl(id, parent, style)`
  - `CreatePageScrollListCtrl(id, parent)`
  - `CreateScroll(id, parent)`
  - `CreateScrollListBox(id, parent, bgType)`
  - `CreateScrollListCtrl(id, parent, index)`

- **W_UNIT** - Unit widget helpers
  - `CreateBuffWindow(id, parent, maxCount, isBuff, isRaidMember)`
  - `CreateLevelLabel(id, parent, useLevelTexture)`

- **W_BAR** - Bar widget helpers
  - `CreateCastingBar(id, parent, unit)`
  - `CreateDoubleGauge(id, parent)`
  - `CreateExpBar(id, parent)`
  - `CreateLaborPowerBar(id, parent)`
  - `CreateHpBar(id, parent)`
  - `CreateSkillBar(id, parent)`
  - `CreateStatusBarOfUnitFrame(id, parent, barType)`
  - `CreateStatusBarOfRaidFrame(id, parent, barType)`
  - `CreateStatusBar(id, parent, style)`

- **W_ICON** - Icon widget helpers (see original file for full list)

### Global Tables

- `chatTabWindow` - Contains the chat windows provided in the game. Window 1 is the default
- `petFrame` - Contains all pet unit frames such as combat pets and mounts
- `combatTextLocale` - Contains the combat text localization strings and font choice

---

## Logging

### ADDON_API.Log:Info(message)

Logs a message to chat in YELLOW.

**Parameters:**
- `message` (string) - The message to log

**Returns:** nil

---

### ADDON_API.Log:Err(message)

Logs a message to chat in RED.

**Parameters:**
- `message` (string) - The message to log

**Returns:** nil

---

## File Handling

### ADDON_API.File:Write(path, tbl)

Serializes a table and writes it to the desired path. Paths are relative to the addons folder.

**Parameters:**
- `path` (string) - The file path
- `tbl` (table) - The table to serialize

**Returns:** nil

---

### ADDON_API.File:Read(path)

Reads the file located at path and deserializes it into a table. Paths are relative to the addons folder.

**Parameters:**
- `path` (string) - The file path

**Returns:** (table) The deserialized table

---

### ADDON_API.GetSettings(addonId)

Get the settings for the desired addonId.

**Parameters:**
- `addonId` (string) - The name of the addon folder. For example, `example_addon`

**Returns:** (table) The settings table

---

### ADDON_API.SaveSettings()

Saves all addon settings. Do not call too often.

**Returns:** nil

---

## Interface

### ADDON_API.Interface:CreateWindow(id, title, x, y, tabs)

Creates a standard looking window. If size is not specified, it will default to 680x615. Tabs can be used to define tabs on the window by default.

**Parameters:**
- `id` (string) - The window ID
- `title` (string) - The window title
- `x` (number) - X position
- `y` (number) - Y position
- `tabs` (table) - Tabs configuration

**Returns:** (table) The created window

---

### ADDON_API.Interface:CreateEmptyWindow(id)

Creates an empty window that does not render anything. Particularly useful to draw things on the HUD.

**Parameters:**
- `id` (string) - The window ID

**Returns:** (table) The created window

---

### ADDON_API.Interface:CreateWidget(type, id, parent)

Creates a Widget bound to parent.

**Types:** button, label, window, slot, checkbutton, window, textbox, message, gametooltip

**Parameters:**
- `type` (string) - The widget type
- `id` (string) - The widget ID
- `parent` (table) - The parent widget

**Returns:** (table) The created widget

---

### ADDON_API.Interface:CreateStatusBar(name, parent, type)

Creates a status bar.

**Parameters:**
- `name` (string) - The name of the status bar
- `parent` (table) - The parent widget
- `type` (string) - The type of the status bar

**Returns:** (table) The created status bar

---

### ADDON_API.Interface:CreateComboBox(window)

Creates a Combo Box (like the grade selector in the equipment folio). Use the dropdownItem member to set the contents.

**Parameters:**
- `window` (table) - The parent window

**Returns:** (table) The created combo box

---

### ADDON_API.Interface:ApplyButtonSkin(btn, skin)

Applies a skin to a button. Possible values are BUTTON_BASIC.{DEFAULT, RESET, BASIC} and more.

**Parameters:**
- `btn` (table) - The button widget
- `skin` (string) - The skin to apply

**Returns:** nil

---

### ADDON_API.Interface:SetTooltipOnPos(text, target, posX, posY)

Sets tooltip at coordinates.

**Parameters:**
- `text` (string) - The tooltip text
- `target` (table) - The target widget
- `posX` (number) - X position
- `posY` (number) - Y position

**Returns:** nil

---

### ADDON_API.Interface:Free(wnd)

Frees the specified window's reference, removing it from the UI. Useful for cleaning up windows in the OnUnload function of addons.

**Parameters:**
- `wnd` (table) - The window to free

**Returns:** nil

---

### ADDON_API.Interface:GetScreenWidth()

Gets the screen width.

**Returns:** (number) The screen width

---

### ADDON_API.Interface:GetScreenHeight()

Gets the screen height.

**Returns:** (number) The screen height

---

### ADDON_API.Interface:GetUIScale()

Gets the UI scale.

**Returns:** (number) The UI scale

---

## Unit

> **Note:** The "unit" parameter or identifier in the below functions can be:
> - `player`
> - `target`
> - `targettarget`
> - `team1` through `team50`
> - `playerpet1` or `playerpet2`

### ADDON_API.Unit:GetUnitNameById(id)

Returns a unit's (player, npc, pet..) name by its ID.

**Parameters:**
- `id` (number) - The unit ID

**Returns:** (string) The unit name

---

### ADDON_API.Unit:GetUnitInfoById(id)

Returns a unit's information by its ID.

**Parameters:**
- `id` (number) - The unit ID

**Returns:** (table) The unit information

---

### ADDON_API.Unit:GetUnitScreenPosition(unit)

Get the screen coordinates of the desired unit. This can be used to overlay UI elements and "track" units.

**Parameters:**
- `unit` (string) - The unit identifier

**Returns:** (number, number) The screen X and Y coordinates

---

### ADDON_API.Unit:UnitDistance(unit)

Returns the distance between the desired unit and the player, in meters.

**Parameters:**
- `unit` (string) - The unit identifier

**Returns:** (number) The distance in meters

---

### ADDON_API.Unit:GetUnitId(unit)

Returns the unit ID for the desired unit. The ID can then be passed to GetUnitNameById and GetUnitInfoById.

**Parameters:**
- `unit` (string) - The unit identifier

**Returns:** (number) The unit ID

---

### ADDON_API.Unit:UnitBuffCount(unit)

Returns the number of buffs a unit has.

**Parameters:**
- `unit` (string) - The unit identifier

**Returns:** (number) The number of buffs

---

### ADDON_API.Unit:UnitBuff(unit, index)

Returns the buff at index for a unit. Nil if no buff is found.

**Parameters:**
- `unit` (string) - The unit identifier
- `index` (number) - The buff index

**Returns:** (table) The buff information

---

### ADDON_API.Unit:UnitWorldPosition(unit)

Returns the unit world coordinates of the desired unit as x, y, z.

**Parameters:**
- `unit` (string) - The unit identifier

**Returns:** (number, number, number) The world X, Y, Z coordinates

---

### ADDON_API.Unit:UnitDeBuffCount(unit)

Returns the number of debuffs a unit has.

**Parameters:**
- `unit` (string) - The unit identifier

**Returns:** (number) The number of debuffs

---

### ADDON_API.Unit:UnitDeBuff(unit, index)

Returns the debuff at index for a unit. Nil if no debuff is found.

**Parameters:**
- `unit` (string) - The unit identifier
- `index` (number) - The debuff index

**Returns:** (table) The debuff information

---

### ADDON_API.Unit:UnitHealth(unit)

Gets the health of a unit.

**Parameters:**
- `unit` (string) - The unit identifier

**Returns:** (number) The current health

---

### ADDON_API.Unit:UnitMaxHealth(unit)

Gets the maximum health of a unit.

**Parameters:**
- `unit` (string) - The unit identifier

**Returns:** (number) The maximum health

---

### ADDON_API.Unit:UnitMana(unit)

Gets the mana of a unit.

**Parameters:**
- `unit` (string) - The unit identifier

**Returns:** (number) The current mana

---

### ADDON_API.Unit:UnitMaxMana(unit)

Gets the maximum mana of a unit.

**Parameters:**
- `unit` (string) - The unit identifier

**Returns:** (number) The maximum mana

---

### ADDON_API.Unit:UnitInfo(unit)

Gets information about a unit.

**Parameters:**
- `unit` (string) - The unit identifier

**Returns:** (table) The unit information

---

### ADDON_API.Unit:UnitModifierInfo(unit)

Gets modifier information about a unit.

**Parameters:**
- `unit` (string) - The unit identifier

**Returns:** (table) The modifier information

---

### ADDON_API.Unit:UnitClass(unit)

Gets the class of a unit.

**Parameters:**
- `unit` (string) - The unit identifier

**Returns:** (string) The unit class

---

### ADDON_API.Unit:UnitGearScore(unit)

Gets the gear score of a unit.

**Parameters:**
- `unit` (string) - The unit identifier

**Returns:** (number) The gear score

---

### ADDON_API.Unit:UnitTeamAuthority(unit)

Gets the team authority of a unit.

**Parameters:**
- `unit` (string) - The unit identifier

**Returns:** (number) The team authority

---

### ADDON_API.Unit:UnitIsTeamMember(unit)

Checks if a unit is a team member.

**Parameters:**
- `unit` (string) - The unit identifier

**Returns:** (boolean) True if the unit is a team member, false otherwise

---

### ADDON_API.Unit:UnitIsForceAttack(unit)

Checks if a unit is currently flagged.

**Parameters:**
- `unit` (string) - The unit identifier

**Returns:** (boolean) True if the unit is flagged for forced PvP, false otherwise

---

### ADDON_API.Unit:GetFactionName(unit)

Gets the faction name of a unit.

**Parameters:**
- `unit` (string) - The unit identifier

**Returns:** (string) The faction name

---

### ADDON_API.Unit:GetUnitScreenNameTagOffset(unit)

Gets the screen name tag offset of a unit.

**Parameters:**
- `unit` (string) - The unit identifier

**Returns:** (number) The offset

---

### ADDON_API.Unit:GetHighAbilityRscInfo()

Gets high ability (Abyssal Charges) resource information.

**Returns:** (table) The resource information

---

### ADDON_API.Unit:UnitIsOffline(unit)

Checks if a unit is offline.

**Parameters:**
- `unit` (string) - The unit identifier

**Returns:** (boolean) True if the unit is offline, false otherwise

---

### ADDON_API.Unit:GetOverHeadMarkerUnitId(markerIndex)

Gets the overhead marker unit ID.

**Parameters:**
- `markerIndex` (number) - The marker index, any number between 1 and 12

**Returns:** (number) The unit ID

---

## Team

### ADDON_API.Team:InviteToTeam(name, party)

Invites the desired player to raid/party. If party is true and the player is not in party/raid, a party will be created. Otherwise, a raid.

**Parameters:**
- `name` (string) - The player name
- `party` (boolean) - True for party, false for raid

**Returns:** nil

---

### ADDON_API.Team:SetRole(role)

Sets the player's role within the party. 0 = Blue

**Parameters:**
- `role` (number) - The role ID

**Returns:** nil

---

### ADDON_API.Team:IsPartyTeam()

Checks if the team is a party.

**Returns:** (boolean) True if the team is a party, false otherwise

---

### ADDON_API.Team:IsPartyRaid()

Checks if the team is a raid.

**Returns:** (boolean) True if the team is a raid, false otherwise

---

### ADDON_API.Team:GetMemberIndexByName(playerName)

Gets the member index by name.

**Parameters:**
- `playerName` (string) - The player name

**Returns:** (number) The member index

---

### ADDON_API.Team:GetTeamDistributorName()

Gets the team distributor name.

**Returns:** (string) The distributor name

---

### ADDON_API.Team:GetTeamPlayerIndex()

Gets the team player index.

**Returns:** (number) The player index

---

### ADDON_API.Team:GetRole(memberIndex)

Gets the role of a team member.

**Parameters:**
- `memberIndex` (number) - The member index

**Returns:** (number) The role ID

---

## Bag

### ADDON_API.Bag:EquipBagItem(bagSlot, isAux)

Equips the item at the bag slot specified.

**Parameters:**
- `bagSlot` (number) - The bag slot index
- `isAux` (boolean) - True for auxiliary slot, false otherwise

**Returns:** nil

---

### ADDON_API.Bag:GetBagItemInfo(bagType, index)

Get item information at the index, in the bag specified. 1 = Inventory

**Parameters:**
- `bagType` (number) - The bag type
- `index` (number) - The item index

**Returns:** (table) The item information

---

### ADDON_API.Bag:CountItems()

Counts the items in the bag.

**Returns:** (number) The item count

---

### ADDON_API.Bag:Capacity()

Gets the bag capacity.

**Returns:** (number) The bag capacity

---

### ADDON_API.Bag:CountBagItemByItemType(type)

Counts the items in the bag by item type.

**Parameters:**
- `type` (number) - The item type

**Returns:** (number) The item count

---

### ADDON_API.Bag:GetCurrency()

Gets the amount of gold in the player's bag.

**Returns:** (number) The currency amount

---

## Cursor

### ADDON_API.Cursor:ClearCursor()

Clears the cursor.

**Returns:** nil

---

### ADDON_API.Cursor:SetCursorImage(image, x, y)

Sets the cursor image.

**Parameters:**
- `image` (string) - The image path
- `x` (number) - X position
- `y` (number) - Y position

**Returns:** nil

---

### ADDON_API.Cursor:GetCursorPickedBagItemIndex()

Gets the picked bag item index from the cursor.

**Returns:** (number) The bag item index

---

### ADDON_API.Cursor:GetCursorPickedBagItemAmount()

Gets the picked bag item amount from the cursor.

**Returns:** (number) The bag item amount

---

### ADDON_API.Cursor:GetCursorInfo()

Gets the item information that is currently on the cursor.

**Returns:** (table) The cursor information

---

## Time

### ADDON_API.Time:GetUiMsec()

Returns milliseconds since the game's UI scripts have started running.

**Returns:** (number) The milliseconds since the game started

---

### ADDON_API.Time:GetLocalTime()

Gets the local time of the player's computer.

**Returns:** (table) The local time

---

### ADDON_API.Time:PeriodTimeToDate(localTime, period)

The amount of time until a date period.

**Parameters:**
- `localTime` (table) - The local time
- `period` (number) - The period time

**Returns:** (table) The time difference between the local time and the period

---

### ADDON_API.Time:TimeToDate(epochTimestamp)

Converts an epoch timestamp to a date.

**Parameters:**
- `epochTimestamp` (number) - The epoch timestamp

**Returns:** (table) The date

---

### ADDON_API.Time:GetGameTime()

Gets the game time.

**Returns:** (table) The in-game time

---

## Quest

### ADDON_API.Quest:IsCompleted(questTypeId)

Checks if a quest is completed.

**Parameters:**
- `questTypeId` (number) - The quest type ID

**Returns:** (boolean) True if the quest is completed, false otherwise

---

### ADDON_API.Quest:GetActiveQuestTitle(questTypeId)

Gets the active quest title.

**Parameters:**
- `questTypeId` (number) - The quest type ID

**Returns:** (string) The quest title

---

### ADDON_API.Quest:GetQuestContextMainTitle(questTypeId)

Gets the main title of the quest context.

**Parameters:**
- `questTypeId` (number) - The quest type ID

**Returns:** (string) The main title

---

### ADDON_API.Quest:GetQuestContextBody(questTypeId)

Gets the body of the quest context.

**Parameters:**
- `questTypeId` (number) - The quest type ID

**Returns:** (string) The body text

---

## Input

### ADDON_API.Input:IsShiftKeyDown()

Checks if the Shift key is pressed.

**Returns:** (boolean) True if the Shift key is pressed, false otherwise

---

### ADDON_API.Input:IsControlKeyDown()

Checks if the Control key is pressed.

**Returns:** (boolean) True if the Control key is pressed, false otherwise

---

### ADDON_API.Input:IsAltKeyDown()

Checks if the Alt key is pressed.

**Returns:** (boolean) True if the Alt key is pressed, false otherwise

---

### ADDON_API.Input:GetMousePos()

Gets the mouse position.

**Returns:** (number, number) The X and Y coordinates of the mouse

---

## Player

### ADDON_API.Player:ChangeAppellation(type)

Changes the player's title.

**Parameters:**
- `type` (number) - The ID of the title to set

**Returns:** (nil|true) True if the title was changed, nil otherwise

---

### ADDON_API.Player:GetShowingAppellation()

Gets the currently showing player title.

**Returns:** (string) The title

---

### ADDON_API.Player:GetGamePoints()

Gets the player's game points.

**Returns:** (number) The game points

---

### ADDON_API.Player:GetCrimeInfo()

Gets the player's crime information.

**Returns:** (table) The crime information

---

## Events & Timers

### ADDON_API.On(event, callback)

Binds callback to the desired event.

**Current events:**
- `UPDATE` (dt) - Called every frame, with dt = time in msec since last frame
- `CHAT_MESSAGE` - Called when a chat message is received
- `TEAM_MEMBERS_CHANGED` - Called when team members change
- `UI_RELOADED` - Called when the UI is reloaded
- `UPDATE_PING_INFO` - Called when the map ping is moved

**Parameters:**
- `event` (string) - The event name
- `callback` (function) - The callback function

**Returns:** nil

---

### ADDON_API:DoIn(msec, callback, ...)

Registers a callback to be executed in a certain duration (in msec). If you need to call this often, consider using On("UPDATE") instead.

**Parameters:**
- `msec` (number) - The duration in milliseconds
- `callback` (function) - The callback function
- `...` (any) - Additional arguments

**Returns:** nil

---

### ADDON_API:Emit(event, ...)

Emits an event. We recommend using event names like "ADDON_MY_EVENT" so that other addons don't get tripped.

**Parameters:**
- `event` (string) - The event name
- `...` (any) - Additional arguments

**Returns:** nil

---

## Ability

### ADDON_API.Ability:GetBuffTooltip(buffId, itemLevel)

Gets the tooltip for a buff.

**Parameters:**
- `buffId` (number) - The buff ID
- `itemLevel` (number) - The item level

**Returns:** (string) The tooltip text

---

### ADDON_API.Ability:GetAbilityFromView(index)

Gets the ability from the view.

**Parameters:**
- `index` (number) - The view index

**Returns:** (table) The ability information

---

### ADDON_API.Ability:IsActiveAbility(ability)

Checks if an ability is active.

**Parameters:**
- `ability` (table) - The ability information

**Returns:** (boolean) True if the ability is active, false otherwise

---

### ADDON_API.Ability:GetSkillsetNameById(skillsetId)

Gets the skillset name by ID.

**Parameters:**
- `skillsetId` (number) - The skillset ID

**Returns:** (string) The skillset name

---

### ADDON_API.Ability:GetUnitClassName(unit)

Gets the unit class name.

**Parameters:**
- `unit` (string) - The unit identifier

**Returns:** (string) The class name

---

### ADDON_API.Ability:GetClassNameFromSkillsetIds(skillset1, skillset2, skillset3)

Gets the class name from skillset IDs.

**Parameters:**
- `skillset1` (number) - The first skillset ID
- `skillset2` (number) - The second skillset ID
- `skillset3` (number) - The third skillset ID

**Returns:** (string) The class name

---

## Item

### ADDON_API.Item:GetItemInfoByType(itemId)

Gets item information by its type.

**Parameters:**
- `itemId` (number) - The item ID

**Returns:** (table) The item information

---

## Skill

### ADDON_API.Skill:GetSkillTooltip(skillId)

Gets the tooltip for a skill.

**Parameters:**
- `skillId` (number) - The skill ID

**Returns:** (string) The tooltip text

---

## Store

### ADDON_API.Store:GetSellerShareRatio()

Returns the Seller % on packs. Usually 80%, it futureproofs your addon if it changes.

**Returns:** (number) The seller share ratio

---

### ADDON_API.Store:GetSpecialtyRatio()

Gets the specialty ratio.

**Returns:** (number) The specialty ratio

---

### ADDON_API.Store:GetProductionZoneGroups()

Gets the production zone groups.

**Returns:** (table) The production zone groups

---

### ADDON_API.Store:GetSellableZoneGroups(startZoneId)

Gets the sellable zone groups starting from a zone ID.

**Parameters:**
- `startZoneId` (number) - The starting zone ID

**Returns:** (table) The sellable zone groups

---

### ADDON_API.Store:GetSpecialtyRatioBetween(startZoneId, finishZoneId)

Gets the specialty ratio between two zones.

**Parameters:**
- `startZoneId` (number) - The starting zone ID
- `finishZoneId` (number) - The finishing zone ID

**Returns:** (number) The specialty ratio

---

## Bank

### ADDON_API.Bank:CountItems()

Counts the items in the bank.

**Returns:** (number) The item count

---

### ADDON_API.Bank:Capacity()

Gets the bank's item capacity.

**Returns:** (number) The bank capacity

---

### ADDON_API.Bank:GetCurrency()

Gets the amount of gold in the player's bank.

**Returns:** (number) The currency amount

---

### ADDON_API.Bank:GetLinkText(inventorySlotId)

Gets the link text for a slot in the bank.

**Parameters:**
- `inventorySlotId` (number) - The bank slot ID

**Returns:** (string) The link text

---

## Equipment

### ADDON_API.Equipment:GetEquippedItemTooltipInfo(slotIdx)

Gets the tooltip information for an equipped item.

**Parameters:**
- `slotIdx` (number) - The slot index

**Returns:** (table) The tooltip information

---

### ADDON_API.Equipment:GetEquippedItemTooltipText(unit, slotIdx)

Gets the tooltip text for an equipped item.

**Parameters:**
- `unit` (string) - The unit identifier
- `slotIdx` (number) - The slot index

**Returns:** (string) The tooltip text

---

### ADDON_API.Equipment:GetEquippedSkillsetLunagems(unit)

Gets the equipped skillset lunagems for a unit.

**Parameters:**
- `unit` (string) - The unit identifier

**Returns:** (table) The lunagems information

---

## Siege Weapon

### ADDON_API.SiegeWeapon:GetSiegeWeaponSpeed()

Gets the siege weapon speed.

**Returns:** (number) The speed

---

### ADDON_API.SiegeWeapon:GetSiegeWeaponTurnSpeed()

Gets the siege weapon's turn speed.

**Returns:** (number) The turn speed

---

## Item Enchant

### ADDON_API.ItemEnchant:GetRatioInfos()

Gets the ratio information for item enchantment.

**Returns:** (table) The ratio information

---

### ADDON_API.ItemEnchant:GetEnchantItemInfo()

Gets the enchant item information.

**Returns:** (table) The enchant item information

---

### ADDON_API.ItemEnchant:GetSupportItemInfo()

Gets the support item information.

**Returns:** (table) The support item information

---

### ADDON_API.ItemEnchant:GetTargetItemInfo()

Gets the target item information.

**Returns:** (table) The target item information

---

## Map

### ADDON_API.Map:ToggleMapWithPortal(portal_zone_id, x, y, z)

Toggles the map with a portal.

**Parameters:**
- `portal_zone_id` (number) - The portal zone ID
- `x` (number) - X position
- `y` (number) - Y position
- `z` (number) - Z position

**Returns:** nil

---

### ADDON_API.Map:GetPlayerSextants()

Gets the player's coordinates in sextant format.

**Returns:** (table) The sextants information

---

## Zone

### ADDON_API.Zone:GetZoneStateInfoByZoneId(zoneId)

Gets the zone state information by zone ID.

**Parameters:**
- `zoneId` (number) - The zone ID

**Returns:** (table) The zone state information
