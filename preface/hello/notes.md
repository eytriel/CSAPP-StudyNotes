# Experimento 01: Anatomía del Binario (Capítulo 1)

## Objetivo
Entender la diferencia de tamaño entre un binario de depuración y uno optimizado.

## Procedimiento
Se compiló `hello.c` usando el toolchain de **Zig** en Windows.
1. Compilación estándar: `zig cc hello.c -o hello.exe`
2. Compilación optimizada: `zig cc -O3 hello.c -o hello-fast.exe`

## Observaciones
| Archivo | Tamaño | Notas |
| :--- | :--- | :--- |
| `hello.exe` | 724 KB | Modo Debug, incluye muchos símbolos. |
| `hello-fast.exe` | 165 KB | Optimización -O3, eliminó redundancias. |

> **Nota Mental:** El archivo `.pdb` también redujo su tamaño a la mitad porque el compilador eliminó funciones que no se usaban.

## Análisis de Ensamblador
Al generar el `.s`, noté que `printf` fue sustituido por `puts`. 
* **Instrucción clave:** `leaq .Lstr(%rip), %rcx` (Carga la dirección del string).
