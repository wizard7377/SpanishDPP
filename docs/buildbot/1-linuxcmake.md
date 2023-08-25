\page linux-cmake-1 CMake en Linux 

Probablemente la forma mas fácil para compilando un proyecto con D++ sea usar `cmake` en Linux con el código fuente, porque no necesita configuración complejo, y es util incluso botes en producción. Antes de empezamos, tu debes asegurar `cmake` es instala, y tu puedes hacerlo con `cmake --version`. En caso de que no es instala, tu necesitas sigue un guía apropiada de tu distribución de linux, o ve si tu distribución es cubre en el final de la guía. En este guía, nosotros compilaremos un proyecto con código en `src/main.cpp` (guías por eso hay [hoy](\ref createbot)).

1. Tu necesitas el código fuente de D++. Primero, tu debes poner librerías en un directorio separado, `libs`, con `mkdir libs && cd libs`. Después, tu puedes copiar el código fuente de D++ con `git clone https://github.com/brainboxdotcc/DPP.git`. Final (en este etapa) tu devuelve a directorio original con `cd ..`, hasta ahora, tu directorio debe mira similar a:
```
    - dpp_bote/
        |-- libs/
            |-- DPP
        |-- src/
            |-- main.cpp
```
2. Hoy, en tu editor preferido, cree un `CMakeLists.txt`, y `cmake` necesita un `cmake_minimum_required`, asi que añade lo:
~~~{.cmake}
cmake_minimum_required(VERSION 3.15)
~~~
3. También, tu necesitas especificar tu proyecto:
~~~{.cmake}
project(dpp_bote)
~~~
4. Añade D++ como un dependencia
~~~{.cmake}
add_subdirectory(libs/DPP)
~~~
5. Especifica `src/main.cpp` como tu código 
~~~{.cmake}
add_executable(${PROJECT_NAME} src/main.cpp)
~~~
6. Une tu proyecto y librería D++
~~~{.cmake}
target_link_libraries(${PROJECT_NAME} dpp)
~~~
7. Especifica las cabeceras de D++
~~~{.cmake}
target_include_directories(${PROJECT_NAME} PRIVATE libs/DPP/include)
~~~
8. Porque D++ usa C++17, tu necesitas poner el versión C++17 o después 
~~~{.cmake}
set_target_properties(${PROJECT_NAME} PROPERTIES
    CXX_STANDARD 17
    CXX_STANDARD_REQUIRED ON
)
~~~
9. Si todo eran bien, tu directorio debe mirar a similar a: 
```
- dpp_bote/
    |-- libs/
        |-- DPP
    |-- src/
        |-- main.cpp
    |-- CMakeLists.txt
```
Y tu `CMakeLists.txt` debe mirar a similar a: 
~~~{.cmake}
cmake_minimum_required(VERSION 3.15)

project(dpp_bote)
 
add_subdirectory(libs/DPP)
 
add_executable(${PROJECT_NAME} src/main.cpp)
 
target_link_libraries(${PROJECT_NAME} dpp)
 
target_include_directories(${PROJECT_NAME} PRIVATE libs/DPP/include)

set_target_properties(${PROJECT_NAME} PROPERTIES
    CXX_STANDARD 17
    CXX_STANDARD_REQUIRED ON
)
~~~
10. Cuando tu [programare tu bote](\ref createbot), tu necesitas compilar tu bote para usarlo. Generalmente, esta mayor compilar en un `build` directorio, ¡y con D++ es no diferente! Tu puedes hacer el directorio con 
```
mkdir build && cd build
```
11. `cmake` haga por crea un `Makefile` con `CMakeLists.txt`, asi que es mayor usar `cmake` en un `build` directorio, y tu puedes lo con:
```
cmake ..
```
12. Finalmente, usa `make` con:
\note `make` solo es muy lento, porque el usa un trabajo de a uno. `make -jn` usa *n* trabajos en paralelo, y *n* debe no mas que numero de tu computadora CPUs. Si tu quires ver algo rompe, usa `make -j`, cuál usas *todos* trabajos en paralelo

```
make
```

## Información en distribuciones.

Si tu sistema no tiene `cmake`, ve por tu sistema debajo.

### Ubuntu y Debian
```
sudo apt update
sudo apt install cmake
```
### Fedora, CentOS, y RedHat 
```
sudo dnf install cmake
```
### OpenSUSE
```
sudo snap install cmake --classic
```
### Arch (y Manjaro)
```
sudo pacman -S cmake
```