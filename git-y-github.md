# Git y GitHub

## 1. Objetivos

Al terminar esta unidad serás capaz de:

- Explicar para qué sirve Git.
- Diferenciar Git y GitHub.
- Crear un repositorio local.
- Guardar cambios mediante commits.
- Consultar el estado y el historial de un proyecto.
- Crear y cambiar entre ramas.
- Fusionar ramas.
- Conectar un repositorio local con GitHub.
- Subir y descargar cambios.
- Trabajar con repositorios compartidos.
- Reconocer y resolver conflictos sencillos.
- Consultar documentación y entender los comandos más habituales.

---

## 2. ¿Qué es Git?

**Git es un sistema de control de versiones.**

Permite guardar la evolución de un proyecto y consultar cómo estaba en diferentes momentos.

Con Git podemos:

- Saber qué archivos han cambiado.
- Recuperar versiones anteriores.
- Crear ramas para trabajar sin modificar directamente la versión principal.
- Unir cambios de diferentes personas.
- Consultar quién realizó un cambio.
- Trabajar localmente sin necesitar conexión permanente a Internet.

### Git y GitHub no son lo mismo

| Herramienta | Función |
|---|---|
| Git | Control de versiones instalado en nuestro ordenador |
| GitHub | Plataforma online para alojar repositorios Git y colaborar |

Git puede utilizarse sin GitHub. GitHub utiliza Git, pero añade alojamiento, colaboración, pull requests, incidencias y otras funciones.

---

## 3. Conceptos básicos

### Repositorio

Un repositorio es una carpeta cuyo historial está gestionado por Git.

Puede ser:

- **Local**: está en nuestro ordenador.
- **Remoto**: está alojado en una plataforma como GitHub.

### Commit

Un commit es una fotografía del estado del proyecto en un momento determinado.

Un commit debe representar un cambio comprensible:

- `Crear estructura inicial de la página`
- `Añadir formulario de contacto`
- `Corregir validación del email`

Evita mensajes poco útiles como:

- `cosas`
- `cambios`
- `arreglos`
- `prueba`

### Working directory, staging area y repository

Git suele explicarse mediante tres zonas:

1. **Working directory**: archivos en los que estamos trabajando.
2. **Staging area**: cambios seleccionados para el próximo commit.
3. **Repository**: historial de commits guardados.

Flujo habitual:

```text
Editar archivos
     ↓
git add
     ↓
Staging area
     ↓
git commit
     ↓
Historial local
```

---

## 4. Configuración inicial

Comprobar que Git está instalado:

```bash
git --version
```

Configurar el nombre:

```bash
git config --global user.name "Tu Nombre"
```

Configurar el correo:

```bash
git config --global user.email "tu-correo@example.com"
```

Consultar la configuración:

```bash
git config --global --list
```

El correo utilizado debe ser coherente con el que se utiliza en GitHub si queremos que los commits aparezcan asociados correctamente a nuestra cuenta.

---

# 5. Crear el primer repositorio local

## 5.1 Crear una carpeta de proyecto

Desde Git Bash:

```bash
mkdir proyecto-git
cd proyecto-git
```

Inicializar Git:

```bash
git init
```

Git mostrará un mensaje indicando que se ha creado un repositorio vacío.

A partir de este momento, la carpeta está preparada para registrar cambios.

## 5.2 Comprobar el estado

```bash
git status
```

`git status` es uno de los comandos más importantes. Conviene utilizarlo constantemente.

Muestra información como:

- Rama actual.
- Archivos modificados.
- Archivos preparados para commit.
- Archivos que Git todavía no está siguiendo.

## 5.3 Crear un archivo

```bash
touch README.md
```

Abrir el proyecto en VS Code y escribir algo parecido a:

```markdown
# Mi primer proyecto con Git

Proyecto de práctica para aprender Git.
```

Volver a consultar el estado:

```bash
git status
```

El archivo aparecerá como no seguido, normalmente bajo `Untracked files`.

---

# 6. Guardar cambios: add y commit

## 6.1 Añadir un archivo al staging

```bash
git add README.md
```

Comprobar el estado:

```bash
git status
```

Ahora el archivo aparece preparado para el próximo commit.

## 6.2 Crear un commit

```bash
git commit -m "Crear README inicial"
```

