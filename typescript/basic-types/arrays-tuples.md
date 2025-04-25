<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript/basic-types/arrays-tuples)
<!-- MULTILANGUAJE MENU END -->

<!--
# Arrays y Tuplas en TypeScript

- Declaración de arrays (`number[]`, `Array<string>`)
- Uso de tuplas (`[string, number]`)
- Tuplas con etiquetas (`[id: number, nombre: string]`)
-->

# Arrays y Tuplas en TypeScript

## Declaración de arrays (`number[]`, `Array<string>`)

En TypeScript, los arrays permiten almacenar colecciones de elementos del mismo tipo, lo que facilita la manipulación y el acceso a datos de manera estructurada y segura. Existen dos formas principales de declarar arrays: utilizando la sintaxis de corchetes (`number[]`) o la forma genérica (`Array<string>`). Ambas son equivalentes en funcionalidad, pero pueden preferirse en distintos contextos según las convenciones del equipo o la claridad del código.

El uso de arrays es fundamental en el desarrollo de aplicaciones, ya que permite gestionar listas de datos, realizar operaciones de filtrado, mapeo y reducción, y aprovechar métodos avanzados de manipulación.

### Ventajas y consideraciones

- Permiten tipado estricto, evitando errores comunes de asignación de tipos.
- Facilitan la integración con métodos de los arrays de JavaScript, pero con comprobación de tipos en tiempo de compilación.
- Es posible definir arrays de cualquier tipo, incluyendo tipos personalizados y objetos complejos.

#### Ejemplo básico de declaración y uso de arrays

```typescript
// Declaración de un array de números
const numbers: number[] = [1, 2, 3, 4, 5];

// Declaración de un array de cadenas usando la sintaxis genérica
const names: Array<string> = ['Alice', 'Bob', 'Charlie'];

// Acceso a elementos y métodos
const firstNumber = numbers[0]; // Primer elemento
names.push('Diana'); // Añade un nuevo nombre
```

En este ejemplo, los identificadores están en inglés y los comentarios explican en castellano neutro cada operación realizada.

### Casos de uso recomendados

- Almacenar listas de datos homogéneos, como identificadores, nombres o resultados de operaciones.
- Procesar colecciones de datos provenientes de APIs o bases de datos.
- Implementar algoritmos que requieran manipulación de secuencias, como ordenamientos o búsquedas.

### Consideraciones importantes y advertencias

{% hint style="warning" %}
El acceso a índices fuera del rango del array no genera errores en tiempo de ejecución, pero puede devolver `undefined`. Es recomendable validar siempre los índices antes de acceder a los elementos.
{% endhint %}

- Los arrays en TypeScript no restringen el tamaño por defecto, lo que puede llevar a errores si no se controla adecuadamente la longitud esperada.
- Es posible crear arrays de tipos complejos, pero se debe prestar atención a la mutabilidad de los objetos almacenados.

### Buenas prácticas

- Utilizar siempre el tipado explícito para arrays, evitando el uso de `any[]` salvo en casos justificados.
- Preferir la sintaxis que mejor se adapte al contexto del proyecto y a las convenciones del equipo.
- Evitar modificar arrays directamente si se requiere inmutabilidad; en su lugar, utilizar métodos que devuelvan nuevos arrays.

### Malas prácticas

- Mezclar tipos dentro de un mismo array, lo que puede provocar errores difíciles de depurar.
- No validar la longitud del array antes de acceder a elementos por índice.
- Utilizar arrays para almacenar datos heterogéneos cuando existen alternativas más adecuadas como las tuplas o los objetos.

### Errores comunes

- Olvidar especificar el tipo de los elementos del array, lo que puede llevar a la inferencia de tipo `any` y pérdida de seguridad de tipos.
- Confundir la sintaxis de arrays con la de objetos, especialmente al inicializar estructuras complejas.

---

## Uso de tuplas (`[string, number]`)

Las tuplas en TypeScript permiten definir colecciones de elementos de diferentes tipos con una longitud fija y un orden específico. Son especialmente útiles cuando se requiere representar una estructura de datos donde cada posición tiene un significado y tipo concreto.

A diferencia de los arrays, donde todos los elementos son del mismo tipo, las tuplas permiten mezclar tipos y asignar un propósito claro a cada posición.

### Ventajas y consideraciones

- Garantizan el orden y el tipo de cada elemento, lo que mejora la legibilidad y la seguridad del código.
- Son ideales para devolver múltiples valores de una función con significado específico.
- Facilitan la desestructuración y el acceso directo a los valores.

