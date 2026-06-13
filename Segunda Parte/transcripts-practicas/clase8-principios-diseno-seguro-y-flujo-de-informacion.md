Bueno, entonces, los principios del diseño seguro, primero, mínimo privilegio.  
¿Qué era eso?  
Que darle el acceso mínimo que necesita a cada persona para hacer la actividad delito.  
Exactamente, muy bien.  
Valores por defecto seguros.  
Esto lo mencionamos el otro día cuando vimos lo de controles de acceso.  
¿Qué significa eso?  
Cuando yo no tengo en la lista alguien que conviene ser en la lista de control de activos, que conviene ser, no da permisos.  
Entonces con eso me aseguro que ante cualquier inconveniente, por las dudas no le doy permiso.  
Después, mecanismos simples, eso con qué tiene que ver.  
Mecanismo simple sería que los mecanismos, como lo dice la palabra, sean simples.  
Porque también van a favorecer a que sean más aceptables y más fáciles de llevar a cabo y de controlar.  
Entonces un mecanismo, si yo lo hago muy complicado, seguramente al final en la práctica no lo usen.  
Entonces mecanismo simple es por eso que digo que está relacionado con el último, que es la aceptación del usuario de los mecanismos.  
O sea, si los mecanismos no son aceptados por el usuario y no los ponen en práctica, bueno, ahí va a fallar toda la seguridad.  
Muchas veces es el problema ahí donde pasa, porque hay cosas muy buenas, pero si después no se aceptan o no son muy fáciles de implementar, al final no se usan.  
Bueno, después, mediación completa.  
Este tiene que ver con que todos los accesos a los objetos sean chequeados.  
O sea, si hay acceso a un objeto, se puede dar de diferentes maneras o en distintas vías.  
Todas esas formas tienen que ser chequeadas, o sea, no debería quedar una forma de acceder sin controlar.  
Después, diseño abierto, ¿con qué tiene que ver eso?  
O sea, como el principio de que la seguridad no consiste en mantener secreto el algoritmo, sino que el algoritmo puede estar abierto y mantener secreto, por ejemplo, la clave.  
Y no hacer tampoco que la complejidad sea parte de la seguridad.  
Después, separación de tareas, es un poco similar a lo de que no puede estar relacionado, que es que no puedo darle todo el control a un solo sujeto.  
Es conveniente separar según los distintos problemas para que no haya que depender de un único sujeto que si ese sujeto se equivoca o lo hace a propósito mal, bueno, todo se ha gestado.  
Mecanismos exclusivos.  
Ah, que los mecanismos usados para acceder a los recursos no deberían estar compartidos.  
Y bueno, y el último que ya dijimos, aceptación psicológica, que parece mentira, pero es uno de los más importantes, porque bueno, si los sujetos no lo aceptan o les cuesta mucho, por ejemplo, cuando acá nos llega cada seis meses, tiene que cambiar la cadena de señas, y bueno, es pesado.  
Entonces, esos principios convienen en los 50, después vamos a volver sobre algunos de estos cuando veamos lo que se ve en aplicaciones y cómo entonces esto nos va a marcar ciertos tipos de errores que vamos a ver, están justamente basados en que alguno de estos principios no se tuvo en cuenta.  
Entonces vamos a ver después qué tipos de vulnerabilidades y de problemas podemos llegar a tener si alguno de estos principios no se tiene en cuenta.  
¿Dudas?

