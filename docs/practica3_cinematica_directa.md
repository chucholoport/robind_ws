# Práctica 3 - Cinemática Directa del UR5e mediante Validación con Gemelo Digital

# Objetivo

Calcular manualmente la pose del efector final de un robot UR5e utilizando Cinemática Directa y validar el resultado mediante el gemelo digital implementado en ROS 2 Kilted, Gazebo Ionic y MoveIt.

---

# Introducción

La Cinemática Directa permite determinar la posición y orientación del efector final de un manipulador a partir de los valores de sus articulaciones.

En la industria, los controladores de robots realizan estos cálculos de forma automática. Sin embargo, para comprender el funcionamiento interno de un manipulador es necesario conocer cómo se construye la cadena cinemática utilizando matrices de transformación homogénea.

En esta práctica se analizará una única configuración articular del robot UR5e. Los estudiantes deberán calcular manualmente la transformación total desde la base hasta el efector final y posteriormente validar el resultado utilizando el gemelo digital proporcionado por el profesor.

---

# Competencias

Al finalizar la práctica el estudiante será capaz de:

* Identificar los parámetros DH de un manipulador serial.
* Construir matrices homogéneas a partir de una tabla DH.
* Resolver una cadena cinemática mediante multiplicación de matrices.
* Obtener la posición final del efector.
* Comparar resultados teóricos contra resultados obtenidos en simulación.
* Utilizar MoveIt como herramienta de validación cinemática.

---

# Configuración Articular

Utilice la siguiente configuración del robot:

| Articulación | Valor |
| ------------- | ----- |
| q1            | 0°   |
| q2            | -90° |
| q3            | 90°  |
| q4            | 0°   |
| q5            | 90°  |
| q6            | 0°   |

---

# Parámetros Denavit-Hartenberg del UR5e

| i | θᵢ | dᵢ (m) | aᵢ (m) | αᵢ  |
| - | ---- | ------- | ------- | ----- |
| 1 | q1   | 0.1625  | 0       | 90°  |
| 2 | q2   | 0       | -0.4250 | 0°   |
| 3 | q3   | 0       | -0.3922 | 0°   |
| 4 | q4   | 0.1333  | 0       | 90°  |
| 5 | q5   | 0.0997  | 0       | -90° |
| 6 | q6   | 0.0996  | 0       | 0°   |

---

# Transformación Homogénea DH

Para cada articulación deberá utilizarse la siguiente ecuación:

$$
T_i^{i+1}
=

\begin{bmatrix}
\cos(\theta_i) &
-\sin(\theta_i)\cos(\alpha_i) &
\sin(\theta_i)\sin(\alpha_i) &
a_i\cos(\theta_i)
\\
\sin(\theta_i) &
\cos(\theta_i)\cos(\alpha_i) &
-\cos(\theta_i)\sin(\alpha_i) &
a_i\sin(\theta_i)
\\
0 &
\sin(\alpha_i) &
\cos(\alpha_i) &
d_i
\\
0 & 0 & 0 & 1
\end{bmatrix}
$$

---

# Desarrollo

## Parte 1. Inicialización del Gemelo Digital

### Procedimiento

1. Iniciar el contenedor proporcionado por el profesor.
2. Ejecutar el entorno de simulación del UR5e.
3. Verificar que Gazebo se inicie correctamente.
4. Verificar que MoveIt detecte el robot.
5. Identificar el sistema de referencia base del manipulador.

---

## Parte 2. Sustitución de Parámetros

### Procedimiento

1. Sustituir los valores articulares proporcionados en la tabla DH.
2. Calcular los valores trigonométricos necesarios.
3. Construir las siguientes matrices:

$$
T_0^1
$$

$$
T_1^2
$$

$$
T_2^3
$$

$$
T_3^4
$$

$$
T_4^5
$$

$$
T_5^6
$$

---

## Parte 3. Resolución de la Cinemática Directa

Multiplicar las matrices obtenidas:

$$
T_0^6
=

T_0^1
\cdot
T_1^2
\cdot
T_2^3
\cdot
T_3^4
\cdot
T_4^5
\cdot
T_5^6
$$

La matriz resultante representará la transformación total del efector final respecto al sistema de referencia base.

---

## Parte 4. Obtención de la Posición Final

A partir de la matriz final:

$$
T_0^6
=

\begin{bmatrix}
r_{11} & r_{12} & r_{13} & x
\\
r_{21} & r_{22} & r_{23} & y
\\
r_{31} & r_{32} & r_{33} & z
\\
0 & 0 & 0 & 1
\end{bmatrix}
$$

Obtener:

$$
X = x
$$

$$
Y = y
$$

$$
Z = z
$$

Registrar las coordenadas finales del efector.

---

## Parte 5. Validación con MoveIt

### Procedimiento

1. Abrir MoveIt.
2. Introducir los valores articulares proporcionados.
3. Mover el robot a la configuración indicada.
4. Obtener la pose final reportada por MoveIt.
5. Comparar el resultado con el obtenido matemáticamente.

---

# Resultados

Complete la siguiente tabla:

| Parámetro | Resultado Teórico | Resultado Simulado | Error |
| ---------- | ------------------ | ------------------ | ----- |
| X (m)      |                    |                    |       |
| Y (m)      |                    |                    |       |
| Z (m)      |                    |                    |       |

---

# Evidencias

Entregar:

* Desarrollo completo de las matrices homogéneas.
* Sustitución de parámetros DH.
* Multiplicación matricial completa.
* Matriz final ($T_0^6$).
* Coordenadas finales calculadas.
* Captura de pantalla de MoveIt.
* Comparación entre resultado teórico y simulado.
* Conclusiones.

---

# Preguntas de Análisis

1. ¿Qué representa físicamente una transformación homogénea?
2. ¿Por qué el orden de multiplicación es importante?
3. ¿Qué información proporciona la última columna de la matriz final?
4. ¿Qué representa la submatriz de rotación?
5. ¿Qué ventajas aporta un gemelo digital durante el desarrollo de aplicaciones robóticas?

---

# Conclusiones

Redacte una conclusión personal sobre:

* El uso de la Cinemática Directa en robots industriales.
* La utilidad de los parámetros DH.
* La relación entre los cálculos matemáticos y la simulación.
* Las ventajas de utilizar ROS 2 y MoveIt para validar modelos cinemáticos.

---

# Criterios de Evaluación

| Concepto                                | Porcentaje |
| --------------------------------------- | ---------: |
| Sustitución correcta de parámetros DH |       20 % |
| Construcción de matrices homogéneas   |       25 % |
| Multiplicación matricial               |       25 % |
| Validación en MoveIt                   |       15 % |
| Análisis y conclusiones                |       15 % |

---

# Bibliografía

1. John J. Craig. *Introduction to Robotics: Mechanics and Control*.
2. Bruno Siciliano. *Robotics: Modelling, Planning and Control*.
3. Documentación de ROS 2.
4. Documentación de MoveIt.
5. Manual técnico del UR5e.
