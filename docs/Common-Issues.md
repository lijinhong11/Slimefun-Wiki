# 常见的游戏内问题 {#common-issues}

本页面包含一些常见的游戏内问题与解决方案。  
大部分解决方案需要管理员权限，如果你没有相应的权限，请联系管理员解决问题!

:::warning

部分问题有多种解决方案，这些方案将由影响程度从小到大排序。我们强烈建议你先尝试影响程度较小的解决方案，因为影响程度越小的解决方案风险更小。如果你已经尝试了所有方案但都未解决问题，请考虑[汇报 Bug](/How-to-report-bugs)！

:::

## 全息投影标签遗留 {#floating-tags}

某些 Slimefun 物品会在被放置后自动创建全息投影标签，特别是[能量调节器](/Energy-Regulator)和[货运管理器](/Cargo-Manager)。  
这些标签应该会在破坏机器后消失。但有时候会出现问题，导致部分全息投影依然存留在世界中。该如何解决这个问题呢?

:::info

确保这不是幽灵方块造成的问题! 请参阅[无法放置的方块](#unplaceable-blocks)。

:::

### 修复方案 1 {#how-to-fix-this-stage-1}

移除任何在全息投影标签下方的调节器/管理器，然后站在该标签附近，以管理员身份运行以下指令:

```minecraft
/execute as <your_name> at @s run execute as @e[type=armor_stand,nbt={Invisible:1b},distance=..3] run data merge entity @s {Invisible:0}
```

现在，你可以摧毁那些盔甲架了。你也许要摧毁多个盔甲架，因为它们可能由不同的插件生成，但堆叠在一起。

:::tip 提示

推荐使用 MyCommands、CommandOverride 或其他类似插件并为该条指令设置一个容易记住的别名（例如 /holokill）。

:::

### 修复方案 2 {#how-to-fix-this-stage-2}

通过以下指令切换到观察者模式：

```minecraft
/gamemode spectator
```

你应该可以看到隐身的盔甲架，并确定脚部的位置。在盔甲架下方的第一个未被盔甲架占据的方块上，放下一个新的调节器（可通过 `/sf cheat` 获取）。  
调节器会尝试将错误的盔甲架替换成属于该调节器自己的盔甲架，就像正常的一样。然后，移除刚刚放下的调节器，来移除所有的全息投影标签。

### 修复方案 3 {#how-to-fix-this-stage-3}

如果全息投影不断重新生成，你可能需要将其当成幽灵方块处理。这与[无法放置的方块](#unplaceable-blocks)类似，但区别就是有全息投影。  
只要按照过程来移除幽灵方块，应该没什么大问题。

## 无法放置的方块 {#unplaceable-blocks}

如果一在某个空的位置尝试放置方块，但放置动作被终止，这说明这可能是个幽灵方块。  
这意味着这里曾经有 Slimefun 物品被放置于此处（通常是[机器人](/Androids)或[货运管理器](/Cargo-Management)等玩家头类型的物品），但此处方块的数据没有被正确移除。

### 如何修复（单个方块） {#hhow-to-fix-this-isolated-block}

以管理员身份执行下方的指令，获取一条调试鱼:

```minecraft
/sf debug_fish
```

使用 Shift + 右键 放置一个占位符方块，然后 Shift + 左键 点击它。此时，你会看到方块被破坏的动画，被取消的事件应该会在聊天栏中展示。这将会移除该位置所有的方块数据。

现在，只需要破坏占位符方块即可。

:::info 可选项

使用 Shift + 右键 在幽灵方块的位置放置一个占位符方块，然后尝试破坏它。如果有代表 Slimefun 物品的玩家头掉落，则该位置确实有幽灵方块（然而，没有掉落并不说明此处不是幽灵方块，因为 Slimefun 物品可能是一个完整的方块，而不是一个玩家头，但这种情况的可能性并不大）。

:::

### 如何修复（一堆方块） {#how-to-fix-this-cluster-of-blocks}

这与处理单一方块的过程类似。但在这情况下，你可以使用 WorldEdit 的指令来加速放置的过程，不必挨个放置假的玩家头。

使用 WorldEdit 的选取工具选中需要处理的范围，并向外扩大一点，然后执行以下指令:

```minecraft
//set stone
```

就像处理单个方块一样，Shift + 左键 点击所有可能有问题的地方，清除这些位置的数据。

最后，执行以下指令来移除所有石头:

```minecraft
//undo
```

## 不掉落基础电路板 {#circuit-boards-not-dropping}

如果你击杀了铁傀儡但没有掉落基础电路板，这可能是因为有冲突的插件。  
像 **MobStacker** 这样的插件或其他类似插件有许多已知的自定义物品掉落相关的问题。

### 如何修复（方法 1）

最好的解决办法就是当这些插件有开启自定义物品掉落的选项时，开启该选项。  
或者去询问作者是否可以切换物品掉落。

### 如何修复（方法 2）

更激进的办法就是换一个支持自定义掉落物的插件。  

:::info

关于这一点，我们并没有一个已确认支持 Slimefun 的自定义生物插件列表。如果你使用的插件没有任何问题，你可以将插件名称告诉我们。详见[扩充 Wiki](/Expanding-the-Wiki)。

:::

## Windows 上遇到了 AccessDeniedException {#access-denied-exception}

在 Windows 系统中，如果你遇到了以下错误:

```shell
An Error occurred while copying a temporary File for Slimefun DEV x
AccessDeniedException：data-storage/Slimefun/stored-blocks/world/BLOCK.sfb.tmp -> data-storage/Slimefun/stored-blocks/world/BLOCK.sfb
```

这通常是因为:

- 文件锁 - 另外一个进程正在访问此文件，因此该文件被"锁"住了，我们无法修改该文件。
- 文件权限 - 你的文件权限有问题

### 如何修复

打开报错内容所描述的目录（示例中为 `data-storage/Slimefun/stored-blocks/world`），将 `BLOCK.sfb.tmp` 重命名为 `BLOCK.sfb`，覆盖原有的文件。

如果你遇到了错误，可能是因为你的权限有问题，你需要先解决你的权限问题。
