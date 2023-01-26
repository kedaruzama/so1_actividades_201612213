# Kernel
El kernel es un elemento de software que sirve como núcleo en un sistema operativo, su principal función es ser el intermediario entre los componentes de software que se ejecutan en el sistema operativo y los componentes de hardware de los que dispone la máquina en la cual se ejecuta dicho sistema operativo.

El kernel se encarga de designar cuánto de cada recurso de hardware se otorgan a otros procesos, así como por cuánto tiempo se le asignan dichos recursos, para esto generalmente hace uso de drivers que le permiten comunicarse con los diferentes dispositivos en el equipo. También se encarga de optimizar la utilización de los recursos de hardware y solucionar los diferentes conflictos que pueden llegar a generarse respecto a la utilización de dichos recursos del sistema.

Existen diferentes clasificaciones para los kernels según su diseño, entre los cuales se pueden destacar los siguientes:

* Monolíticos
* Microkernels
* Híbridos
* Exokernels

## Kernels Monolíticos
Un kernel monolítico contiene todo el código necesario para llevar a cabo todas las tareas concernientes al kernel en un solo programa, este tipo de kernels permiten un amplio y basto control a los dispositivos de hardware del equipo al tener todas sus operaciones contenidas en un mismo programa. Sin embargo, la concentración de todas sus operaciones en un mismo programa puede ser también una de sus mayores desventajas ya que en dado caso falle alguna de sus operaciones (a causa de un bug en algún driver, por ejemplo), dicha falla tendría el potencial de corromper segmentos de memoria correspondientes a otros procesos importantes del kernel e incluso provocar un crasheo total del sistema a causa de ello.

## Microkernels
El diseño de un microkernel extrae la gran mayoría de las tareas que se llevan a cabo en un kernel monolítico y las convierte en un conjunto de _servidores_ que llevan a cabo dichas funcionalidades pero ejecutándose en un nivel de privilegios de usuario y no de kernel como se hace en un kernel monolítico.

La idea principal de este tipo de kernels es que se ejecute la menor cantidad de software posible en _kernel mode_ y se ejecute la mayor cantidad posible de software en _user mode_. Por esta razón, en un microkernel se crea una capa de abstracción por encima del hardware con exclusivamente lo más fundamental para la comunicación con este mediante un conjunto de _system calls_ fundamentales y posteriormente se utiliza este conjunto de _system calls_ para ejecutar todas las tareas correspondientes al kernel pero en un modo de ejecución con menos privilegios.

## Kernels Híbridos
Un kernel híbrido es una especie de mezcla entre un microkernel y un kernel monolítico. En general se podría decir que este tipo de kernels es muy similar a los microkernels, con el detalle que estos incluyen en el código ejecutado en _kernel mode_ ciertas funcionalidades que un microkernel ejecutaría en _user mode_, esto se hace con el fin de optimizar su ejecución dado que ser ejecutadas en un mismo programa les hace más eficientes que ejecutarse en servidores como lo realizan en un microkernel. Aún así, al igual que con los microkernels se procura mantener en _user mode_ la mayor cantidad de software posible e incluír en _kernel mode_ únicamente aquello que tiene un impacto clave en la eficiencia del sistema.

## Exokernels
Los exokernels son todavía un concepto experimental en cuanto a diseño de kernels y la idea principal de su diseño es el implementar la menor cantidad de abstracciones posibles entre el hardware y el software ejecutado en el sistema, debido a esto, los exokernels se enfocan principalmente en brindar protección y multiplexar los recursos de hardware del equipo, permitiendo de esta manera que los desarrolladores de aplicaciones para el sistema tengan un gran control sobre el uso de los recursos en lugar de verse obligados a utilizar capas de abstracción extra forzadas por el kernel (como pasa con otros diseños de kernel como los monolíticos). Sin embargo, a pesar de que un exokernel permite un control bastante directo de los recursos de hardware, esto también implica que el desarrollador de aplicaciones para sistemas con este tipo de kernel tenga que desarrollar por cuenta propia y prácticamente desde cero muchas de las funcionalidades que otro tipo de kernel le provee por defecto, lo cual tiene un costo y dificultad más elevado y por lo cual se vuelve una opción menos atractiva a la hora de iniciar un proyecto de desarrollo.

## Principales diferencias
| Kernel Monolítico | Microkernel | Kernel Híbrido | Exokernel |
| :--- | :--- | :--- | :--- |
| Mayor rapidez al ser un solo programa, ya que no necesita comunicarse con ningún proceso externo. | Gracias a que este tipo de kernel divide las tareas del kernel en servidores, el código fuente tiende a estar mejor organizado. | Mayor facilidad en la integración de tecnologías de terceros. | Mayor libertad y aprovechamiento de los recursos de hardware del equipo en el que se ejecuta el sistema. |
| Debido a que es un sólo componente de software, su código fuente al igual que el binario compilado tienden a ser pequeños. | La mejor organización del código hace que sea mucho más fácil darle mantenimiento. | Debido a la gran cantidad de interfaces con las que se debe lidiar, es más probable que exista algún tipo de bug en el sistema. | Incremento en la dificultad que conlleva la creación de un nuevo producto de software para un sistema con este tipo de kernel. |
| Usualmente el proceso de desarrollo es tedioso debido a que con frecuencia se debe reiniciar el sistema para poder probar las modificaciones en el kernel. | El proceso de desarrollo suele ser más ágil debido a que no se requiere de un reinicio total del sistema para probar el código nuevo. | Ágil proceso de desarrollo ya que al igual que con los microkernels, se pueden probar nuevas piezas de código sin necesidad de reiniciar el sistema. | Tecnología experimental que aún no se ha visto realmente aplicada en entornos de producción apropiados a las demandas de la época actual. |
| Su relativa menor cantidad de código hace que sea menos probable que existan errores y por esto puede hacerlo más seguro. | La cantidad de memoria que suele requerirse para la ejecución del kernel tiende a ser mayor debido a la gran cantidad de servidores que se deben ejecutar para llevar a cabo las tareas. | | |

# Niveles de privilegio

## Kernel mode
Es un nivel de privilegios proporcionado por el CPU mediante el cual se puede ejecutar **TODO** tipo de instrucción disponible, desde ejecución de operaciones en el CPU hasta administración de direcciones de memoria y demás dispositivos conectados al sistema. Este es el nivel de privilegios más alto y se utiliza para ejecutar exclusivamente procesos de alta importancia para el sistema y en los cuales se tiene el más alto grado de confianza.

## User mode
Este es un nivel de privilegios en el cual se ejecutan los componentes de software del sistema que no requieren de acceso directo a los componentes de hardware del equipo, y en lugar de acceder directamente, lo hacen mediante APIs proporcionadas por el kernel mediante las cuales puede hacer uso del hardware pero sólo mediante la supervisión de los procesos ejecutados en kernel mode que proveen dichas APIs.