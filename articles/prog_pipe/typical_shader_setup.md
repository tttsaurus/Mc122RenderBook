Here's a typical terrain rendering shader setup for Minecraft.

> **Notice**: 
> `viewRot` is essentially a `modelView` matrix but Minecraft implements the position offset via `position - camPos`
> and `modelView` is merely used to express the camera rotation.

**VS**
```glsl
#version 330 core

layout(location = 0) in vec3 position;
layout(location = 1) in vec4 color;
layout(location = 2) in vec2 texCoord0;
layout(location = 3) in vec2 texCoord1;

out vec2 TexCoord0;
out vec2 TexCoord1;
out vec4 Color;

uniform vec3 camPos;
uniform vec3 offset;

uniform mat4 viewRot;
uniform mat4 projection;

void main()
{
    gl_Position = projection * viewRot * vec4(position - camPos + offset, 1.0);
    TexCoord0 = texCoord0;
    TexCoord1 = texCoord1;
    Color = color;
}
```

**FS**
```glsl
#version 330 core

in vec2 TexCoord0;
in vec2 TexCoord1;
in vec4 Color;

out vec4 FragColor;

uniform sampler2D tex;
uniform sampler2D lightmap;

void main(void)
{
    vec4 baseColor = texture(tex, TexCoord0);
    vec4 lightColor = texture(lightmap, (TexCoord1 + vec2(8.0, 8.0)) / vec2(256.0, 256.0));
    FragColor = baseColor * Color * lightColor;
}
```

**Render Setup**
```java
// pseudocode
program.use();
program.setUniform("modelView", MinecraftRenderUtils.getModelViewMatrix());
program.setUniform("projection", MinecraftRenderUtils.getProjectionMatrix());
program.setUniform("tex", 0); // meaning: pass the current bounded texture at texture unit 0
program.setUniform("lightmap", 1); // meaning: pass the current bounded texture at texture unit 1
program.setUniform("camPos",
        MinecraftRenderUtils.getWorldOffset().x, 
        MinecraftRenderUtils.getWorldOffset().y, 
        MinecraftRenderUtils.getWorldOffset().z);

program.setUniform("offset", renderChunk.getPosition().getX(), renderChunk.getPosition().getY(), renderChunk.getPosition().getZ());
mesh.render();

program.unuse();
```

> **Notice:**
> You might want to check out the concept of texture unit.
> They are like slots and every slot can have a bounded texture.

**Vertex Attribute**
```java
// pseudocode
BLOCK_ATTRIBUTE_LAYOUT.push(new Stride(28)
        .push(new Slot(Type.FLOAT, 3))
        .push(new Slot(Type.UNSIGNED_BYTE, 4).setNormalize(true))
        .push(new Slot(Type.FLOAT, 2))
        .push(new Slot(Type.SHORT, 2)));
```

**Camera Info Fetching**
```java
public Vector3f getWorldOffset()
{
    // Do not plus eye height!!! This is why the function is named "getWorldOffset"
    Entity camera = Minecraft.getMinecraft().getRenderViewEntity();
    if (camera == null) camera = MethodHolder.getPlayer(Minecraft.getMinecraft());
    double partialTicks = getPartialTicks();
    double camX = camera.lastTickPosX + (camera.posX - camera.lastTickPosX) * partialTicks;
    double camY = camera.lastTickPosY + (camera.posY - camera.lastTickPosY) * partialTicks;
    double camZ = camera.lastTickPosZ + (camera.posZ - camera.lastTickPosZ) * partialTicks;
    return new Vector3f((float)camX, (float)camY, (float)camZ);
}
```
```java
// this code snippet is from Minecraft class ActiveRenderInfo
// all you need to do is accessing MODELVIEW and PROJECTION via reflection
private static final FloatBuffer MODELVIEW = GLAllocation.createDirectFloatBuffer(16); // this is for viewRot
private static final FloatBuffer PROJECTION = GLAllocation.createDirectFloatBuffer(16); // this is for projection
```
