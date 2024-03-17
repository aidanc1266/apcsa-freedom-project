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
First, I had to create multiple directories ending in the recipes folder. Here I made the `.json` files for each crafting recipe. These `.json` files can be named anything.
![image](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/17c03783-aff1-4863-aa1f-20932ebab045)

Below is the code from the `sapphire_from_sapphire_block.json` file. This recipe is shapeless, meaning there is no definite pattern.
```java
{
  "type": "minecraft:crafting_shapeless",         //block can be placed anywhere in the 2x2 or 3x3 crafting grid to proceed
  "catagory": "misc",
  "ingredients": [
    {
      "item": "freedomproject:sapphire_block"     //sapphire block needed
    }
  ],
  "result": {
    "item": "freedomproject:sapphire",            //gives 9 sapphire as a result
    "count": 9
  }
}
```

From `Block of Sapphire` to `Sapphire`

![image](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/93fc0d29-74ca-4891-9d10-9995513f2705)

From `Block of Raw Sapphire` to `Raw Sapphire`

![image](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/e8799f54-bb54-4d99-953c-7e86dee55dcf)

From `Block of Ruby` to `Ruby`

![image](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/9be32fd9-f9fc-46f2-a9c9-bf521bc61b37)

This is an example of a shaped recipe from the `sapphire_block_from_sapphire.json` file.
```java
{
  "type": "minecraft:crafting_shaped",        //marked as shaped
  "catagory": "misc",
  "pattern": [
    "###",                                    //this mini array marks the 3x3 crafting array
    "###",
    "###"
  ],
  "key": {
    "#": {
      "item": "freedomproject:sapphire"       //key symbolizes each # as a sapphire item
    }
  },
  "result": {
    "item": "freedomproject:sapphire_block"   //returns sapphire block
  }
}
```

From `Sapphire` to `Block of Sapphire`

![image](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/dd2a39ad-0634-4318-b3ff-39f7b97cd6df)

From `Raw Sapphire` to `Block of Raw Sapphire`

![image](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/23485e1a-6688-49e9-b5d3-dbd600a60313)

From `Ruby` to `Block of Ruby`

![image](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/c9fff2d4-aaa0-49fa-8a25-9f68700de16e)

**Smelting Items Using a Furnace**

In this section, I created smelting recipes to go from `Raw Sapphire` to `Sapphire` and `Ruby Ore` to `Ruby`.
Below is a snippet of code from `sapphire_from_blasting_raw_sapphire.json` file. This allows the mod to convert `Raw Sapphire` into `Sapphire` using a `Blast Furnace`.
```java
{
  "type": "minecraft:blasting",            //type of furnace (blast)
  "category": "misc",
  "cookingtime": 100,                      //time to smelt, regular furnace is 200 but since this is blast it is 100
  "experience": 0.7,                       //experience points gained after smelting
  "group": "sapphire",
  "ingredient": {
    "item": "freedomproject:raw_sapphire"  //input: Raw Sapphire
  },
  "result": "freedomproject:sapphire"      //result: sapphire
}
```

Example with the regular furnace from `ruby_from_smelting_ruby_ore.json` file.
```java
{
  "type": "minecraft:smelting",        //type of furnace (regular)
  "category": "misc",
  "cookingtime": 200,                  //regular smelt time
  "experience": 0.7,
  "group": "ruby",
  "ingredient": {
    "item": "freedomproject:ruby_ore"  //input: Ruby Ore
  },
  "result": "freedomproject:ruby"      //output: ruby
}
```


[Previous](entry03.md) | [Next](entry05.md)

[Home](../README.md)
