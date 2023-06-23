
# Proyecto final - Robótica Industrial.


En el desarrollo de este proyecto, se tuvo como objetivo el diseño y la creación de una rutina para un sistema robotizado, enfocado en la automatización del proceso de Pick & Place y alistamiento de pedidos.
La introducción de procesos industriales en el campo de la robótica implica planificar y diseñar las etapas de producción que involucran el uso de robots y tecnología automatizada. Este proceso incluye el análisis de viabilidad, la selección y distribución de equipos robóticos, el diseño de flujos de trabajo y la capacitación del personal. El objetivo es lograr una producción eficiente y de alta calidad utilizando la tecnología robótica en los procesos industriales.


### Procedimiento llevado a cabo

-	Diseño y manufactura de la herramienta multipropósito de trabajo, la cual debía contener en ella los dos tcp, un gancho metálico para la manipulación del balde y una ventosa para la manipulación de la ventosa.
-	Creación de rutinas en RobotStudio teniendo en cuenta criterios de desempeño y diseño.
-	Prueba y ajuste de alturas de la rutina en el LabSir.
-	Registro de resultados a través de vídeos.
-	Análisis de tiempo, respondiendo a la pregunta que tan rápido puede realizarse el proceso.

### Descripción del problema

En la industria se encuentran distintos tipos de procesos susceptibles a ser optimizados con el objetivo de proteger a las personas, aumentar la calidad en los procesos y maximizar la eficiencia de producción. Los procesos de Pick & Place en el alistamiento de pedidos se componen de tareas tradicionalmente humanas. Se desea establecer la velocidad máxima (mínimo tiempo de operación) que se puede alcanzar utilizando de forma segura un brazo manipulador industrial.

### Descripción de la solución

Dado que el requerimiento es la optimización de una tarea que se puede bien desempeñar con el empleo de un operario, se busca la manera que reducir el tiempo en el que el operario lo haría con el uso de una herramienta automatizada en conjunto con la herramienta apropiada para tal función. En este informe se desarrolla la solución a dicho problema empezando por el diseño de una herramienta idónea que permita su acople con una ventosa y un gancho (gripper) para luego dar paso a la simulación en software que permita llevar la idea a la realidad en un robot ABB IRB 140. Finalmente se plantea la discusión de optimización de la ejecución de la tarea pasando de manera manual a automatizada.

