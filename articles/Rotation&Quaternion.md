## EG1
Rotate a plane

Notice: all gl states are being handled implicitly.
```java
int x = 10, y = 10, width = 20, height = 20;

Tessellator tessellator = Tessellator.getInstance();
BufferBuilder bufferbuilder = tessellator.getBuffer();
bufferbuilder.begin(GL11.GL_QUADS, DefaultVertexFormats.POSITION);
bufferbuilder.pos(x, y, 0).endVertex();
bufferbuilder.pos(x, y + height, 0).endVertex();
bufferbuilder.pos(x + width, y + height, 0).endVertex();
bufferbuilder.pos(x + width, y, 0).endVertex();

GlStateManager.pushMatrix();
GlStateManager.rotate(45f, 1, 1, 0);
tessellator.draw();
GlStateManager.popMatrix();
```
![Snipaste_2025-01-18_22-54-53](https://github.com/user-attachments/assets/d7b40cca-580b-45f2-b3c9-f8c15b3b4687)

- A transformed plane is being rendered
