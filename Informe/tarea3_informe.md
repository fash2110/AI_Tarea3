# Tarea 03: Generación de imágenes con autoencoder

**PyTorch Lightning · Hydra · Weights & Biases**

*Instituto Tecnológico de Costa Rica — Escuela de Ingeniería en Computación — Curso de Inteligencia Artificial*

**Fabricio Mena Mejía** — fabriciomena11@estudiantec.cr  
**Anthony Fuentes Calvo** — anthonyfuentescalvo@estudiantec.cr  
**Fernando Sánchez Hidalgo** — Fer.sanchez@estudiantec.cr

---

## Abstract

Este documento describe el desarrollo de la **Tarea 03** del curso de Inteligencia Artificial: la implementación y evaluación de modelos generativos basados en **autoencoders** para la **reconstrucción de imágenes** industriales. Siguiendo el enunciado, se experimentan dos arquitecturas — un **Variational Autoencoder (VAE)** y un **autoencoder tipo U-Net** con conexiones salto — sobre el dataset **MVTec AD**, restringido a las clases *cable*, *capsule*, *screw* y *transistor*, con imágenes de entrada de 128×128 píxeles y tres canales. El entrenamiento se estructura con **PyTorch Lightning**, la gestión modular de hiperparámetros con **Hydra** y el registro comparativo de experimentos en **Weights & Biases**. Para cada arquitectura se prevén al menos cuatro corridas que difieren en la función de pérdida (L1, L2, SSIM y SSIM+L1), con el fin de analizar reconstrucciones en validación y prueba, el comportamiento del espacio latente (t-SNE) y la separabilidad del error de reconstrucción entre muestras normales y anómalas.

**Index Terms—** Autoencoder, variational autoencoder, U-Net, detección de anomalías, MVTec AD, PyTorch Lightning, Hydra, Weights & Biases, reconstrucción de imágenes.

---

## I. Introducción

La detección de defectos en entornos industriales puede abordarse de forma no supervisada aprendiendo a reconstruir únicamente patrones **normales** de producto o textura. Cuando un autoencoder se entrena con imágenes sin anomalías, las reconstrucciones tienden a ser fieles en casos normales y a degradarse ante defectos no vistos, lo que convierte el **error de reconstrucción** en una señal útil para la detección.

La **Tarea 03** del curso de Inteligencia Artificial solicita implementar y comparar dos familias de modelos — **VAE** y **U-Net** — sobre el benchmark **MVTec AD**, utilizando un subconjunto de cuatro clases y resolución fija de 128×128. El flujo experimental debe ser reproducible mediante configuraciones **Hydra**, el ciclo de entrenamiento debe organizarse con **PyTorch Lightning**, y los resultados (pérdidas, reconstrucciones y visualizaciones del espacio latente) deben centralizarse en **WandB** para su comparación sistemática entre funciones de pérdida.

Este informe acompaña el notebook de la tarea y documentará, en secciones posteriores, el dataset y preprocesamiento, el diseño de arquitecturas, los experimentos de entrenamiento y el análisis crítico de reconstrucciones en imágenes normales y defectuosas.
