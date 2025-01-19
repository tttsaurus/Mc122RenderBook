## EG1
Notice: gl state leakage is handled implicitly.
```java
GlStateManager.disableTexture2D();
GlStateManager.enableBlend();
GlStateManager.disableAlpha();
GlStateManager.tryBlendFuncSeparate(GlStateManager.SourceFactor.SRC_ALPHA, GlStateManager.DestFactor.ONE_MINUS_SRC_ALPHA, GlStateManager.SourceFactor.ONE, GlStateManager.DestFactor.ZERO);
GlStateManager.disableCull();

int x = 10, y = 10, width = 20, height = 20;

GL11.glBegin(GL11.GL_QUADS);
GL11.glVertex2f(x, y);
GL11.glVertex2f(x + width, y);
GL11.glVertex2f(x + width, y + height);
GL11.glVertex2f(x, y + height);
GL11.glEnd();
```

- A white square is being drawn

***

## EG2
```java
GlStateManager.disableTexture2D();
GlStateManager.enableBlend();
GlStateManager.disableAlpha();
GlStateManager.tryBlendFuncSeparate(GlStateManager.SourceFactor.SRC_ALPHA, GlStateManager.DestFactor.ONE_MINUS_SRC_ALPHA, GlStateManager.SourceFactor.ONE, GlStateManager.DestFactor.ZERO);
GlStateManager.disableCull();

int x = 10, y = 10, width = 20, height = 20;
        
Tessellator tessellator = Tessellator.getInstance();
BufferBuilder bufferbuilder = tessellator.getBuffer();
bufferbuilder.begin(GL11.GL_QUADS, DefaultVertexFormats.POSITION);
bufferbuilder.pos(x, y, 0).endVertex();
bufferbuilder.pos(x + width, y, 0).endVertex();
bufferbuilder.pos(x + width, y + height, 0).endVertex();
bufferbuilder.pos(x, y + height, 0).endVertex();
tessellator.draw();
```

- This example is totally equivalent to the first one
- But we use the Minecraft class `Tessellator` instead
- It's recommended to use `Tessellator` & `BufferBuilder` instead

