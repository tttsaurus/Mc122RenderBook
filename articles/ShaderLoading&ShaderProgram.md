## Shaders
These three lines are exactly how we create a shader.
```java
int shaderID = GL20.glCreateShader(GL20.GL_VERTEX_SHADER or GL20.GL_FRAGMENT_SHADER);
// shaderSource is a string
GL20.glShaderSource(shaderID, shaderSource);
GL20.glCompileShader(shaderID);
```

And this tells you the status of your shader;
```java
if (GL20.glGetShaderi(shaderID, GL20.GL_COMPILE_STATUS) == GL11.GL_FALSE)
    String errorLog = GL20.glGetShaderInfoLog(shaderID, 1024);
```

However, having a `shaderID` is not enough. We want a shader program.
We will attach a vertex shader and a fragment shader to the shader program, defining the rendering pipeline.

If you are wondering what are vertex and fragment shaders, take a look at the picture below.
![The-graphics-pipeline-in-OpenGL-consists-of-these-5-steps-in-the-new-generation-of-cards](https://github.com/user-attachments/assets/77d23449-40ff-4e68-a186-23f8b70a67a1)
> source: https://www.researchgate.net/figure/The-graphics-pipeline-in-OpenGL-consists-of-these-5-steps-in-the-new-generation-of-cards_fig1_235696712

## Shader Program

