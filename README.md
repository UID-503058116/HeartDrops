#可能修复了一个Bug.
起因是这样：我遇到一个崩溃。

---- Minecraft Crash Report ----
// Lolis deobfuscated this stacktrace using MCP's stable-39 mappings.
// Uh... Did I do that?

Time: 2025-02-02 00:40:47 CST
Description: Exception ticking world

java.lang.IllegalStateException: Duplicate Capability Key: heartdrops:drop_hearts com.deflatedpickle.heartdrops.capability.DropHearts$Provider@22de4753
    at net.minecraftforge.event.AttachCapabilitiesEvent.addCapability(AttachCapabilitiesEvent.java:69)
    at com.deflatedpickle.heartdrops.event.ForgeEventHandler.onAttachCapabilitiesEventEntity(ForgeEventHandler.kt:271)
    at net.minecraftforge.fml.common.eventhandler.ASMEventHandler_389_ForgeEventHandler_onAttachCapabilitiesEventEntity_AttachCapabilitiesEvent.invoke(.dynamic)
    at net.minecraftforge.fml.common.eventhandler.ASMEventHandler.invoke(ASMEventHandler.java:92)
    at net.minecraftforge.fml.common.eventhandler.EventBus.post(EventBus.java:182)
    at net.minecraftforge.event.ForgeEventFactory.gatherCapabilities(ForgeEventFactory.java:1462)
    at net.minecraft.entity.Entity.<init>(Entity.java:219)
    at net.minecraft.entity.item.EntityItem.<init>(EntityItem.java:48)
    at net.minecraft.entity.item.EntityItem.<init>(EntityItem.java:61)
    at net.minecraft.block.Block.spawnAsEntity(Block.java:597)
    at net.minecraft.block.Block.dropBlockAsItemWithChance(Block.java:578)
    at net.minecraft.block.Block.dropBlockAsItem(Block.java:564)
    at net.minecraft.block.BlockTorch.checkForDrop(BlockTorch.java:199)
    at net.minecraft.block.BlockTorch.onBlockAdded(BlockTorch.java:145)
    at net.minecraft.world.chunk.Chunk.setBlockState(Chunk.java:614)
    at net.minecraft.world.World.setBlockState(World.java:343)
    at net.minecraft.world.gen.structure.StructureComponent.setBlockState(StructureComponent.java:236)
    at net.minecraft.world.gen.structure.StructureComponent.randomlyPlaceBlock(StructureComponent.java:342)
    at net.minecraft.world.gen.structure.StructureMineshaftPieces$Corridor.placeSupport(SourceFile:542)
    at net.minecraft.world.gen.structure.StructureMineshaftPieces$Corridor.addComponentParts(SourceFile:463)
    at net.minecraft.world.gen.structure.StructureStart.generateStructure(StructureStart.java:47)
    at net.minecraft.world.gen.structure.MapGenStructure.generateStructure(MapGenStructure.java:94)
    at biomesoplenty.common.world.ChunkGeneratorOverworldBOP.populate(ChunkGeneratorOverworldBOP.java:505)
    at net.minecraft.world.chunk.Chunk.populate(Chunk.java:1019)
    at net.minecraft.world.chunk.Chunk.populate(Chunk.java:999)
    at net.minecraft.world.gen.ChunkProviderServer.provideChunk(ChunkProviderServer.java:157)
    at net.minecraft.server.management.PlayerChunkMapEntry.providePlayerChunk(PlayerChunkMapEntry.java:126)
    at net.minecraft.server.management.PlayerChunkMap.tick(PlayerChunkMap.java:226)
    at net.minecraft.world.WorldServer.tick(WorldServer.java:227)
    at net.minecraft.server.MinecraftServer.updateTimeLightAndEntities(MinecraftServer.java:756)
    at net.minecraft.server.MinecraftServer.tick(MinecraftServer.java:668)
    at net.minecraft.server.integrated.IntegratedServer.tick(IntegratedServer.java:279)
    at net.minecraft.server.MinecraftServer.run(MinecraftServer.java:526)
    at java.lang.Thread.run(Thread.java:1570)


A detailed walkthrough of the error, its code path and all known details is as follows:
---------------------------------------------------------------------------------------

-- Affected level --
  Level name: 新的世界
  All players: 1 total; [EntityPlayerMP['Mamehys'/2854, l='新的世界', x=707.71, y=178.89, z=2228.81]]
  Chunk stats: ServerChunkCache: 446 Drop: 0
  Level seed: -5054971050733222110
  Level generator: ID 06 - BIOMESOP, ver 0. Features enabled: true
  Level generator options: 
  Level spawn location: World: (72,64,252), Chunk: (at 8,4,12 in 4,15; contains blocks 64,0,240 to 79,255,255), Region: (0,0; contains chunks 0,0 to 31,31, blocks 0,0,0 to 511,255,511)
  Level time: 19921 game time, 19921 day time
  Level dimension: 0
  Level storage version: 0x04ABD - Anvil
  Level weather: Rain time: 132325 (now: false), thunder time: 4420 (now: false)
  Level game mode: Game mode: creative (ID 1). Hardcore: false. Cheats: true

根据 DeepSeek 的看法：

根据提供的代码和崩溃报告，问题出在 onAttachCapabilitiesEventEntity 方法中。当前代码未检查实体是否已经附加了 DropHearts Capability，导致重复附加并抛出 Duplicate Capability Key 异常。

重点修复 onAttachCapabilitiesEventEntity 方法。

修复 onAttachCapabilitiesEventEntity 方法

添加了检查逻辑：仅当实体是 EntityLivingBase 且未附加过 DropHearts Capability 时，才附加该能力。

使用 hasCapability 方法避免重复附加。

优化逻辑，确保 Capability 附加逻辑仅在必要时执行，避免重复附加导致的崩溃。

# Heart-Drops
A Minecraft mod that adds hearts that may be picked up for extra health.
