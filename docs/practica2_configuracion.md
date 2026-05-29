# Práctica: Introducción a Gemelos Digitales y Espacios de Movimiento en Robots Industriales

## Objetivo

Implementar y analizar un gemelo digital de un robot industrial Universal Robots utilizando ROS 2 Kilted y Gazebo Ionic, con el fin de estudiar de manera práctica los conceptos de:

* Workspace
* Task Space
* Espacio de Configuración (Configuration Space o C-Space)

---

# Competencias a desarrollar

Al finalizar la práctica, el alumno será capaz de:

* Utilizar un entorno de desarrollo robótico basado en contenedores.
* Ejecutar un gemelo digital de un robot industrial.
* Identificar y diferenciar workspace, task space y espacio de configuración.
* Relacionar posiciones cartesianas del efector final con configuraciones articulares.
* Manipular un robot industrial en simulación mediante ROS 2 y MoveIt.
* Analizar restricciones cinemáticas del robot.

---

# Introducción teórica

Los gemelos digitales permiten representar sistemas físicos mediante modelos virtuales sincronizados. En robótica industrial, un gemelo digital permite:

* Validar trayectorias.
* Simular movimientos antes de ejecutarlos físicamente.
* Analizar colisiones.
* Estudiar restricciones cinemáticas.
* Optimizar procesos de manufactura.

En esta práctica se utilizará un robot colaborativo de Universal Robots dentro de un entorno virtual basado en ROS 2 y Gazebo.

