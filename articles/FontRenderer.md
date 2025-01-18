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

```java
FloatBuffer floatBuffer = ByteBuffer.allocateDirect(16 << 2).order(ByteOrder.nativeOrder()).asFloatBuffer();

GL11.glGetFloat(GL11.GL_CURRENT_COLOR, floatBuffer);
r = floatBuffer.get(0);
g = floatBuffer.get(1);
b = floatBuffer.get(2);
a = floatBuffer.get(3);
        
Minecraft.getMinecraft().fontRenderer.drawString("test", 0, 0, Color.RED.getRGB());

GlStateManager.color(r, g, b, a);
```
![Snipaste_2025-01-17_22-05-05](https://github.com/user-attachments/assets/4e1f492e-bcbe-4173-b1eb-588004763fd0)

- Avoid the state leakage by recording the value and restoring it

***

```java
Minecraft.getMinecraft().fontRenderer.drawString("test", 10, 0, Color.RED.getRGB());
```
![Snipaste_2025-01-17_22-11-01](https://github.com/user-attachments/assets/7ea812b4-23f4-4fa1-bf35-9403b353a06a)

- X-axis is like our normal x-axis

***

```java
Minecraft.getMinecraft().fontRenderer.drawString("test", 0, 10, Color.RED.getRGB());
```
![Snipaste_2025-01-17_22-13-13](https://github.com/user-attachments/assets/f3b16f41-6482-4b44-87f6-6c4180aa03bb)

- Y-axis is flipped
