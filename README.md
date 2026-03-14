# deteccion-cascos-seguridad-obra-yolo26
Prototipo de Computer Vision para detectar colores de cascos de seguridad en obras de construcción utilizando YOLOv26, con el objetivo de identificar roles dentro de la obra.

**📌 Problema AECO**
En obras de construcción, el color del casco identifica el **rol** de cada trabajador. Identificar automáticamente qué roles están presentes en una imagen permite mejorar el monitoreo de seguridad, verificar la presencia de supervisores y detectar accesos no autorizados.

**Criterios de éxito:**
- mAP50 ≥ 0.70 sobre el conjunto de validación
- Detección confiable de las 5 clases de casco en condiciones variables de iluminación y distancia
- Pipeline reproducible 100% en la nube (Google Colab), sin instalaciones locales
