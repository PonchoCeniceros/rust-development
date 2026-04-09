---
theme: default
title: Rust Development
info: |
  ## Resumen del lenguaje de programación Rust
transition: slide-left
layout: center
---

# Primitivas

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

---
layout: center
---

## Mónadas


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
