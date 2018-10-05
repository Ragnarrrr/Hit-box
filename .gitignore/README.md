import java.util.ArrayList;
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
import net.minecraftforge.fml.common.Mod;
import net.minecraftforge.fml.common.eventhandler.SubscribeEvent;
import net.minecraftforge.fml.relauncher.Side;
import net.minecraftforge.fml.relauncher.SideOnly;

@Mod.EventBusSubscriber
public class Pedestal extends BlockBase{

	public static final AxisAlignedBB under = new AxisAlignedBB(0.0625D, 0, 0.0625D, 0.9375D, 0.125D, 0.9375D);
	public static final AxisAlignedBB middle = new AxisAlignedBB(0.1875D, 0.125D, 0.1875D, 0.8125D, 0.75D, 0.8125D);
	public static final AxisAlignedBB top = new AxisAlignedBB(0.1875D, 0.125D, 0.1875D, 0.8125D, 0.75D, 0.8125D);
	public static final ArrayList<AxisAlignedBB> all = new ArrayList<AxisAlignedBB>();
	
	public Pedestal(String name, Material material, CreativeTabs creativetab) {
		super(name, material, creativetab);
		setSoundType(SoundType.STONE);
		setHardness(3.0F);
		setHarvestLevel("pickaxe", 1);
		setResistance(35.0F);
	}	
	
    public BlockRenderLayer getBlockLayer() {
        return BlockRenderLayer.CUTOUT;
    }

    public boolean isFullCube(IBlockState state) {
        return false;
    }
    
    public boolean isOpaqueCube(IBlockState state) {
        return false;
    }
    
    @SideOnly(Side.CLIENT)
    public void randomDisplayTick(IBlockState stateIn, World worldIn, BlockPos pos, Random rand)
    {
        double x = (double)pos.getX();
        double y = (double)pos.getY() + 1.14D;
        double z = (double)pos.getZ();
        
        worldIn.spawnParticle(EnumParticleTypes.SMOKE_NORMAL, x + 0.5D, y + 0.2D, z + 0.5D , 0.0D, 0.0D, 0.0D);
        worldIn.spawnParticle(EnumParticleTypes.FLAME, x + 0.5D, y, z + 0.5D, 0.0D, 0.0D, 0.0D); 
       
        worldIn.spawnParticle(EnumParticleTypes.SMOKE_NORMAL, x + 0.25D, y + 0.2D, z + 0.25D , 0.0D, 0.0D, 0.0D);
        worldIn.spawnParticle(EnumParticleTypes.FLAME, x + 0.25D, y, z + 0.25D, 0.0D, 0.0D, 0.0D);
        
        worldIn.spawnParticle(EnumParticleTypes.SMOKE_NORMAL, x + 0.25D, y + 0.2D, z + 0.75D , 0.0D, 0.0D, 0.0D);
        worldIn.spawnParticle(EnumParticleTypes.FLAME, x + 0.25D, y, z + 0.75D, 0.0D, 0.0D, 0.0D);
        
        worldIn.spawnParticle(EnumParticleTypes.SMOKE_NORMAL, x + 0.75D, y + 0.2D, z + 0.25D , 0.0D, 0.0D, 0.0D);
        worldIn.spawnParticle(EnumParticleTypes.FLAME, x + 0.75D, y, z + 0.25D, 0.0D, 0.0D, 0.0D);

        worldIn.spawnParticle(EnumParticleTypes.SMOKE_NORMAL, x + 0.75D, y + 0.2D, z + 0.75D , 0.0D, 0.0D, 0.0D);
        worldIn.spawnParticle(EnumParticleTypes.FLAME, x + 0.75D, y, z + 0.75D, 0.0D, 0.0D, 0.0D);
    }
    
	@SubscribeEvent
	public static void drawBlockHighlightEvent(final DrawBlockHighlightEvent event) {
			final EntityPlayer player = event.getPlayer();
			if (player == null) {
				return;
			}
			final RayTraceResult rayTraceResult = event.getTarget();
			if ((rayTraceResult == null) || (rayTraceResult.typeOfHit != RayTraceResult.Type.BLOCK)) {
				return;
			}
			final World world = player.world;
			if (world == null) {
				return;
			}
			final float partialTicks = event.getPartialTicks();
			final BlockPos pos = rayTraceResult.getBlockPos();
			final IBlockState blockState = world.getBlockState(pos);
			if ((blockState.getMaterial() == Material.AIR) || !world.getWorldBorder().contains(pos)) {
				return;
			}
			final Block block = blockState.getBlock();
			if (!(block instanceof Pedestal)) {
				return;
			}
			all.add(under);
			all.add(middle);
			all.add(top);
			event.setCanceled(true);			
			blockState.addCollisionBoxToList(world, pos, new AxisAlignedBB(pos), all, player, false);
			final double renderX = player.lastTickPosX + ((player.posX - player.lastTickPosX) * partialTicks);
			final double renderY = player.lastTickPosY + ((player.posY - player.lastTickPosY) * partialTicks);
			final double renderZ = player.lastTickPosZ + ((player.posZ - player.lastTickPosZ) * partialTicks);
			GlStateManager.enableBlend();
			GlStateManager.tryBlendFuncSeparate(GlStateManager.SourceFactor.SRC_ALPHA, GlStateManager.DestFactor.ONE_MINUS_SRC_ALPHA, GlStateManager.SourceFactor.ONE, GlStateManager.DestFactor.ZERO);
			GlStateManager.glLineWidth(2.0F);
			GlStateManager.disableTexture2D();
			GlStateManager.depthMask(false);
			for (AxisAlignedBB box : all) {
				if (box==Block.NULL_AABB) {
					continue;
				}
				final AxisAlignedBB renderBox = box.grow(0.0020000000949949026D).offset(-renderX, -renderY, -renderZ);
				event.getContext().drawSelectionBoundingBox(renderBox, 0.0F, 0.0F, 0.0F, 0.4F);
			}
			GlStateManager.depthMask(true);
			GlStateManager.enableTexture2D();
			GlStateManager.disableBlend();
	}
}
