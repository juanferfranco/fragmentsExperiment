# Un píxel, varios fragmentos

Experimento en C++ y OpenGL que demuestra cómo varios fragmentos pueden contribuir al mismo píxel. Dos triángulos, uno rojo y otro verde, se superponen; la tecla **B** permite comparar reemplazo y suma de colores.

## Requisitos

- Compilador C++.
- GLFW y GLAD.
- GPU y controlador compatibles con OpenGL 4.6.

Compila y ejecuta el proyecto con estas dependencias configuradas.

## Controles

| Acción | Resultado en la zona compartida |
| --- | --- |
| Sin presionar B | Verde: el segundo triángulo reemplaza el color del primero |
| Mantener B | Amarillo: se suman rojo y verde |
| ESC | Cerrar la aplicación |

Fuera de la superposición, cada triángulo conserva su color.

## Qué demuestra

1. El **vertex shader** posiciona los vértices de cada triángulo.
2. La **rasterización** genera fragmentos para las posiciones cubiertas.
3. El **fragment shader** calcula el color de cada fragmento: rojo o verde.
4. Los resultados se escriben en el **framebuffer**, reemplazando o sumando colores según esté desactivado o activado el blending.

En la zona compartida, cada triángulo genera un fragmento para la misma posición de píxel. Con blending aditivo:

```text
Rojo (1, 0, 0) + Verde (0, 1, 0) = Amarillo (1, 1, 0)
```

**El shader no produce amarillo: ese color aparece al combinar las contribuciones después del shader.** Hay dos fragmentos y un único color final en ese píxel.

El experimento desactiva la prueba de profundidad y el multisampling. La suma usa `glBlendEquation(GL_FUNC_ADD)` y `glBlendFunc(GL_ONE, GL_ONE)`.

Referencia: [Blending — Khronos](https://wikis.khronos.org/opengl/Draw_Buffer_Blend).
