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
