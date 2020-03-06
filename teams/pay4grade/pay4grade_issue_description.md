Main Issue that we're trying to solve:
https://github.com/runelite/runelite/issues/10655

Will also fix this bug if the above issue is solved:
https://github.com/runelite/runelite/issues/10718

The "open herb sack" referenced in the Main Issue is a new item introduced by the game developers. It can automatically collect harvested herbs into the sack so the limited inventory space is not cluttered.

Runelite's loot tracker plugin however has not been updated for this new item. The current implementation of loot tracker will take a snapshot of the player's inventory before and after the harvesting action, calculate the difference, and put the new items that shows up in the inventory into the loot tracker.

With the open herb sack however, the inventory snapshot will not see a change in inventory items, as the loot went straight into the sack instead, and therefore the loot tracker plugin won't track the loot.

We will attempt to fix this issue as suggested by reading the in game messages and capturing the herb sack additions from there. We would then have to strip the strings for the actual item name, find the item's ID, and update loot tracker with it.

Test cases would also be written for this new enhancement per contribution guidelines.

(This feature will also help me personally in game as I do plan on training the Hunting skill and tracking the loot here)