El repositorio utilizado proporciona modelos, controladores y configuraciones listas para simulación de manipuladores UR. ([github.com](https://github.com/UniversalRobots/Universal_Robots_ROS2_GZ_Simulation?utm_source=chatgpt.com))

---

# Conceptos fundamentales

## 1. Workspace

Es el volumen físico alcanzable por el efector final del robot.

Corresponde al conjunto de puntos del espacio cartesiano que el robot puede alcanzar considerando:

* Longitudes de eslabones.
* Límites articulares.
* Restricciones mecánicas.

Ejemplo:

* Un UR5e posee un workspace aproximadamente esférico con restricciones geométricas.

---

## 2. Task Space

Es el espacio donde se define la tarea del robot.

Normalmente incluye:

$(x,y,z)$

y orientación:

$(roll,pitch,yaw)$

El task space describe la tarea desde el punto de vista cartesiano del proceso industrial.

---

## 3. Espacio de Configuración (C-Space)

Representa todas las configuraciones articulares posibles del robot.

Para un robot de 6 GDL:

$q=[q_1,q_2,q_3,q_4,q_5,q_6]$

Cada punto del C-Space representa una postura única del robot.

El robot puede alcanzar un mismo punto cartesiano mediante múltiples configuraciones articulares distintas.

---

# Material requerido

## Software

* Ubuntu 24.04
* Docker Engine o Docker Desktop
* Git
* VSCode (opcional)

## Entorno del curso

El profesor proporcionará un entorno preconfigurado mediante el repositorio:

[robind_ws](https://github.com/chucholoport/robind_ws?utm_source=chatgpt.com)

El entorno ya contiene:

* ROS 2 Kilted
* Gazebo Ionic
* Dependencias de Universal Robots
* Workspace configurado
* Herramientas de simulación
* MoveIt 2

---

# Desarrollo de la práctica

# Parte 1 — Configuración del entorno del curso

## Importante

Para esta práctica NO será necesario instalar manualmente ROS 2, Gazebo ni las dependencias de Universal Robots.

El entorno de desarrollo será ejecutado mediante el comando:

```bash
robot dev
```

La instalación y configuración de este comando se encuentra documentada dentro del repositorio del curso.

---

## 1. Clonar el repositorio del curso

```bash
git clone https://github.com/chucholoport/robind_ws.git
```

---

## 2. Entrar al directorio del proyecto

```bash
cd robind_ws
```

---

## 3. Inicializar el entorno de desarrollo

```bash
robot dev
```

---

## 4. Verificar ROS 2

Dentro del entorno de desarrollo ejecutar:

```bash
ros2 --version
```

---

## 5. Verificar el workspace

```bash
pwd
```

El alumno deberá confirmar que se encuentra dentro del workspace configurado del curso.

---

# Parte 2 — Ejecución del gemelo digital

## 1. Lanzar la simulación

```bash
ros2 launch ur_simulation_gz ur_sim_control.launch.py
```

El repositorio oficial de Universal Robots proporciona integración con Gazebo y ROS 2 para robots UR. ([github.com](https://github.com/UniversalRobots/Universal_Robots_ROS2_GZ_Simulation?utm_source=chatgpt.com))

---

## 2. Identificar componentes del gemelo digital

El alumno deberá identificar:

* Robot UR simulado.
* Mundo de Gazebo Ionic.
* Joint states.
* TF frames.
* Controladores ROS 2.
* Topics activos.

---

## 3. Verificar tópicos ROS

```bash
ros2 topic list
```

---

## 4. Verificar articulaciones

```bash
ros2 topic echo /joint_states
```

---

# Parte 3 — Exploración del Workspace

## Actividad

Mover manualmente el robot desde Gazebo o RViz y registrar:

* Posiciones máximas alcanzables.
* Regiones inaccesibles.
* Restricciones mecánicas.

---

## Evidencia solicitada

Realizar capturas de:

* Robot completamente extendido.
* Robot en postura plegada.
* Límite máximo del workspace.

---

## Preguntas de análisis

1. ¿Qué forma geométrica aproxima el workspace del robot?
2. ¿Existen zonas muertas o inaccesibles?
3. ¿Cómo afectan los límites articulares al workspace?

---

# Parte 4 — Exploración del Task Space

## Actividad

Utilizar MoveIt para mover el efector final a posiciones cartesianas específicas.

Ejecutar:

```bash
ros2 launch ur_simulation_gz ur_sim_moveit.launch.py
```

---

## Objetivo

Mover el TCP (Tool Center Point) a:

| Punto | X    | Y     | Z    |
| ----- | ---- | ----- | ---- |
| A     | 0.30 | 0.20  | 0.40 |
| B     | 0.40 | -0.20 | 0.30 |
| C     | 0.50 | 0.00  | 0.20 |

---

## Actividades

Para cada punto:

* Registrar orientación del efector final.
* Validar si el punto es alcanzable.
* Analizar singularidades.
* Verificar colisiones.

---

## Preguntas de análisis

1. ¿Todos los puntos del task space son alcanzables?
2. ¿Qué ocurre cuando el robot se aproxima a una singularidad?
3. ¿Qué diferencia existe entre workspace y task space?

---

# Parte 5 — Exploración del Espacio de Configuración

## Actividad

Registrar los valores articulares necesarios para alcanzar un mismo punto cartesiano.

Usar:

```bash
ros2 topic echo /joint_states
```

---

## Objetivo

Identificar múltiples soluciones articulares para un mismo TCP.

---

## Tabla de resultados

| Configuración | q1 | q2 | q3 | q4 | q5 | q6 |
| -------------- | -- | -- | -- | -- | -- | -- |
| 1              |    |    |    |    |    |    |
| 2              |    |    |    |    |    |    |

---

## Preguntas de análisis

1. ¿Puede el robot alcanzar el mismo punto con distintas configuraciones?
2. ¿Qué ventajas tiene seleccionar diferentes configuraciones?
3. ¿Cómo se relaciona el espacio de configuración con la cinemática inversa?

---

# Parte 6 — Análisis del Gemelo Digital

## Actividad de reflexión

Responder:

1. ¿Qué ventajas ofrece un gemelo digital en robótica industrial?
2. ¿Qué riesgos industriales pueden reducirse mediante simulación?
3. ¿Qué diferencias existen entre simular y operar un robot real?
4. ¿Qué limitaciones tiene la simulación?

---

# Entregables

El reporte deberá incluir:

* Capturas de pantalla.
* Evidencia de ejecución del entorno del curso.
* Evidencia de ejecución del robot.
* Tablas de configuraciones articulares.
* Respuestas de análisis.
* Conclusiones individuales.

---

# Rúbrica sugerida

| Criterio                            | Valor |
| ----------------------------------- | ----- |
| Configuración correcta del entorno | 15%   |
| Ejecución correcta de simulación  | 20%   |
| Análisis del workspace             | 15%   |
| Análisis del task space            | 15%   |
| Análisis del C-space               | 20%   |
| Calidad del reporte                 | 15%   |

---

# Conclusión esperada

El alumno deberá comprender que:

* El workspace describe el alcance físico del robot.
* El task space describe las tareas cartesianas.
* El espacio de configuración describe las posturas articulares internas.
* Un gemelo digital permite validar estrategias de control y planificación antes de implementar en un sistema físico real.

---

# Referencias

* [Universal Robots ROS2 GZ Simulation](https://github.com/UniversalRobots/Universal_Robots_ROS2_GZ_Simulation?utm_source=chatgpt.com)
* [ROS 2 Kilted Documentation](https://docs.ros.org/en/kilted/index.html?utm_source=chatgpt.com)
* [robind_ws](https://github.com/chucholoport/robind_ws?utm_source=chatgpt.com)
