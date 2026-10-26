# 1: indentación de los bloques de prefijos

Cosas como `public:` o `end` están indentadas con -2 espacios, está bien? Debería cambiarse?
Y cuando se anidan?
Opciones:
A - Sí, está bien para notarlo, y se queda en los pisos acumulativos del editor.
B - Deberían estar al mismo nivel que su interior.
C - El interior debería indentarse completamente, dejando los prefijos al nivel base.
D - Los prefijos al mismo nivel pero `end` a -2, o viceversa.
E - Los prefijos a -2, y `end` a -1. La razón es que en las breadcrumbs por tabulación
    se vea el bloque y también si ya ha sido el final, aunque puede ser algo particular.
F - Otras ideas de formateo.

# 2: indentación de `case`

Los `case` pueden indentarse de varias maneras, en el código del interior de los casos.
Opciones:
A - Al nivel -4 y el resto a tabulación normal de llaves.
B - Al nivel -2 y el resto al nivel normal también.
C - Al nivel de llaves e indentar una vez extra el código.
D - Otras ideas de formateo.

# 3: orden en versionado de arrays con campos

Cuando se usan campos se pierde el orden, como si fueran argumentos por nombre en python.
Para decir que se usa el nombre de una posición pero mantener el orden se usa `..`, es bueno?
Opciones:
A - Sí, está bien por claridad, aunque al principio sea algo a tener en cuenta.
B - No, solo molesta, el compilador debería saber si estaba en orden o no, aunque puede hacer
    que haya magia de compilador, y que este deba ser más complejo.
C - Debería ser opcional, mediante flags, aunque eso rompe la coherencia del ecosistema.
D - Otras ideas de gramática.

# 4: los tipos con arrays abiertos o cerrados

Al especificar un tipo array con posibles más elementos, se usan puntos suspensivos. Eso impide
que un tipo fijo pueda ser heredado para uno con más elementos, que de otra manera deberá ser compatible
con un slice de esa parte, y quizá un casteo. Se mantienen los puntos suspensivos o son demasiado restrictivos?
A - Se quitan, que los de más elementos se recorten silenciosamente, pero sean compatibles si así se declararon.
B - Mantener los puntos suspensivos, por coherencia y por transparencia, a costa de requerir más código para
    que sea compatible.
C - Otras ideas de gramática.

# 5: nivel de personalización del formateo

Debería admitir configuración o ser más fijo como el de Go? Siendo clang-format el 10 de flexibilidad
y 0 el formateador de Go, o el de Gleam, qué nivel debería tener?
Opciones:
A - Nivel intermedio.
B - Debería ser flexible, al igual que la sintaxis de Monom es flexible y a veces algunas cosas son mejores compactas,
    o de otras maneras. En todo caso, no debería unificar estilos de condicionales, por ejemplo.
C - Muy estricto y organizado, a costa de más espacio vertical.
D - Estilo coherente, pero siendo más complejo (y lento quizá) para tener un formateo rico basado en el contexto
    y las longitudes de las expresiones, pudiendo mezclar estilos.
E - Otras ideas de formateo.

# 6: formateo de espaciado cuando hay paréntesis juntos

Cuando hay un par de paréntesis con paréntesis justo en su interior, aunque sean de pares distintos,
el formateador debe añadir un espacio para legibilidad. Es decir, de `((), ())` se pasa a `( (), () )`,
y de `((x, y) => 5 * (x + y))` se pasa a `( (x, y) => 5 * (x + y) )`.
Opciones:
A - No, el formateo estándar actual es al que están acostumbrados la mayoría de desarrolladores, y complicaría
    el formateador innecesariamente para una opción que debería ser voluntaria, no impuesta.
B - Es buena idea para legibilidad, la metemos. Quizá como estándar oficial.
C - No merece la pena el trabajo y la complejidad, ignoramos la opción y quizá en el futuro se añada como voluntaria.
D - Es buena idea, pero con la sutileza de que paréntesis cerrados o lambdas no son afectadas,
    a costa de complejidad y lentitud.
E - Otras ideas de formateo.

# 7: formateo del tipo en funciones

Cuando se pone el tipo de una función (llamada mejor dicho), se ponen los dos puntos a continuación,
al estilo `f(): Int32`. Mejor así, espaciando siempre o espaciando solo cuando se trata de una declaración?
Hablamos de espaciado a poner en su lugar `f(): Int32`.
Opciones:
A - Dejarlo como está, da consistencia, es más común y ahorra un caracter.
B - Espaciarlo cuando se trata de una llamada, a costa de un pequeño desperdicio de espacio si se usa
    mucho en el código. Es consistente entre llamadas, aunque ya no con lo demás.
