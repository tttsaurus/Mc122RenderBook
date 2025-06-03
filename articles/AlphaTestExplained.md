In the fixed-function pipeline, we use alpha testing to discard pixels.
```java
GlStateManager.alphaFunc(alphaFunc, alphaRef);
```

Here are some `alphaFunc`.

| Constant     | Meaning                        |
|:-------------|:--------------------------------|
| `GL_NEVER`   | Never passes                    |
| `GL_LESS`    | Passes if `alpha < alphaRef`    |
| `GL_EQUAL`   | Passes if `alpha == alphaRef`   |
| `GL_LEQUAL`  | Passes if `alpha <= alphaRef`   |
| `GL_GREATER` | Passes if `alpha > alphaRef`    |
| `GL_NOTEQUAL`| Passes if `alpha != alphaRef`   |
| `GL_GEQUAL`  | Passes if `alpha >= alphaRef`   |
| `GL_ALWAYS`  | Always passes                   |

And of course `alphaRef` is between `0.0` and `1.0`.

When alpha testing is enabled, for each fragment (pixel):
- The pixel’s alpha value is compared with `alphaRef` using `alphaFunc`
- If it fails the test, the pixel is discarded. That is, not writing it to color buffer, no depth test, and no blend.

So, when you don't want to blend things and want a hard cutoff, you could do
```java
GlStateManager.enableAlpha();
GlStateManager.alphaFunc(GL11.GL_GREATER, 0.0f);
```

Don't forget to restore states if needed.
```java
boolean alphaTest = GL11.glIsEnabled(GL11.GL_ALPHA_TEST);
GL11.glGetInteger(GL11.GL_ALPHA_TEST_FUNC, intBuffer);
int alphaFunc = intBuffer.get(0);
GL11.glGetFloat(GL11.GL_ALPHA_TEST_REF, floatBuffer);
float alphaRef = floatBuffer.get(0);

// render

if (alphaTest)
    GlStateManager.enableAlpha();
else
    GlStateManager.disableAlpha();
GlStateManager.alphaFunc(alphaFunc, alphaRef);
```