Consultar el estado:

```bash
git status
```

Si no hay cambios pendientes, Git indicará que el árbol de trabajo está limpio.

## 6.3 Añadir todos los cambios

```bash
git add .
```

El punto significa: añadir los cambios del directorio actual y sus subdirectorios.

También puede utilizarse:

```bash
git add -A
```

Para una primera aproximación, `git add .` será suficiente, pero es importante entender que selecciona cambios para el commit; no crea el commit.

## 6.4 Flujo fundamental

```bash
git status
git add .
git commit -m "Descripción del cambio"
```

Este flujo se repetirá muchas veces durante el bootcamp.

---

# 7. Consultar el historial

## 7.1 Historial completo

```bash
git log
```

Muestra los commits con información como:

- Identificador.
- Autor.
- Fecha.
- Mensaje.

## 7.2 Historial resumido

```bash
git log --oneline
```

Ejemplo:

```text
a12bc34 Añadir sección de instalación
f45de67 Crear README inicial
```

## 7.3 Historial visual de ramas

```bash
git log --oneline --graph --all
```

Este comando será especialmente útil al trabajar con ramas.

---

# 8. Consultar y comparar cambios

## 8.1 Ver cambios todavía no preparados

Modificar `README.md` y ejecutar:

```bash
git diff
```

Muestra diferencias entre el archivo actual y la última versión confirmada.

## 8.2 Ver cambios preparados

Después de ejecutar:

```bash
git add README.md
```

Consultar:

```bash
git diff --staged
```

Muestra lo que entraría en el próximo commit.

## 8.3 Ver los cambios de un commit

```bash
git show ID_DEL_COMMIT
```

También puede utilizarse el identificador corto:

```bash
git show a12bc34
```

---

# 9. Ignorar archivos con `.gitignore`

No todos los archivos deben guardarse en Git.

Ejemplos habituales:

- Dependencias instaladas.
- Archivos temporales.
- Logs.
- Claves y secretos.
- Configuraciones personales.
- Archivos generados automáticamente.

Crear un archivo llamado `.gitignore`:

```bash
touch .gitignore
```

Ejemplo:

```gitignore
node_modules/
.env
*.log
.DS_Store
```

### Importante

`.gitignore` evita que determinados archivos nuevos o no seguidos se añadan accidentalmente.

No elimina del historial un archivo que ya se haya confirmado en un commit.

---

# 10. Ramas

Una rama permite trabajar en una línea de desarrollo independiente.

Ejemplos:

- `main`: versión principal.
- `feature-login`: desarrollo del login.
- `feature-contacto`: desarrollo del formulario de contacto.
- `fix-header`: corrección de un problema.

## 10.1 Ver ramas

```bash
git branch
```

La rama actual aparece marcada con `*`.

## 10.2 Crear una rama

```bash
git branch feature-contacto
```

Este comando crea la rama, pero no cambia a ella.

## 10.3 Cambiar de rama

```bash
git switch feature-contacto
```

## 10.4 Crear y cambiar en un solo paso

```bash
git switch -c feature-contacto
```

Es una forma muy habitual de trabajar.

## 10.5 Volver a main

```bash
git switch main
```

En repositorios antiguos, también puede aparecer:

```bash
git checkout main
```

`git switch` es más claro para explicar específicamente el cambio de ramas.

---

# 11. Trabajar con ramas

Partimos de la rama `main`.

Crear una rama:

```bash
git switch -c feature-contacto
```

Modificar `README.md` y añadir una sección:

```markdown
## Contacto

Formulario de contacto pendiente de desarrollo.
```

Guardar el cambio:

```bash
git add .
git commit -m "Añadir sección de contacto"
```

Consultar el historial:

```bash
git log --oneline --graph --all
```

Volver a `main`:

```bash
git switch main
```

Comprobar que el cambio de la rama no aparece todavía en `main`.

---

# 12. Fusionar ramas: merge

`merge` integra los cambios de una rama en otra.

Desde `main`:

```bash
git merge feature-contacto
```

La rama actual recibe los cambios de `feature-contacto`.

## Flujo habitual

```bash
git switch main
git merge feature-contacto
```

### Idea importante

La rama en la que estamos situados es la rama que recibe la fusión.

Esto:

