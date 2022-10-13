# IconAnimationTechniques
## Drawing Path

VectorDrawable : Allow us to create scalable, density independent assets by representing each icon as a series of lines and shape called ***paths***.

Each path's shape is determined by a sequence of ***drawing commands***, represented by a **space/comma-separated** string using asubset of the [SVG path data spec](https://www.w3.org/TR/SVG11/paths.html#PathData).

### Summarized spec commands

| Command                 | Description                                                                                                                           |
| ------------------------| --------------------------------------------------------------------------------------------------------------------------------------|
| M x,y                   | Begin a new subpath by moving to (x,y)                                                                                                |
| L x,y                   | Draw a line to (x,y)                                                                                                                  |
| C x1,y1,x2,y2,x,y       | Draw a cubic [bezier curve](https://fr.wikipedia.org/wiki/Courbe_de_B%C3%A9zier) to (x,y) using control points (x1,y1) and (x2,y2)    |
| Z                       | Close the path by drawing a line back to the beginning of the current subpath                                                         |


All paths come in one of two forms: **filled** or **stroked**.
If the path is filled, the interiors of its shape will be painted.
If the path is stroked, the paint will be applied along the outline of its shape.

Both types of paths have their own set of animatable attributes that further modify their appearance.

| Property name           | Element type | Value type | Min |  Max  |
| ------------------------| -------------|------------|-----|------ |
| android:fillAlhpa       | \<path>      |float       |0    |1      |
| android:fillColor       | \<path>      |integer     |---  |---    |
| android:strokeAlpha     | \<path>      |float       |0    |1      |
| android:strokeColor     | \<path>      |integer     |---  |---    |
| android:strokeWidth     | \<path>      |float       |0    |---    |

#### Example : Play, pause, record icon for music application
We can represent each icon using a single path

```
  <vector
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:width="48dp"
  android:height="48dp"
  android:viewportHeight="12"
  android:viewportWidth="12">

  <!-- This path draws an orange triangular play icon. -->
  <path
    android:fillColor="#FF9800"
    android:pathData="M 4,2.5 L 4,9.5 L 9.5,6 Z"/>

  <!-- This path draws two green stroked vertical pause bars. -->
  <path
    android:pathData="M 4,2.5 L 4,9.5 M 8,2.5 L 8,9.5"
    android:strokeColor="#0F9D58"
    android:strokeWidth="2"/>

  <!-- This path draws a red circle. -->
  <path
    android:fillColor="#DB4437"
    android:pathData="M 2,6 C 2,3.8 3.8,2 6,2 C 8.2,2 10,3.8 10,6 C 10,8.2 8.2,10 6,10 C 3.8,10 2,8.2 2,6"/>

</vector>
```
![image](https://user-images.githubusercontent.com/23448445/195714203-a328aa7d-63e2-4038-90ae-8fa522719973.png)

We can animate their individual paths using the ***AnimatedVectorDrawable*** class.
***AnimatedVectorDrawables*** are the glue that connect ***VectorDrawables*** with ***ObjectAnimators***. 
The ***VectorDrawables*** assigns each animated path (or groupe of paths) a unique name, and the ***AnimatedVectorDrawable*** **map** each of thses names to their corresponding ***ObjectAnimators***.

## Transforming groups of paths

