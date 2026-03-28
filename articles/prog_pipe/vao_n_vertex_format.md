## VAO Reference Behavior

- VAO keeps references to all the vertex attribute configurations set while it is bound.
- You can link multiple VBOs to a VAO using different attribute indices.
- However, **only one EBO** can be bound to a VAO at a time. The VAO remembers the EBO bound at the moment of setup.

## Setup

Below is a typical setup phase for a VAO with multiple VBOs and a single EBO:

```java
vaoID = GL30.glGenVertexArrays();
GL30.glBindVertexArray(vaoID);

// bind the first VBO and configure attribute 0
// this gets recorded in the VAO
GL15.glBindBuffer(GL15.GL_ARRAY_BUFFER, vboID_0);
GL20.glVertexAttribPointer(0, ...);
GL20.glEnableVertexAttribArray(0);
GL33.glVertexAttribDivisor(0, divisor); // optional: if you want instancing

// bind the second VBO and configure attribute 1
GL15.glBindBuffer(GL15.GL_ARRAY_BUFFER, vboID_1);
GL20.glVertexAttribPointer(1, ...);
GL20.glEnableVertexAttribArray(1);
GL33.glVertexAttribDivisor(1, divisor);

// bind EBO
// this gets recorded in the VAO
GL15.glBindBuffer(GL15.GL_ELEMENT_ARRAY_BUFFER, eboID);

// VAO setup is done
// unbind if necessary
GL30.glBindVertexArray(0); // bind to 0 or the prev binding depends on your need
GL15.glBindBuffer(GL15.GL_ARRAY_BUFFER, 0);
GL15.glBindBuffer(GL15.GL_ELEMENT_ARRAY_BUFFER, 0);
```

## Draw Phase
To draw with the previously configured VAO and everything, all you need to do is re-bind the VAO:
```java
GL30.glBindVertexArray(vaoID);
GL11.glDrawElements(...); // or any other variant draw calls
```

> **Notice:**
> - `DrawArrays` series draw calls don't require an EBO to guide the drawing process.
> - `DrawElements` series draw calls do require an EBO for drawing.
> - It's preferred to use `DrawElements` when vertex reuse is necessarily, which is most of the scenarios.

For `GL11.glDrawElements(GL11.GL_TRIANGLES, count, GL11.GL_UNSIGNED_INT, offset)`
- `GL_TRIANGLES` represents the primitive type
- `count` represents the number of indices
- `GL11.GL_UNSIGNED_INT` represents how GPU interprets the EBO
  (e.g. every index is a 32-bit unsigned integer; you could use short or byte too)
- `offset` represents the reading offset of the EBO in bytes
