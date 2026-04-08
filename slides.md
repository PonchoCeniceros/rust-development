---
theme: default
title: Rust Development
info: |
  ## Resumen del lenguaje de programación Rust
transition: slide-left

---

# Primitivas

| Tipo | Descripción | Ejemplo |
|------|-------------|---------|
| `i8, i16, i32, i64, i128, isize` | Enteros con signo (complemento a 2) | `let x: i32 = -42;` |
| `u8, u16, u32, u64, u128, usize` | Enteros sin signo | `let x: u8 = 255;` |
| `f32, f64` | Flotantes IEEE 754 | `let x: f64 = 3.14;` |
| `bool` | Booleano | `let flag: bool = true;` |
| `char` | Carácter Unicode (32 bits) | `'a'`, `'ñ'`, `'好'` |
| `str` | String slice (referencia, sin ownership) | `"hola"` |
| `String` | String con ownership (heap) | `String::from("hola")` |
| `[T; N]` | Array con longitud fija | `[1, 2, 3]` |
| `(T, U, ...)` | Tupla con tipos heterogéneos | `(42, "hola", true)` |
| `()` | Unit (equivalente a void) | `()` |

> **Nota**: `usize`/`isize` dependen de la arquitectura (64 bits en sistemas64-bit).

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


| Tipo         | Estados         | Binding        | Uso principal                |
|--------------|-----------------|----------------|------------------------------|
| `Option<T>`  | `Some`, `None`  | `.and_then`    | Valores opcionales           |
| `Result<T,E>`| `Ok`, `Err`    | `?`            | Manejo de errores            |
| `Box<T>`     | `Box<T>`        | --             | Recursión, trait objects     |
| `Iterator`   | `Item`          | `.flat_map`    | Colecciones, "muchos valores" |
| `Future`     | `Output`        | `.await`       | Valor asíncrono              |


---

### `Option<T>` - Representa ausencia o presencia de valor

```rust
// Creación
let algun_dato: Option<i32> = Some(42);
let sin_dato: Option<i32> = None;

// Pattern matching
let valor = match algun_dato {
    Some(x) => x * 2,
    None    => 0,
};

// Métodos comunes
algun_dato.unwrap_or(100);                  // 42
sin_dato.unwrap_or(100);                    // 100
sin_dato.unwrap_or_else(|| 99);             // 99 (lazy)

let doubled = algun_dato.map(|x| x * 2);    // Some(84)
let filtered = sin_dato.filter(|x| *x > 50); // None

// Chaining con and_then
fn parse_num(s: &str) -> Option<i32> { s.parse().ok() }
let r = Some("42").and_then(parse_num);     // Some(42)
let r = Some("abc").and_then(parse_num);    // None
```

---

### `Result<T, E>` - Representa éxito o error

```rust
// Creación
let ok: Result<i32, &str> = Ok(42);
let err: Result<i32, &str> = Err("error");

// Operador ? para propagar errores
fn dividir(a: f64, b: f64) -> Result<f64, String> {
    if b == 0.0 {
        Err("División por cero".to_string())
    } else {
        Ok(a / b)
    }
}

fn procesar() -> Result<f64, String> {
    let a = dividir(10.0, 2.0)?;  // 5.0
    let b = dividir(a, 2.0)?;     // 2.5
    Ok(b)
}

// Métodos comunes
ok.map(|x| x * 2);                   // Ok(84)
ok.map_err(|e| e.to_uppercase());    // Ok(42)
err.map(|x| x * 2);                  // Err("error")

// Conversión
let option: Option<i32> = ok.ok();  // Some(42)
let option: Option<i32> = err.ok(); // None
```

---

### `Box<T>` - Smart pointer

```rust
// Creación y desreferencia
let valor = Box::new(42);           // Box<i32>
let x = *valor + 10;               // 52

// Uso en recursion (listas enlazadas)
enum Lista<T> {
    Cons(T, Box<Lista<T>>),
    Nil,
}

let lista: Lista<i32> = Lista::Cons(
    1,
    Box::new(Lista::Cons(2, Box::new(Lista::Nil)))
);

// Trait objects (polimorfismo dinámico)
trait Dibujable {
    fn dibujar(&self);
}

struct Circulo { radio: f64 }
struct Cuadrado { lado: f64 }

impl Dibujable for Circulo {
    fn dibujar(&self) { println!("Circulo r={}", self.radio); }
}

impl Dibujable for Cuadrado {
    fn dibujar(&self) { println!("Cuadrado l={}", self.lado); }
}

let formas: Vec<Box<dyn Dibujable>> = vec![
    Box::new(Circulo { radio: 5.0 }),
    Box::new(Cuadrado { lado: 3.0 }),
];

for forma in formas {
    forma.dibujar();
}
```