### Diseño de la herramienta multipropósito y del gripper: 

 A continuación, se muestra a la solución planteada para la herramienta orientada a cumplir la tarea de transportar piezas y un balde empezando en una estantería y terminando en una banda transportadora.

 ![image](https://github.com/jhoncale/ProyectoFinal/assets/38961990/bdf40dad-e98d-4e0e-aa5b-28fa0a709c0f)

 Figura 1. Herramienta para ventosa y gripper

 Como se puede observar en la imagen, se implementa una pieza herramienta de acrílico que ubica la ventosa (orificio grande) a 90° el plato del robot. La herramienta a su vez incorpora una punta la cual sirve como referencia de ubicación del sistema tcp en el software de programación de rutinas, RobotStudio. Finalmente, el gripper se simboliza simplemente como el gancho acomodado a un costado de la herramienta, a 90° de la punta, dado que allí físicamente se coloca un chazo adherido con pegamento en el cual se enrosca el gancho.

 ### Diseño de las fichas
 
En el mismo material de la herramienta (acrílico) se diseñan piezas para cumplir con 2 requisitos importantes, uno es el de no ser tan pequeño para que la ventosa sea capaz de generar el vacío necesario al entrar en contacto con la pieza y se pueda levantar. El otro criterio es que su tamaño tampoco sea tan grande para poder entrar en el balde, por lo que la mejor manera de diseñar estas fichas fue de manera circular como se muestra en la imagen a continuación, las cuales tenían un diámetro de 6 cm.

![image](https://github.com/jhoncale/ProyectoFinal/assets/38961990/c076b54f-89f8-44ad-86ab-a076cfb2146d)

Figura 2. Fichas para levantar con la ventosa

![image](https://github.com/jhoncale/ProyectoFinal/assets/38961990/8fcf37f7-1e34-4f62-a35d-1e9b3feb64ad)


 ### Metodología para la creación de rutinas Pick & Place

 Para llevar a cabo el desarrollo de las rutinas que debía seguir el robot IBR 140, se utilizó el software RobotStudio y algunos modelados CAD para poder simular el entorno del robot dentro del programa. 
Para iniciar la resolución del proyecto fue necesario el modelado del estante (lugar al que tenía que llegar el robot para seleccionar y agarrar las fichas indicadas).


![image](https://github.com/jhoncale/ProyectoFinal/assets/38961990/2d8bd135-0df0-4ac8-a32e-c780fa41dc54)


Figura 3. Estantería de madera

Posteriormente, para el área de trabajo de RobotStudio, se modeló una barra guía con el objetivo de tener una superficie y poder acomodar más fácilmente los puntos de trayectoria del robot.
![image](https://github.com/jhoncale/ProyectoFinal/assets/38961990/e683aaf7-9383-4e37-b18c-0c1569a11521)


![image](https://github.com/jhoncale/ProyectoFinal/assets/38961990/1caa971f-9dfc-4243-ad5d-d4d0ffb276b2)

Figura 4. Guía de trabajo en RobotStudio

Asi mismo, la ubicación del balde tanto en la zona inferior del robot (suelo), como en la banda transportadora fue de gran importancia para tener una noción de hasta dónde debería llegar la trayectoria del robot diseñada.



![image](https://github.com/jhoncale/ProyectoFinal/assets/38961990/337b541a-7765-44e9-8ce3-61f2b5f018b7)


Figura 6. Ubicaciones posibles del balde. Banda transportadora y suelo

Una vez, ya creado el escenario en el cual trabajará el robot, se procede al diseño de las rutinas donde se tienen en cuenta los criterios de evaluación y también metodologías planeadas para hacer más sencillo la solución a problemas futuros. La explicación se dividirá en 3 subrutinas: alistamiento de almacenamiento, alistamiento de estantería y posicionamiento de elementos.
Cabe resaltar que previamente a la selección de puntos de trayectoria, se realizó una verificación del workspace del robot, con el fin de asegurar que todos los elementos involucrados se encontraban dentro del alcance del robot. 

### Alistamiento de almacenamiento

Para el diseño de esta rutina se creó un workObject en una esquina inferior de la banda transportadora. Las indicaciones del proyecto estaban dirigidas a que el manipulador debía tomar el contenedor de la banda transportadora y dejarlo en el suelo, y así mismo tomarlo del suelo y llevarlo nuevamente a la banda transportadora.
Para obtener eficiencia en ambos recorridos y evitar problemas tales como que sí era posible dejar el balde en el suelo, pero no recogerlo de este, la propuesta realizada fue en un principio hacer una rutina de llevar el balde desde el suelo hasta la banda transportadora, y posteriormente para la rutina de regreso, lo único que se hizo fue copiar cada uno de los targets, pero de manera inversa, es decir, el último punto sería el primero y así sucesivamente. Con esta metodología implementada se pudo asegurar que el manipulador fuera capaz de agarrar el balde donde lo hay dejado. 
Una de las estrategias para estas dos rutinas fue siempre llevar el manipulador a home después de agarrar el balde, es decir, se tomaba el balde, se llevaba a home y posteriormente se colocaba en el sitio correspondiente. Esto ayudó bastante en el tema de la evasión de singularidades.
Otro punto para tener en cuenta fue la velocidad manejada, durante todas las pruebas siempre se estableció una velocidad mínima para este recorrido debido a que se requería más que rapidez, una precisión en poder agarrar y dejar el balde de la mejor manera posible. 

### Alistamiento de estantería

La estantería tenía 6 divisiones, en las cuales se debía ubicar una ficha diseñada y debía llegar el manipulador para poder succionar la indicada. 
La estrategia utilizada en esta rutina nuevamente es la creación de su propio WorkObject con el objetivo principal de que, si se debía mover el estante, todos los puntos creados se pudieran trasladar con este. 
Los 6 grados de libertad que caracterizan al IRB 140, y especialmente la muñeca que lo conforma fueron de bastante ayuda para diseñar cada una de las 6 rutinas, esto debido a que una vez acercado el manipulador a una de las divisiones del estante, se podía reorientar la posición del efector final (TCP) para que la succión de la ventosa fuera lo más preciso posible. 


![image](https://github.com/jhoncale/ProyectoFinal/assets/38961990/657a78b2-a21b-483b-ae96-1e5cc69c1373)

Figura 7. Rutina de trabajo en RobotStudio, interacción del brazo robótico con la estantería

### Posicionamiento de los elementos: 

Cada una de las fichas seleccionadas y tomadas por el manipulador debían ser posicionadas dentro del balde, en esto consiste el proceso de Pick & Place. 
Para poder diseñar esta rutina, lo que se tuvo en cuenta fue el cómo era la manera óptima para poder hacer la entrega de las fichas al balde. Se decidió por realizar una orientación paralela al balde con tal de que, al apagar la succión de la ventosa, la ficha pudiera caer de manera vertical asegurando la entrada al balde sin ningún tipo de obstáculo.

![image](https://github.com/jhoncale/ProyectoFinal/assets/38961990/c030f42d-8cab-4a1c-aaee-5aee34cd0388)

Figura 8. Rutina de trabajo en RobotStudio, posicionamiento de los elementos en el balde

### Automatización del proceso apoyado en el código RAPID

La creación de las trayectorias ciertamente es indispensable para poder llevar a cabo el movimiento del manipulador, pero la programación en RAPID permite perfeccionar el proceso automatizado, por ejemplo, accionar salidas siempre y cuando se hayan ejecutado algunas rutinas o también ejecutarlas solo si una condición ya ha sido cumplida. 
A continuación, se explicará la programación de código realizada mediante un ejemplo.
DI_01 = Entrada digital de botón de inicio
DO_03 = Salida digital de lámpara
DO_01 = Salida digital de válvula
DO_02 = Salida digital de válvula

![image](https://github.com/jhoncale/ProyectoFinal/assets/38961990/36936029-33ac-4058-bbe7-2b91f26008e5)

Figura 9. Extracto del código RAPID, inicio del programa

En primera instancia, se condiciona la rutina para que funcione únicamente si el botón de inicio ha sido presionado, posteriormente a esa instrucción, se acciona la lámpara y se procede a hacer la toma de balde desde la banda transportadora para posicionar en el suelo.

![image](https://github.com/jhoncale/ProyectoFinal/assets/38961990/6078de21-138e-43c2-8060-a13fc10e4c30)

Figura 10. Extracto del código RAPID, continuación

Una vez el manipulador haya llegado a la división del estante indicada para recoger la ficha, se hace el accionamiento de la electroválvula (doble) y se esperan 3 segundos para proceder con la rutina. Este tiempo de espera ayudó para asegurar que sí se tomara la ficha de la manera adecuada. 
Así mismo, para la entrega de la ficha en el balde, primero se posiciona el manipulador cerca del balde, se espera un tiempo y se suelta la ficha. Esto porque, en pruebas anteriores sin un tiempo de espera, el manipulador se corría y hacía que la ficha tuviera movimientos imprevistos que no le daban precisión para entrar en el balde. 


![image](https://github.com/jhoncale/ProyectoFinal/assets/38961990/9900499d-b55e-4b0c-a149-43e183591019)

Figura 9. Extracto del código RAPID, fin del programa


Finalmente, se apaga la luz de la lámpara.

### Ventajas solución planteada

-	Creación de distintos WorkObjects
-	Creación de una punta en la herramienta diseñada (facilitó la ubicación del TCP).
-	Diseño de guías para establecer los puntos de trayectorias más fácil.
-	Manipulación de targets desde el FlexPendant.
-	Banda transportadora apagada

### Limitaciones

-	La superficie de las tablas donde se ubica el robot IRB 140 en el LabSir son muy inestables y a veces se movía, situación que hacía que tocara volver a ajustar las rutinas pasadas.
-	Singularidades del robot.

Finalmente, se logra responde a la pregunta inicial del proyecto: 

###  ¿Qué tan rápido lo puedo hacer?


El proceso de Pick & Place automatizado y el realizado manualmente sin manipulador se puede comparar desde diferentes aspectos, uno de ellos puede ser la destreza que se tiene, las capacidades o el alcance de robot y la velocidad con la que se opera. 
En cuanto a la velocidad, se pudo observar que el robot IRB 140 siempre operaba a la velocidad indicada, en general los manipuladores realizan sus rutinas con velocidad constante durante todo el proceso sin verse afectados por el cansancio o la falta de concentración, como lo es el caso de un ser humano. 
Otro factor para evaluar es la precisión con la que pueden realizar una actividad a la misma velocidad, el ser humano si utiliza más velocidad no puede medir su fuerza o su precisión en las rutinas, razón por la cual si o si, debe bajar su magnitud de velocidad. 
Es decir, si se compara el tiempo, al poder trabajar con mayor velocidad el robot su tiempo de trabajo se ve totalmente reducido. Así, se puede ver que es posible optimizar la tarea no solo en tiempo sino en el empleo de personal, en los videos demostrativos podemos ver como para el ejercicio manual se emplean 2 personas, una para el transporte de los objetos y otra para la manipulación del registro del aire para generar el vacío de la ventosa. Además de la coordinación entre los 2 operarios, se puede ver que, a largo plazo, una persona puede terminar cansada después de unas horas de trabajo en la misma tarea. Por ello, la implementación por medio de una herramienta automatizada, además de mostrar una disminución del tiempo de ejecución, claramente puede funcionar sin parar, sin importar la larga jornada. En el video de prueba del robot, se muestra una rutina que dura alrededor de 3 minutos en ser ejecutada a la velocidad que una persona lo realizaría, pero este tiempo puede verse reducido en unas 20 veces de acuerdo con una escogencia de velocidad mayor por lo que el tiempo podría llegar a reducirse a menos de 10 segundos de ejecución, comparado con los 40 segundos que duran los operarios del video de demostración manual. 

### planos y fotografias del gripper diseñado


![image](https://github.com/jhoncale/ProyectoFinal/assets/38961990/970d1e00-f5f8-4598-912c-221e003e3b3f)


Descarga:


[Herramienta+gancho.pdf](https://github.com/jhoncale/ProyectoFinal/files/11828425/Herramienta%2Bgancho.pdf)


![image](https://github.com/jhoncale/ProyectoFinal/assets/38961990/d41305b4-99f3-441e-b387-12d9c898db26)



### Código en RAPID comentado rutina 1

PROC main()
    WaitDI DI_01,1;  // Espera hasta que la entrada DI_01 sea igual a 1.
    SETDO DO_03,1;   // Establece la salida DO_03 en 1.
    homee;           // Mueve el robot a la posición de origen (home).
    BaldeR;          // Toma el balde de la banda.
    homee;           // Vuelve a la posición de origen.
    BaldeR1;         // posiciona el balde.
    homee;           // Vuelve a la posición de origen.
    UNO;             // Toma ficha.
    SETDO DO_01,0;   // Establece la salida DO_01 en 0.
    SETDO DO_02,1;   // Establece la salida DO_02 en 1.
    WaitTime 3;      // Espera durante 3 segundos.
    UNO_;            // Retira la ficha.
    homee;           // Vuelve a la posición de origen.
    dejarFichas;     // Deja la ficha en el balde.
    WaitTime 3;      // Espera durante 3 segundos.
    SETDO DO_01,1;   // Establece la salida DO_01 en 1.
    SETDO DO_02,0;   // Establece la salida DO_02 en 0.
    WaitTime 2;      // Espera durante 2 segundos.
    homee;           // Vuelve a la posición de origen.

    DOS;             // Toma ficha.
    SETDO DO_01,0;   // Establece la salida DO_01 en 0.
    SETDO DO_02,1;   // Establece la salida DO_02 en 1.
    WaitTime 3;      // Espera durante 3 segundos.
    DOS_;            // Retira la ficha.
    homee;           // Vuelve a la posición de origen.
    dejarFichas;     // Deja la ficha en el balde.
    WaitTime 3;      // Espera durante 3 segundos.
    SETDO DO_01,1;   // Establece la salida DO_01 en 1.
    SETDO DO_02,0;   // Establece la salida DO_02 en 0.
    WaitTime 2;      // Espera durante 2 segundos.
    homee;           // Vuelve a la posición de origen.

    TRES;            // toma ficha.
    SETDO DO_01,0;   // Establece la salida DO_01 en 0.
    SETDO DO_02,1;   // Establece la salida DO_02 en 1.
    WaitTime 3;      // Espera durante 3 segundos.
    TRES_;           // Retira la ficha.
    homee;           // Vuelve a la posición de origen.
    dejarFichas;     // Deja la ficha en el balde.
    WaitTime 3;      // Espera durante 3 segundos.
    SETDO DO_01,1;   // Establece la salida DO_01 en 1.
    SETDO DO_02,0;   // Establece la salida DO_02 en 0.
    WaitTime 2;      // Espera durante 2 segundos.
    homee;           // Vuelve a la posición de origen.

    Balde1;          // Toma el balde.
    homee;           // Vuelve a la posición de origen.
    Balde2;          // Deja el balde en la banda.
    homee;           // Vuelve a la posición de origen.

    SETDO DO_03,0;   // Establece la salida DO_03 en 0.
ENDPROC

### Código en RAPID comentado rutina 2

PROC main()
    WaitDI DI_01,1;  // Espera hasta que la entrada DI_01 sea igual a 1.
    SETDO DO_03,1;   // Establece la salida DO_03 en 1.
    homee;           // Mueve el robot a la posición de origen (home).
    BaldeR;          // Toma el balde de la banda.
    homee;           // Vuelve a la posición de origen.
    BaldeR1;         // posiciona el balde.
    homee;           // Vuelve a la posición de origen.
    DOS;             // Toma ficha.
    SETDO DO_01,0;   // Establece la salida DO_01 en 0.
    SETDO DO_02,1;   // Establece la salida DO_02 en 1.
    WaitTime 3;      // Espera durante 3 segundos.
    DOS_;            // Retira la ficha.
    homee;           // Vuelve a la posición de origen.
    dejarFichas;     // Deja la ficha en el balde.
    WaitTime 3;      // Espera durante 3 segundos.
    SETDO DO_01,1;   // Establece la salida DO_01 en 1.
    SETDO DO_02,0;   // Establece la salida DO_02 en 0.
    WaitTime 2;      // Espera durante 2 segundos.
    homee;           // Vuelve a la posición de origen.

    CUATRO;             // Toma ficha.
    SETDO DO_01,0;   // Establece la salida DO_01 en 0.
    SETDO DO_02,1;   // Establece la salida DO_02 en 1.
    WaitTime 3;      // Espera durante 3 segundos.
    CUATRO_;            // Retira la ficha.
    homee;           // Vuelve a la posición de origen.
    dejarFichas;     // Deja la ficha en el balde.
    WaitTime 3;      // Espera durante 3 segundos.
    SETDO DO_01,1;   // Establece la salida DO_01 en 1.
    SETDO DO_02,0;   // Establece la salida DO_02 en 0.
    WaitTime 2;      // Espera durante 2 segundos.
    homee;           // Vuelve a la posición de origen.

    UNO;            // toma ficha.
    SETDO DO_01,0;   // Establece la salida DO_01 en 0.
    SETDO DO_02,1;   // Establece la salida DO_02 en 1.
    WaitTime 3;      // Espera durante 3 segundos.
    UNO_;           // Retira la ficha.
    homee;           // Vuelve a la posición de origen.
    dejarFichas;     // Deja la ficha en el balde.
    WaitTime 3;      // Espera durante 3 segundos.
    SETDO DO_01,1;   // Establece la salida DO_01 en 1.
    SETDO DO_02,0;   // Establece la salida DO_02 en 0.
    WaitTime 2;      // Espera durante 2 segundos.
    homee;           // Vuelve a la posición de origen.

    Balde1;          // Toma el balde.
    homee;           // Vuelve a la posición de origen.
    Balde2;          // Deja el balde en la banda.
    homee;           // Vuelve a la posición de origen.

    SETDO DO_03,0;   // Establece la salida DO_03 en 0.
ENDPROC

### Código en RAPID comentado rutina 3

PROC main()
    WaitDI DI_01,1;  // Espera hasta que la entrada DI_01 sea igual a 1.
    SETDO DO_03,1;   // Establece la salida DO_03 en 1.
    homee;           // Mueve el robot a la posición de origen (home).
    BaldeR;          // Toma el balde de la banda.
    homee;           // Vuelve a la posición de origen.
    BaldeR1;         // posiciona el balde.
    homee;           // Vuelve a la posición de origen.
    CINCO;             // Toma ficha.
    SETDO DO_01,0;   // Establece la salida DO_01 en 0.
    SETDO DO_02,1;   // Establece la salida DO_02 en 1.
    WaitTime 3;      // Espera durante 3 segundos.
    CINCO_;            // Retira la ficha.
    homee;           // Vuelve a la posición de origen.
    dejarFichas;     // Deja la ficha en el balde.
    WaitTime 3;      // Espera durante 3 segundos.
    SETDO DO_01,1;   // Establece la salida DO_01 en 1.
    SETDO DO_02,0;   // Establece la salida DO_02 en 0.
    WaitTime 2;      // Espera durante 2 segundos.
    homee;           // Vuelve a la posición de origen.

    CUATRO;             // Toma ficha.
    SETDO DO_01,0;   // Establece la salida DO_01 en 0.
    SETDO DO_02,1;   // Establece la salida DO_02 en 1.
    WaitTime 3;      // Espera durante 3 segundos.
    CUATRO_;            // Retira la ficha.
    homee;           // Vuelve a la posición de origen.
    dejarFichas;     // Deja la ficha en el balde.
    WaitTime 3;      // Espera durante 3 segundos.
    SETDO DO_01,1;   // Establece la salida DO_01 en 1.
    SETDO DO_02,0;   // Establece la salida DO_02 en 0.
    WaitTime 2;      // Espera durante 2 segundos.
    homee;           // Vuelve a la posición de origen.

    TRES;            // toma ficha.
    SETDO DO_01,0;   // Establece la salida DO_01 en 0.
    SETDO DO_02,1;   // Establece la salida DO_02 en 1.
    WaitTime 3;      // Espera durante 3 segundos.
    TRES_;           // Retira la ficha.
    homee;           // Vuelve a la posición de origen.
    dejarFichas;     // Deja la ficha en el balde.
    WaitTime 3;      // Espera durante 3 segundos.
    SETDO DO_01,1;   // Establece la salida DO_01 en 1.
    SETDO DO_02,0;   // Establece la salida DO_02 en 0.
    WaitTime 2;      // Espera durante 2 segundos.
    homee;           // Vuelve a la posición de origen.

    Balde1;          // Toma el balde.
    homee;           // Vuelve a la posición de origen.
    Balde2;          // Deja el balde en la banda.
    homee;           // Vuelve a la posición de origen.

    SETDO DO_03,0;   // Establece la salida DO_03 en 0.
ENDPROC

### Videos Practicas


https://github.com/jhoncale/ProyectoFinal/assets/38961990/eac9ea50-bf1c-4f78-a1bb-fe7e2439c565

## Link Video Practicas 1 :

https://drive.google.com/file/d/1oFnev54upwZJVqec9BJZPHnk7ruoa0DC/view?usp=sharing


https://github.com/jhoncale/ProyectoFinal/assets/38961990/0977e3f6-2d6e-45ab-b5c7-4acc791ac598


## Link Video Practicas 2:
https://drive.google.com/file/d/1oBXAJYnB9Pi0RpYk_sdbtGO2Jzgpy5Zl/view?usp=sharing


https://github.com/jhoncale/ProyectoFinal/assets/38961990/bc895c56-8c8e-4b5e-8f59-21ec5178b4ff

## Link Video Practicas 3:

https://drive.google.com/file/d/1o6IiLWZXW2qHetl45A-ulAo5ej4qDYc9/view?usp=sharing

### Videos Simulación


https://github.com/jhoncale/ProyectoFinal/assets/38961990/b699a21c-0a3e-431a-a13d-ef605c1ea5ca


https://github.com/jhoncale/ProyectoFinal/assets/38961990/23d31a96-2b1d-4fd9-8a18-9e1661bbe68b


https://github.com/jhoncale/ProyectoFinal/assets/38961990/ad7f7f6f-3e10-4cc6-b14b-8e51e5d886f0

### Videos ensayo manual


https://github.com/jhoncale/ProyectoFinal/assets/38961990/5645f9ab-1016-45e5-b0a1-7da17e7b02ca

## Link Video ensayo manual:

https://drive.google.com/file/d/1xLvYPt9U0iRJQcUBBdBOks4ML3qNelqq/view?usp=sharing

## Video Final

[![Alt text](https://img.youtube.com/vi/P5YUQemaV34/0.jpg)](https://www.youtube.com/watch?v=P5YUQemaV34)

## Link Video Final:
https://www.youtube.com/watch?v=P5YUQemaV34
