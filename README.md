# Simulador de lanzamiento de cohete: método de Euler

Proyecto académico de **Métodos Numéricos** que simula el ascenso
vertical idealizado de un cohete. Compara el método de **Euler
explícito** con la solución analítica de un modelo diferencial sencillo.
Incluye gráficas, animación del cohete sobre las curvas, consulta de
valores por instante y exportación CSV.

> **Contexto educativo:** el proyecto se inspira en la importancia de
> las matemáticas durante la carrera espacial, como se presenta en la
> película *Talentos ocultos (Hidden Figures)*. No reproduce los
> cálculos oficiales de una misión NASA ni afirma que Euler fuera el
> único método utilizado en los lanzamientos reales.

## 1. Objetivos

-   Aplicar Euler a un sistema de ecuaciones diferenciales.
-   Calcular velocidad y altura en distintos instantes.
-   Obtener la solución analítica del mismo modelo.
-   Comparar ambos métodos y cuantificar sus errores.
-   Visualizar los resultados mediante gráficas y una animación
    interactiva.
-   Analizar la influencia del tamaño del paso en la precisión.

## 2. Modelo físico

Se considera un cohete que asciende verticalmente. Se suponen tres
fuerzas:

-   Empuje constante (T), hacia arriba.
-   Peso (mg), hacia abajo.
-   Resistencia lineal (kv), opuesta al movimiento ascendente.

Por la segunda ley de Newton:

\[ m`\frac{dv}{dt}`{=tex}=T-mg-kv \]

Dividiendo entre la masa:

\[ `\frac{dv}{dt}`{=tex}=`\frac{T}{m}`{=tex}-g-`\frac{k}{m}`{=tex}v \]

Definimos:

\[ a=`\frac{T}{m}`{=tex}-g,`\qquad `{=tex}b=`\frac{k}{m}`{=tex} \]

El sistema que resuelve la aplicación es:

\[ `\boxed{
\begin{aligned}
v'(t)&=a-bv(t)\\
y'(t)&=v(t)
\end{aligned}}`{=tex} \]

con condiciones iniciales (v(0)=v_0) y (y(0)=y_0).

## 3. Parámetros iniciales

La aplicación inicia con estos valores, que pueden modificarse desde el
formulario. El paso de integración se mantiene fijo en (0.5) segundos.

  Parámetro                      Símbolo   Valor inicial Unidad
  ---------------------------- --------- --------------- --------
  Masa                             \(m\)            1000 kg
  Empuje                           \(T\)           15000 N
  Gravedad                         \(g\)            9.81 m/s²
  Coeficiente de resistencia       \(k\)             100 kg/s
  Velocidad inicial                (v_0)               0 m/s
  Altura inicial                   (y_0)               0 m
  Paso                             \(h\)             0.5 s
  Tiempo total                     (t_f)              20 s

Con los datos iniciales:

\[ a=`\frac{15000}{1000}`{=tex}-9.81=5.19 `\text{m/s}`{=tex}\^2 \]

\[ b=`\frac{100}{1000}`{=tex}=0.1 `\text{s}`{=tex}\^{-1} \]

Por tanto:

\[ `\boxed{v'=5.19-0.1v}`{=tex} \]

El número de iteraciones es:

\[ N=`\frac{t_f}{h}`{=tex}=`\frac{20}{0.5}`{=tex}=40 \]

La tabla muestra **41 registros**: el instante inicial (t=0) y los 40
pasos hasta (t=20) segundos.

## 4. Método de Euler explícito

