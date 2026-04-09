---
theme: default
title: Rust Development
info: |
  ## Resumen del lenguaje de programación Rust
transition: slide-left
layout: center
---

# Primitivas

Tipos básicos integrados al lenguaje, sin overhead de heap allocation y base para construir tipos complejos.

---
layout: center
---

## Numéricos

| Tipo | Descripción | Ejemplo |
|------|-------------|---------|
| `i8, i16, i32, i64, i128, isize` | Enteros con signo (complemento a 2) | `let x: i32 = -42;` |
| `u8, u16, u32, u64, u128, usize` | Enteros sin signo | `let x: u8 = 255;` |
| `f32, f64` | Flotantes IEEE 754 | `let x: f64 = 3.14;` |


> **Nota**: `usize`/`isize` dependen de la arquitectura (64 bits por lo general).

---
layout: center
---

## Caracteres

| Tipo | Descripción | Ejemplo |
|------|-------------|---------|
| `char` | Carácter Unicode (32 bits) | `'a'`, `'ñ'`, `'👨🏻‍💻'` |
| `str` | String slice (referencia, sin ownership) | `"hola"` |
| `String` | String con ownership (heap) | `String::from("hola")` |

---
layout: center
---

## Especiales

| Tipo | Descripción | Ejemplo |
|------|-------------|---------|
| `bool` | Booleano | `let flag: bool = true;` |
| `()` | Unit (equivalente a void) | `()` |


---
layout: center
---

## Colecciones

| Tipo | Descripción | Ejemplo |
|------|-------------|---------|
| `[T; N]` | Array con longitud fija | `[1, 2, 3]` |
| `(T, U, ...)` | Tupla con tipos heterogéneos | `(42, "hola", true)` |

---
layout: center
---

# Estructuras de control

Flujo del programa (secuencia, selección, repetición) donde `if/loop/match` son expresiones y todo bloque retorna un valor.

---

## Bloques (llaves)

<br>

```rust

fn main() {

  let resl = {
    let a = 10;
    let b = 5;
    a + b  // última expresión es el valor de retorno
  };

  println!("resultado == {}", resl);
}
```

---

## Condicionales

<br>

```rust
let calificacion = 85;

let mensaje = if calificacion >= 90 {
    "Aprobado con honores"
} else if calificacion >= 70 {
    "Aprobado"
} else {
    "Reprobado"
};
```
<br>

```rust
let valor = if true { 42 } else { 0 };
```

---

## _pattern matching_

<br>

```rust
let clima = "lluvia";

match clima {
    "sol"    => println!("Lleva gafas de sol"),
    "lluvia" => println!("Lleva paraguas"),
    // El guion bajo es el "comodín" (cualquier otra cosa)
    _        => println!("Mejor quédate en casa"),
}
```

---

## Ciclos

<br>

### `loop`

<br>

````md magic-move

```rust
let mut tries = 0;

let resl = loop {
    tries += 1;
};
```

```rust
let mut tries = 0;

let resl = loop {
    tries += 1;
    if tries == 3 {
        break "Listo después de 3 intentos";
    }
};

```
````

---

## Ciclos

<br>

### `while`

<br>

```rust
let mut count = 0;

while count < 5 {
    println!("Contando: {}", count);
    count += 1;
}
```

---

## Ciclos

<br>

### `for`

<br>

````md magic-move

```rust
//iterar sobre rangos
for i in 0..=5 { // 0, 1, 2, 3, 4, 5
    print!("{} ", i);
}
```

```rust
//iterar sobre rangos
for i in 0..=5 { // 0, 1, 2, 3, 4, 5
    print!("{} ", i);
}

// iterar sobre colecciones
let colors = ["rojo", "verde", "azul"];

for color in colors.iter() {
    println!("Color: {}", color);
}
```

```rust
//iterar sobre rangos
for i in 0..=5 { // 0, 1, 2, 3, 4, 5
    print!("{} ", i);
}

// iterar sobre colecciones
let colors = ["rojo", "verde", "azul"];

for color in colors.iter() {
    println!("Color: {}", color);
}

// Iterar con enumerate (índice + valor)
let arr = [10, 20, 30];

for (idx, val) in arr.iter().enumerate() {
    println!("[{}] = {}", idx, val);
}
```
````

---
layout: center
---

# Estructuras de datos

Tipos compuestos para organizar información, mónadas para manejo de ausencia/errores, y collections para grupos de elementos.

