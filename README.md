# 🧪 Proyecto de Detección y Análisis de Líquidos en Tiempo Real

Este proyecto utiliza **YOLOv11** junto con **OpenCV** y **SORT** para detectar recipientes con líquidos en tiempo real desde una cámara, identificar el color del líquido, estimar el porcentaje de llenado y clasificar su peligrosidad. Toda la información se visualiza en pantalla y se guarda en un archivo `.txt`.

---

## 📸 Funcionalidades

- Detección en tiempo real de:
  - Recipientes (`Bottle`, `Cup`, `Glass`)
  - Líquidos
- Estimación del **porcentaje de llenado** en cada recipiente
- Cálculo del **color promedio** del líquido
- Clasificación de **peligrosidad** según el color
- Grabación de la demostración en video con `cv2.VideoWriter`
- Registro detallado en `detecciones.txt`

---

## 🔧 Requisitos

- Python 3.8+
- Librerías:
  - `ultralytics`
  - `opencv-python`
  - `numpy`
  - `sort` (implementación del tracker)

Puedes instalarlas con:

```bash
pip install ultralytics opencv-python numpy
```

---

## 🧠 Modelo

- El modelo YOLOv11 fue entrenado en Roboflow para detectar:
  - Recipientes: `Bottle, Cup, Glass`
  - `Liquid`: región del líquido en el recipiente

Archivo del modelo: `best.pt`

---

## 🎥 Cómo usar

1. 📷 Asegúrate de tener una cámara conectada.
2. ▶️ Ejecuta el script principal: `app.py`.
3. ❌ Presiona `q` para finalizar la detección.
4. 💾 Al finalizar, se guardarán los siguientes archivos:
   - `detecciones.txt`: contiene el resumen de las detecciones.
   - `demo_output.avi`: grabación del análisis en tiempo real.

---

## 📁 Estructura
```
📦 Análisis y Detección de Líquidos
 ┣ 📁 app
 ┣ 📁 datasets
 ┣ 📁 runs
 ┣ 📜 train-yolo11-instance-segmentation-on-custom-dataset
 ┣ 📜 yolo11l-seg.pt
 ┗ 📜 yolo11n.pt
 ┗ 📜 yolo11s-seg.pt
```

Dentro de `app` se encuentra `app.py` que es la ejecución de la aplicación. Adicionalmente se encuentra `best.pt` que es el modelo entrenado, `detecciones.txt` y finalmente una demo en video.

Dentro de `datasets` se encuentran las carpetas `train`, `valid`, `test`, que se encargaron de distribuir las imágenes en las distintas fases del entrenamiento y posterior validación del modelo.

Dentro de `runs` se encuentran `predict2`, `train`, `valid`. En `predict2` están las predicciones hechas. En `train` se encuentran los archivos generados del entrenamiento con respectivos pesos y finalmente en `valid` se encuentran una serie de métricas y gráficas asociadas a la validación del modelo.

Cabe destacar que estas carpetas son generadas por el notebook automático de Roboflow: `train-yolo11-instance-segmentation-on-custom-dataset`. En él se encuentra todo el trabajo de código: importación de datos, entrenamiento con 20 épocas, matriz de confusión, etc.

---

# 📊 Resultados y Análisis

## Análisis de la Matriz de Confusión

- **Desempeño por clase:**
  - **Glass (Vaso)**: Mejor rendimiento con 96% de precisión
  - **Bottle (Botella)**: Buen rendimiento con 90% de precisión
  - **Liquid (Líquido)**: Rendimiento aceptable con 86% de precisión
  - **Cup (Taza)**: Rendimiento más bajo con 72% de precisión

- **Principales confusiones:**
  - Confusión significativa entre Liquid y Background (43%)
  - Confusión moderada entre Cup y Background (14%)

## Curvas de Precisión-Recall

- La clase Glass mantiene el mejor equilibrio entre precisión y recall (AUC 0.987)
- Bottle muestra buen desempeño general (AUC 0.933)
- Cup y Liquid presentan caídas más pronunciadas en precisión al aumentar el recall
- El modelo global alcanza un mAP@0.5 de 0.875, indicando buen rendimiento general

## Análisis de F1-Score

- El punto óptimo de equilibrio entre precisión y recall se alcanza con un umbral de confianza de 0.520
- Glass mantiene F1-Scores altos a través de diferentes umbrales de confianza
- Cup es la clase más sensible a cambios en el umbral de confianza

## Recomendaciones de Mejora

1. **Para la clase Cup**: Aumentar datos de entrenamiento o aplicar técnicas de data augmentation específicas
2. **Para la confusión Liquid-Background**: Mejorar la segmentación de líquidos con características adicionales de textura y transparencia
3. **Optimización de umbrales**: Utilizar 0.520 como umbral de confianza para un equilibrio óptimo entre precisión y recall
4. **Manejo de clases desbalanceadas**: Si existe desbalance, considerar técnicas de oversampling, undersampling o ajuste de pesos

El modelo presenta un buen desempeño global con un F1-Score de 0.85, pero tiene margen de mejora en la detección de tazas y en la distinción entre líquidos y fondo.

---

## 🧪 Notas

- El análisis depende de la calidad de la detección y puede no ser perfecto en todos los casos.
- El color se determina por el promedio de pixeles en la región del líquido.
- Este sistema se desarrolló como parte de un proyecto académico.

🔗 [Accede aquí a la carpeta en Drive](https://drive.google.com/drive/folders/1eh-tDIGAUCd1ms8JWlKoMbtMFhKTm4XF?usp=sharing)

🔗 [Accede aquí al dataset en Roboflow](https://universe.roboflow.com/franziskos-workspace/deteccion-y-analisis-de-liquidos/browse?queryText=&pageSize=50&startingIndex=0&browseQuery=true)