Para una ecuación (x'=f(t,x)), Euler utiliza:

\[ x\_{n+1}=x_n+h f(t_n,x_n) \]

### 4.1. Actualización de la velocidad

Aplicando Euler a (v'=a-bv):

\[ `\boxed{v_{n+1}=v_n+h(a-bv_n)}`{=tex} \]

Con (h=0.5):

\[ v\_{n+1}=v_n+0.5(5.19-0.1v_n) \]

\[ `\boxed{v_{n+1}=0.95v_n+2.595}`{=tex} \]

### 4.2. Actualización de la altura

Como (y'=v):

\[ `\boxed{y_{n+1}=y_n+h v_n}`{=tex} \]

Esta fórmula usa la **velocidad al inicio del intervalo** (v_n), no la
velocidad nueva (v\_{n+1}). Así está implementado Euler explícito en el
programa.

### 4.3. Primeras iteraciones

Condiciones iniciales:

\[ t_0=0,`\quad `{=tex}v_0=0,`\quad `{=tex}y_0=0 \]

**Iteración 1: de 0 a 0.5 segundos**

\[ v_1=0+0.5(5.19-0.1(0))=2.595 `\text{m/s}`{=tex} \]

\[ y_1=0+0.5(0)=0 `\text{m}`{=tex} \]

**Iteración 2: de 0.5 a 1 segundo**

\[ v_2=2.595+0.5(5.19-0.1(2.595)) =4.93025 `\text{m/s}`{=tex} \]

\[ y_2=0+0.5(2.595)=1.2975 `\text{m}`{=tex} \]

Se repite el procedimiento hasta llegar a los 20 segundos.

## 5. Solución analítica

La solución analítica calcula los valores del modelo sin aproximar
mediante pasos.

### 5.1. Velocidad

Partimos de:

\[ `\frac{dv}{dt}`{=tex}=a-bv \]

Separando e integrando, para (b`\ne0`{=tex}), se obtiene:

\[ `\boxed{v(t)=\frac{a}{b}+
\left(v_0-\frac{a}{b}\right)e^{-bt}}`{=tex} \]

Definiendo (v\_`\infty`{=tex}=a/b):

\[ `\boxed{v(t)=v_\infty+(v_0-v_\infty)e^{-bt}}`{=tex} \]

### 5.2. Altura

Integramos (y'=v(t)) y aplicamos (y(0)=y_0):

\[ `\boxed{
y(t)=y_0+v_\infty t+
\frac{v_0-v_\infty}{b}(1-e^{-bt})
}`{=tex} \]

### 5.3. Caso especial: resistencia nula

Si (k=0), entonces (b=0), y se usan las fórmulas:

\[ `\boxed{v(t)=v_0+at}`{=tex} \]

\[ `\boxed{y(t)=y_0+v_0t+\frac12at^2}`{=tex} \]

La aplicación contempla este caso para evitar divisiones entre cero.

## 6. Comprobación con los valores iniciales

Con (a=5.19), (b=0.1), (v_0=0) y (y_0=0):

\[ v(t)=51.9(1-e\^{-0.1t}) \]

\[ y(t)=51.9t-519(1-e\^{-0.1t}) \]

A los 20 segundos:

\[ v(20)=51.9(1-e\^{-2})`\approx44.8761`{=tex} `\text{m/s}`{=tex} \]

\[ y(20)=51.9(20)-519(1-e\^{-2})
`\approx589.2390`{=tex} `\text{m}`{=tex} \]

Euler con (h=0.5) segundos produce aproximadamente:

  Magnitud a los 20 s           Euler     Analítica
  --------------------- ------------- -------------
  Velocidad               45.2302 m/s   44.8761 m/s
  Altura                   585.6978 m    589.2390 m

El error absoluto se calcula como:

\[ E_v=\|v\_{`\text{analítica}`{=tex}}-v\_{`\text{Euler}`{=tex}}\| \]

\[ E_y=\|y\_{`\text{analítica}`{=tex}}-y\_{`\text{Euler}`{=tex}}\| \]

Resultados aproximados:

-   Error de velocidad: **0.3541 m/s**.
-   Error de altura: **3.5412 m**.

Los valores se muestran redondeados a cuatro decimales; JavaScript
realiza los cálculos con su precisión numérica habitual.

## 7. Funciones de la aplicación

### Parámetros de entrada

Permite modificar masa, empuje, gravedad, coeficiente de resistencia,
velocidad inicial, altura inicial y tiempo total. El paso permanece fijo
en 0.5 segundos. El tiempo total debe ser múltiplo de 0.5.

### Indicadores de resultados

Muestra número de iteraciones, velocidad y altura calculadas por Euler y
por la solución analítica, y errores absolutos finales.

### Gráficas comparativas

Incluye dos gráficas:

-   **Velocidad vs. tiempo:** compara Euler con la solución analítica.
-   **Altura vs. tiempo:** compara Euler con la solución analítica.

### Animación interactiva

Un control deslizante permite elegir un instante. El cohete se desplaza
sobre la curva de Euler y se muestran los datos de ese instante: tiempo,
velocidad Euler, velocidad analítica, altura Euler y altura analítica.
Los controles permiten reproducir, pausar y volver al inicio.

La animación representa el avance por los puntos de la gráfica; no es
una simulación tridimensional del vehículo.

### Tabla de iteraciones

Presenta 41 registros (con los parámetros predeterminados) y las
siguientes columnas:

  Columna               Contenido
  --------------------- ---------------------------------------
  n                     Número de iteración
  t                     Tiempo en segundos
  Velocidad Euler       Velocidad aproximada
  Altura Euler          Altura aproximada
  Velocidad analítica   Velocidad exacta del modelo
  Altura analítica      Altura exacta del modelo
  Error velocidad       Diferencia absoluta entre velocidades
  Error altura          Diferencia absoluta entre alturas

### Exportación CSV

Permite descargar la tabla de resultados para abrirla en Excel u otro
programa compatible.

### Conclusiones

La aplicación incorpora una sección final con conclusiones sobre Euler,
la comparación analítica, el efecto del tamaño de paso y las
limitaciones físicas.

## 8. Cómo ejecutar el proyecto

1.  Descarga o clona este repositorio.
2.  Abre el archivo `Simulador_Cohete_Euler.html` (o el nombre HTML que
    hayas guardado).
3.  Utiliza un navegador actualizado: Chrome, Firefox, Edge o Safari.
4.  No requiere instalación de dependencias ni servidor web; las
    funciones incluidas se ejecutan localmente.
5.  Modifica los parámetros y pulsa **Ejecutar simulación**.

### Estructura sugerida del repositorio

``` text
simulador-lanzamiento-cohete/
├── README.md
├── Simulador_Cohete_Euler.html
└── calculos_lanzamiento_cohete_euler.xlsx
```

## 9. Relación con la carrera espacial y *Talentos ocultos*

La película *Talentos ocultos (Hidden Figures, 2016)* presenta el
trabajo de Katherine Johnson, Dorothy Vaughan y Mary Jackson en la NASA
durante la carrera espacial. Muestra la importancia de los cálculos
matemáticos y la verificación de trayectorias en el programa espacial
estadounidense.

Este proyecto toma ese contexto como motivación para estudiar ecuaciones
diferenciales y métodos numéricos. Es una **actividad educativa
inspirada en ese contexto histórico**, no una reconstrucción de los
algoritmos, datos ni procedimientos oficiales utilizados en las misiones
Mercury o Apollo.

## 10. Limitaciones del modelo

Este modelo es educativo. No debe utilizarse para diseñar, controlar ni
predecir un lanzamiento real.

Suposiciones:

-   Movimiento estrictamente vertical.
-   Masa constante.
-   Empuje constante.
-   Gravedad constante.
-   Resistencia lineal (kv) con coeficiente constante.
-   No se consideran viento, variación de densidad atmosférica,
    calentamiento, vibraciones ni etapas del vehículo.

Un modelo real debe considerar consumo de combustible y variación de
masa, empuje variable, aerodinámica dependiente de velocidad y altitud,
geometría del vehículo, movimiento en varias dimensiones y condiciones
atmosféricas.

Una trayectoria lunar requiere modelar el movimiento orbital y las
interacciones gravitacionales entre la nave, la Tierra y la Luna. Por
eso, este simulador no representa una trayectoria lunar ni una misión
espacial real.

## 11. Actividad propuesta para estudiantes

**Desafío:** actuar como un equipo de análisis numérico que verifica el
modelo de ascenso de un cohete experimental.

1.  Ejecutar el modelo con los parámetros iniciales.
2.  Registrar los resultados de Euler y analíticos a los 5, 10, 15 y 20
    segundos.
3.  Repetir la simulación con diferentes valores de masa, empuje y
    resistencia.
4.  Comparar resultados con distintos pasos de integración en una
    versión ampliada o mediante una hoja de cálculo.
5.  Explicar cómo cambia el error y por qué.
6.  Identificar las limitaciones del modelo frente a un lanzamiento
    real.
7.  Presentar conclusiones con tablas, gráficas y cálculos.

### Preguntas de reflexión

-   ¿Por qué es importante verificar los cálculos antes de una misión
    espacial?
-   ¿Qué riesgos produce un paso de integración demasiado grande?
-   ¿Por qué una aproximación puede parecer razonable y acumular
    errores?
-   ¿Qué variables reales del lanzamiento no contempla el modelo?
-   ¿Por qué una trayectoria lunar necesita ecuaciones más complejas?

## 12. Tecnologías utilizadas

-   **HTML5:** estructura de la aplicación.
-   **CSS3:** estilos y diseño adaptable.
-   **JavaScript:** cálculos numéricos, solución analítica, animación,
    gráficas y exportación CSV.
-   **Excel:** libro complementario con parámetros, iteraciones,
    fórmulas y resumen de resultados.

## 13. Propósito académico

Proyecto académico para la enseñanza de **Métodos Numéricos**, con
énfasis en ecuaciones diferenciales, método de Euler, solución
analítica, análisis de errores y visualización de resultados.

**Propósito:** demostrar cómo un modelo matemático puede convertirse en
una herramienta interactiva para comprender un fenómeno físico y evaluar
la precisión de los métodos numéricos.

------------------------------------------------------------------------

*Nota: los parámetros del cohete son hipotéticos y fueron elegidos con
fines didácticos. No corresponden a especificaciones de un vehículo
espacial real.*