Vamos a ver un poco del flujo de información.  
No sé si lo llegaron a ver esto, y aparte lo vamos a relacionar con lo del trabajo práctico y lo del completo de llamada.  
Flujo de información tiene que ver con lo dijimos un poco el día que hablamos de Bell-LaPadula, que tenía que ver con los procesos de escritura.  
Entonces por ejemplo en el modelo Bell-LaPadula la escritura se da hacia abajo, quiere decir que el flujo de información es hacia abajo.  
O sea, flujo de información, traspaso de información, y eso se puede llegar a medir y lo vamos a medir con la entropía.  
Entonces, ¿a qué llamamos flujo de información?  
Llamamos flujo de información si después de una secuencia de comandos, la información en X afecta o se propaga, pasa a la información en Y.  
Entonces, eso lo vamos a escribir de esta manera.  
Tenemos en el estado inicial los valores de los objetos, y el estado final los valores de los objetos.  
Y si yo puedo medir que hay algo en Y que me permite saber algo de lo que pasaba en X en el estado inicial, quiere decir que entonces hubo traspaso de información.  
Por ejemplo, si vos tenés que la variable Y se ve afectada por el valor de X, hubo flujo de información porque cuando yo veo Y, sé quién fue X.  
Entonces hay momentos donde eso puede ser más importante, porque a lo mejor es una fase de información que no quiero que exista y de alguna manera, por ahí tan directa, se puede llegar a dar.  
Entonces eso puede ser necesario medirlo o controlar.  
Para medirlo se hace con un concepto que se llama entropía.  
Entonces el concepto de entropía, en realidad nosotros hablamos de flujo de información sin hablar, si el flujo de información cuando mencionamos, ¿se acuerdan? la probabilidad de que el mensaje fuera M, tenía que ser igual la probabilidad de que el mensaje fuera M dado C igual a C.  
Cuando decíamos que esto tenía que ser igual para que tuviera secreto perfecto, es porque no queríamos que el cifrado me diera información sobre el mensaje original.  
O sea, no queríamos que hubiera traspaso de información al cifrado.  
Entonces esto está relacionado, cuando nosotros no queremos, por ejemplo, ahora vamos a relacionarlo con el tema del secreto de Shamir.  
¿Por qué? Porque en el secreto de Shamir se acuerdan que nosotros tenemos un esquema T, N, donde N es el número total de sombras y T son las que se necesitan para recuperar el secreto.  
Entonces, si yo quiero recuperar con menos de T sombras, se supone que no puedo.  
Bueno, en ese caso no debería poder.  
¿Qué significa eso? Que no hay traspaso de información del secreto a menos de T sombras.  
Ahora igual lo vamos a escribir apropiadamente con el concepto de entropía.  
Pero bueno, la idea es que entonces, si yo tengo menos de T sombras, no debería haber traspaso de información, pero cuando tengo justo T o más, ahí sí, y recupero el secreto.

Bien, entonces, ¿cómo se va a definir la entropía?  
Bueno, entropía además tiene un concepto físico, que es la cantidad de bits necesarias para codificar el X.  
O sea, la cantidad de información que hay en X.  
Entonces, ¿cómo es la cantidad de bits necesarias para codificar el X?  
Va a estar relacionado con el logaritmo base 2\.  
O sea, si yo necesito, por ejemplo, codificar valores entre 0 y 8, ¿cuántos bits necesito?  
Entre 0 y 7 serían 3 bits.  
Bueno, entonces ¿cómo lo sacamos esto? Porque es el logaritmo en base 2 de 8, que son los valores que tenemos ahí.  
Entonces ahora van a ver que en las fórmulas va a aparecer el logaritmo en base 2 porque eso me va a estar diciendo cuánto es lo que necesito para representar esta información.  
La cuestión es que entonces la entropía me va a medir la incertidumbre, y a mayor entropía, mayor incertidumbre.  
¿Por qué? Porque me falta información.  
Cuando empieza a haber traspaso de información, la entropía va a bajar.  
Y entonces reduce la incertidumbre porque con más información tengo más certeza.  
Por ejemplo, en pandemia, ¿y cuántos meses falta para que vuelva a estar todo abierto? y no teníamos la información, por lo tanto había mucha incertidumbre, porque nos faltaban datos para saber qué podía llegar a pasar.  
Entonces, mayor entropía, mayor incertidumbre.  
Baja la entropía, baja la incertidumbre y está asociado que tengo más información.  
Entonces, el límite va a ser que la entropía se convierta en cero.  
Cuando se convierte en cero es porque tengo mucha información, yo tengo toda la información que necesito, y eso me da certeza.  
Entonces, el flujo de información, o sea, al medir si se convirtió de algo, de un valor a un valor más bajo, es como que hubo traspaso de información.  
Acá tenía más incertidumbre, pasé a menos incertidumbre, quiere decir que acá tengo más información, o sea, hubo un flujo de información que permitió que baje la entropía.