C - Espaciar solo en las llamadas de declaraciones, por claridad y para que resalte en base al formato
    que se trata de algo especial (como es una declaración). Sin embargo, complica un poco el formateador
    por deber este saber si se encuentra en una declaración. Se convierte en una excepción, que quizá por serlo
    llama la atención.
D - Espaciar en todo, al estilo de OCaml, aunque es menos común. Tiene de ventaja que es más legible la firma,
    aunque puede ser menos popular. Ocupa un poco más de espacio, pero da claridad.
E - Otras ideas de formateo.

# 8: indentación de `with` con `in`

En las formas sintácticas con llaves, qué formateo sería el más adecuado? Por un lado, lo del scope de `with` es visible
en el de `in`, por lo que una indentación que así lo reflejara no estaría mal. Sin embargo, que el formateo sea especial,
como el de secciones de prefijo, podría dar sensación de abuso de diferentes indentaciones, por hacer la de -2 más frecuente
de lo que quizá debería. Quizá un formateo especial dificulta la flexibilidad de expresiones.
Opciones:
A - Dejar `with` e `in` (delimitadores) al nivel base e indentar el interior.
B - Poner los delimitadores al nivel -2 y los interiores al base.
C - Poner los delimitadores al nivel base pero indentar el interior a 2.
D - Otras ideas de formateo.

# 9: opinión sobre la redundancia de operadores

Tiene ventajas y desventajas. La principal ventaja es de legibilidad, pienso, aunque tiene de inconveniente
que no es muy frecuente y que entender los enlaces a priori y a posteriori puede costar.

# 10: los dos puntos en las longitudes de los arrays

Hasta ahora habíamos dejado las longitudes con los dos puntos, y solo permitido omitirlos en los de longitud
indeterminada. No obstante, puede parecer un engorro de sintaxis. Se hace por simplicidad del parser, aunque
puede ser que no sea tan complicado solventarlo. Requeriría más lookahead del parser, y entonces invalidaría en parte
la excepcionalidad de esos trozos de sintaxis para poder meter características futuras como comas o enumeraciones.
Opciones:
A - Están bien, no son tan molestos.
B - Molestan demasiado, paguemos el precio en el compilador y del lenguaje a cambio de limpieza.
C - Otras ideas de gramática.

# TODO: la captura del entorno en lambdas

Las closures deberían ser más explícitas? Cómo? Quizá con warnings y control del compilador, por sintaxis,
o dejando el funcionamiento actual, a costa de rendimiento y consumo de memoria? LA forma actual da mucha libertad
y limpieza, pero como siempre, eso permite ser menos consciente de posibles implicaciones de rendimiento.

Se admiten las reflexiones y análisis para elaborar las opciones.

# TODO: hacer visibles las optimizaciones del compilador en opciones

Que cuando tienes `[](A | B)` frente a `[]A | []B` funcione sin pensarlo más o que requiera que seas consciente
de que el hecho de que cada elemento pueda ser diferente frente a todos homogéneos tiene coste de rendimiento?
Habría que considerar si de verdad se necesita saber eso del rendimiento, pues si no basta con no usarlo,
sin requerir en caso de hacerlo nada más. No obstante, la sintaxis tan limpia (una ventaja) puede hacer
que caer en el error, tan sutil, sea fácil, cuando no se necesita polimorfismo individual. En caso de querer
hacerlo visible, podría optarse por algo que haga explícita la optimización de la implementación del compilador,
lo que abriría las puertas a que el usuario tenga más control para crear tipos propios, aunque haría que la transitividad en tipos complejos se vea gravemente resentida.
Opciones:
A - Dejar la sintaxis limpia, asumiendo los pequeños lastres de rendimiento del usuario a cambio de comodidad.
    Quizá emitir warnings configurables (o errores si así se promocionan).
B - Control explícito, a costa de firmas de tipos notablemente recargadas. Menos magia.
C - Dejar la sintaxis limpia, siendo más permisivos incluso, a costa de un compilador que debería estudiar los caminos
    del código para optimizar y que, si se usa la forma poco eficiente como la más eficiente, mute el tipo
    para mejorar el código, con tiempos de compilación algo más lentos y mayor complejidad.
D - Otras ideas de gramática.