## EG1
Render a wired cube
```java
@SubscribeEvent
public static void onRenderWorldLast(RenderWorldLastEvent event)
{
    storeCommonGlStates();

    GL11.glPushMatrix();

    GlStateManager.disableCull();
    GlStateManager.enableDepth();
    GlStateManager.disableTexture2D();
    GlStateManager.disableLighting();
    GlStateManager.disableBlend();

    EntityPlayerSP player = Minecraft.getMinecraft().player;
    if (player == null) return;

    double playerX = player.lastTickPosX + (player.posX - player.lastTickPosX) * event.getPartialTicks();
    double playerY = player.lastTickPosY + (player.posY - player.lastTickPosY) * event.getPartialTicks();
    double playerZ = player.lastTickPosZ + (player.posZ - player.lastTickPosZ) * event.getPartialTicks();

    // minus camera pos and then add target pos
    // pos: 0, 100, 0
    GlStateManager.translate(-(float)(playerX), -(float)(playerY) + 100, -(float)(playerZ));
    GlStateManager.scale(10f, 10f, 10f);

    GlStateManager.glLineWidth(3.0F);

    Tessellator tessellator = Tessellator.getInstance();
    BufferBuilder buffer = tessellator.getBuffer();

    buffer.begin(GL11.GL_LINES, DefaultVertexFormats.POSITION_COLOR);

    double size = 1.0;

    buffer.pos(0, 0, 0).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();
    buffer.pos(size, 0, 0).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();
    buffer.pos(size, 0, 0).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();
    buffer.pos(size, size, 0).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();

    buffer.pos(size, size, 0).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();
    buffer.pos(0, size, 0).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();
    buffer.pos(0, size, 0).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();
    buffer.pos(0, 0, 0).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();

    buffer.pos(0, 0, size).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();
    buffer.pos(size, 0, size).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();

    buffer.pos(size, 0, size).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();
    buffer.pos(size, size, size).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();

    buffer.pos(size, size, size).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();
    buffer.pos(0, size, size).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();

    buffer.pos(0, size, size).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();
    buffer.pos(0, 0, size).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();

    buffer.pos(0, 0, 0).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();
    buffer.pos(0, 0, size).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();

    buffer.pos(size, 0, 0).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();
    buffer.pos(size, 0, size).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();

    buffer.pos(size, size, 0).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();
    buffer.pos(size, size, size).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();

    buffer.pos(0, size, 0).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();
    buffer.pos(0, size, size).color(1.0F, 1.0F, 1.0F, 1.0F).endVertex();

    tessellator.draw();

    GL11.glPopMatrix();

    restoreCommonGlStates();
}
```
![Snipaste_2025-02-02_13-58-02](https://github.com/user-attachments/assets/8bed7621-b541-4d47-9c02-afed672dd829)