Entonces, a ver si podemos escribir esto con la noción de entropía.  
Por ejemplo, suponiendo que T sea igual a 3, y S es el secreto, y S1, S2, S3 son las otras sombras.  
Entonces, si yo estoy diciendo que con menos de tres sombras no hay traspaso de información:  
\- La entropía del secreto dado S1 va a ser igual a la entropía del secreto, porque no sé nada más que lo que sabría cuando tenía el secreto solamente.  
\- La entropía del secreto dado S1 y S2, también va a ser eso mismo.  
\- Y la entropía de S dado S1, S2 y S3 daría 0\. ¿Por qué? Porque ya tengo toda la información.  
O sea que acá tenemos entropía 0, mientras que acá no hay flujo de información, si está bien hecho el sistema.  
Y si ven en algunos documentos que hablan sobre el secreto de Shamir o que muestran algún algoritmo de secreto compartido, van a ver esta anotación como para demostrar que el esquema es válido.

Entonces vamos a usar unas fórmulas muy interesantes, y tienen la siguiente fórmula que usan probabilidad y probabilidades condicionales cuando son cosas así como esta que tienen entropía condicional.  
Entonces si yo tengo que usar, las voy a anotar acá para poder usarlas para un ejemplo.  
Ahí es donde aparece el logaritmo base 2\.  
Entonces se suma las probabilidades de las variables que puede tomar esa variable por el logaritmo base 2 de esa probabilidad.  
Y la entropía condicional usa las probabilidades condicionales dado Y sub j para todas las posibilidades.

Bueno, ahora lo vemos en un ejemplo y vamos a ver cómo funciona esto.  
Entonces, supóngase que tenemos esta situación donde X es una variable que representa la actividad de un lado, y tenemos después que vemos otra variable Y que representa la suma de la tirada de un dado azul y un dado rojo.  
Dado azul y dado rojo para distinguir uno del otro, son dos dados que tienen los mismos seis valores.  
Entonces, ¿qué pasa? Cuando tengamos la variable que representa la suma, algo de información me va a dar sobre los dados que era, porque si me da por ejemplo 12 puedo saber cuáles eran.  
Si me dicen la suma pero no cuáles eran los dados, entonces acá va a haber flujo de información en la variable Y respecto de X.  
Pero bueno, vamos a ver a medirla.  
Entonces para medirla vamos a usar las formulitas.  
Por un lado vamos a utilizar la entropía de un dado solo, o sea, la entropía de X, que sería del dado solo, donde X entonces toma valores entre 1 y 6\.  
Y después vamos a sacar la entropía de X dado Y, donde Y puede tomar valores entre 2 y 12\.

Bueno, entonces vamos a ir aplicando las formulitas y a ver qué nos da.  
Ahí tenemos entonces que las probabilidades de X van a ser todas iguales porque si el dado no está cargado, se supone que todos salen con igual probabilidad.  
Pero la probabilidad de los valores de la suma ya no va a ser tan igual, porque hay más probabilidades de que me dé 7 a que me dé 12 o que me dé 2, porque 2 me puede dar de una sola manera y 12 me puede dar de una sola manera.  
Entonces ahí las probabilidades ya hacen que esto no sea uniforme.

Entonces ahora vamos a hacer a reemplazar en la fórmula y ver si hubo traspaso de información.  
Esto tendría que ser más grande que esto, o sea, la idea es probar que la entropía de X debería ser mayor que la entropía de X dado Y.  
Bueno, entonces vamos a escribir con esta fórmula.  
Primero usamos esta para la entropía de X:  
La entropía de X va a ser igual a menos la probabilidad, son todas iguales, así que yo acá puedo hacer 6 por 1/6 por el logaritmo en base 2 de 1/6.  
Esto va a dar menos el logaritmo en base 2 de 1/6.  
El logaritmo en base 2 de 1/6 va a ser negativo, y por eso es que se pone este menos a propósito, para que me quede positivo.

Y después hacemos la otra, la entropía de X dado Y, donde tenemos que considerar las probabilidades de las Y con todas las otras combinaciones.  
Entonces la entropía de X dado Y igual a:  
Menos la probabilidad de que Y valga 2, por ejemplo.  
Probabilidad de Y igual a 2 es 1/36. Pero la probabilidad de que X sea 1 cuando Y es igual a 2, es 1 (solo puede ser 1 con 1).  
Entonces logaritmo en base 2 de 1 va a dar 0\.  
Para Y igual a 3, ahora se empieza a repartir un poco más, porque si la suma es 3, puede ser 1 con 2 o 2 con 1\.  
Entonces la probabilidad de que X sea 1 cuando Y da la suma 3 es 0.5.  
Entonces sería 0.5 por logaritmo base 2 de 0.5 más 0.5 por logaritmo base 2 de 0.5.  
Y así se va resolviendo cada caso.