```bash
git switch main
git merge feature-contacto
```

significa:

> Fusiona `feature-contacto` dentro de `main`.

No significa que `main` se fusione dentro de `feature-contacto`.

## Eliminar una rama ya fusionada

```bash
git branch -d feature-contacto
```

Eliminar una rama no elimina los commits que ya forman parte de otra rama.

---

# 13. Conflictos de merge

Un conflicto aparece cuando Git no puede decidir automáticamente qué versión conservar.

Suele ocurrir cuando dos ramas modifican la misma zona de un archivo de maneras incompatibles.

Git puede marcar el archivo así:

```text
<<<<<<< HEAD
Contenido de main
=======
Contenido de feature
>>>>>>> feature-contacto
```

## Resolver un conflicto

1. Ejecutar el merge.
2. Consultar los archivos en conflicto:

   ```bash
   git status
   ```

3. Abrir el archivo.
4. Decidir qué contenido conservar.
5. Eliminar las marcas:

   ```text
   <<<<<<<
   =======
   >>>>>>>
   ```

6. Guardar el archivo.
7. Añadirlo al staging:

   ```bash
   git add archivo.txt
   ```

8. Completar el merge:

   ```bash
   git commit
   ```

En muchos casos Git propone automáticamente el mensaje del commit de merge.

### Cancelar un merge en curso

Si todavía no se ha terminado el merge:

```bash
git merge --abort
```

Esto intenta devolver el repositorio al estado anterior al merge.

---

# 14. GitHub y repositorios remotos

## 14.1 Crear un repositorio en GitHub

Desde GitHub:

1. Iniciar sesión.
2. Crear un repositorio nuevo.
3. Elegir un nombre.
4. Seleccionar si será público o privado.
5. Crear el repositorio.

Para esta práctica, si ya tenemos un repositorio local, es recomendable no añadir automáticamente un README desde GitHub para evitar tener dos historiales iniciales diferentes.

## 14.2 Conectar el repositorio local

GitHub proporcionará una dirección remota parecida a:

```text
https://github.com/usuario/proyecto-git.git
```

Añadirla como remoto:

```bash
git remote add origin https://github.com/usuario/proyecto-git.git
```

Consultar los remotos:

```bash
git remote -v
```

`origin` es el nombre habitual del repositorio remoto principal.

## 14.3 Subir la rama a GitHub

```bash
git push -u origin main
```

- `push`: enviar commits al remoto.
- `origin`: remoto de destino.
- `main`: rama que se sube.
- `-u`: establece la relación entre la rama local y la remota.

Después de configurar la relación, normalmente bastará con:

```bash
git push
```

---

# 15. Clonar un repositorio

Clonar significa descargar un repositorio remoto completo, incluyendo su historial.

```bash
git clone https://github.com/usuario/proyecto-git.git
```

Git creará una carpeta con el nombre del repositorio.

Entrar en ella:

```bash
cd proyecto-git
```

Comprobar el estado:

```bash
git status
```

Consultar el remoto:

```bash
git remote -v
```

## Diferencia entre clone y download ZIP

| Clonar con Git | Descargar ZIP |
|---|---|
| Descarga el historial | Normalmente solo descarga los archivos |
| Permite hacer pull y push | No mantiene la conexión Git |
| Permite trabajar con ramas | No es un repositorio Git funcional por sí mismo |
| Está pensado para desarrollo | Está pensado para obtener una copia puntual |

---

# 16. Descargar y enviar cambios

## 16.1 `git fetch`

Descarga información y commits del remoto, pero no integra automáticamente esos cambios en nuestra rama actual.

```bash
git fetch
```

Es una operación de actualización de información.

## 16.2 `git pull`

Descarga cambios del remoto y trata de integrarlos en la rama actual.

```bash
git pull
```

De forma simplificada, suele equivaler a:

```text
fetch + integración
```

La integración puede realizarse mediante merge o, según la configuración, mediante rebase.

## 16.3 Flujo de colaboración básico

Antes de comenzar:

```bash
git pull
```

Después de trabajar:

```bash
git status
git add .
git commit -m "Descripción del cambio"
git push
```

En un equipo real, conviene actualizar la rama antes de comenzar y revisar los cambios antes de subirlos.

---

# 17. Remotos y ramas remotas

