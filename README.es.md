# SethXR

Middleware DIY que convierte hardware viejo en entrada para juegos, con el objetivo de llegar eventualmente a soportar VR completamente DIY.

SethXR reutiliza hardware como **controles PlayStation Move, cámaras PS3 Eye, sensores de teléfonos y gamepads** para convertirlos en entradas utilizables para juegos.

El proyecto comienza con entradas simples y convencionales para juegos y gradualmente se expande hacia controles con tracking, seguimiento de la cabeza y, eventualmente, una configuración completa de VR DIY.

## ¿Qué es SethXR?

La idea básica es agarrar hardware que nunca fue diseñado para funcionar junto y crear la capa de software que permita hacerlo útil.

Por ejemplo, la primera configuración utiliza un PS Move y una PS3 Eye:

```text
PS Move + PS3 Eye -> SethXR -> señal de cámara/entrada interpretada -> Juego
```

SethXR se encarga del tracking, la interpretación y la traducción entre el hardware y el juego.

La idea no es hacer que todos los juegos tengan soporte directo para el hardware de PlayStation Move. En cambio, el middleware proporciona entradas convencionales cuando es posible, permitiendo que juegos existentes utilicen el hardware sin necesidad de modificarlos.

## Estado

*Pre-alpha, desarrollo activo, todavía no hay una versión publicada.*

El repositorio actualmente es una implementación básica y todavía está en desarrollo.

La primera versión se publicará una vez que S1 funcione de principio a fin: que la orientación de un PS Move controle la rotación de cámara dentro de un juego real.

## Configuraciones

SethXR utiliza el término "configuraciones" para describir diferentes configuraciones de hardware y métodos de entrada.

Las configuraciones describen diferentes formas de utilizar el sistema y no son etapas obligatorias de actualización. Un usuario puede seguir utilizando una configuración anterior mientras otra está siendo desarrollada.

### S1 — Entrada en monitor

La primera configuración utiliza:

* Un PS Move
* Una cámara PS3 Eye
* Un gamepad convencional para el movimiento
* Un monitor normal

El PS Move proporciona la entrada de cámara/apuntado mientras que el gamepad se encarga del movimiento convencional.

Su modelo de control de cámara está inspirado en la implementación de PlayStation Move de Portal 2: apuntar el control en una dirección produce una rotación continua de cámara en lugar de simplemente mapear la orientación del control directamente a la cámara.

S1 es especialmente adecuado para juegos del motor Source, pero el concepto general está pensado para funcionar con juegos y sistemas de entrada convencionales en general. Minecraft y otros juegos también son posibles objetivos.

### Configuraciones futuras

Las configuraciones futuras pueden experimentar con:

* Tracking de cabeza mediante el teléfono
* Teléfono + VR Box
* Múltiples cámaras PS3 Eye
* Dos controles PS Move
* Tracking de HMD
* Cámaras de tracking montadas en el HMD
* Renderizado estéreo y corrección de lentes
* Runtimes de VR como OpenXR

Estas son direcciones experimentales y no requisitos fijos del proyecto.

## Cómo funciona

El pipeline actual de S1 es:

```text
PS Move + PS3 Eye -> middleware/move_reader.py -> middleware/camera_mapper.py -> middleware/virtual_gamepad.py -> el juego recibe entrada convencional
```

La arquitectura está pensada para evolucionar a medida que se agreguen nuevas fuentes de tracking, métodos de entrada y backends de salida.

## Soporte de plataformas

*Actualmente el desarrollo está enfocado en NixOS/Linux.*

El objetivo a largo plazo es soportar múltiples plataformas, incluyendo:

* Linux
* Windows

Actualmente utilizo Nix como entorno de desarrollo porque es el sistema que estoy usando y ya empecé el desarrollo acá.

*NixOS no está pensado como un requisito para ejecutar la versión final.*

## Desarrollo

Actualmente, el entorno de desarrollo soportado es el shell de desarrollo de Nix:

```bash
nix develop
python middleware/main.py
```

El proyecto se está desarrollando de forma incremental. Las versiones del software se llevarán de forma independiente de las configuraciones de hardware descritas anteriormente.

## Estructura del proyecto

```text
sethxr/
├── flake.nix
├── vendor/
│   └── psmoveapi/
├── middleware/       # S1: lectura de entrada, mapeo y emisión
├── tracking/         # tracking compartido y manejo de poses
├── rendering/        # configuraciones futuras: renderizado estéreo y corrección de lentes
├── config/
│   └── settings.toml
└── notes/
    └── s1-log.md
```

La estructura del proyecto probablemente cambiará a medida que SethXR avance y se implementen nuevas configuraciones.

## Licencia

Este proyecto está bajo la licencia MIT.

Ver [LICENSE](LICENSE) para más detalles.

Las dependencias de terceros mantienen sus respectivas licencias.
