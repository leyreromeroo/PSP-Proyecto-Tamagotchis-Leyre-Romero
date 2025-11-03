# Ejercicio 4 – Hilos y Procesos Concurrentes (PSP)

## Descripción del proyecto

Este proyecto simula el funcionamiento de varios Tamagotchis que viven de forma concurrente utilizando **hilos en Java**.  
El programa principal actúa como **cuidador**, encargado de crear, lanzar y controlar el ciclo de vida de varios Tamagotchis, cada uno con un comportamiento independiente.

El objetivo del ejercicio es **aprender a trabajar con hilos y sincronización básica en Java**, representando procesos que ocurren al mismo tiempo pero deben coordinarse correctamente.

---

## Funcionamiento general

### Programa principal: Cuidador

- Crea y lanza **X Tamagotchis**, cada uno identificado con un nombre único.  
- Supervisa el estado de cada uno y puede **interactuar** con ellos mediante acciones (alimentar, limpiar, jugar, etc.).  
- Puede **consultar el estado** de todos los Tamagotchis.  
- Puede **matar** un Tamagotchi, pero solo si está ocioso.

---

## Comportamiento de los Tamagotchis

Cada Tamagotchi se ejecuta en su propio **hilo**, teniendo así **vida propia** e independiente del resto.

### Ciclo de vida

- Los Tamagotchis **viven durante 5 minutos**.  
  Al finalizar ese tiempo, **avisan y mueren automáticamente**.

### Alimentación

- Cada Tamagotchi **come a un ritmo distinto**.  
  Si el cuidador les da una manzana, cada uno **tarda un tiempo diferente en terminarla**.  
- Avisan cuando **empiezan** y cuando **terminan de comer**.

### Juego

- Cuando quieren jugar, el Tamagotchi propone una **suma de dos números** (de una cifra y con resultado menor que 10) al cuidador.  
- El cuidador debe responder con el resultado.  
- Si la respuesta es incorrecta, **el Tamagotchi sigue jugando** hasta que el cuidador acierte.

### Limpieza y suciedad

- La **suciedad** aumenta **1 punto cada 20 segundos**, en una escala de **0 a 10**.  
- Al llegar a **5**, el Tamagotchi avisa de que **empieza a estar sucio**.  
- Si llega a **10**, **muere**.  
- Al limpiarlo, su suciedad vuelve a **0**, y el proceso de limpieza **dura 5 segundos**.

---

## Acciones disponibles para el cuidador

| Acción | Descripción |
|--------|--------------|
| Poner pilas | Inicia los hilos de los Tamagotchis. |
| Alimentar | Hace que el Tamagotchi coma (con duración variable). |
| Jugar | Responde a una suma propuesta por el Tamagotchi. |
| Limpiar | Restablece la suciedad a 0 tras 5 segundos. |
| Matar | Elimina el Tamagotchi si está ocioso. |
| Estado | Muestra el estado actual (vida, suciedad, actividad). |

---

## Reglas de tiempo

| Evento | Intervalo / Duración |
|--------|-----------------------|
| Incremento de suciedad | Cada 20 segundos |
| Limpieza | 5 segundos |
| Tiempo de vida | 5 minutos |
| Ritmo de comida | Variable según Tamagotchi |

---

## Objetivos de aprendizaje

- Comprender el uso de **hilos (`Thread`, `Runnable`)** en Java.  
- Aprender a manejar **procesos concurrentes**.  
- Utilizar **sincronización** para evitar conflictos entre acciones simultáneas.  
- Simular un entorno en el que múltiples entidades actúan de forma **independiente pero coordinada**.

---

---

## Cómo descargar y ejecutar el proyecto en Eclipse

### 1. Clonar o descargar el repositorio

Puedes obtener el proyecto de dos formas:

**Opción 1: Clonar con Git**

Abre una terminal y ejecuta:

git clone + link del repositorio


**Opción 2: Descargar como ZIP**

- Entra en el repositorio de GitHub.  
- Haz clic en **Code → Download ZIP**.  
- Extrae el archivo ZIP en tu equipo.

---

### 2. Importar el proyecto en Eclipse

1. Abre **Eclipse IDE**.  
2. Ve al menú **File → Import...**.  
3. Selecciona **Existing Maven Projects** (si es un proyecto Maven) o **Existing Projects into Workspace** (si es un proyecto Java normal).  
4. Haz clic en **Next**.  
5. En **Root Directory**, selecciona la carpeta del proyecto descargado.  
6. Asegúrate de que Eclipse detecta el proyecto y pulsa **Finish**.

---

### 3. Ejecutar el programa

1. Abre la clase principal `Cuidador.java`.  
2. Haz clic derecho dentro del editor → **Run As → Java Application**.  
3. Observa en la consola cómo los Tamagotchis ejecutan sus acciones de forma concurrente.

---

## Autor y asignatura

**Asignatura:** Programación de Servicios y Procesos (PSP)  
**Ejercicio:** Nº 4 – Hilos para hacer procesos concurrentes  
**Tema:** Concurrencia en Java  
**Autor:** Leyre Romero