Consultar remotos:

```bash
git remote -v
```

Consultar ramas locales y remotas:

```bash
git branch -a
```

Consultar ramas remotas:

```bash
git branch -r
```

Crear una rama local basada en una rama remota:

```bash
git switch -c feature-menu origin/feature-menu
```

En versiones modernas de Git, si la rama remota existe, también puede funcionar:

```bash
git switch feature-menu
```

---

# 18. `git restore`

`git restore` permite recuperar el contenido de un archivo desde otra referencia, normalmente el último commit.

## Descartar cambios no confirmados de un archivo

```bash
git restore README.md
```

Esto elimina los cambios locales no confirmados de ese archivo.

**Atención:** es una operación destructiva para esos cambios.

## Sacar un archivo del staging

```bash
git restore --staged README.md
```

Esto quita el archivo del staging, pero conserva sus cambios en el directorio de trabajo.

---

# 19. `git stash`

`stash` guarda temporalmente cambios que todavía no queremos confirmar.

Es útil cuando:

- Estamos trabajando en una tarea.
- Necesitamos cambiar de rama urgentemente.
- Todavía no tenemos un cambio terminado para hacer commit.

Guardar cambios:

```bash
git stash
```

Ver la lista de stashes:

```bash
git stash list
```

Recuperar el último stash y eliminarlo de la lista:

```bash
git stash pop
```

Aplicar un stash sin eliminarlo:

```bash
git stash apply
```

Eliminar un stash concreto:

```bash
git stash drop stash@{0}
```

No conviene utilizar `stash` como sustituto habitual de commits bien organizados.

---

# 20. `git revert`

`revert` crea un nuevo commit que deshace los cambios de un commit anterior.

```bash
git revert ID_DEL_COMMIT
```

Es una opción adecuada cuando el commit ya se ha compartido con otras personas, porque no elimina el historial existente.

### Diferencia conceptual

- `restore`: recupera archivos y descarta cambios locales.
- `revert`: crea un nuevo commit que deshace otro commit.
- `reset`: mueve referencias del historial y debe utilizarse con cuidado.

---

# 21. `git reset`

`reset` puede mover la rama actual a otro commit.

## `--soft`

```bash
git reset --soft HEAD~1
```

Deshace el último commit, pero conserva los cambios en staging.

## `--mixed`

```bash
git reset HEAD~1
```

Deshace el último commit y deja los cambios en el directorio de trabajo, fuera del staging.

## `--hard`

```bash
git reset --hard HEAD~1
```

Deshace el último commit y elimina también los cambios de los archivos afectados.

**Precaución:** `--hard` puede provocar pérdida de trabajo. No debe utilizarse sin comprender exactamente qué se está eliminando.

En repositorios compartidos, normalmente se prefiere `git revert` antes que reescribir el historial.

---

# 22. `git rebase`

`rebase` recoloca los commits de una rama sobre otra base.

Ejemplo conceptual:

```text
Antes:

main:    A---B
              \
feature:        C---D
```

Si `main` avanza:

```text
main:    A---B---E
              \
feature:        C---D
```

Con rebase, los commits de `feature` se reaplican sobre `E`:

```text
main:    A---B---E
                  \
feature:            C'---D'
```

Comando habitual:

```bash
git switch feature
git rebase main
```

### Importante

Rebase puede cambiar los identificadores de los commits. Por eso no conviene hacer rebase de commits que otras personas ya están utilizando sin coordinarse con ellas.

Para el trabajo inicial, es suficiente entender:

- `merge` conserva las líneas de historia y crea una integración.
- `rebase` reorganiza la base de una rama para obtener una historia más lineal.

---

# 23. `git cherry-pick`

`cherry-pick` aplica en la rama actual los cambios introducidos por un commit concreto.

No fusiona una rama completa: selecciona un commit específico.

Ejemplo:

```bash
git switch main
git cherry-pick ID_DEL_COMMIT
```

Es útil cuando:

- Necesitamos una corrección concreta de otra rama.
- Queremos aplicar un cambio puntual sin fusionar todo el trabajo.
- Queremos recuperar un commit específico.

### Precauciones

- Puede producir conflictos.
- El commit aplicado tendrá un nuevo identificador.
- No debe utilizarse como sustituto habitual de `merge` o `rebase`.
- Conviene entender qué cambios contiene el commit antes de aplicarlo.

