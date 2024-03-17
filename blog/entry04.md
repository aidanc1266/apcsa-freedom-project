# Entry 4
##### 3/16/2024

**Implementing `Block of Ruby` and `Ruby Ore`**

This process was similar to adding the `Block of Sapphire/Raw Sapphire` which can be found in the [previous blog](https://github.com/aidanc1266/apcsa-freedom-project/blob/main/blog/entry03.md).
- Started off with registering the blocks
    ```java
    public static final RegistryObject<Block> RUBY_BLOCK = registerBlock("ruby_block",
            () -> new Block(BlockBehaviour.Properties.copy(Blocks.IRON_BLOCK).sound(SoundType.STONE)));
    public static final RegistryObject<Block> RUBY_ORE = registerBlock("ruby_ore",
            () -> new Block(BlockBehaviour.Properties.copy(Blocks.IRON_BLOCK).sound(SoundType.DIAMOND_ORE)));
    ```
- Create all textures on all sides of the block
- Note: only `Block of Ruby is shown`
    ```java
    {
    "parent": "minecraft:block/cube_all",
    "textures": {
      "all": "freedomproject:block/ruby_block"
    }
    ```
- Give displayed in-game names to both blocks
    ```java
    "block.freedomproject.ruby_block": "Block of Ruby",
    "block.freedomproject.ruby_ore": "Ruby Ore",
    ```
- Give display in inventory and while holding
- Note: only `Block of Ruby` is shown
    ```java
    {
    "parent": "freedomproject:block/ruby_block"
    }
    ```
- Upload both textures for blocks  ![ruby_block](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/2ed76bee-8469-4bb3-abdd-a3a9e45741ad) ![ruby_ore](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/15408df0-c677-4998-a15b-c9b71192eacc)

**Block of Ruby**
![2024-03-16_16 07 54](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/77d852f2-e63a-45f5-9715-ad3f7aa76d98)
![2024-03-16_16 13 09](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/0fe0f884-7493-405d-bb44-e40c429017a0)

**Ruby Ore**
![2024-03-16_16 08 01](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/a7f5ca71-01ad-42ab-88d8-42b37a3b229b)
![2024-03-16_16 15 15](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/7c03c9fe-0260-457d-a3ff-77b312756612)

**Adding Crafting Recipies for Blocks**

I used this [guide](https://www.youtube.com/watch?v=NppdgWsSVec) to add crafting recipes for all the blocks added in the game.


[Previous](entry03.md) | [Next](entry05.md)

[Home](../README.md)
