i﻿mport java.util.ArrayList; 
import java.util.List; 
import java.util.Random;

import javax.annotation.Nullable;

import net.minecraft.block.Block; 
import net.minecraft.block.SoundType; 
import net.minecraft.block.material.Material; 
import net.minecraft.block.state.IBlockState; 
import net.minecraft.client.renderer.GlStateManager; 
import net.minecraft.creativetab.CreativeTabs; 
import net.minecraft.entity.Entity; 
import net.minecraft.entity.player.EntityPlayer; 
import net.minecraft.util.BlockRenderLayer; 
import net.minecraft.util.EnumParticleTypes; 
import net.minecraft.util.math.AxisAlignedBB; 
import net.minecraft.util.math.BlockPos; 
import net.minecraft.util.math.RayTraceResult; 
import net.minecraft.util.math.RayTraceResult.Type;
import net.minecraft.world.IBlockAccess; 
import net.minecraft.world.World; 
import net.minecraftforge.client.event.DrawBlockHighlightEvent; 
import net.minecraftforge.fml.common.eventhandler.SubscribeEvent; 
import net.minecraftforge.fml.relauncher. 
import net.minecraftforge.fml.relauncher.SideOnly;

classe pública Pedestal estende BlockBase {

    public static final AxisAlignedBB under = novo AxisAlignedBB (0,0625D, 0, 0,0625D, 0,9375D, 0,125D, 0,9375D); 
    axisAlignedBB final estattico pblico = novo AxisAlignedBB (0.1875D, 0.125D, 0.1875D, 0.8125D, 0.75D, 0.8125D); 
    axisAlignedBB final estático público top = new AxisAlignedBB (0.1875D, 0.125D, 0.1875D, 0.8125D, 0.75D, 0.8125D); 
    public static final ArrayList <AxisAlignedBB> all = novo ArrayList <AxisAlignedBB> (); 
    
    pedestal público (nome da cadeia, material material, creativetab CreativeTabs) { 
        super (nome, material, creativetab); 
        setSoundType (SoundType.STONE); 
        setHardness (3.0F); 
        setHarvestLevel ("picareta", 1); 
        setResistance (35.0F); 
    }    
    
    public BlockRenderLayer getBlockLayer () { 
        return BlockRenderLayer.CUTOUT; 
    }

    public boolean isFullCube (estado IBlockState) { 
        return false; 
    } 
    
    public boolean isOpaqueCube (estado IBlockState) { 
        return false; 
    } 
    
    @SideOnly (Side.CLIENT) 
    public void randomDisplayTick (IBlockState, stateIn, World worldIn, pos BlockPos, rand aleatório) 
    { 
        double x = (double) pos.getX (); 
        duplo y = (duplo) pos.getY () + 1,14D; 
        duplo z = (duplo) pos.getZ (); 
        
        worldIn.spawnParticle (EnumParticleTypes.SMOKE_NORMAL, x + 0,5 D, y + 0,2 D, z + 0,5 D, 0,0 D, 0,0 D, 0,0 D); 
        worldIn.spawnParticle (EnumParticleTypes.FLAME, x + 0,5 D, y, z + 0,5 D, 0,0 D, 0,0 D, 0,0 D); 
       
        worldIn.spawnParticle (EnumParticleTypes.SMOKE_NORMAL, x + 0,25 D, y + 0,2 D, z + 0,25 D, 0,0 D, 0,0 D, 0,0 D); 
        worldIn.spawnParticle (EnumParticleTypes.FLAME, x + 0,25 D, y, z + 0,25 D, 0,0 D, 0,0 D, 0,0 D); 
        
        worldIn.spawnParticle (EnumParticleTypes.SMOKE_NORMAL, x + 0,25 D, y + 0,2 D, z + 0,75 D, 0,0 D, 0,0 D, 0,0 D); 
        worldIn.spawnParticle (EnumParticleTypes.FLAME, x + 0,25 D, y, z + 0,75 D, 0,0 D, 0,0 D, 0,0 D); 
        
        worldIn.spawnParticle (EnumParticleTypes.SMOKE_NORMAL, x + 0,75 D, y + 0,2 D, z + 0,25 D, 0,0 D, 0,0 D, 0,0 D); 
        worldIn.spawnParticle (EnumParticleTypes.FLAME, x + 0,75 D, y, z + 0,25 D, 0,0 D, 0,0 D, 0,0 D);

        worldIn.spawnParticle (EnumParticleTypes.SMOKE_NORMAL, x + 0,75 D, y + 0,2 D, z + 0,75 D, 0,0 D, 0,0 D, 0,0 D); 
        worldIn.spawnParticle (EnumParticleTypes.FLAME, x + 0,75 D, y, z + 0,75 D, 0,0 D, 0,0 D, 0,0 D); 
    } 
    
    @SubscribeEvent 
    public static void drawBlockHighlightEvent (evento final DrawBlockHighlightEvent) { 
        try { 
            final EntityPlayer player = event.getPlayer (); 
            if (jogador == null) { 
                return; 
            } 
            final RayTraceResult rayTraceResult = event.getTarget (); 
            if ((rayTraceResult == null) || (rayTraceResult.typeOfHit! = RayTraceResult.Type.BLOCK)) { 
                retorno;
            } 
            final World world = player.world; 
            if (world == null) { 
                retorno; 
            } 
            final float partialTicks = event.getPartialTicks (); 
            BlockPos final pos = rayTraceResult.getBlockPos (); 
            final blockState IBlockState = world.getBlockState (pos); 
            if ((blockState.getMaterial () == Material.AIR) ||! world.getWorldBorder (). contém (pos)) { 
                retorno; 
            } 
            final Block block = blockState.getBlock (); 
            if (! (block instanceof Pedestal)) { 
                retorno; 
            }
            event.setCanceled (true); 
            all.add (sob); 
            all.add (meio); 
            all.add (em cima); 
            all.add (candle1); 
            all.add (candle2); 
            all.add (candle3); 
            all.add (candle4); 
            all.add (candle5); 
            blockState.addCollisionBoxToList (world, pos, novo AxisAlignedBB (pos), todos, player, false); 
            double renderX final = player.lastTickPosX + ((player.posX - player.lastTickPosX) * partialTicks); 
            double renderY final = player.lastTickPosY + ((player.posY - player.lastTickPosY) * partialTicks);
            double renderZ final = player.lastTickPosZ + ((player.posZ - player.lastTickPosZ) * partialTicks); 
            GlStateManager.enableBlend (); 
            GlStateManager.tryBlendFuncSeparate (GlStateManager.SourceFactor.SRC_ALPHA, GlStateManager.DestFactor.ONE_MINUS_SRC_ALPHA, GlStateManager.SourceFactor.ONE, GlStateManager.DestFactor.ZERO); 
            GlStateManager.glLineWidth (2.0F); 
            GlStateManager.disableTexture2D (); 
            GlStateManager.depthMask (false); 
            para (AxisAlignedBB box: all) { 
                if (box == Bloco.NULL_AABB) { 
                    continuar; 
                }
                final AxisAlignedBB renderBox = box.grow (0.0020000000949949026D) .offset (-renderX, -renderY, -renderZ); 
                event.getContext (). drawSelectionBoundingBox (renderBox, 0.0F, 0.0F, 0.0F, 0.4F); 
            } 
            GlStateManager.depthMask (true); 
            GlStateManager.enableTexture2D (); 
            GlStateManager.disableBlend (); 
        } catch (final Exception e) { 
            event.setCanceled (false); 
        } 
    } 
    
    public static void addCollisionBoxToList (estado IBlockState, World worldIn, pos BlockPos, AxisAlignedBB entityBox, lista <AxisAlignedBB> collidingBoxes, @Nullable Entity entityIn) {
        addCollisionBoxToList (pos, entityBox, collidingBoxes, sob); 
        addCollisionBoxToList (pos, entityBox, collidingBoxes, meio); 
        addCollisionBoxToList (pos, entityBox, collidingBoxes, em cima); 
    } 
}
