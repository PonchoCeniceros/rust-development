---
theme: default
title: Rust Development
info: |
  ## Resumen del lenguaje de programación Rust
transition: slide-left
---

# Lorem ipsum
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed magna lorem, maximus eu ante at, tristique posuere lorem. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus.


```rust
pub fn main() -> Result<(), Box<dyn Error>> {
  println!("Hola mundo!");
  Ok(())
}
```

---

# Primitivas

```rust
i8, i16, i32, i64, i128, usize  // enteros con signo
u8, u16, u32, u64, u128, usize  // enteros sin signo
f32, f64                        // flotantes
char                            // caracteres
str                             // string en el stack
bool                            // booleanos
()                              // void
[T; N]                          // array estático
(R, S, T, ...)                  // tupla
```

---

# Estructuras de control

### a. llaves
```rust
{
  resl = expr;
  resl
}
```

<br>

### b. condicional
```rust
if cond { resl_1 } else { resl_2 }
```

<br>

### c. _matching_
```rust
match expr {
  expected_resp_1 => resl_1,
  expected_resp_2 => resl_2
}
```

---

# Estructuras de control

### d. ciclos
```rust
loop {
  expr;
  // break
}
```

<br>

```rust
while cond {
  expr;
}
```

<br>

```rust
for i in iter {
expr;
}
```

---

# Mónadas


|Contexto      | Salida (Estado)    | Función de Binding | Función de Avance|
|--------------|--------------------|---------------------|------------------|
|`Option`        | `Some`, `None`         | `.and_then`           | --              |
|`Result`        | `Ok`, `Err`            | operador `?`          | --              |
|`Poll`          | `Ready`, `Pending`     | `.and_then`           | --              |
|`Iterator`      | `Item`               | `.flat_map`           | `.next()`          |
|`Future`        | `Output`             | `.await` / `.then`      | `.poll()`          |
|`Box`           | `Box<T>`                | --   | --              |


`Iterator` gestiona el contexto de "muchos valores" y `Future` gestiona el contexto de "valor en el futuro".

---
