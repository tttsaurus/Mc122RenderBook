## EG1
Rotate a plane

Notice: all gl states are being handled implicitly.
```java
int x = 10, y = 10, width = 20, height = 20;

Tessellator tessellator = Tessellator.getInstance();
BufferBuilder bufferbuilder = tessellator.getBuffer();
bufferbuilder.begin(GL11.GL_QUADS, DefaultVertexFormats.POSITION_COLOR);
bufferbuilder.pos(x, y, 0).color(1f, 1f, 1f, 0.5f).endVertex();
bufferbuilder.pos(x, y + height, 0).color(1f, 1f, 1f, 0.5f).endVertex();
bufferbuilder.pos(x + width, y + height, 0).color(1f, 1f, 1f, 0.5f).endVertex();
bufferbuilder.pos(x + width, y, 0).color(1f, 1f, 1f, 0.5f).endVertex();

GlStateManager.pushMatrix();
GlStateManager.rotate(45f, 1, 1, 0);
tessellator.draw();
GlStateManager.popMatrix();
```
