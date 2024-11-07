### Descripción del problema
Los deportistas de alto rendimiento generan adaptaciones en el corazón, derivando en un corazón de atleta. Estas adaptaciones pueden ser factores de riesgo para personas no deportistas, por lo que es importante detectar estas anomalías y evaluar si corresponde a un riesgo para el deportista. Debido a que un mal diagnóstico puede derivar en una muerte cardíca súbita.

Para solucionar este problema se plantea un modelo que busca predecir el diagnóstico de un ECG de un deportista. **Con este modelo se espera clasificar si es un ECG normal o de riesgo.**


### Descripción de los datos
Se tiene una base de datos de ECG de 12 derivaciones de 28 atletas de alto rendimiento de Noruega. Debido a la baja cantidad de datos se optó por expandir la cantidad de datos, para esto se usó “Lobachevsky University Electrocardiography Database”. Esta base de datos contiene 200 ECG de 12 derivaciones de personas que no son necesariamente deportistas, y para aplicar estos datos hacia un enfoque de detectar anomalías en deportistas, solo se considera los datos de los pacientes hasta 45 años.

Las bases de datos tienen diferencias entre el formato del diagnóstico, **el diagnóstico es realizado por cardiólogos**, en el caso de la BD de Atletas Noruegos es un cardiólogo especializado en medicina deportiva. En la BD de deportistas, las etiquetas se presentan en un listado, donde primero se indica el tipo de ritmo, posterior a esto se agregan si es que existen anomalías cardíacas, y finalmente se indica el diagnóstico general del ECG, si es normal, limítrofe o anormal. En el caso de la BD de Lobachevsky University, se indica el tipo de ritmo cardiaco, el eje eléctrico del corazón y se agregan si es que existen anomalías, en este caso no indica el diagnóstico general del ECG.

### Generación de diagnósticos de la nueva base de datos

Para asignar un diagnóstico a los ECG de la BD de Lobachevsky University se evaluaron las etiquetas de las anomalías en base al “Consenso Internacional de los Criterios para la Interpretación del ECG en Atletas”. Estos criterios buscan disminuir los falsos positivos mejorando la detección de anomalías que pueden derivar en una muerte cardiaca súbita.
Los hallazgos se clasifican según el riesgo que presenta una de estas anomalías hacia los deportistas. 
Para los datos de Lobachevsky University, se tomó en cuenta las etiquetas presentes, en donde se clasificó como borderline una desviación del eje eléctrico del corazón, crecimiento en el auricular derecho o izquierdo y un bloqueo completo de la rama derecha. Y como caso anormal cuando se tiene alguna isquemia.

### Selección y Justificación de Modelos
Para predecir el diagnóstico general de un ECG, identificando si es **normal, o de riesgo** se utilizó el modelo de **K-nearest neighbors (KNN)** utilizando **Dynamic Time Warping (DTW)** como métrica de distancia, para así, comparar los ECGs y así encontrar el más cercano. 
Se utilizó esta técnica debido a la baja cantidad de datos, lo que dificultaba aplicar técnicas que requieren un mayor volumen de datos como Random Forest o algún algoritmo de aprendizaje supervisado.
Una de las principales ventajas de utilizar DTW en el análisis de ECGs, es su capacidad para alinear y comparar señales de diferentes longitudes o con variaciones en el tiempo. Para el caso de los ECGs, esto es útil debido a las siguientes razones.
Manejo de Variabilidad Temporal: Los ECGs suelen tener variaciones en la frecuencia y duración de los latidos cardíacos. DTW permite alinear estas señales, ignorando las pequeñas diferencias temporales, permitiendo detectar patrones de una mejor manera.
Eficiencia con Pocos Datos: Al no requerir grandes volúmenes de datos para entrenamiento, DTW se adapta de manera ideal para conjuntos de datos pequeños y permitiendo encontrar patrones relevantes.

Este modelo es relevante debido a que permite comparar las señales de ondas sin que afecte los pequeños desfases de tiempos, además al comparar señales permite comparar entre las formas de las ondas, detectando con precisión las similitudes entre las señales.

### Implementación y Evaluación de Modelos
El modelo de K-nearest neighbors (KNN) con Dynamic Time Warping (DTW) como métrica de distancia se entrena con la BD de Lobachevsky University. Este modelo permite identificar el diagnóstico más probable de un ECG al encontrar las señales de entrenamiento más similares y asignar la clase dominante entre los vecinos más cercanos.
Para entrenar el modelo se utiliza la BD de Atletas Noruegos y para probar precisión del modelo se utiliza BD de Atletas Noruegos.
La implementación de KNN con DTW consiste en que, para cada ECG de la BD de Atletas Noruegos, se calcula las distancias DTW con respecto a cada ECG del conjunto de entrenamiento. Con las distancias calculadas con respecto a todos los ECGs de entrenamiento, los k vecinos más cercanos fueron seleccionados, y la etiqueta más frecuente se asigna como el diagnóstico final.

Al evaluar el modelo este entrega una precisión del 85,7%, esto debido a que el modelo detecta los casos de riesgo pero indica algunos casos normales como de riesgos. Sin embargo, es importante que los casos de riesgo no son detectado como normales.

Debido a que en la BD de Atletas Noruegos además contiene un diagnóstico del algortimo Marquette SL12, cuando el modelo predice un ECG como de riesgo y el cardiólgo no lo comtempló de esta manera, se evalúa el disgnóstico del algortimo SL12. 

Al contemplar este diagnóstico adicional, se obtiene una precisión del 96,4%, en donde en este caso existen un diagnósticos normales que se detectaron como de riesgo. El cual corresponde a una hipertrofia ventricular izquierda, anomalía que se presenta con frecuencia en deportistas debido al desarrollo de corazón de atleta