### Cancelar un cherry-pick en conflicto

```bash
git cherry-pick --abort
```

---

# 24. Práctica guiada completa

## Práctica 1 — Crear un repositorio y realizar commits

### Objetivo

Crear un repositorio local y guardar varios cambios.

### Pasos

1. Crear una carpeta:

   ```bash
   mkdir practica-git
   cd practica-git
   ```

2. Inicializar Git:

   ```bash
   git init
   ```

3. Crear `README.md`.

4. Escribir un título y una breve descripción.

5. Consultar el estado:

   ```bash
   git status
   ```

6. Añadir el archivo:

   ```bash
   git add README.md
   ```

7. Crear el primer commit:

   ```bash
   git commit -m "Crear README inicial"
   ```

8. Añadir una sección nueva.

9. Crear un segundo commit:

   ```bash
   git add .
   git commit -m "Añadir descripción del proyecto"
   ```

10. Consultar el historial:

    ```bash
    git log --oneline
    ```

### Comprobación

- ¿Cuántos commits hay?
- ¿Qué mensaje tiene cada uno?
- ¿Qué muestra `git status`?

---

## Práctica 2 — Staging y diferencias

1. Crear `notas.txt`.
2. Añadir una primera línea.
3. Ejecutar:

   ```bash
   git add .
   git commit -m "Crear notas"
   ```

4. Añadir varias líneas más.
5. Ejecutar:

   ```bash
   git diff
   ```

6. Añadir el archivo al staging.
7. Ejecutar:

   ```bash
   git diff --staged
   ```

8. Crear el commit.

### Preguntas

- ¿Qué diferencia hay entre `git diff` y `git diff --staged`?
- ¿Qué ocurre si modificas un archivo después de hacer `git add`?
- ¿Qué cambios entran en el commit?

---

## Práctica 3 — Ramas

1. Crear una rama llamada `feature-presentacion`.
2. Cambiar a esa rama.
3. Crear un archivo `presentacion.txt`.
4. Hacer un commit.
5. Volver a `main`.
6. Consultar el contenido de la carpeta.
7. Fusionar la rama.
8. Consultar el historial con:

   ```bash
   git log --oneline --graph --all
   ```

9. Eliminar la rama fusionada.

---

## Práctica 4 — GitHub

1. Crear un repositorio vacío en GitHub.
2. Conectar el repositorio local:

   ```bash
   git remote add origin URL_DEL_REPOSITORIO
   ```

3. Comprobar el remoto:

   ```bash
   git remote -v
   ```

4. Subir la rama:

   ```bash
   git push -u origin main
   ```

5. Abrir GitHub en el navegador.
6. Comprobar que aparecen los archivos y commits.

---

## Práctica 5 — Clonar y colaborar

Por parejas:

### Persona A

1. Crea un repositorio en GitHub.
2. Añade a la persona B como colaboradora.
3. Sube un archivo `equipo.md`.

### Persona B

1. Acepta la invitación.
2. Clona el repositorio:

   ```bash
   git clone URL_DEL_REPOSITORIO
   ```

3. Añade su nombre al archivo.
4. Hace commit.
5. Hace push.

### Persona A

1. Ejecuta:

   ```bash
   git pull
   ```

2. Comprueba el cambio.
3. Añade una segunda modificación.
4. Hace commit y push.

---

## Práctica 6 — Resolver un conflicto

Por parejas:

1. Partir del mismo repositorio.
2. Ambas personas modifican la misma línea de un archivo.
3. Cada una hace un commit en su rama.
4. Intentar fusionar las ramas.
5. Consultar el estado.
6. Resolver manualmente el conflicto.
7. Añadir el archivo resuelto.
8. Completar el merge.
9. Consultar el historial.

### Preguntas

- ¿Por qué Git no ha podido fusionar automáticamente?
- ¿Qué significan las marcas `<<<<<<<`, `=======` y `>>>>>>>`?
- ¿Qué hay que hacer después de resolver el archivo?

---

## Práctica 7 — Stash

1. Modificar un archivo sin hacer commit.
2. Guardar temporalmente los cambios:

   ```bash
   git stash
   ```

