Usually, you do the rendering stuff like this.
```java
// you want to use textures so you enableTexture2D
GlStateManager.enableTexture2D();

// draw

// then you set it back
// but what if other parts of the mod are currently using the textures 
// and you just disabled that
GlStateManager.disableTexture2D();
```

In some scenarios, we store and then restore the states.
```java
boolean texture2D = GL11.glIsEnabled(GL11.GL_TEXTURE_2D);

// you want to use textures so you enableTexture2D
GlStateManager.enableTexture2D();
// and then draw

if (texture2D)
    GlStateManager.enableTexture2D();
else
    GlStateManager.disableTexture2D();
```

I use `GlStateManager` instead of direct gl calls because Minecraft may need to track state changes. 
`GlStateManager` is a wrapper class in Minecraft for gl calls.

Here are some examples you might find useful.
```java
GL11.glGetInteger(GL11.GL_TEXTURE_BINDING_2D, intBuffer);
textureID = intBuffer.get(0);
GL11.glGetFloat(GL11.GL_CURRENT_COLOR, floatBuffer);
r = floatBuffer.get(0);
g = floatBuffer.get(1);
b = floatBuffer.get(2);
a = floatBuffer.get(3);
blend = GL11.glIsEnabled(GL11.GL_BLEND);
lighting = GL11.glIsEnabled(GL11.GL_LIGHTING);
texture2D = GL11.glIsEnabled(GL11.GL_TEXTURE_2D);
alphaTest = GL11.glIsEnabled(GL11.GL_ALPHA_TEST);
GL11.glGetInteger(GL11.GL_SHADE_MODEL, intBuffer);
shadeModel = intBuffer.get(0);
depthTest = GL11.glIsEnabled(GL11.GL_DEPTH_TEST);
cullFace = GL11.glIsEnabled(GL11.GL_CULL_FACE);
```

```java
if (cullFace)
    GlStateManager.enableCull();
else
    GlStateManager.disableCull();
if (depthTest)
    GlStateManager.enableDepth();
else
    GlStateManager.disableDepth();
GlStateManager.shadeModel(shadeModel);
if (alphaTest)
    GlStateManager.enableAlpha();
else
    GlStateManager.disableAlpha();
if (texture2D)
    GlStateManager.enableTexture2D();
else
    GlStateManager.disableTexture2D();
if (lighting)
    GlStateManager.enableLighting();
else
    GlStateManager.disableLighting();
if (blend)
    GlStateManager.enableBlend();
else
    GlStateManager.disableBlend();
GlStateManager.color(r, g, b, a);
GlStateManager.bindTexture(textureID);
```
