# deteccion-cascos-seguridad-obra-yolo26
---
Prototipo de Computer Vision para detectar colores de cascos de seguridad en obras de construcción utilizando YOLOv26, con el objetivo de identificar roles dentro de la obra.

---

## 📌 Problema AECO

En obras de construcción, el color del casco identifica el **rol** de cada trabajador. Identificar automáticamente qué roles están presentes en una imagen permite mejorar el monitoreo de seguridad, verificar la presencia de supervisores y detectar accesos no autorizados.

**Criterios de éxito:**
- mAP50 ≥ 0.70 sobre el conjunto de validación
- Detección confiable de las 5 clases de casco en condiciones variables de iluminación y distancia
- Pipeline reproducible 100% en la nube (Google Colab), sin instalaciones locales

---

## 🏷️ Clases y Reglas de Etiquetado

| Clase | Color | Rol típico en obra |
|---|---|---|
| `white_helmet` | Blanco | Ingeniero / Director de obra / Visita |
| `yellow_helmet` | Amarillo | Operario general / Construcción |
| `green_helmet` | Verde | Inspector de seguridad / HSE |
| `blue_helmet` | Azul | Supervisor / Capataz |
| `red_helmet` | Rojo | Electricista / Peligro especial |

**Reglas de etiquetado:**
- Se etiqueta el casco completo (bounding box ajustado, sin exceso de fondo)
- Se ignoran cascos parcialmente visibles (<30% visible)
- Se etiqueta el color dominante visible, no el color en sombra
- Imágenes obtenidas de [Unsplash](https://unsplash.com) (licencia libre para uso académico)

---
