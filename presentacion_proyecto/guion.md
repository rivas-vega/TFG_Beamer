# Guión del Presentador (V3 - Defensa Técnica)

> **Trabajo de Fin de Grado:** *Fundamentos Matemáticos de los Energy-Based Models y las Joint Embedding Architectures*
> **Autor:** Pedro Rivas Vega — Universidad de Sevilla
> **Total Diapositivas de Contenido:** 64 (más Bibliografía).

---
## 1. Portada y 2. Cronología
«Buenos días al tribunal. Mi trabajo explora los fundamentos matemáticos que sostienen el aprendizaje de representaciones continuo. La historia de estas arquitecturas es una convergencia entre teoremas de la estadística clásica, como Cramér-Wold de 1936, y el deep learning contemporáneo.»

---
## Capítulo 1: Introducción (Diapositivas 3 a 7)
> **(4) Observación:** «El aprendizaje humano es mayoritariamente auto-supervisado por observación pasiva.»
> **(5) Discriminativo/Generativo:** «Modelos como la clasificación tiran información, y los generativos gastan capacidad intentando predecir detalles estocásticos de los píxeles.»
> **(6) JEA:** «La alternativa son las Joint Embedding Architectures, que comparan representaciones latentes abstractas.»
> **(7) Visual JEA:** «Esta es la abstracción fundamental: un encoder y una pérdida de similitud latente.»

---
## Capítulo 2: Modelos Basados en Energía (Diapositivas 8 a 13)
> **(9-10) Energía:** «Para modelar matemáticamente esta similitud sin probabilidades que sumen 1, usamos los Modelos Basados en Energía. La inferencia es buscar el estado que minimice la función $E_\theta$.»
> **(11-12) Jaynes:** «Si quisiéramos usar probabilidades, Jaynes demostró que la distribución óptima es la de Boltzmann, normalizada por la función de partición $Z$.»
> **(13) Cuello de botella:** «El problema central es que integrar la función de partición en un espacio continuo (imágenes) es intratable. Por eso no usamos Boltzmann y volvemos a la energía escalar pura.»

---
## Capítulo 3: Redes Siamesas (Diapositivas 14 a 18)
> **(15) Bromley 1993:** «Al usar solo energía (distancia latente), la primera arquitectura, las Redes Siamesas, atrae los pares similares.»
> **(16) Mínimo Global:** «El teorema fundamental del colapso: si el encoder mapea cualquier entrada a un vector constante $c$, la pérdida es cero y el gradiente es nulo. Es un atractor estable.»
> **(17) Visual:** (Pasar a la figura del colapso de firmas para mostrarlo visualmente).
> **(18) Dimensional:** «Incluso si no colapsa a un punto, puede colapsar a un subespacio lineal perdiendo dimensión efectiva.»

---
## Capítulo 4: Contrastive Learning (Diapositivas 19 a 24)
> **(20-21) Repulsión:** «En 2006 surge la solución contrastiva: añadir un margen empujando representaciones de imágenes distintas.»
> **(22) Visual Fuerzas:** (Mostrar el diagrama de muelles y repulsión activa).
> **(23) Límite Epsilon-Red:** «La debilidad de este método. Para que la esfera no colapse, la cantidad de negativos que repelen debe cubrir la epsilon-red. Esto crece exponencialmente con la dimensión: $\mathcal{O}(e^d)$.»
> **(24) Visual Zonas Ciegas:** «El resultado con *batches* normales son estas zonas oscuras donde el espacio puede colapsar libremente.»

---
## Capítulo 5: Revolución No Contrastiva (Diapositivas 25 a 43)
> **(26-29) Información Mutua:** «Ante el límite exponencial negativo, volvemos a la biología. El Principio de Barlow dicta reducir redundancia. Para ello, necesitamos diagonalizar la matriz de Información Mutua, basada en la entropía compartida de Shannon.»
> **(30-32) Puente de Jaynes:** «Calcular la Información Mutua es intratable. Pero, si asumimos Gaussianidad, el *Puente de Jaynes* nos dice que la correlación lineal es suficiente: $R=I_d$ asegura la independencia.»
> **(33-35) Barlow Twins:** «Barlow Twins lo implementa centrando empíricamente los batches y forzando a que la matriz de Correlación Cruzada $C$ tienda a la identidad. Hay un término diagonal de invarianza y uno cruzado de redundancia.»
> **(36-37) Problemas BT:** «El problema es que $C$ está normalizada (pierde la escala de la varianza), lo que oculta colapsos dimensionales puros. Además, su único hiperparámetro acopla ambos objetivos.»
> **(38-42) VICReg:** «Para solucionarlo, VICReg usa la matriz de Covarianza Empírica $\Sigma$. Desacopla la pérdida en Invarianza (MSE explícito), Varianza (*hinge loss* explícita sobre las dispersiones de $\Sigma$) y Covarianza (ortogonalidad fuera de la diagonal).»
> **(43) Visual Comparativa:** (Detenerse aquí para contrastar los esquemas de cálculo matricial de BT frente al desacoplamiento paralelo de VICReg).

---
## Capítulo 6: World Models (Diapositivas 44 a 50)
> **(45-46) JEA Temporal:** «Añadimos tiempo: queremos que un predictor imagine el futuro en el espacio latente. La flecha violeta indica la asimetría temporal.»
> **(47) Inestabilidad:** «Intentar usar VICReg en el tiempo (PLDM) falla: la varianza repele frames temporales que el predictor intenta atraer.»
> **(48-50) V-JEPA:** «V-JEPA abandona el formalismo de VICReg y usa una Media Móvil Exponencial (EMA) para los pesos y un Stop-Gradient. (Mostrar diagrama). Esto frena el colapso, pero es solo una heurística.»

---
## Capítulo 7: SIGReg (Diapositivas 51 a 60)
> **(52) La brecha teórica:** «El Puente de Jaynes asume normalidad, pero ni VICReg ni V-JEPA la fuerzan. Si la distribución no es gaussiana, decorrelacionar falla.»
> **(53-55) Cramér-Wold:** «Para forzar la normalidad en $d$ dimensiones, recurrimos al Teorema de Cramér-Wold: basta con forzar normalidad en proyecciones univariantes aleatorias.»
> **(56-57) Epps-Pulley:** «Para evaluar esa normalidad de forma diferenciable (sin la operación de supremo), usamos Epps-Pulley, basado en la función característica. Su formulación es una suma exponencial $\mathcal{C}^\infty$.»
> **(58-59) LeWorldModel:** (Mostrar diagrama de SIGReg). «Un solo término probabilístico evalúa la normalidad de las proyecciones y reemplaza toda la heurística temporal.»
> **(60) Arco Narrativo:** «El resumen del viaje: de los problemas del colapso (rojo) a las soluciones matriciales (azul) hasta llegar al cierre formal de la normalidad (verde).»

---
## Cierre (Diapositivas 61 a 64)
> **(62) Conclusiones:** «SIGReg no solo independiza, sino que, como demostró Klindt, forzar normalidad isótropa es matemáticamente necesario para que las variables latentes coincidan con los factores físicos del mundo.»
> **(63) Futuro:** «Aplicar estos simuladores a física de fluidos o control multinivel (H-JEPA).»
> **(64) Final:** «Termino con la frase sobre LeCun: tras décadas esperando, ¿es el momento histórico de las JEA?»
