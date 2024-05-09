# Entry 5
##### 5/9/24
Delayed in prepreation for the AP Chem exam.

Implemented `Metal Detector` item.
This special item will detect the highest y-level ore when right-clicked on a given x coordinate.
(Done with the help of this [guide](https://www.youtube.com/watch?v=TPfNvwfgXAU))

First, I had to create the `Metal Detector Class` which is a child class of `Item`. This is not required but is a good way to organize code by special item.
There are many methods in the `Item` class that can be used for many custom items.

```java
package net.aidan.freedomproject.item.custom;

import net.minecraft.client.resources.language.I18n;
import net.minecraft.core.BlockPos;
import net.minecraft.network.chat.Component;
import net.minecraft.world.InteractionResult;
import net.minecraft.world.entity.player.Player;
import net.minecraft.world.item.Item;
import net.minecraft.world.item.context.UseOnContext;
import net.minecraft.world.level.block.Block;
import net.minecraft.world.level.block.Blocks;
import net.minecraft.world.level.block.state.BlockState;

public class MetalDetectorItem extends Item {
    public MetalDetectorItem(Properties pProperties) {
        super(pProperties);
    }

    @Override
    public InteractionResult useOn(UseOnContext pContext) {
        if (!pContext.getLevel().isClientSide()) {
            BlockPos positionClicked = pContext.getClickedPos();
            Player player = pContext.getPlayer();
            boolean foundBlock = false;

            for (int i = 0; i <= positionClicked.getY() + 64; i++) {
                BlockState state = pContext.getLevel().getBlockState(positionClicked.below(i));
                if (isValuableBlock(state)) {
                    outputValuableCoordinates(positionClicked.below(i), player, state.getBlock());
                    foundBlock = true;
                    break;
                }
            }
            if (!foundBlock) {
                player.sendSystemMessage(Component.literal("Nothin here jitto"));
            }
        }

        pContext.getItemInHand().hurtAndBreak(1, pContext.getPlayer(), player -> player.broadcastBreakEvent(player.getUsedItemHand()));

        return InteractionResult.SUCCESS;
    }

  //helper methods
    private void outputValuableCoordinates(BlockPos blockPos, Player player, Block block) {
        player.sendSystemMessage(Component.literal("Found " + I18n.get(block.getDescriptionId()) + " at " + "(" + blockPos.getX() + ", " + blockPos.getY() + "," + blockPos.getZ() + ")"));
    }

    private boolean isValuableBlock(BlockState state) {
        return state.is(Blocks.IRON_ORE) || state.is(Blocks.DIAMOND_ORE) || state.is(Blocks.GOLD_ORE) || state.is(Blocks.COAL_ORE);
    }
}
```

That is a lot of code, so let's break it down.

First is the `useOn` method. When the `Metal Detector` item is right-clicked on a block, a `for` loop checks every block in that x-coordinate column. If an ore is found, the coordinates and type of ore will be printed in the chat. If not, then "Nothin here jitto" will be printed in the chat.
```java
public InteractionResult useOn(UseOnContext pContext) {
        if (!pContext.getLevel().isClientSide()) {
            BlockPos positionClicked = pContext.getClickedPos();
            Player player = pContext.getPlayer();
            boolean foundBlock = false;

            for (int i = 0; i <= positionClicked.getY() + 64; i++) {
                BlockState state = pContext.getLevel().getBlockState(positionClicked.below(i));
                if (isValuableBlock(state)) {
                    outputValuableCoordinates(positionClicked.below(i), player, state.getBlock());
                    foundBlock = true;
                    break;
                }
            }
            if (!foundBlock) {
                player.sendSystemMessage(Component.literal("Nothin here jitto"));
            }
        }

        pContext.getItemInHand().hurtAndBreak(1, pContext.getPlayer(), player -> player.broadcastBreakEvent(player.getUsedItemHand()));

        return InteractionResult.SUCCESS;
    }
```

Iron ore is found. "Found Iron Ore at ([coordinates]) is printed.
![2024-05-09_18 00 23](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/0d081aa1-906d-4af6-ae7e-a920e4d13b6c)

When there is no ore, "Nothin here jitto" is printed in the chat.
![2024-05-09_18 01 50](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/8fc435a2-2815-48a2-a109-220ce4b3cc93)

However, if there is more than one ore in an x-coordinate column, only the closer ore to the surface will be printed.
In this example, only `Iron Ore` and its coordinates are printed in the chat.
![2024-05-09_18 03 30](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/5632dba5-2726-4fb2-aa00-5675b26404c7)

Additionally, the item was added to the custom creative mode inventory tab.
```java
public static final RegistryObject<CreativeModeTab> FREEDOM_TAB = CREATIVE_MODE_TABS.register("freedom_tab",
            () -> CreativeModeTab.builder().icon(() -> new ItemStack(ModItems.SAPPHIRE.get()))
                    .title(Component.translatable("creativetab.freedom_tab"))
                    .displayItems((pParameters, pOutput) -> {
                        //materials
                        pOutput.accept(ModItems.SAPPHIRE.get());
                        pOutput.accept(ModItems.RAW_SAPPHIRE.get());
                        pOutput.accept(ModItems.RUBY.get());
                        pOutput.accept(ModItems.PEARL.get());

                        //blocks
                        pOutput.accept(ModBlocks.SAPPHIRE_BLOCK.get());
                        pOutput.accept(ModBlocks.RAW_SAPPHIRE_BLOCK.get());
                        pOutput.accept(ModBlocks.RUBY_BLOCK.get());
                        pOutput.accept(ModBlocks.RUBY_ORE.get());

                        //special items METAL DETECTOR IS HERE
                        pOutput.accept(ModItems.METAL_DETECTOR.get());
                    }
```
![image](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/3de9c2c2-8ffa-40cf-8cad-8cfa3f889f49)

The jepg of the `Metal Detector` was also uploaded.
![image](https://github.com/aidanc1266/apcsa-freedom-project/assets/145048443/ef33ca5e-ae69-442a-9b00-b9b1679f66c4)

I also created a [video](https://youtu.be/lhLLhA6S1WM) showcasing the mod thus far.

So, what is next?

By the next blog, I plan to do the following:
- Add drops to all blocks using this [tutorial](https://www.youtube.com/watch?v=kSXP_GXdNGg&t=7s)
- Add advanced blocks with custom effects using this [tutorial](https://www.youtube.com/watch?v=HCbmrPkDV7A)
- Add custom food using implemented materials using this [tutorial](https://www.youtube.com/watch?v=8c9BZM3RAIA)

At this point, I am in between the "Create" and "Test" stages in the Engineering Design Process since I am both creating new items/blocks and testing them in-game.

Over the past few months, I worked on my collaboration and time management. For collaboration, I worked with one of my friends Alex Xiao, who gave me the models/textures for the Ruby, Ruby Ore, and Pearl items. Being able to offload a little bit of the workload allowed me to hone in on other aspects of the Freedom Projects, such as implementation, In terms of time management, as I reflect on the last couple of months, I think my time management was a bit iffy. Because of ongoing AP exams, it was very difficult to balance my classwork demands with the Freedom Project. Finding time to work on the Freedom Project is still something I am working on.

Text

[Previous](entry04.md) | [Next](entry06.md)

[Home](../README.md)