---
layout: center
---

## `Vec<T>`

| Método | Descripción | Ejemplo |
|--------|-------------|---------|
| `new()` | Crea vector vacío | `Vec::<i32>::new()` |
| `vec![]` | Macro para inicializar | `vec![1, 2, 3]` |
| `push()` | Agrega elemento | `v.push(4)` |
| `pop()` | Retira último elemento | `v.pop()` |
| `get()` | Acceso seguro con índice | `v.get(0)` |
| `len()` | Cantidad de elementos | `v.len()` |

---

### `Vec<T>` - Ejemplo

<br>

````md magic-move

```rust
let mut v: Vec<i32> = Vec::new();

v.push(10);
v.push(20);
v.push(30);

println!("Vector: {:?}", v);
```

```rust
let mut v: Vec<i32> = Vec::new();

v.push(10);
v.push(20);
v.push(30);

println!("Vector: {:?}", v);
println!("Longitud: {}", v.len());

// Acceso seguro con get()
if let Some(elem) = v.get(1) {
    println!("Elemento en [1]: {}", elem);
}
```
````

---

### `Vec<T>` - Iteración

<br>

```rust
let v = vec![10, 20, 30];

for elem in &v {
    println!("{}", elem);
}
```

---
layout: center
---

## `HashMap<K,V>`

| Método | Descripción | Ejemplo |
|--------|-------------|---------|
| `new()` | Crea HashMap vacío | `HashMap::<String, i32>::new()` |
| `insert()` | Agrega par clave-valor | `map.insert("edad", 25)` |
| `get()` | Obtiene valor por clave | `map.get("edad")` |
| `contains_key()` | Verifica existencia | `map.contains_key("edad")` |

---

### `HashMap` - Ejemplo

<br>

```rust
use std::collections::HashMap;

let mut edades = HashMap::new();

edades.insert("Ana", 25);
edades.insert("Luis", 30);

if let Some(edad) = edades.get("Ana") {
    println!("Edad de Ana: {}", edad);
}
```

---
layout: center
---

## `HashSet<T>`

| Método | Descripción | Ejemplo |
|--------|-------------|---------|
| `new()` | Crea HashSet vacío | `HashSet::<i32>::new()` |
| `insert()` | Agrega elemento | `set.insert(1)` |
| `contains()` | Verifica existencia | `set.contains(&1)` |

---

### `HashSet` - Ejemplo

<br>

```rust
use std::collections::HashSet;

let mut set: HashSet<i32> = HashSet::new();

set.insert(1);
set.insert(2);
set.insert(3);

println!("Contiene 2: {}", set.contains(&2));
```

---
layout: center
---

## Mónadas

Tipos que encapsulan ausencia de valor (`Option`) o posibles errores (`Result`), permitiendo encadenar operaciones de forma segura.

| Tipo          | Estados         | Encadenamiento | Uso principal |
|---------------|----------------|----------------|--------------|
| `Option<T>`   | `Some`, `None` | `.and_then` / `?` | Valor opcional |
| `Result<T,E>` | `Ok`, `Err`    | `.and_then` / `?` | Manejo de errores |

---

### `Option<T>`

<br>

````md magic-move
```rust
fn divide(a: f64, b: f64) -> Option<f64> {
  if b == 0.0 { None } else { Some(a / b) }
}
```

```rust
fn divide(a: f64, b: f64) -> Option<f64> {
  if b == 0.0 { None } else { Some(a / b) }
}

fn main() {
    let a = divide(10.0, 2.0); // Some(5.0)
}
```

```rust
fn divide(a: f64, b: f64) -> Option<f64> {
  if b == 0.0 { None } else { Some(a / b) }
}

fn main() {
  let a = divide(10.0, 2.0);              // Some(5.0)
  // and_then encadena operaciones que regresan Option
  let b = a.and_then(|x| divide(x, 0.0)); // None
}
```

```rust
fn divide(a: f64, b: f64) -> Option<f64> {
  if b == 0.0 { None } else { Some(a / b) }
}

fn main() {
  let a = divide(10.0, 2.0);
  let b = a.and_then(|x| divide(x, 0.0));

  // manejamos la respuesta obtenida
  match b {
    Some(x) => println!("resultado: {}", x),
    None => println!("no hay valor"),
  }
}
```
````

---

### `Result<T, E>`

<br>

````md magic-move

