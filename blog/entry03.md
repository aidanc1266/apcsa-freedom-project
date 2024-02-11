# Entry 3
##### 2/5/24

Text
What happened
Continuing off of the last blog, I added a few new items, both blocks and materials.
I added two new material items. The methodology for creating these items can be referenced in [Blog 2](https://github.com/aidanc1266/apcsa-freedom-project/blob/main/blog/entry02.md).
- Ruby
  
  ![2024-02-10_01 34 43](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/95a90f9b-7f1a-44a1-875d-114ee3cc2003)
  ![image](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/3ff96fcb-e932-445a-b325-c3698a721e46)

- Pearl
  
  ![2024-02-10_01 34 06](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/402a3ad4-7e1e-4ff9-a213-95ae4f1c80ef)
  ![image](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/c898e908-72b2-4711-abde-e2ceed221bba)

I also added two new blocks into the game. The process of creating blocks is slightly more complex than the item method.

First, I had to `register` the `ModBlocks` class in the `FreedomProject` class
```java
public class FreedomProject {
    // Define mod id in a common place for everything to reference
    public static final String MOD_ID = "freedomproject";
    // Directly reference a slf4j logger
    private static final Logger LOGGER = LogUtils.getLogger();
    public FreedomProject() {
        IEventBus modEventBus = FMLJavaModLoadingContext.get().getModEventBus();

        ModCreativeModeTabs.register(modEventBus);

        //register here
        ModItems.register(modEventBus);
        ModBlocks.register(modEventBus);

        modEventBus.addListener(this::commonSetup);

        MinecraftForge.EVENT_BUS.register(this);
        modEventBus.addListener(this::addCreative);
    }
```

Next, I created the `ModBlocks` class, where I registered all the mod's blocks. Including `copy(Blocks.IRON_BLOCK).sound(SoundType.AMETHYST)` in both blocks uses the shape of a `Block of Iron` and the breaking/placing sound of `Amethyst`, both items in Vanilla Minecraft. 
Note: The `IRON_BLOCK` could have been replaced by any block in Minecraft. It is only copying the shape of a regular block in Minecraft.
```java
package net.aidan.freedomproject.block;

import net.aidan.freedomproject.FreedomProject;
import net.aidan.freedomproject.item.ModItems;
import net.minecraft.world.item.BlockItem;
import net.minecraft.world.item.Item;
import net.minecraft.world.level.block.Block;
import net.minecraft.world.level.block.Blocks;
import net.minecraft.world.level.block.SoundType;
import net.minecraft.world.level.block.state.BlockBehaviour;
import net.minecraftforge.eventbus.api.IEventBus;
import net.minecraftforge.registries.DeferredRegister;
import net.minecraftforge.registries.ForgeRegistries;
import net.minecraftforge.registries.RegistryObject;

import java.util.function.Supplier;

public class ModBlocks {
    public static DeferredRegister<Block> BLOCKS =
            DeferredRegister.create(ForgeRegistries.BLOCKS, FreedomProject.MOD_ID);

    //the blocks
    public static final RegistryObject<Block> SAPPHIRE_BLOCK = registerBlock("sapphire_block",
            () -> new Block(BlockBehaviour.Properties.copy(Blocks.IRON_BLOCK).sound(SoundType.AMETHYST)));
    public static final RegistryObject<Block> RAW_SAPPHIRE_BLOCK = registerBlock("raw_sapphire_block",
            () -> new Block(BlockBehaviour.Properties.copy(Blocks.IRON_BLOCK).sound(SoundType.AMETHYST)));

    //helper methods
    private static <T extends Block> RegistryObject<T> registerBlock(String name, Supplier<T> block) {
        RegistryObject<T> toReturn = BLOCKS.register(name, block);
        registerBlockItem(name, toReturn);
        return toReturn;
    }
    private static <T extends Block> RegistryObject<Item> registerBlockItem(String name, RegistryObject<T> block) {
        return ModItems.ITEMS.register(name, () -> new BlockItem(block.get(), new Item.Properties()));
    }
    public static void register(IEventBus eventBus) {
        BLOCKS.register(eventBus);
    }
}
```

Then, I added the new blocks and items to the mod's custom creative mode inventory tab.
```java
public static final RegistryObject<CreativeModeTab> FREEDOM_TAB = CREATIVE_MODE_TABS.register("freedom_tab",
            () -> CreativeModeTab.builder().icon(() -> new ItemStack(ModItems.SAPPHIRE.get()))
                    .title(Component.translatable("creativetab.freedom_tab"))
                    .displayItems((pParameters, pOutput) -> {
                        //materials
                        pOutput.accept(ModItems.SAPPHIRE.get());
                        pOutput.accept(ModItems.RAW_SAPPHIRE.get());
                        pOutput.accept(ModItems.RUBY.get());                  //new item
                        pOutput.accept(ModItems.PEARL.get());                 //new item
                        pOutput.accept(Items.DIAMOND);

                        //blocks
                        pOutput.accept(ModBlocks.SAPPHIRE_BLOCK.get());       //new item
                        pOutput.accept(ModBlocks.RAW_SAPPHIRE_BLOCK.get());   //new item
                    })
```
![image](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/2731f8db-e350-4116-9b38-d37af54c54ec)

Then I created the `blockstates` folder. This is where I created `.json` files for each block. This tells us that the block takes on normal properties (e.g. full 1x1 block; not a half slab, stairs, etc.). For example, this is what is coded for the `Block of Sapphire`.
```java
{
  "variants": {
    "": {
      "model": "freedomproject:block/sapphire_block"
    }
  }
}
```

I later created the `block` directory under the `models` folder. In `block`, a `.json` file is created for each block (like before). This determines what texture/property is used on each side of the block. In this case, the block will use the same texture for all four sides. For example, this is what is coded for the `Block of Sapphire`.
```java
{
  "parent": "minecraft:block/cube_all",
  "textures": {
    "all": "freedomproject:block/sapphire_block"
  }
}
```

This is also done for the `item` directory under the `models` folder. This gives the block a texture while in the inventory.
```java
{
  "parent": "freedomproject:block/sapphire_block"
}
```

Then the blocks/items are given a name in the `en_us.json` file under the `lang` folder. The text in quotes is the name displayed for each item.
```java
{
  "item.freedomproject.sapphire": "Sapphire",
  "item.freedomproject.raw_sapphire": "Raw Sapphire",
  "item.freedomproject.ruby": "Ruby",                                  //new item
  "item.freedomproject.pearl": "Pearl",                                //new item

  "block.freedomproject.sapphire_block": "Block of Sapphire",          //new item
  "block.freedomproject.raw_sapphire_block": "Block of Raw Sapphire",  //new item

  "creativetab.freedom_tab": "Sapphire Test Tab" //name of custom inventory tab
}
```

Finally, the textures are uploaded for each item.
- Ruby ![ruby](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/03770705-105e-490c-b28c-9de089e501c4)
- Pearl ![pearl harbor](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/e5fb557b-b3c2-4f44-aa0c-1c7d857900f3)
- Block of Sapphire ![raw_sapphire_block](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/c35d9a2c-b520-4219-9e7a-24d8c9d48d18)
  ![2024-02-11_00 29 38](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/1a8ae470-f2a8-47fa-8c9a-4a66bdc5abd1)
  ![2024-02-11_00 31 39](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/285333cd-4b49-4567-ae8a-b11c5564e6c3)
  ![2024-02-11_00 32 59](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/06bcee22-9c86-47b7-92fb-a5f1ce264fa1)


- Block of Raw Sapphire ![sapphire_block](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/c23d58bc-7da2-4d82-a321-29efe91da6b7)
  ![2024-02-11_00 29 51](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/d514a879-499b-433a-9711-a12fbfb58423)
  ![2024-02-11_00 32 00](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/f85c1be6-2c47-4a07-910f-e2a9e560a9b8)
  ![2024-02-11_00 32 48](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/13a46e44-51ab-4f3a-935f-72a9e51a1ebe)

  




However, blocks are implemented differently. Here is what I did to create the `Block of Sapphire` and `Block of Raw Sapphire`. I learned how to do this using this [video](https://www.youtube.com/watch?v=C_VO6tD6Y1g).
Note: these blocks cannot be broken and will not drop anything if broken. This will be added in the future.


what's next
Add the Block of Ruby and Pearl.
Learn how to create armor, tools, and weapons.
Learn how to create crafting recipies using the materials.
Now that college applications are over, more will be done in a shorter amount of time.

[Previous](entry02.md) | [Next](entry04.md)

[Home](../README.md)
