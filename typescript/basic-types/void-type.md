<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript/basic-types/void-type)
<!-- MULTILANGUAJE MENU END -->

<!--
# El tipo `void` y su uso en funciones

- Diferencias entre `void` y `undefined` en retornos
- Uso en funciones sin retorno explícito 
-->

# El tipo `void` y su uso en funciones

## Diferencias entre `void` y `undefined` en retornos

El tipo `void` en TypeScript se utiliza principalmente para indicar que una función no retorna ningún valor útil. Aunque en JavaScript el valor de retorno por defecto de una función sin `return` explícito es `undefined`, en TypeScript existe una distinción semántica importante entre ambos conceptos. Comprender esta diferencia es fundamental para escribir código más claro y seguro, especialmente en proyectos donde la tipificación estricta es relevante.

Cuando se declara una función con tipo de retorno `void`, se está indicando que el propósito de la función no es devolver un resultado, sino ejecutar una acción. Por el contrario, el tipo `undefined` puede utilizarse para señalar explícitamente que una función retorna el valor `undefined`, lo cual puede ser útil en casos donde se requiere distinguir entre la ausencia de valor y otros posibles resultados.

### Consideraciones técnicas y semánticas

- `void` restringe el uso de valores de retorno, permitiendo únicamente `undefined` o la ausencia de valor.
- `undefined` es un valor concreto en JavaScript y puede ser retornado explícitamente por una función.
- Utilizar `void` en la firma de una función comunica la intención de que el resultado no debe ser utilizado.

#### Ejemplo de función con retorno `void`

```typescript
function logMessage(message: string): void {
  // Esta función solo imprime un mensaje y no retorna ningún valor
  console.log(message);
}
```

#### Ejemplo de función con retorno `undefined`

```typescript
function returnUndefined(): undefined {
  // Retorna explícitamente el valor undefined
  return undefined;
}
```

### Casos de uso diferenciados

- Utilizar `void` es recomendable en funciones de tipo callback, controladores de eventos o cualquier función cuyo resultado no deba ser aprovechado por el llamador.
- El tipo `undefined` puede ser útil en APIs donde se requiere distinguir entre un valor retornado y la ausencia explícita de valor.

{% hint style="info" %}
Aunque en la práctica ambos enfoques pueden parecer equivalentes, el uso correcto de `void` y `undefined` mejora la legibilidad y la intención del código, facilitando el mantenimiento y la detección de errores.
{% endhint %}

### Buenas y malas prácticas

- **Buena práctica:** Utilizar `void` en funciones que solo ejecutan efectos secundarios y no retornan información relevante.
- **Mala práctica:** Retornar valores desde funciones tipadas como `void`, ya que puede generar confusión y advertencias del compilador.

---

## Uso en funciones sin retorno explícito

En TypeScript, si una función no especifica un tipo de retorno y no incluye una instrucción `return` con valor, el compilador infiere automáticamente el tipo de retorno como `void`. Esto es común en funciones que realizan acciones, como imprimir en consola, modificar el estado de una variable global o interactuar con el entorno, pero no devuelven información al llamador.

### Ejemplo de función sin retorno explícito

```typescript
function updateCounter(): void {
  // Incrementa un contador global, no retorna ningún valor
  globalCounter++;
}
```

### Casos de uso habituales

- Funciones de logging, notificación o auditoría.
- Controladores de eventos en interfaces gráficas o aplicaciones web.
- Callbacks en métodos como `forEach`, donde el valor de retorno es ignorado.

### Consideraciones importantes

- Si se intenta retornar un valor distinto de `undefined` en una función declarada como `void`, TypeScript generará un error de compilación.
- El uso de `void` ayuda a documentar la intención de la función y previene errores por mal uso del valor de retorno.

{% hint style="warning" %}
No abuses de funciones con tipo de retorno `void` para operaciones que realmente deberían devolver un resultado. Esto puede ocultar errores de diseño y dificultar la depuración.
{% endhint %}

### Errores comunes y trampas habituales

- Olvidar especificar el tipo de retorno en funciones que deberían devolver un valor puede llevar a errores sutiles, especialmente en proyectos grandes.
- Utilizar `void` en funciones que, en realidad, necesitan retornar información relevante puede dificultar la comprensión y el mantenimiento del código.

### Buenas y malas prácticas

- **Buena práctica:** Declarar explícitamente el tipo de retorno `void` en funciones que no retornan valor, para mejorar la claridad del código.
- **Mala práctica:** Ignorar el tipo de retorno y permitir que el compilador lo infiera en funciones complejas, ya que puede llevar a malentendidos sobre el propósito de la función. 