```rust
fn divide(a: f64, b: f64) -> Result<f64, String> {
  if b == 0.0 { Err("error".to_string()) } else { Ok(a / b) }
}
```

```rust {*|6|7|*}
fn divide(a: f64, b: f64) -> Result<f64, String> {
  if b == 0.0 { Err("error".to_string()) } else { Ok(a / b) }
}

// main también puede regresar Result
fn main() -> Result<(), String> {
  Ok(())
}
```

```rust
fn divide(a: f64, b: f64) -> Result<f64, String> {
  if b == 0.0 { Err("error".to_string()) } else { Ok(a / b) }
}

fn main() -> Result<(), String> {
  let a = divide(10.0, 2.0);              // Ok(5.0)
  let b = a.and_then(|x| divide(x, 0.0)); // Err("error")

  Ok(())
}
```

```rust
fn divide(a: f64, b: f64) -> Result<f64, String> {
  if b == 0.0 { Err("error".to_string()) } else { Ok(a / b) }
}

fn main() -> Result<(), String> {
  let a = divide(10.0, 2.0);              // Ok(5.0)
  let b = a.and_then(|x| divide(x, 0.0)); // Err("error")

  // manejamos la respuesta obtenida
  match b {
    Ok(x) => println!("resultado: {}", x),
    Err(e) => println!("error: {}", e),
  }

  Ok(())
}
```
````

---

### Operador `?`

<br>

````md magic-move

```rust
let resl = divide(10.0, 2.0)
           .and_then(|x| divide(x, 5.0))
           .and_then(|y| divide(y, 0.0));

```

```rust
// internamente el valor de la operacion se maneja asi
let resl = divide(10.0, 2.0)        // Ok(5.0)
           .and_then(|x|            // x = 5.0
                     divide(x, 5.0) // Ok(1.0)
           )
           .and_then(|y|            // y = 1.0
                     divide(y, 0.0) // Err("error")
           )
```

```rust
// si queremos ponerlo en terminos de match pattern
let x = match divide(10.0, 2.0) {
    Ok(v) => v,
    Err(e) => return Err(e),
};

let y = match divide(x, 5.0) {
    Ok(v) => v,
    Err(e) => return Err(e),
};

let resl = match divide(y, 0.0) {
    Ok(v) => v,
    Err(e) => return Err(e),
};

```

```rust
let x = divide(10.0, 2.0)?;    // 5.0
let y = divide(x, 5.0)?;       // 1.0
let resl = divide(y, 0.0)?;    // Err → termina aquí
```
````

---
layout: center
---

# Iteradores

Un iterador produce una secuencia de valores compuesta por fuente (`iter()`), adaptadores que la transforman (`map`, `filter`) y consumidores que producen el resultado final (`collect`, `sum`).

---
layout: center
---


## Adaptadores comunes

| Método | Descripción | Ejemplo |
|--------|-------------|---------|
| `map` | Transforma cada elemento | `.map(\|x\| x * 2)` |
| `filter` | Filtra elementos | `.filter(\|x\| x > 0)` |
| `take(n)` | Toma los primeros n | `.take(3)` |
| `skip(n)` | Omite los primeros n | `.skip(2)` |
| `enumerate` | Incluye índice | `.enumerate()` |
| `zip` | Combina dos iteradores | `.zip(b.iter())` |

---
layout: center
---

## Consumidores comunes

| Método | Descripción | Ejemplo |
|--------|-------------|---------|
| `collect` | Convierte a colección | `.collect::<Vec<_>>()` |
| `sum` | Suma todos los elementos | `.sum()` |
| `fold` | Acumulador personalizado | `.fold(0, \|acc, x\| acc + x)` |
| `for_each` | Ejecuta para cada elemento | `.for_each(print!)` |

---

## Iteradores - Pipeline básico

<br>

````md magic-move

```rust
let nums = vec![1, 2, 3, 4, 5];

let result: Vec<i32> = nums.iter()
    .map(|x| x * 2)
    .collect();
```

```rust
let nums = vec![1, 2, 3, 4, 5];

let result: Vec<i32> = nums.iter()
    .map(|x| x * 2)
    .filter(|x| x > &4)
    .collect();

println!("{:?}", result); // [6, 8, 10]
```
````

---

## Iteradores - enumerate

<br>

```rust
let frutas = vec!["manzana", "pera", "uva"];

for (i, fruta) in frutas.iter().enumerate() {
    println!("[{}] {}", i, fruta);
}
```
