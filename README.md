# deteccion-cascos-seguridad-obra-yolo26
---
Prototipo de Computer Vision para detectar colores de cascos de seguridad en obras de construcción utilizando YOLOv26, con el objetivo de identificar roles dentro de la obra.

---

## 📌 Problema AECO

En obras de construcción, el color del casco identifica el **rol** de cada trabajador. Identificar automáticamente qué roles están presentes en una imagen permite mejorar el monitoreo de seguridad, verificar la presencia de supervisores y detectar accesos no autorizados.

**Criterios de éxito:**
- mAP50 ≥ 0.70 sobre el conjunto de validación.
- Detección de las 5 clases de casco en condiciones variables de iluminación y distancia.
- Pipeline reproducible 100% en la nube (Google Colab), sin instalaciones locales.

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
- Se etiqueta el casco completo (bounding box ajustado, sin exceso de fondo).
- Se ignoran cascos parcialmente visibles (<30% visible).
- Imágenes obtenidas de [Unsplash](https://unsplash.com).

---

## 📦 Dataset

- **Fuente:** Roboflow — dataset propio etiquetado manualmente
- **Enlace:**

```python
!pip install roboflow

from roboflow import Roboflow
rf = Roboflow(api_key="g0oBodyeNNOa3ClBHPVn")
project = rf.workspace("jonathans-workspace-lsuhr").project("m4t3-helmet-role-detection")
version = project.version(1)
dataset = version.download("yolo26")
```
                
- **Split:** 70% entrenamiento / 20% validación / 10% test
- **Formato de exportación:** YOLO26
- **Total de imágenes:** 150

---

## 🚀 Cómo Reproducir (Google Colab)

### Opción A — Ejecutar entrenamiento completo

1. Abrí el notebook desde GitHub:  
   [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/TU_USUARIO/helmet-color-detection/blob/main/notebooks/02_training_evaluation.ipynb)