#### Ejemplo de declaración y uso de tuplas

```typescript
// Declaración de una tupla con un string y un número
const user: [string, number] = ['Alice', 30];

// Acceso a los elementos de la tupla
const name = user[0]; // 'Alice'
const age = user[1]; // 30
```

En este ejemplo, se observa cómo cada posición de la tupla tiene un tipo y un propósito definido.

### Casos de uso recomendados

- Representar pares clave-valor o estructuras de datos compactas.
- Devolver múltiples valores de una función, como resultado y estado.
- Modelar datos que requieren un orden y tipo fijo, como coordenadas o configuraciones.

### Consideraciones importantes y advertencias

{% hint style="info" %}
Las tuplas pueden extenderse mediante métodos como `push`, pero esto puede romper la seguridad de tipos si no se controla adecuadamente. Es recomendable evitar modificar la longitud de las tuplas una vez definidas.
{% endhint %}

- El acceso a elementos fuera del rango definido por la tupla puede resultar en errores de tipo en tiempo de compilación.
- Las tuplas no son adecuadas para colecciones de tamaño variable o datos heterogéneos sin un orden fijo.

### Buenas prácticas

- Definir siempre el tipo exacto y el orden de los elementos de la tupla.
- Utilizar tuplas solo cuando el significado de cada posición sea claro y necesario.
- Evitar modificar la longitud de la tupla después de su declaración.

### Malas prácticas

- Usar tuplas para almacenar datos sin un orden o significado claro.
- Modificar la longitud de la tupla, lo que puede llevar a inconsistencias y errores de tipo.
- Confundir tuplas con arrays convencionales, ya que su propósito y uso son diferentes.

### Errores comunes

- Intentar acceder a posiciones no definidas en la tupla.
- Olvidar el orden y el tipo de los elementos al desestructurar la tupla.

---

## Tuplas con etiquetas (`[id: number, nombre: string]`)

TypeScript permite añadir etiquetas descriptivas a los elementos de una tupla, mejorando la legibilidad y el mantenimiento del código. Las etiquetas no afectan la ejecución, pero proporcionan contexto sobre el propósito de cada elemento.

Esta característica es especialmente útil en proyectos grandes o cuando se trabaja en equipo, ya que facilita la comprensión del significado de cada posición en la tupla.

### Ventajas y consideraciones

- Mejoran la documentación y la claridad del código sin afectar el rendimiento.
- Ayudan a evitar errores al dejar claro el propósito de cada elemento.
- Son compatibles con todas las funcionalidades de las tuplas estándar.

#### Ejemplo de tupla con etiquetas

```typescript
// Declaración de una tupla con etiquetas
const product: [id: number, name: string] = [101, 'Laptop'];

// Acceso a los elementos
const productId = product[0]; // 101
const productName = product[1]; // 'Laptop'
```

En este ejemplo, las etiquetas `id` y `name` aclaran el propósito de cada posición, facilitando el mantenimiento y la revisión del código.

### Casos de uso recomendados

- Modelar entidades con pocos atributos y significado claro para cada posición.
- Mejorar la documentación interna del código en funciones que devuelven tuplas.
- Facilitar la revisión y el mantenimiento en equipos de desarrollo.

### Consideraciones importantes y advertencias

{% hint style="success" %}
Las etiquetas en las tuplas son solo descriptivas y no afectan la ejecución ni el tipado en tiempo de ejecución. Su uso es recomendable para mejorar la claridad, pero no sustituye una documentación adecuada.
{% endhint %}

- No deben utilizarse como sustituto de los objetos cuando se requiere flexibilidad o un número variable de propiedades.
- Las etiquetas no son accesibles en tiempo de ejecución, solo sirven como ayuda visual en el código fuente.

### Buenas prácticas

- Utilizar etiquetas descriptivas y concisas que reflejen el propósito de cada elemento.
- Documentar el uso de tuplas con etiquetas en funciones y estructuras complejas.
- Revisar periódicamente la claridad de las etiquetas para evitar ambigüedades.

### Malas prácticas

- Usar etiquetas genéricas o poco descriptivas que no aporten claridad.
- Abusar de las tuplas con etiquetas en lugar de emplear objetos cuando sea más adecuado.
- Olvidar actualizar las etiquetas al modificar la estructura de la tupla.

### Errores comunes

- Creer que las etiquetas afectan el comportamiento en tiempo de ejecución.
- No mantener la coherencia entre las etiquetas y el significado real de los datos.
