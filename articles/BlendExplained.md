In the fixed-function pipeline, we use blending to combine the color of a fragment (pixel) with the color already in the framebuffer.

For RGB channel:
```java
finalColor_RGB = (sourceColor_RGB * sourceColorFactor) + (destColor_RGB * destColorFactor)
```
- `sourceColor_RGB`: the color output of the fragment
- `destColor_RGB`: the existing color already stored in the framebuffer
- `sourceColorFactor` and `destColorFactor`: values that control how much each color contributes to the final result

For alpha channel:
```java
finalAlpha = (sourceAlpha * sourceAlphaFactor) + (destAlpha * destAlphaFactor)
```
- `sourceAlpha` is the alpha component of the source color
- `destAlpha` is the alpha component of the destination color
- `sourceAlphaFactor` and `destAlphaFactor`: similar factors as `sourceColorFactor` and `destColorFactor`, but we usually use `GL_ONE` or `GL_ZERO` here

You can do
```java
GlStateManager.blendFunc(sourceColorFactor, destColorFactor);
```
which is basically playing with `sourceColorFactor` and `destColorFactor`.

Use the code below if you also want to set `sourceAlphaFactor` and `destAlphaFactor`.
```java
GlStateManager.tryBlendFuncSeparate(sourceColorFactor, destColorFactor, sourceAlphaFactor, destAlphaFactor);
```

Here are some common values you’ll use for `sourceFactor` and `destFactor`:

| Constant                | Meaning                               |
|:------------------------|:--------------------------------------|
| `GL_ZERO`                | `0.0`                                 |
| `GL_ONE`                 | `1.0`                                 |
| `GL_SRC_ALPHA`           | Source fragment’s alpha value         |
| `GL_ONE_MINUS_SRC_ALPHA` | `1.0 -` Source fragment’s alpha value |
| `GL_DST_ALPHA`           | Destination alpha value               |
| `GL_ONE_MINUS_DST_ALPHA` | `1.0 -` Destination alpha value       |

Here's a common example:
```java
GlStateManager.blendFunc(GL11.GL_SRC_ALPHA, GL11.GL_ONE_MINUS_SRC_ALPHA);
```

Don't forget to restore states if needed.
```java
GL11.glGetInteger(GL14.GL_BLEND_SRC_RGB, intBuffer);
int blendSrcRgb = intBuffer.get(0);
GL11.glGetInteger(GL14.GL_BLEND_DST_RGB, intBuffer);
int blendDstRgb = intBuffer.get(0);
GL11.glGetInteger(GL14.GL_BLEND_SRC_ALPHA, intBuffer);
int blendSrcAlpha = intBuffer.get(0);
GL11.glGetInteger(GL14.GL_BLEND_DST_ALPHA, intBuffer);
int blendDstAlpha = intBuffer.get(0);

// render

GlStateManager.tryBlendFuncSeparate(blendSrcRgb, blendDstRgb, blendSrcAlpha, blendDstAlpha);
```