3. Comprobar que el directorio de trabajo vuelve a estar limpio.
4. Consultar la lista:

   ```bash
   git stash list
   ```

5. Recuperar los cambios:

   ```bash
   git stash pop
   ```

6. Comprobar que los cambios vuelven a aparecer.

---

## Práctica 8 — Cherry-pick controlado

Esta práctica debe realizarse en un repositorio de prueba.

1. Crear una rama `feature-aviso`.
2. Crear un archivo `aviso.txt`.
3. Hacer un commit:

   ```bash
   git add .
   git commit -m "Añadir aviso importante"
   ```

4. Copiar el identificador del commit:

   ```bash
   git log --oneline
   ```

5. Volver a `main`.
6. Aplicar el commit:

   ```bash
   git cherry-pick ID_DEL_COMMIT
   ```

7. Comprobar que el archivo aparece en `main`.

### Pregunta

¿Qué diferencia hay entre hacer `cherry-pick` de un commit y hacer `merge` de toda la rama?

---

# 25. Ejercicios individuales

## Nivel esencial

1. Crear un repositorio llamado `mi-portafolio`.
2. Crear un `README.md`.
3. Realizar al menos tres commits con mensajes descriptivos.
4. Crear un `.gitignore`.
5. Crear una rama `feature-proyectos`.
6. Añadir una sección de proyectos.
7. Fusionar la rama en `main`.
8. Subir el repositorio a GitHub.

## Nivel práctica

1. Crear dos ramas:
   - `feature-sobre-mi`
   - `feature-contacto`
2. Realizar al menos un commit en cada rama.
3. Fusionar ambas ramas en `main`.
4. Consultar el grafo del historial.
5. Eliminar las ramas ya fusionadas.

## Nivel ampliación

1. Crear una rama de corrección.
2. Hacer dos commits.
3. Volver a `main`.
4. Aplicar únicamente el primer commit con `cherry-pick`.
5. Comprobar qué contenido se ha incorporado.
6. Explicar por qué el segundo commit no se ha aplicado.

## Nivel refuerzo

1. Crear un repositorio nuevo.
2. Repetir exclusivamente este flujo hasta realizarlo sin mirar apuntes:

   ```bash
   git init
   git status
   git add .
   git commit -m "..."
   git log --oneline
   ```

3. Crear una rama.
4. Cambiar de rama.
5. Volver a `main`.
6. Fusionar.

## Reto opcional

Simular un pequeño proyecto colaborativo:

- Una rama para la portada.
- Una rama para la sección de proyectos.
- Una rama para el contacto.
- Al menos un merge.
- Un conflicto resuelto.
- Un repositorio remoto en GitHub.
- Dos personas trabajando mediante clone, pull y push.
- Un breve documento explicando el flujo utilizado.

---

# 26. Preguntas de comprobación

- ¿Qué diferencia hay entre Git y GitHub?
- ¿Qué hace `git init`?
- ¿Qué información muestra `git status`?
- ¿Qué diferencia hay entre `git add` y `git commit`?
- ¿Qué representa un commit?
- ¿Qué diferencia hay entre `git diff` y `git diff --staged`?
- ¿Qué es una rama?
- ¿Qué rama recibe los cambios en un merge?
- ¿Qué diferencia hay entre `git clone` y `git pull`?
- ¿Qué hace `git push`?
- ¿Qué es un conflicto?
- ¿Cuándo puede ser útil `git stash`?
- ¿Qué diferencia hay entre `revert` y `reset`?
- ¿Qué hace `cherry-pick`?
- ¿Por qué hay que tener cuidado con `reset --hard` y `rebase`?

---

# 27. Ideas importantes

- Utiliza `git status` con frecuencia.
- Haz commits pequeños y comprensibles.
- Escribe mensajes que expliquen el cambio.
- No subas contraseñas, claves ni archivos `.env`.
- Actualiza el repositorio antes de comenzar a trabajar en equipo.
- No trabajes directamente sobre `main` cuando estés desarrollando una funcionalidad independiente.
- Antes de resolver un conflicto, entiende qué cambios están intentando integrarse.
- `revert` suele ser más seguro que reescribir el historial compartido.
- Los comandos destructivos deben utilizarse en repositorios de prueba hasta comprenderlos.
- Git no sustituye a las copias de seguridad.
