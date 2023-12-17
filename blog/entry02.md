# Entry 2
##### 12/11/23

I continued using the [guide](https://www.youtube.com/watch?v=o6Xbp2dTEGA&t=3s) I found last blog. I made a few test items while watching the guide. Now, I will be making my own custom items.

To create these items, I had to traverse through various files. 
First, I had to create the ModItems class. This class holds all the items that will be registered in the mod. 
```java
package net.aidan.freedomproject.item;

import net.aidan.freedomproject.FreedomProject;
import net.minecraft.world.item.Item;
import net.minecraftforge.eventbus.api.IEventBus;
import net.minecraftforge.registries.DeferredRegister;
import net.minecraftforge.registries.ForgeRegistries;
import net.minecraftforge.registries.RegistryObject;

public class ModItems {
    public static final DeferredRegister<Item> ITEMS =
            DeferredRegister.create(ForgeRegistries.ITEMS, FreedomProject.MOD_ID);

    public static final RegistryObject<Item> SAPPHIRE = ITEMS.register("sapphire",
            () -> new Item(new Item.Properties()));
    public static final RegistryObject<Item> RAW_SAPPHIRE = ITEMS.register("raw_sapphire",
            () -> new Item(new Item.Properties()));

    public static void register(IEventBus eventBus) {
        ITEMS.register(eventBus);
    }
}
```

Then, I added the items to the `Ingredients` tab in the Minecraft vanilla inventory. But later, I make my own custom tab for these items.
```java
private void addCreative(BuildCreativeModeTabContentsEvent event) {
        if (event.getTabKey() == CreativeModeTabs.INGREDIENTS) {
            event.accept((ModItems.SAPPHIRE));
            event.accept((ModItems.RAW_SAPPHIRE));
        }
    }
```
![image](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/3e06f0a0-4b92-4527-a3d2-9d29d40ebd0c)

Then, I made the `lang`, `models`, and `textures` folders.
In the `lang` folder, I made the `en_us.json` file which assigns a display name to each item and inventory tab.
```java
{
  "item.freedomproject.sapphire": "Sapphire",         //sapphire item
  "item.freedomproject.raw_sapphire": "Raw Sapphire", //raw sapphire item
  "creativetab.freedom_tab": "Sapphire Test Tab"      //creative mode inventory tab
}
```
![image](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/0b827984-97a0-4da9-b6c7-85d280220419) ![image](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/155ed53f-b207-45ae-8d79-6e6d01e862d0) ![image](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/6367be5d-6d32-4577-8936-5238d05ad8f2)

In the `models` folder, I made the `item` folder which contains the `raw_sapphire` and `sapphire` files. 
```java
{
  "parent": "item/generated",                     //takes the 2D texture and gives it depth so it has a 3D model
  "textures": {                                   //textures calls the textures folder (see next part) for the item
    "layer0": "freedomproject:item/raw_sapphire"  //cd to the item folder (see next part) which contains the jpeg of the item
  }
}
```
Note: this is only the raw_sapphire file, the sapphire file is the same thing except replacing raw_sapphire with sapphire.

In the `textures` file, I created the `item` folder which contains the jpeg for each item. The jpegs were available for download in the guide [repo](https://github.com/Tutorials-By-Kaupenjoe/Forge-Tutorial-1.20.X/tree/2-customItems).

Now like I said earlier, I made a custom tab in the inventory for the new items and a diamond.
```java
public static final RegistryObject<CreativeModeTab> FREEDOM_TAB = CREATIVE_MODE_TABS.register("freedom_tab",
            () -> CreativeModeTab.builder().icon(() -> new ItemStack(ModItems.SAPPHIRE.get()))
                    .title(Component.translatable("creativetab.freedom_tab"))
                    .displayItems((pParameters, pOutput) -> {
                        pOutput.accept(ModItems.SAPPHIRE.get());
                        pOutput.accept(ModItems.RAW_SAPPHIRE.get());

                        pOutput.accept(Items.DIAMOND);
                    })
                    .build());
```
![image](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/6367be5d-6d32-4577-8936-5238d05ad8f2)

**Skills**

I improved my collaboration skills during this time. While asking one of my friends to help me make the textures for future items/blocks, some of my other friends asked if they can join in on the project. Essentially, they would make their own things in the mod and we can compile it all together and make it connect somehow. Will ask Mr. Mueller if this is allowed.

**Freedom Project Goals for Winter Break**

Right now in the engineering process, I'm in the brainstorming step, coming up with items and blocks that I should add to the mod. My friend agreed to make the textures for the mod using the Dept. of Education provided Adobe Creative Cloud. I would like to make the `Block of Sapphire` item to make a full circle with `sapphire` and `Raw Sapphire` using the next episode in the [guide](https://www.youtube.com/watch?v=C_VO6tD6Y1g).
![image](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/d6c87891-6f9d-4f8f-b938-c1212c495cc3)


[Previous](entry01.md) | [Next](entry03.md)

[Home](../README.md)
