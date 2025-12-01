Este es el archivo `README.md` completo para tu repositorio de GitHub, generado con el formato estándar profesional.

-----

# Simulador de Pipeline MIPS 32-bit (5 Etapas)

## Universidad de Guadalajara - Centro Universitario de Ciencias Exactas e Ingenierías

**Materia:** Arquitectura de Computadoras

-----

## Descripción del Proyecto

Este proyecto consiste en la implementación completa de un **Procesador MIPS de 32 bits** con arquitectura **Pipeline de 5 etapas (IF, ID, EX, MEM, WB)**, desarrollado en lenguaje **Verilog HDL**.

El proyecto se complementa con una **Herramienta Decodificadora** con interfaz gráfica (**GUI**) desarrollada en **Python**. Esta herramienta es fundamental, ya que permite a los usuarios traducir código ensamblador MIPS a su representación binaria, formateada en **Big Endian**, para la precarga de la Memoria de Instrucciones y la Memoria de Datos del procesador.

El objetivo principal es demostrar la funcionalidad de un *datapath* segmentado, incluyendo la lógica de control, unidades funcionales y registros de pipeline.

-----

## Características Principales

### 1\. Pipeline MIPS 32-bit en Verilog

  * **5 Etapas Clásicas:** `Instruction Fetch (IF)`, `Instruction Decode (ID)`, `Execute (EX)`, `Memory (MEM)`, `Write Back (WB)`.
  * **Componentes Modulares:** Incluye módulos separados para todas las unidades funcionales: PC, ALU, Control Unit, Banco de Registros (BR), Memorias y *Buffers* de Pipeline.
  * **Memoria Precargada:** La Memoria de Instrucciones y la Memoria de Datos se precargan con archivos de texto (`instrucciones.txt` y `datos.txt`) generados por el decodificador Python.
  * **Diseño Síncrono:** La lógica del procesador está sincronizada con una señal de reloj (`clk`).

### 2\. Conjunto de Instrucciones Soportadas

El diseño es compatible con una selección de instrucciones comunes en la arquitectura MIPS-I:

| Tipo | Instrucciones (Ejemplos) |
| :--- | :--- |
| **R-Type** | `ADD`, `SUB`, `AND`, `OR`, `SLT` |
| **I-Type (Aritméticas/Lógicas)** | `ADDI`, `ANDI`, `ORI`, `XORI`, `SLTI` |
| **I-Type (Control/Memoria)** | `LW`, `SW`, `BEQ` |
| **J-Type** | `J` |

### 3\. Decodificador Gráfico (Python GUI)

  * **Ensamblador a Binario:** Transforma instrucciones MIPS en formato de 32 bits binario.
  * **Formato Big Endian:** Genera el binario en formato Big Endian (byte más significativo primero), tal como lo requiere el módulo `MemInstrucciones_Procesador.v` para su inicialización.
  * **Generación de Archivos:** Produce los archivos `.txt` (`instrucciones.txt` y `datos.txt`) listos para ser utilizados por el simulador Verilog.

-----

## 🛠 Estructura de Archivos

El repositorio está organizado de la siguiente manera:

