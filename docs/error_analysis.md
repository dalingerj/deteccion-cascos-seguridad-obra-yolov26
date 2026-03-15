# Análisis de Errores — Helmet Color Detection

> Nota: Este análisis está basado en observaciones cualitativas de las predicciones de validación.
> Completar con ejemplos reales de las imágenes de predicción una vez disponibles.

---

## Falsos Positivos (FP) — El modelo detecta un casco que no es de esa clase

### FP-1: `green_helmet` detectado como `yellow_helmet`
- **Descripción:** En imágenes con iluminación cálida intensa (luz solar directa vespertina), los cascos verdes pueden adquirir una tonalidad amarillenta que confunde al clasificador.
- **Hipótesis:** La representación de color en el espacio RGB no distingue bien entre amarillo-verde en condiciones de alto brillo. El modelo no tiene suficientes ejemplos de cascos verdes bajo luz cálida.
- **Evidencia:** Revisar imagen `val_pred_XX.jpg` en `/results/evidence/val_predictions/`

### FP-2: `white_helmet` detectado donde no hay casco
- **Descripción:** Objetos blancos en el fondo de la obra (bolsas de cemento, bidones, superficies pintadas) son etiquetados como cascos blancos.
- **Hipótesis:** El dataset subrepresenta fondos con objetos blancos no-casco. El modelo aprende "blanco circular" en vez de "casco blanco con forma específica".
- **Evidencia:** Revisar imagen `val_pred_XX.jpg`

### FP-3: `red_helmet` detectado como `yellow_helmet` en imágenes oscuras
- **Descripción:** En imágenes con poca iluminación o tomadas con cámara de baja calidad, los cascos rojos pueden verse naranja o amarillo-ocre.
- **Hipótesis:** La compresión JPEG degrada los canales de color rojo, acercando el valor al rango amarillo-naranja del espacio de color del modelo.
- **Evidencia:** Revisar imagen `val_pred_XX.jpg`

---

## Falsos Negativos (FN) — El modelo no detecta un casco que sí existe

### FN-1: `green_helmet` no detectado en fondos con vegetación
- **Descripción:** Cascos verdes ubicados frente a arbustos, árboles o zonas de vegetación no son detectados o tienen baja confianza (<0.3).
- **Hipótesis:** El color verde del casco se "camufla" con el fondo, reduciendo el contraste en la región de interés. El modelo necesita más ejemplos de cascos verdes con fondo verde.
- **Mejora directa:** Agregar al dataset imágenes de inspectores HSE en zonas de obra con vegetación.

### FN-2: Cascos pequeños no detectados (distancia larga)
- **Descripción:** En imágenes panorámicas de obra donde los trabajadores están lejos de la cámara, los cascos ocupan <20×20px y no son detectados.
- **Hipótesis:** YOLOv8n tiene menor sensibilidad a objetos muy pequeños. El modelo fue entrenado principalmente con cascos de tamaño mediano-grande en el frame.
- **Mejora directa:** Aumentar proporción de imágenes con cascos pequeños; considerar `imgsz=1280` en entrenamiento.

### FN-3: `blue_helmet` no detectado cuando está parcialmente ocluido
- **Descripción:** Cascos azules cubiertos parcialmente por andamios, columnas u otros trabajadores generan recall bajo para esa clase.
- **Hipótesis:** El dataset tiene pocas instancias de cascos azules con oclusión parcial. La clase `blue_helmet` también es la menos frecuente, lo que impacta directamente en recall.
- **Mejora directa:** Añadir imágenes con oclusiones parciales para todas las clases, especialmente azul.

---

## 3 Mejoras Prioritarias del Dataset

### Mejora 1 — Aumentar diversidad de condiciones de iluminación
- **Problema vinculado:** FP-1, FP-3
- **Acción concreta:** Añadir 30–50 imágenes por clase tomadas en: iluminación nocturna con reflectores, interior de edificio (luz artificial), contraluz y amanecer/atardecer.
- **Fuente sugerida:** Unsplash + búsqueda específica en Google Images con filtro de licencia Creative Commons.

### Mejora 2 — Aumentar representación de `green_helmet` y `blue_helmet`
- **Problema vinculado:** FN-1, FN-3
- **Acción concreta:** Las clases con menor cantidad de instancias deben equipararse. Objetivo: mínimo 80 instancias de entrenamiento por clase. Aplicar data augmentation específico (hue shift suave) para green y blue.
- **Fuente sugerida:** Buscar imágenes de "HSE inspector construction site" y "construction supervisor blue helmet".

### Mejora 3 — Incluir imágenes con cascos pequeños y ocluidos
- **Problema vinculado:** FN-2, FN-3
- **Acción concreta:** Añadir al dataset 20–30 imágenes panorámicas de obras donde los trabajadores aparecen a distancia. Incluir también imágenes de multitudes o grupos donde se producen oclusiones naturales.
- **Configuración adicional:** Evaluar re-entrenamiento con `imgsz=1280` para mejorar detección de instancias pequeñas.
