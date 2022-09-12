# LootQuantity Bug Demo

7 Days to Die Modlet: A demonstration of `LootQuantity` not applying its quantity-boosting effect to container or entity tags (but still applying to item tags).

The impact of this is that the perk Treasure Hunter does not actually do what it says it does in terms of an increase of Loot Quantity.

[YouTube Demonstration Video](https://youtu.be/9jYGf_Eoqys)

## Container Test

1. Install this mod
    - this will update the loot to fixed values (each item set to 100 before bonuses)
2. Start a *new game* and log in
3. Enable debug mode and creative mode with admin commands:
   - `dm`
   - `cm`
4. Learn `Treasure Hunter` level 3, which *should* (but does not) boost your general item yield for treasure chest containers by 30%.
   - use admin command `giveself 200000` to quickly get enough levels to increase Perception to level 7 and Treasure Hunter to level 3
5. Learn the book (use creative menu) `perkLuckyLooterDukes` which is expected to (and will) boost your dukes yield by 10%.
6. Give yourself the treasure_taylor quest with admin command `givequest treasure_taylor`.
7. Activate the Taylor quest in your quest menu, click the map button for the quest, and `CTRL`+`Right-Click` this quest location on your map.
8. Once there, enable fly mode with `Q` and use the `C` key to drop into the ground and easily identify the visible chest.
9. Unlock the chest (recommendation to use the admin digger tool with a single shot to break the lock).
10. When you open the chest, you will see that `Old Cash` and `CasinoCoin` are multiplied by 10% (item tag)... but no items are multiplied by the additional 30%, though they should be.

## Entity Container Test

To show that the same issue (container tags + LootQuantity) don't apply to Entity containers either, I've added the `buriedTreasure` tag to zombieArlene's `LootListOnDeath` as well.

Try spawning her with the F6 spawn tool while on land, kill her, and search her body.

You should see the same result you did with the treasure_taylor quest box.

## Could `count="all"` Be the Problem?

I added a commented-out modification in `loot.xml` that changes the groupBuriedTreasure loot group from `count="all"` to `count="1"` if you'd like to test that as well, you can uncomment this line and restart the game.

In my experience, count of all/1 still presents with this bug.

## Snippet from vanilla configs for reference

This snippet is a reference to the Vanilla Configs showing the expected bonus to `LootQuantity`.

`progression.xml`, Line 463

```xml
 <perk name="perkTreasureHunter" max_level="3" parent="skillPerceptionScavenging" name_key="perkTreasureHunterName" desc_key="perkTreasureHunterDesc" icon="ui_game_symbol_treasure">
  <!-- ... -->

  <effect_group>
   <passive_effect name="TreasureBlocksPerReduction" operation="base_add" level="0,1,2,3" value="0,-3,-5,-7"/><!-- base value in quests is 10 -->
   <passive_effect name="LootQuantity" operation="perc_add" level="1,2,3" value=".1,.3" tags="buriedTreasure"/>
   <!-- ... -->
  </effect_group>
</perk>
```
