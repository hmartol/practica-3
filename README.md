# Práctica 3: Viajando en el Tiempo y Corrigiendo el Historial

**Alumno:** Hugo Martín Toledano  
**Curso:** 2º DAW

**Repositorio:** [Enlace al repositorio](PEGA_AQUI_EL_ENLACE)

---

# Fase 1: Inicialización y primeros commits

## Paso 1: Creación del proyecto

Primero configuré `main` como rama por defecto y creé la carpeta del proyecto.

```bash
git config --global init.defaultBranch main
mkdir git-historial-practica
cd git-historial-practica
git init
```

![Creación del repositorio](capturas/1.png)

---

## Paso 2: Commit C1

Creé el archivo `calculadora.py` con una función básica para sumar dos números.

Después añadí el archivo al repositorio e hice el primer commit:

```bash
git add calculadora.py
git commit -m "C1: Creación de la función sumar."
```

![Commit C1](capturas/2.png)

El código en este momento solo tenía la función `sumar`.

![Función sumar](capturas/3.png)

---

## Paso 3: Commit C2

Después modifiqué `calculadora.py` para añadir la función `restar`.

![Función restar](capturas/4.png)

Guardé los cambios utilizando:

```bash
git commit -a -m "C2: Añadida la función restar."
```

![Commit C2](capturas/5.png)

---

# Fase 2: Inspección y etiquetado del historial

## Paso 1: Ver el historial completo

Para ver los commits junto con los cambios realizados utilicé:

```bash
git log -p --stat
```

![Historial detallado](capturas/6.png)

En el commit C2 se realizaron **8 inserciones y 1 eliminación**.

---

## Paso 2: Historial resumido

Después mostré los commits en una sola línea utilizando:

```bash
git log --pretty=format:"%h - %an, %ar : %s"
```

![Historial resumido](capturas/7.png)

Los hashes abreviados fueron:

- **C1:** `2c09ea1`
- **C2:** `96f05ce`

---

## Paso 3: Crear una etiqueta

Creé una etiqueta para marcar C2 como una versión estable:

```bash
git tag v1.0-estable
```

Después comprobé que se había creado:

```bash
git tag
```

![Etiqueta v1.0-estable](capturas/8.png)

La etiqueta `v1.0-estable` aparece correctamente.

---

## Paso 4: Comparar versiones

Comparé C1 con la versión `v1.0-estable`:

```bash
git diff 2c09ea1 v1.0-estable
```

![Comparación entre versiones](capturas/9.png)

En la salida se puede ver que se añadió la función `restar` y su línea de prueba.

---

# Fase 3: Deshaciendo y corrigiendo cambios

## Paso 1: Cambio sin staging

Añadí un comentario incorrecto al archivo y comprobé que Git detectaba el cambio:

```bash
git status
```

Después utilicé:

```bash
git restore calculadora.py
```

y volví a comprobar el estado.

![Uso de git restore](capturas/10.png)

El comentario desapareció y el repositorio volvió a estar limpio.

---

## Paso 2: Cambio añadido al staging

Volví a realizar el cambio, pero esta vez lo añadí al staging:

```bash
git add calculadora.py
git status
```

Después lo saqué del staging:

```bash
git restore --staged calculadora.py
git status
```

Finalmente descarté el cambio completamente:

```bash
git restore calculadora.py
git status
```

![Uso de restore con staging](capturas/11.png)

Primero el archivo aparece preparado para hacer commit, después vuelve a estar modificado fuera del staging y finalmente queda limpio.

---

## Paso 3: Commit C3 con un error

Añadí la función `multiplicar`, pero de forma intencionada tenía un error porque realizaba una suma:

```python
def multiplicar(a, b):
    return a + b
```

Hice el commit:

```bash
git commit -a -m "C3: Añadida la función multiplicar (con un bug conceptual)."
```

![Commit C3](capturas/12.png)

Este era el código con el error:

![Código con el error](capturas/13.png)

---

## Paso 4: Deshacer C3 con revert

Para deshacer C3 sin eliminarlo del historial utilicé:

```bash
git revert HEAD --no-edit
```

Después comprobé los últimos commits:

```bash
git log --pretty=oneline -3
```

![Uso de git revert](capturas/14.png)

Git creó un nuevo commit que deshace C3, pero el commit con el error sigue apareciendo en el historial.

---

## Paso 5: Corregir el último commit

Volví a añadir la función `multiplicar`, esta vez de forma correcta:

```python
def multiplicar(a, b):
    return a * b
```

![Función multiplicar corregida](capturas/15.png)

Después añadí el archivo y modifiqué el último commit:

```bash
git add calculadora.py
git commit --amend -m "C4: Revertido C3 y añadida correctamente la función multiplicar."
```

Para terminar comprobé el historial:

```bash
git log
```

![Historial final](capturas/16.png)

El nuevo hash de C4 es `e5a0ca4`.

Antes del `amend`, el commit de revert tenía el hash `5da5cc7`, por lo que se puede ver que el hash cambió.

---

# Ejercicios de Autoevaluación

## 1. ¿Cuál es el hash abreviado de C1?

El hash abreviado de C1 es:

```text
2c09ea1
```

---

## 2. Comando para viajar al commit anterior a `v1.0-estable`

```bash
git switch --detach v1.0-estable^
```

---

## 3. ¿Qué habría pasado usando `git reset --hard` en lugar de `git revert`?

Se habrían eliminado commits de mi historial local.

Si el repositorio estuviera compartido con más personas podría dar problemas, porque ellos podrían seguir teniendo esos commits y nuestros historiales serían diferentes.

Con `git revert` esto no pasa, porque no borra el historial, sino que añade otro commit para deshacer los cambios.

---

## 4. ¿Dónde apunta HEAD al terminar?

Al terminar, `HEAD` está apuntando a la rama `main`.

Por tanto, no está en modo `Detached HEAD`.