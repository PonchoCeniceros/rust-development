---
theme: default
title: Rust Development
info: |
  ## Resumen del lenguaje de programación Rust
transition: slide-left

---

# Primitivas

---

## Numéricos

| Tipo | Descripción | Ejemplo |
|------|-------------|---------|
| `i8, i16, i32, i64, i128, isize` | Enteros con signo (complemento a 2) | `let x: i32 = -42;` |
| `u8, u16, u32, u64, u128, usize` | Enteros sin signo | `let x: u8 = 255;` |
| `f32, f64` | Flotantes IEEE 754 | `let x: f64 = 3.14;` |


> **Nota**: `usize`/`isize` dependen de la arquitectura (64 bits por lo general).

---

## Caracteres

| Tipo | Descripción | Ejemplo |
|------|-------------|---------|
| `char` | Carácter Unicode (32 bits) | `'a'`, `'ñ'`, `'👨🏻‍💻'` |
| `str` | String slice (referencia, sin ownership) | `"hola"` |
| `String` | String con ownership (heap) | `String::from("hola")` |

---

## Especiales

| Tipo | Descripción | Ejemplo |
|------|-------------|---------|
| `bool` | Booleano | `let flag: bool = true;` |
| `()` | Unit (equivalente a void) | `()` |


---

## Colecciones

| Tipo | Descripción | Ejemplo |
|------|-------------|---------|
| `[T; N]` | Array con longitud fija | `[1, 2, 3]` |
| `(T, U, ...)` | Tupla con tipos heterogéneos | `(42, "hola", true)` |

---

# Estructuras de control

---

## Bloques (llaves)

<br>

```rust
let resultado = {
    let a = 10;
    let b = 5;
    a + b  // última expresión es el valor de retorno
};
// resultado == 15
```

<br>

```rust
// Uso práctico
let mensaje = {
    let nombre = "Rust";
    let version = 2021;
    format!("{} v{}", nombre, version)
};
```
---

## Condicionales

<br>

```rust
// If como expresión (devuelve valor)
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
// If con inicialización
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

<br>

```rust
let regalo = Some("Bicicleta");

match regalo {
    Some(cosa) => println!("¡Recibiste una {}!", cosa),
    None => (), // No hacer nada
}
```

---

## Ciclos

<br>

```rust
// loop - uso para operaciones retry o infinite loops
let mut intentos = 0;
let resultado = loop {
    intentos += 1;
    if intentos == 3 {
        break "Listo después de 3 intentos";
    }
};
```

<br>

```rust
// while - cuando necesitas condición
let mut contador = 0;
while contador < 5 {
    println!("Contando: {}", contador);
    contador += 1;
}
```

---

## Ciclos

<br>

```rust
// for - iterar sobre rangos
for i in 0..=5 {           // 0, 1, 2, 3, 4, 5
    print!("{} ", i);
}

// iterar sobre colecciones
let colores = ["rojo", "verde", "azul"];
for color in colores.iter() {
    println!("Color: {}", color);
}

// Iterar con enumerate (índice + valor)
for (idx, valor) in [10, 20, 30].iter().enumerate() {
    println!("[{}] = {}", idx, valor);
}
```

---

# Mónadas


| Tipo         | Estados         | Binding        | Uso principal                |
|--------------|-----------------|----------------|------------------------------|
| `Option<T>`  | `Some`, `None`  | `.and_then`    | Representa ausencia o presencia de valor           |
| `Result<T,E>`| `Ok`, `Err`    | `?`            | Representa éxito o error / Manejo de errores            |
| `Box<T>`     | `Box<T>`        | --             |  _smart pointer_    |


<br>

> `Box`no entraría como tal en la categoría de mónada pero es un objeto que sirve de almacenamiento, tal como lo hacen`Option`y`Result`

---

## `Option<T>`

```rust
use std::error::Error;

fn divide(a: f64, b: f64) -> Option<f64> {
  if b == 0.0 { None } else { Some(a / b) }
}

fn main() -> Result<(), Box<dyn Error>> {
    let a = dividir(10.0, 2.0);              // Some(5.0)
    let b = a.and_then(|x| divide(x, 0.0));  // None

    match b {
        Some(x) => Ok(()),
        None    => Err("Error".into()),      // Box<Error: "error">
                                             // into es un "smart casting"
    }
}
```

---

## `Result<T, E>`

```rust
use std::error::Error;

fn divide(a: f64, b: f64) -> Result<f64, String> {
  if b == 0.0 { Err("error".to_string()) } else { Ok(a / b) }
}

fn main() -> Result<(), Box<dyn Error>> {
    let  a = divide(10.0, 2.0);  // Ok(5.0)
    let  x = a?;                 // 5.0
    let  b = divide(x, 0.0);     // Err("error")
    let _y = b?;                 // Box<Error: "error">

    // podriamos cambiar el comportamiento del error
    // let _y = b                // Box<Error: "ERROR">
    //         .map_err(
    //             |e| e.to_uppercase()
    //         );

    Ok(())
}
```

