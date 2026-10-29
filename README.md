# 🎰 Tragamonedas en Python — Trabajo Final Integrador

**Curso:** Introducción a la Programación Orientado a Python
**Institución:** Polo Tecnológico La Nube
**Docente:** Esp. Julián Escalante
**Año:** 2026

---

## Tabla de contenidos

1. [Descripción](#descripción)
2. [Objetivos del trabajo](#objetivos-del-trabajo)
3. [Estructura del repositorio](#estructura-del-repositorio)
4. [Requisitos](#requisitos)
5. [Instalación y ejecución](#instalación-y-ejecución)
6. [Modo de uso](#modo-de-uso)
7. [Reglas del juego](#reglas-del-juego)
8. [Arquitectura del programa](#arquitectura-del-programa)
9. [Flujo de ejecución](#flujo-de-ejecución)
10. [Resolución de los TODO](#resolución-de-los-todo)
11. [Persistencia de datos](#persistencia-de-datos)
12. [Errores comunes](#errores-comunes)
13. [Limitaciones conocidas](#limitaciones-conocidas)
14. [Desafíos Bonus Track](#desafíos-bonus-track)
15. [Entrega](#entrega)
16. [Autoría](#autoría)
17. [Licencia](#licencia)

---

## Descripción

Este repositorio contiene la resolución del **Trabajo Final Integrador (TFI)** del curso de Introducción a la Programación con Python del Polo Tecnológico La Nube.

El trabajo parte de un programa "dañado": una máquina tragamonedas por consola a la que se le borraron partes importantes del código. La consigna consiste en **restaurar el programa completando 15 tareas pendientes (`TODO`)**, siguiendo la guía de trabajo provista por la cátedra (`docs/TPF.pdf`).

El resultado es un juego funcional que:

- Simula tres rodillos con símbolos elegidos al azar.
- Cobra una apuesta fija por tirada y paga premios según las coincidencias.
- Permite cargar fichas virtuales.
- **Recuerda el saldo** entre ejecuciones, guardándolo en un archivo de texto.
- Registra un **historial de cada partida** en un archivo propio, identificado por fecha y hora (desafíos Bonus Track).

> ⚠️ El juego utiliza exclusivamente **fichas virtuales** con fines educativos. No involucra dinero real.

## Objetivos del trabajo

El TFI integra los contenidos fundamentales del curso:

| Contenido | Dónde se aplica |
|---|---|
| Variables y constantes | `simbolos`, `APUESTA`, `ARCHIVO` |
| Listas | Lista de símbolos y lista de rodillos |
| Estructuras condicionales (`if` / `elif` / `else`) | Cálculo del premio y menú |
| Operadores lógicos (`and`, `or`) | Comparación de rodillos |
| Bucles (`for`, `while`) | Giro de rodillos y ciclo principal del juego |
| Funciones (definición, parámetros, `return`) | `cargar_saldo`, `guardar_saldo`, `girar`, `calcular_premio` |
| Módulos de la biblioteca estándar | `random`, `os`, `datetime` |
| Manejo de archivos de texto | Lectura y escritura de `saldo.txt`; modo agregar (`"a"`) en el historial |
| Conversión de tipos | `int()` y `str()` |

## Estructura del repositorio

```
Curso-LaNube-IntroduccionPython/
├── docs/
│   └── TPF.pdf              # Consigna y guía de trabajo del TFI
├── src/
│   └── tragamonedas.py      # Programa corregido (entregable)
├── .gitignore               # Excluye los archivos generados al jugar
├── LICENSE                  # Licencia MIT
└── README.md                # Este documento
```

Durante la ejecución se generan además, en el directorio desde el que se ejecuta el programa:

- `saldo.txt`: saldo acumulado entre partidas.
- `historial_partida_AAAA-MM-DD_HH-MM.txt`: un archivo por partida con el detalle de cada tirada.

Ambos están excluidos del control de versiones mediante `.gitignore`.

## Requisitos

- **Python 3.6 o superior** (el código utiliza *f-strings*). Probado con Python 3.10.
- No requiere bibliotecas externas: solo se usan módulos de la biblioteca estándar (`os`, `random`, `datetime`).
- Una terminal con soporte para **UTF-8**, para visualizar correctamente los símbolos 🍒 🍋 🔔 ⭐.

## Instalación y ejecución

1. Clonar el repositorio:

   ```bash
   git clone https://github.com/LucasCalivari/Curso-LaNube-IntroduccionPython.git
   cd Curso-LaNube-IntroduccionPython
   ```

2. Ejecutar el programa:

   ```bash
   python src/tragamonedas.py
   ```

   En algunos sistemas (Linux / macOS) el comando puede ser `python3`.

> 💡 **Windows:** si los emojis se ven como signos `?` o cuadrados, se recomienda usar **Windows Terminal** o ejecutar `chcp 65001` antes de iniciar el programa.

## Modo de uso

Al iniciar, el programa muestra el saldo actual y un menú:

```
=== 🎰 TRAGAMONEDAS (fichas virtuales) 🎰 ===
Primera vez: empezás con 100 fichas.

Saldo: 100 fichas | Cada tirada cuesta 10
[Enter] palanca | [+] cargar fichas | [s] salir:
```

| Tecla | Acción |
|---|---|
| `Enter` | Tira de la palanca: cobra la apuesta, gira los rodillos y paga el premio. |
| `+` | Solicita una cantidad de fichas y las suma al saldo. |
| `s` | Guarda el saldo y finaliza el programa. |
| Otra | Muestra el mensaje `Opción inválida.` |

Ejemplo de una tirada:

```
  [ 🍋 | 🍋 | ⭐ ]
Dos iguales: recuperás tu apuesta.
```

## Reglas del juego

| Concepto | Valor |
|---|---|
| Saldo inicial (primera ejecución) | 100 fichas |
| Costo de cada tirada (`APUESTA`) | 10 fichas |
| Símbolos posibles | 🍒 🍋 🔔 ⭐ 7 |

| Resultado | Premio |
|---|---|
| Tres símbolos iguales | 10 × apuesta (100 fichas) |
| Dos símbolos iguales | Se recupera la apuesta (10 fichas) |
| Ningún símbolo igual | 0 fichas |

Si el saldo es menor que la apuesta, no se permite girar y se sugiere cargar fichas con `+`.

## Arquitectura del programa

El código está organizado en tres secciones:

| Parte | Contenido | Responsabilidad |
|---|---|---|
| **Datos** | `simbolos`, `APUESTA`, `ARCHIVO`, `ARCHIVO_HISTORIAL` | Valores globales utilizados por todo el programa. |
| `cargar_saldo()` | Lectura de `saldo.txt` | Recupera el saldo guardado; si el archivo no existe, devuelve 100. |
| `guardar_saldo(saldo)` | Escritura de `saldo.txt` | Persiste el saldo actual (sobrescribe el valor anterior). |
| `girar()` | Bucle `for` de 3 iteraciones | Devuelve una lista con 3 símbolos elegidos con `random.choice()`. |
| `calcular_premio(rodillos)` | Comparaciones con `==`, `and`, `or` | Determina el premio según las coincidencias. |
| `registrar_tirada(rodillos, premio, saldo)` | Escritura en modo `"a"` | Agrega una línea al historial de la partida (Bonus Track). |
| **Programa principal** | Bucle `while True` con menú | Integra todas las funciones y gestiona la interacción con el usuario. |

## Flujo de ejecución

1. Se importan los módulos `os`, `random` y `datetime`.
2. Se definen las constantes, la lista de símbolos y el nombre del historial de la partida (a partir de la fecha y hora de inicio).
3. Python registra las cinco funciones (sin ejecutarlas).
4. Se imprime el título del juego.
5. Se invoca `cargar_saldo()` y su resultado se asigna a `saldo`.
6. Se ingresa al bucle `while True`, que muestra el menú en cada iteración.
7. Según la opción elegida:
   - **Enter:** verificar saldo → cobrar apuesta → `girar()` → `calcular_premio()` → sumar premio → mostrar mensaje → `guardar_saldo()` → `registrar_tirada()`.
   - **`+`:** leer cantidad → sumar al saldo → `guardar_saldo()`.
   - **`s`:** `guardar_saldo()` → mensaje de despedida → `break`.

## Resolución de los TODO

| N.º | Bloque | Solución aplicada |
|---|---|---|
| 1 | Datos | `simbolos = ["🍒", "🍋", "🔔", "⭐", "7"]` |
| 2 | Datos | `APUESTA = 10` |
| 3 | Datos | `ARCHIVO = "saldo.txt"` |
| 4 | Archivos | `saldo = int(archivo.read())` |
| 5 | Archivos | `archivo.write(str(saldo))` |
| 6 | Funciones | `for i in range(3): rodillos.append(random.choice(simbolos))` |
| 7 | Funciones | `if rodillos[0] == rodillos[1] and rodillos[1] == rodillos[2]:` |
| 8 | Funciones | `return APUESTA * 10` |
| 9 | Programa principal | `saldo = cargar_saldo()` |
| 10 | Programa principal | `saldo = saldo - APUESTA` |
| 11 | Programa principal | `rodillos = girar()` |
| 12 | Programa principal | `premio = calcular_premio(rodillos)` |
| 13 | Programa principal | `saldo = saldo + premio` |
| 14 | Programa principal | `guardar_saldo(saldo)` |
| 15 | Programa principal | `break` |

## Persistencia de datos

El saldo se almacena como un número entero en texto plano dentro de `saldo.txt`:

- **Lectura:** al iniciar, si el archivo existe, se lee su contenido y se convierte a entero con `int()`.
- **Escritura:** después de cada tirada, de cada carga de fichas y al salir, se sobrescribe el archivo (modo `"w"`) con el saldo convertido a texto mediante `str()`.

Para reiniciar el juego con el saldo inicial de 100 fichas, basta con eliminar el archivo `saldo.txt`.

### Historial de partidas

Al iniciar el programa se genera, **una única vez**, el nombre del archivo de historial con la fecha y hora de inicio:

```python
ARCHIVO_HISTORIAL = "historial_partida_" + datetime.now().strftime("%Y-%m-%d_%H-%M") + ".txt"
```

En la hora se usan guiones (`-`) en lugar de dos puntos (`:`), ya que Windows no admite ese carácter en nombres de archivo.

Después de cada tirada, `registrar_tirada()` abre el archivo en modo `"a"` (agregar) y escribe una línea con la hora, los símbolos obtenidos, el premio y el saldo resultante. El archivo se abre con `encoding="utf-8"` para que los emojis se guarden correctamente también en Windows. Ejemplo de `historial_partida_2026-10-29_17-34.txt`:

```
17:34:20 | ⭐ | 7 | 7 | Premio: 10 | Saldo: 100
17:34:22 | 🍒 | 🔔 | 🍋 | Premio: 0 | Saldo: 90
17:34:25 | 🔔 | 🔔 | 🔔 | Premio: 100 | Saldo: 180
```

Cada nueva ejecución del programa crea un archivo distinto, sin modificar los historiales anteriores. El archivo se crea recién con la primera tirada, por lo que una partida sin tiradas no genera historial.

## Errores comunes

Tabla de diagnóstico incluida en la guía de la cátedra:

| Síntoma | Causa probable |
|---|---|
| Los rodillos aparecen vacíos: `[ ]` | Falta el `for` del TODO 6 o la invocación del TODO 11. |
| El premio nunca llega | Falta el TODO 7 o el TODO 8. |
| El saldo no cambia al jugar | Faltan los TODO 10 y 13, o no hay asignación (`saldo = ...`). |
| El saldo no se recuerda al reabrir | Faltan los TODO 5 o 14, o el nombre del archivo (TODO 3). |
| Al reabrir siempre arranca en 0 | Falta el TODO 4 o el TODO 9. |
| `TypeError: write() argument must be str` | Falta `str()` en el TODO 5. |
| `ValueError` al leer el archivo | El archivo está vacío o contiene texto en lugar de un número. |
| El programa nunca termina | Falta el `break` del TODO 15. |

## Limitaciones conocidas

- Al cargar fichas con `+`, si se ingresa un valor no numérico el programa finaliza con `ValueError`, ya que la entrada no se valida.
- No se impide cargar cantidades negativas de fichas.
- `saldo.txt` y los historiales se crean en el **directorio de trabajo actual**, no necesariamente en la carpeta `src/`.
- Como el nombre del historial tiene precisión de minutos, dos partidas iniciadas dentro del mismo minuto comparten el mismo archivo (las tiradas de la segunda se agregan al final).

Estas limitaciones forman parte del diseño original de la consigna y no fueron modificadas para respetar el alcance del trabajo.

## Desafíos Bonus Track

La guía propone dos mejoras opcionales, ambas implementadas en `src/tragamonedas.py` (ver [Historial de partidas](#historial-de-partidas)):

| Desafío | Descripción | Estado |
|---|---|---|
| 1. Historial de la partida | Registrar en un archivo de texto una línea por tirada con los símbolos obtenidos, el premio y el saldo final. | ✅ Implementado |
| 2. Archivo por partida | Crear un archivo distinto en cada partida, con el formato `historial_partida_AAAA-MM-DD_HH-MM.txt` (usando `datetime` y `strftime`, y abriendo el archivo en modo `"a"`). | ✅ Implementado |

## Entrega

- **Fecha de entrega:** 04/11/2026 (extendible hasta el 11/11/2026).
- **Medio:** correo electrónico al docente, con el asunto
  `TFI - Introducción a la programación - Polo Tecnológico La Nube`.
- **Adjunto:** `tragamonedas.py` corregido (incluye los desafíos Bonus Track).
- En el cuerpo del correo debe indicarse si el trabajo se realizó de forma individual o grupal, mencionando a los integrantes en este último caso.

## Autoría

Trabajo realizado en forma **grupal** por:

| Integrante |
|---|
| Ana Lucía Colazo |
| Lucas Calivari |
| Melina Mas Duarte |

Repositorio mantenido por [@LucasCalivari](https://github.com/LucasCalivari).

Consigna y guía de trabajo: **Esp. Julián Escalante** — Polo Tecnológico La Nube.

## Licencia

Este proyecto se distribuye bajo la licencia **MIT**. Consultar el archivo [LICENSE](LICENSE) para más detalles.
