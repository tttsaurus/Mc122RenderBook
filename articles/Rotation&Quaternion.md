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

When it comes to quaternion, it's actually equivalent to do so
```java
GlStateManager.rotate(new Quaternion(
                (float)Math.sin(Math.PI / 4d / 2d) / (float)Math.sqrt(2),
                (float)Math.sin(Math.PI / 4d / 2d) / (float)Math.sqrt(2),
                0f,
                (float)Math.cos(Math.PI / 4d / 4d)));
```
<br>

### Intro to Quaternion

Quaternion _q_ = [_w_, _u_] where _w_ ∈ ℝ and _u_ ∈ ℝ^3 which is a vector

equivalently, we write

_q_ = _w_ + _a_ **_i_** + _b_ **_j_** + _c_ **_k_** where _w_, _a_, _b_, _c_ ∈ ℝ, and **_i_**, **_j_**, **_k_** are imaginary numbers <br>where

<table>
  <tr>
    <td>
      <table>
        <tr>
          <th>i</th>
          <th>0</th>
          <th>0</th>
        </tr>
        <tr>
          <td>0</td>
          <td>j</td>
          <td>0</td>
        </tr>
        <tr>
          <td>0</td>
          <td>0</td>
          <td>k</td>
        </tr>
      </table>
    </td>
    <td>
      *
    </td>
    <td>
      <table>
        <tr>
          <th>i</th>
          <th>j</th>
          <th>k</th>
        </tr>
        <tr>
          <th>i</th>
          <th>j</th>
          <th>k</th>
        </tr>
        <tr>
          <th>i</th>
          <th>j</th>
          <th>k</th>
        </tr>
      </table>
    </td>
    <td>
      =
    </td>
    <td>
      <table>
        <tr>
          <th>-1</th>
          <th>k</th>
          <th>-j</th>
        </tr>
        <tr>
          <th>-k</th>
          <th>-1</th>
          <th>i</th>
        </tr>
        <tr>
          <th>j</th>
          <th>-i</th>
          <th>-1</th>
        </tr>
      </table>
    </td>
  </tr>
</table>

and yeah _u_ = (_a_, _b_, _c_)^T

When _u_ is our rotation axis and _θ_ is our rotation angle (in radian), we write that quaternion _q_ = [_cos θ/2_, _normalized u_ * _sin θ/2_]


### How It Works

Let's say we have a point _p_ = (x0, y0, z0)^T and define _q_ = [_cos θ/2_, _normalized u_ * _sin θ/2_]

Consider _P_ = [0, _p_]

Then _P'_ = [0，_p'_] = _q_ * _P_ * _q^-1_ = _q_ * _P_ * _q*_ where _p'_ is the rotated point

= [_cos θ/2_, _normalized u_ * _sin θ/2_] * [0, _p_] * [_cos θ/2_, - _normalized u_ * _sin θ/2_]

We can treat them as imaginary numbers and do the multiplication, or use Hamilton product.
