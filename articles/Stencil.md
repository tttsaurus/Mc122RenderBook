```java
@SubscribeEvent
public static void onRenderGameOverlay(RenderGameOverlayEvent event)
{
    if (!Minecraft.getMinecraft().getFramebuffer().isStencilEnabled())
        Minecraft.getMinecraft().getFramebuffer().enableStencil();

    GlStateManager.disableTexture2D();
    GlStateManager.disableCull();

    GL11.glEnable(GL11.GL_STENCIL_TEST);

    // can be 0 to 255
    int stencilValue = 1;
    GlStateManager.depthMask(false);
    GlStateManager.colorMask(false, false, false, false);
    GL11.glStencilFunc(GL11.GL_ALWAYS, stencilValue, 0xFF);
    GL11.glStencilOp(GL11.GL_REPLACE, GL11.GL_REPLACE, GL11.GL_REPLACE);

    // mask area
    GL11.glStencilMask(0xFF);

    int x = 0, y = 0, width = 20, height = 20;
    GL11.glBegin(GL11.GL_QUADS);
    GL11.glVertex2f(x, y);
    GL11.glVertex2f(x + width, y);
    GL11.glVertex2f(x + width, y + height);
    GL11.glVertex2f(x, y + height);
    GL11.glEnd();

    GL11.glStencilMask(0x00);

    GlStateManager.depthMask(true);
    GlStateManager.colorMask(true, true, true, true);
    // i'll change this line for the next example
    GL11.glStencilFunc(GL11.GL_EQUAL, stencilValue, 0xFF);
    GL11.glStencilOp(GL11.GL_KEEP, GL11.GL_KEEP, GL11.GL_KEEP);

    GlStateManager.enableTexture2D();
    GlStateManager.enableCull();

    // draw
    Minecraft.getMinecraft().fontRenderer.drawString("ABCDEF", 0, 0, Color.GREEN.getRGB());

    GL11.glDisable(GL11.GL_STENCIL_TEST);
}
```

***

```java
GL11.glStencilFunc(GL11.GL_NOTEQUAL, stencilValue, 0xFF);
```