```
.
├── src/                                # Contiene todos los módulos Verilog del procesador
│   ├── datapath/
│   │   ├── PROCESADOR.v                # Módulo Top-Level (Datapath principal)
│   ├── control_unit/
│   │   ├── Control_Procesador.v        # Unidad de Control principal
│   │   ├── ALU_Control_Procesador.v    # Lógica de control de la ALU
│   ├── functional_units/
│   │   ├── ALU_Procesador.v            # Unidad Aritmético Lógica (ALU)
│   │   ├── ADD.v, AND.v                # Módulos aritméticos/lógicos auxiliares
│   │   ├── PC_Procesador.v             # Registro de Contador de Programa (PC)
│   │   ├── Shift_Left_2_Procesador.v   # Lógica de salto y ramificación
│   │   ├── Sign-Extend_Procesador.v    # Extensión de Signo
│   │   ├── MUX_Procesador.v            # Multiplexor de 32 bits
│   │   └── MUX_5bit_Procesador.v       # Multiplexor de 5 bits (destino de registro)
│   ├── memories_registers/
│   │   ├── BR_Procesador.v             # Banco de Registros (Register File)
│   │   ├── MemInstrucciones_Procesador.v # Memoria de Instrucciones
│   │   └── ram_sync_Procesador.v       # Memoria de Datos (ram_sync)
│   └── pipeline_buffers/
│       ├── IF_ID_Procesador.v          # Buffer de etapa Fetch/Decode
│       ├── ID_EX_Procesador.v          # Buffer de etapa Decode/Execute
│       ├── EX_MEM_Procesador.v         # Buffer de etapa Execute/Memory
│       └── MEM_WB_Procesador.v         # Buffer de etapa Memory/Write Back
├── decoder/                            # Herramienta de traducción Ensamblador-Binario
│   └── GUI_Decodificador.py            # Interfaz gráfica en Python
├── test_files/                         # Archivos de prueba para la simulación
│   ├── Ensamblador_Test.asm            # Código fuente en ensamblador MIPS de prueba
│   ├── instrucciones.txt               # Contenido binario Big Endian para Memoria de Instrucciones
│   └── datos.txt                       # Contenido inicial de la Memoria de Datos
└── PROCESADOR_TB.v                     # Testbench principal para simulación en Verilog
```

*(Nota: Se asume que los archivos .v se agruparán en carpetas lógicas dentro de una carpeta `src/` para mantener el orden).*

-----

## Uso y Simulación

### 1\. Preparación de Instrucciones (Python)

1.  Ejecuta la herramienta decodificadora:
    ```bash
    python decoder/GUI_Decodificador.py
    ```
2.  Utiliza la GUI para cargar un archivo `.asm` (como `test_files/Ensamblador_Test.asm`) o ingresa el código ensamblador directamente.
3.  Presiona **"Convertir"** y luego **"Guardar Resultado"** para generar los archivos `instrucciones.txt` y `datos.txt` necesarios en el formato Big Endian.
      * **IMPORTANTE:** Asegúrate de que los archivos `instrucciones.txt` y `datos.txt` estén en el mismo directorio de tu simulación Verilog, o que los módulos `MemInstrucciones_Procesador.v` y `ram_sync_Procesador.v` tengan la ruta correcta hacia ellos.

### 2\. Simulación (Verilog)

El proyecto está diseñado para ser simulado utilizando herramientas estándar de Verilog como **ModelSim**, **Icarus Verilog (iverilog)** o **Vivado**.

1.  **Compilación:** Compila todos los módulos `.v` junto con el *Testbench* (`PROCESADOR_TB.v`).

    *Ejemplo con iverilog:*

    ```bash
    iverilog -o procesador PROCESADOR_TB.v src/**/*.v
    ```

2.  **Ejecución:** Ejecuta el archivo compilado para generar el archivo *Value Change Dump* (`.vcd`).

    *Ejemplo con vvp:*

    ```bash
    vvp procesador
    ```

3.  **Visualización:** Abre el archivo `procesador.vcd` con un visualizador de *waveforms* (como **GTKWave**) para analizar el comportamiento del *datapath* en cada una de las 5 etapas, verificando los valores en el Banco de Registros y la Memoria de Datos.

-----

## Contribuciones y Autores

Este proyecto fue desarrollado por el siguiente equipo de estudiantes de Ingeniería Informática de la Universidad de Guadalajara:

| Nombre del Integrante | Rol Principal |
| :--- | :--- |
| **Edgar Andrés Basulto Silva** | Desarrollo y Módulos Verilog |
| **Juan José Bugarin Salvatierra** | Lógica de Control y Testbench |
| **María Fernanda Lavadores Morgado** | Decodificador Python y Pruebas |

-----

**© 2024 Proyecto de Arquitectura de Computadoras - UDG**
