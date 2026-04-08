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

### a. Bloques (llaves)
```rust
let resultado = {
    let a = 10;
    let b = 5;
    a + b  // última expresión es el valor de retorno
};
// resultado == 15

// Uso práctico
let mensaje = {
    let nombre = "Rust";
    let version = 2021;
    format!("{} v{}", nombre, version)
};
```

---

### b. Condicional
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

// If con inicialización (común en Rust)
let valor = if true { 42 } else { 0 };
```

---

### c. Match (pattern matching)
```rust
enum Direccion {
    Norte, Sur, Este, Oeste,
    Diagonal(i32, i32),  // con datos asociados
}

fn mover(dir: Direccion) -> (i32, i32) {
    match dir {
        Norte          => (0, 1),
        Sur            => (0, -1),
        Este           => (1, 0),
        Oeste          => (-1, 0),
        Diagonal(x, y) => (x, y),
        otra           => {
            println!("Dirección ignorada: {:?}", otra);
            (0, 0)
        }
    }
}

// Match con guard (condición adicional)
let num = 7;
match num {
    n if n % 2 == 0 => println!("{} es par", n),
    n               => println!("{} es impar", n),
}
```

---

### d. Ciclos
```rust
// loop - uso para operaciones retry o infinite loops
let mut intentos = 0;
let resultado = loop {
    intentos += 1;
    if intentos == 3 {
        break "Listo después de 3 intentos";
    }
};

// while - cuando necesitas condición
let mut contador = 0;
while contador < 5 {
    println!("Contando: {}", contador);
    contador += 1;
}

// for - iterar sobre colecciones/rangos
for i in 0..=5 {           // 0, 1, 2, 3, 4, 5
    print!("{} ", i);
}

let colores = ["rojo", "verde", "azul"];
for color in colores.iter() {
    println!("Color: {}", color);
}

// Iterar con enumerate (índice + valor)
for (idx, valor) in [10, 20, 30].iter().enumerate() {
    println!("[{}] = {}", idx, valor);
}

// Loop anidado con label
' externo: for i in 1..=3 {
    for j in 1..=3 {
        if i == 2 && j == 2 {
            break 'externo;  // sale del loop externo
        }
        println!("({}, {})", i, j);
    }
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
