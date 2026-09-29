# EDI - Gestor de Servidores de Juego

Práctica universitaria de **Estructura de Datos I** desarrollada en **C++**, centrada en el diseño e implementación de estructuras de datos dinámicas aplicadas a un sistema de gestión de servidores para juegos multijugador.

El programa permite desplegar y administrar servidores, controlar su estado y gestionar jugadores conectados o en espera utilizando estructuras implementadas manualmente.

> Este repositorio tiene finalidad académica y refleja el trabajo realizado durante la práctica.

---

## Objetivo

El objetivo principal es aplicar estructuras de datos y gestión dinámica de memoria a un problema de mayor tamaño.

El sistema representa una infraestructura de servidores de juego donde cada servidor puede:

- estar activo, inactivo o en mantenimiento;
- alojar jugadores hasta una capacidad máxima;
- mantener una cola de espera cuando no quedan plazas;
- expulsar jugadores;
- transferir o redistribuir usuarios cuando un servidor deja de estar disponible.

---

## Funcionalidades

El menú principal permite:

1. Mostrar información de un servidor o de todos los servidores.
2. Crear un nuevo servidor.
3. Eliminar un servidor.
4. Activar un servidor.
5. Desactivar un servidor.
6. Poner un servidor en mantenimiento.
7. Conectar un jugador.
8. Expulsar un jugador.
9. Salir del programa.

---

## Estructuras de datos implementadas

### Lista dinámica

La clase `lista` almacena los jugadores conectados a un servidor.

Está implementada mediante un vector dinámico cuyo tamaño crece o disminuye en bloques de cuatro posiciones.

Entre sus operaciones se encuentran:

- insertar;
- eliminar;
- añadir por la izquierda o derecha;
- consultar elementos;
- modificar;
- buscar;
- concatenar listas;
- consultar longitud.

En el sistema se utiliza para mantener los jugadores actualmente conectados a cada servidor.

### Cola circular dinámica

La clase `cola` implementa una cola FIFO mediante almacenamiento dinámico y gestión circular de índices.

Permite:

- encolar jugadores;
- desencolar;
- consultar el primer elemento;
- comprobar si está vacía;
- consultar su longitud.

Se utiliza para gestionar los jugadores que están esperando una plaza libre en un servidor.

### Lista enlazada de servidores

`GestorServidores` mantiene una secuencia enlazada de objetos `Servidor`.

Cada servidor dispone de un puntero al siguiente servidor y el gestor conserva la referencia al primero de la estructura.

Los servidores se insertan ordenados según su localización geográfica.

---

## Modelo de jugador

Los jugadores se representan mediante la estructura `Jugador`:

```cpp
struct Jugador {
    cadena nombreJugador;
    int ID;
    bool activo;
    int latencia;
    long puntuacion;
    cadena pais;
};
```

Cada jugador mantiene información sobre:

- nombre de usuario;
- identificador;
- estado de conexión;
- latencia;
- puntuación;
- país de origen.

---

## Gestión de servidores

La clase `Servidor` representa cada nodo del sistema.

Cada servidor almacena, entre otros datos:

- dirección o hostname;
- identificador;
- juego instalado;
- puerto;
- localización geográfica;
- capacidad máxima de jugadores conectados;
- capacidad máxima de jugadores en espera;
- estado actual;
- lista de jugadores conectados;
- cola de jugadores en espera;
- referencia al siguiente servidor.

Los estados utilizados son:

```text
INACTIVO
ACTIVO
MANTENIMIENTO
```

---

## Gestión de jugadores

Cuando un jugador intenta conectarse, el sistema busca servidores activos compatibles con el juego solicitado.

La lógica contempla:

- alojar al jugador si existe una plaza disponible;
- enviarlo a una cola de espera si los servidores están llenos;
- impedir que un mismo jugador esté alojado más de una vez;
- expulsar jugadores;
- mover automáticamente al primer jugador de la cola de espera a la lista de conectados cuando queda una plaza libre.

La lista de conectados se mantiene ordenada según la puntuación de los jugadores.

---

## Redistribución al desactivar servidores

Una de las partes más relevantes de la práctica es la gestión de un servidor que deja de estar activo.

El gestor contempla la redistribución de jugadores hacia otros servidores compatibles y activos, teniendo en cuenta criterios como:

- disponibilidad de plazas;
- juego instalado;
- capacidad de las colas de espera;
- latencia de los jugadores.

Si no existe espacio disponible para alojar o mantener en espera a un jugador, este puede quedar fuera del sistema.

---

## Arquitectura del proyecto

```text
EDI_Practica2/
├── cpp/
│   ├── GestorServidores.cpp
│   ├── Servidor.cpp
│   ├── cola.cpp
│   ├── lista.cpp
│   └── main.cpp
├── h/
│   ├── GestorServidores.h
│   ├── Servidor.h
│   ├── cola.h
│   └── lista.h
├── Declaraciones.h
├── cabeceras.h
├── CMakeLists.txt
├── .gitignore
└── README.md
```

### `main.cpp`

Contiene el menú interactivo y coordina las operaciones solicitadas por el usuario.

### `GestorServidores`

Gestiona la colección completa de servidores y las operaciones globales del sistema.

Entre sus responsabilidades se encuentran:

- desplegar servidores;
- buscar servidores;
- activar y desactivar;
- realizar mantenimiento;
- eliminar servidores;
- localizar jugadores;
- alojar y expulsar jugadores;
- redistribuir jugadores.

### `Servidor`

Representa un servidor individual y administra sus jugadores conectados y su cola de espera.

### `lista`

Implementación propia de una lista dinámica basada en un array redimensionable.

### `cola`

Implementación propia de una cola circular dinámica.

---

## Tecnologías

- **C++14**
- **CMake**
- Gestión dinámica de memoria
- Punteros
- Listas enlazadas
- Arrays dinámicos
- Colas circulares
- Programación orientada a objetos

---

## Compilación

El proyecto utiliza CMake y requiere una versión compatible con la indicada en `CMakeLists.txt`.

Desde la raíz del repositorio:

```bash
cmake -S . -B build
cmake --build build
```

El ejecutable generado corresponde al objetivo:

```text
Practica2
```

---

## Ejecución

Después de compilar, ejecuta el binario generado por CMake.

En Windows, normalmente:

```powershell
.\build\Practica2.exe
```

La ubicación exacta puede variar según el generador de CMake utilizado.

---

## Estado del proyecto

El repositorio contiene la implementación de las estructuras principales y del gestor de servidores utilizado en la práctica.

Al tratarse de un proyecto académico:

- la interfaz es por consola;
- los datos se mantienen únicamente en memoria durante la ejecución;
- no existe persistencia en archivos o bases de datos;
- se utilizan cadenas de caracteres de tamaño fijo;
- algunas decisiones de implementación responden a los requisitos concretos de la práctica y no a un diseño de producción.

---

## Conceptos trabajados

Esta práctica permite trabajar de forma aplicada con:

- memoria dinámica;
- constructores y destructores;
- gestión manual de recursos;
- punteros y nodos enlazados;
- listas dinámicas;
- colas FIFO;
- redimensionamiento de estructuras;
- búsqueda e inserción ordenada;
- encapsulación mediante clases;
- separación entre interfaz y lógica;
- resolución de un problema mediante estructuras de datos personalizadas.

---

## Contexto académico

Proyecto desarrollado como **Práctica 2 de EDI**.

Su finalidad principal es aplicar estructuras de datos implementadas manualmente dentro de un caso práctico de gestión de servidores y jugadores multijugador.
