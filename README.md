# Explainable AI aplicada a mantenimiento predictivo

**¿Por qué el modelo dice que esta máquina va a fallar?**

Ejemplo computacional de la exposición *Explainable AI* — Aprendizaje Automático, MCDA 2026-2, Universidad EAFIT (profesor Andrés Vásquez Restrepo).



[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/tomasguzman0925-ship-it/AXL/blob/main/XAI_mantenimiento_predictivo.ipynb)

## De qué trata

Entrenamos un modelo de caja negra (XGBoost) que predice fallas en una máquina de mecanizado y lo explicamos con cuatro técnicas de XAI: **importancia por permutación**, **PDP/ICE**, **SHAP** y **LIME**.

El dataset (AI4I 2020) es sintético y sus fallas se generaron con **reglas físicas documentadas**. Eso permite hacer algo poco habitual: **comprobar si las explicaciones coinciden con la causa real** de cada falla.

## Qué encontramos

| Pregunta | Resultado |
|---|---|
| ¿Alcanza con un modelo transparente? | No con las variables originales: la regresión logística detecta 12 de 85 fallas y un árbol pequeño 16 de 85. |
| ¿Cuánto mejora la caja negra? | XGBoost detecta 55 de 85 fallas (F1 = 0,74). |
| ¿Las explicaciones dicen la verdad? | En buena medida: la variable principal según SHAP es una causa física real en el 89 % al 100 % de las fallas, según el tipo. |
| ¿Qué riesgos tiene XAI? | Variables correlacionadas que reparten un solo fenómeno, LIME inestable y poco fiel (R² local cercano a 0,1), y explicaciones convincentes de predicciones equivocadas. |
| ¿Se puede evitar la caja negra? | Aquí sí: con tres variables físicas sugeridas por las explicaciones, un árbol de 10 reglas alcanza F1 = 0,88 y redescubre los umbrales reales. |

![Lo que aprendió el modelo frente a la física](figuras/13_mapa_riesgo_vs_fisica.png)

*Cada punto es una máquina del conjunto de prueba. El color es el aporte SHAP conjunto de las dos variables de cada panel; las líneas punteadas son las fronteras físicas con las que se generaron las fallas. El modelo nunca vio esas reglas.*

## Contenido del repositorio

```
├── XAI_mantenimiento_predictivo.ipynb   Notebook principal, ejecutado y con explicaciones paso a paso
├── data/ai4i2020.csv                    Dataset AI4I 2020 (10.000 registros)
├── figuras/                             Figuras que genera el notebook
├── requirements.txt                     Versiones exactas con las que se ejecutó
└── README.md
```

El notebook tiene 11 secciones: preparación, datos, modelos transparentes, caja negra, explicaciones globales, explicaciones locales, validación contra la física, riesgos, modelo interpretable final y conclusiones.

## Cómo reproducirlo

**Opción 1 — Google Colab (sin instalar nada).** Abrir el notebook con el botón de arriba y elegir *Entorno de ejecución → Ejecutar todo*. La primera celda instala `shap` y `lime` si hacen falta, y los datos se descargan de UCI si no está la carpeta `data/`.

**Opción 2 — En el computador.**

```bash
git clone https://github.com/tomasguzman0925-ship-it/AXL.git
cd AXL
python -m venv .venv
source .venv/bin/activate        # En Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook XAI_mantenimiento_predictivo.ipynb
```

La ejecución completa tarda uno o dos minutos. Todas las operaciones aleatorias usan la semilla 42, así que los resultados se repiten en cada ejecución.

**Sobre las cifras exactas.** Los números del notebook corresponden a las versiones de `requirements.txt` (Python 3.13). También se probó con versiones anteriores (pandas 2.2, scikit-learn 1.6, XGBoost 2.1, shap 0.48): funciona igual y las métricas cambian en el tercer decimal (por ejemplo, el F1 de XGBoost pasa de 0,738 a 0,733), sin afectar las conclusiones.

## Datos

AI4I 2020 Predictive Maintenance Dataset. UCI Machine Learning Repository. DOI: [10.24432/C5HS5C](https://doi.org/10.24432/C5HS5C). Licencia [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

Artículo original: Matzka, S. (2020). *Explainable Artificial Intelligence for Predictive Maintenance Applications*. Third International Conference on Artificial Intelligence for Industries (AI4I).

## Referencias principales

- Lundberg, S. M. y Lee, S.-I. (2017). *A Unified Approach to Interpreting Model Predictions*. NeurIPS.
- Ribeiro, M. T., Singh, S. y Guestrin, C. (2016). *"Why Should I Trust You?": Explaining the Predictions of Any Classifier*. KDD.
- Rudin, C. (2019). *Stop explaining black box machine learning models for high stakes decisions and use interpretable models instead*. Nature Machine Intelligence.
- Molnar, C. *Interpretable Machine Learning*. https://christophm.github.io/interpretable-ml-book/

La lista completa está al final del notebook.
