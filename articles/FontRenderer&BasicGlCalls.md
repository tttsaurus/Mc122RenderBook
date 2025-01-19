## EG1
Render a text
```java
@SubscribeEvent
public static void onRenderGameOverlay(RenderGameOverlayEvent event)
{
    Minecraft.getMinecraft().fontRenderer.drawString("test", 0, 0, Color.RED.getRGB());
}
```
![Snipaste_2025-01-17_21-57-14](https://github.com/user-attachments/assets/f1e94383-8c6e-4cd3-8805-f9884385caa8)

- The default rendering pivot is the top-left corner
- Gl is a state machine so the color change affects the crosshair

***

## EG2
Render a text with state leakage handled
```java
FloatBuffer floatBuffer = ByteBuffer.allocateDirect(16 << 2).order(ByteOrder.nativeOrder()).asFloatBuffer();

GL11.glGetFloat(GL11.GL_CURRENT_COLOR, floatBuffer);
float r = floatBuffer.get(0);
float g = floatBuffer.get(1);
float b = floatBuffer.get(2);
float a = floatBuffer.get(3);
        
Minecraft.getMinecraft().fontRenderer.drawString("test", 0, 0, Color.RED.getRGB());

GlStateManager.color(r, g, b, a);
```
![Snipaste_2025-01-17_22-05-05](https://github.com/user-attachments/assets/4e1f492e-bcbe-4173-b1eb-588004763fd0)

- Avoid the state leakage by recording the value and restoring it

***

## EG3
Changing x
```java
Minecraft.getMinecraft().fontRenderer.drawString("test", 10, 0, Color.RED.getRGB());
```
![Snipaste_2025-01-17_22-11-01](https://github.com/user-attachments/assets/7ea812b4-23f4-4fa1-bf35-9403b353a06a)

- X-axis is like our normal x-axis

***

## EG4
Changing y
```java
Minecraft.getMinecraft().fontRenderer.drawString("test", 0, 10, Color.RED.getRGB());
```
![Snipaste_2025-01-17_22-13-13](https://github.com/user-attachments/assets/f3b16f41-6482-4b44-87f6-6c4180aa03bb)

- Y-axis is flipped

***

## EG5
Transformation
```java
// push the transformation matrix
GlStateManager.pushMatrix();
GlStateManager.translate(20, 20, 0);
GlStateManager.scale(2, 2, 0);
Minecraft.getMinecraft().fontRenderer.drawString("test", 0, 0, Color.RED.getRGB());
// pop the transformation matrix
GlStateManager.popMatrix();
```

![Snipaste_2025-01-18_01-38-55](https://github.com/user-attachments/assets/81e69100-d2db-4a7b-970a-fe862beae427)

- We want to use `pushMatrix` & `popMatrix` to define the scope of transformation first
- And this is how `scale` & `translate` work

***

## EG6
Transformation order
```java
GlStateManager.pushMatrix();
// notice the order of `scale` and `translate`
GlStateManager.scale(2, 2, 0);
GlStateManager.translate(20, 20, 0);
Minecraft.getMinecraft().fontRenderer.drawString("test", 0, 0, Color.RED.getRGB());
GlStateManager.popMatrix();
```

![Snipaste_2025-01-18_01-42-01](https://github.com/user-attachments/assets/0da12f25-363c-43c4-8ab3-3889e65bf84c)

- The `test` moves further because we apply the `x2 scale` first
- Be careful with the order of `scale` calls