Cuestión que entonces el resultado final da 1,894.  
La entropía de X daba 2,58.  
La entropía de X dado Y da 1,894.  
Por lo tanto, hubo traspaso de información, porque la entropía de X dado Y es menor que la entropía de X.  
Quiere decir que cuando yo veo la suma, sé algo más sobre cuáles eran los dados.  
Entonces esto se verificó matemáticamente.

Eso sería como la forma de medirlo matemáticamente si uno quiere hacer una comprobación de un algoritmo.  
O como lo del sistema compartido o un esquema secreto, si quieren mostrar que realmente sirve, tienen que probarlo con este tipo de fórmula.

La cuestión es que el flujo de información puede darse de manera directa, por una asignación directa, donde ya digo bueno, Y es función de X, o sea que hay un traspaso seguro de la variable X a la variable Y.  
Y puede ser indirecta, a través de una asignación no explícita, algún comportamiento que hace que una variable dependa de la otra.  
Puede ser algo así, no tan complejo, o puede ser algo más complejo, pero que termina dándole a una variable el valor de otra, o parte del valor de otra, pero que te permite calcular de quién se trataba esa primera variable.

Entonces, si nosotros no queremos que haya flujos de información y aparecen flujos de información no deseados, vamos a hablar de canales ocultos.  
O sea, canales que me están permitiendo pasar información, pero que no son canales previstos para ese transporte de información.  
Esto es bastante complicado en algunos momentos de controlar.  
¿Por qué? Porque además hay situaciones donde se pueden usar recursos compartidos, como por ejemplo recursos espaciales —activos, directorios— o puede ser recursos temporales, o sea, usar la relación temporal entre accesos, siempre a un recurso compartido, para poder mandar información.  
Por ejemplo, poner un archivo y borrarlo en un determinado tiempo.  
Entonces si yo escribo un archivo y en un determinado momento se borra, se puede considerar como que se puede mandar información.  
Si cada tantos segundos no se borra, sería un cero; si cada tantos segundos aparece, sería un cero.  
Entonces sería un canal complicado, pero que se puede.  
Hay algunos experimentos que son esos, donde se puede mandar información a partir de usar un recurso temporal.  
Pero en general se va a poder hacer siempre que haya algún recurso compartido. Es bastante difícil a veces detectar este tipo de cosas.

La cuestión es que el aislamiento total es imposible, porque nosotros tendríamos que tener ese tipo de cosas, cosa que hoy por hoy no existe.  
No comparte recursos, comparte internet, comparte redes, comparte informaciones que necesita transmitir.  
Entonces, el aislamiento total no existe, pero se puede tratar de minimizar qué es lo que se comparte y en qué lugares.  
Entonces, aparecen algunos recursos que son, por ejemplo, aplicar mecanismos que refuercen el mínimo privilegio, o sea, dar al sujeto solamente los privilegios necesarios para completar alguna tarea.  
Aplicar algún mecanismo de control de acceso.  
Y sino utilizar estos dos recursos que se suelen dar, por ejemplo, virtual machine —o sea, máquinas virtuales— y sandboxes, que también igual están trabajando sobre algún recurso compartido, o sea que no es absoluto, pero limitan la transferencia de información, porque controlan rutas esperadas para el movimiento de datos.  
En la máquina virtual es la que regula el hardware de otros sistemas como si tuviera otro sistema.  
Y sandboxes para restringir algunas acciones de un proceso de acuerdo a una política de seguridad.  
Los sandboxes también se llaman "caja de arena", o sea, de arenero.  
Entonces es un entorno controlado. Digo, bueno, voy a probar tales cosas en este entorno controlado para no afectar al resto del sistema.

Y bueno, eso sería todo lo que necesitamos saber sobre el flujo de información.  
Después si quieren pueden investigar más sobre este tema.  
El concepto con la entropía, acuérdense que la entropía mínima va a ser cero y la máxima va a ser log2(n) dependiendo de la manera en que lo hagan.

Bueno, esto tiene para hacer unos ejercicios en la guía. Primero hay unos ejercicios para ver los principios de diseño y después hay unos ejercicios de todo el capítulo.

