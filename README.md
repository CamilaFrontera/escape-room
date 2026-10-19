# ERROR 500 · Sala de acertijos

Juego para el stand de 3.º año de la **Tecnicatura Superior en Desarrollo de Software** del ISFDyT N° 125 en la Noche del Instituto.

El sistema del instituto "se cayó" y un grupo de 3 a 6 visitantes tiene **10 minutos** para resolver cuatro acertijos en cuatro terminales, armar el código y restaurarlo desde la pantalla del servidor.

## Direcciones para cada PC

Las terminales siguen el orden real de un proyecto: primero se escucha al cliente, después se diseña, se programa y se prueba.

| PC | Abrir | Desafío |
|---|---|---|
| 1 ◆ Cliente | https://camilafrontera.github.io/escape-room/#cliente | Descifrar la palabra en clave del mensaje del cliente |
| 2 ★ Diseño | https://camilafrontera.github.io/escape-room/#diseno | Armar el botón «Inscribirme» igual que el manual de marca del cliente: fondo, letra, esquinas, tamaño y mayúsculas, viendo el CSS que se genera |
| 3 ● Código | https://camilafrontera.github.io/escape-room/#codigo | Averiguar qué número muestra un programa |
| 4 ▲ Calidad | https://camilafrontera.github.io/escape-room/#calidad | Contar los errores entre la pantalla esperada y la obtenida |
| Servidor (pantalla principal) | https://camilafrontera.github.io/escape-room/#servidor | ERROR 500, timer de 10 minutos con música y código final |

Sin nada después del `#` se abre un menú con la dirección exacta de cada notebook, el estado de la conexión y **el código de la partida en curso** (para el equipo).

## Niveles y ranking

**Antes de cada partida**, en la pantalla del servidor:

1. Se elige el **nivel**:
   - **Nivel 1 · Inicial:** para quien nunca vio nada de informática.
   - **Nivel 2 · Intermedio:** un paso más.
   - **Nivel 3 · Avanzado:** pensado para chicos de escuela técnica con orientación informática.
2. Se cargan la **escuela**, el **año/curso** y los **nombres de los jugadores**. Sin escuela o sin jugadores no se puede iniciar.

| Terminal | Nivel 1 | Nivel 2 | Nivel 3 |
|---|---|---|---|
| ◆ Cliente | Palabra en clave con la clave y el abecedario a la vista | Sin las ayudas de apoyo (sin abecedario en César, sin la segunda fila en el invertido) | César sin decir cuánto se corrió: se deduce con la firma del cliente |
| ★ Diseño | Muestras de color con nombre y manual en palabras | Manual con el color en hexadecimal; hay que comparar el código | Se elige el valor de cada línea del CSS (hex, px, `padding`) |
| ● Código | Pseudocódigo con glosario | Pseudocódigo que combina bucle y condición, sin glosario | Python corto con `for` y `range` |
| ▲ Calidad | Comparar esperado contra obtenido | Formulario con reglas: contar los datos que rompen su regla | Igual, con una regla de rango con bordes ("entre 17 y 99, incluidos") |

Los tres niveles tienen 10 minutos.

**Ranking:** al terminar cada partida, el resultado se guarda y se muestra la tabla de la noche, con el puesto del grupo resaltado. El orden es:

1. Primero, los que restauraron el sistema: más nivel primero y, dentro del mismo nivel, más tiempo sobrante.
2. Después, los que no lo restauraron: más fragmentos recuperados y, después, más nivel.

- El ranking se guarda en la notebook del servidor y, con Firebase, también en la base. Por eso hay que agregar `ranking` en las reglas, como figura más abajo.
- `#ranking` es una pantalla extra opcional: muestra la tabla grande y se actualiza sola.
- En el menú hay un botón **⬇ RANKING (CSV)** para bajar todos los resultados de la noche y abrirlos en Excel.
- **Partidas de prueba:** en el inicio del servidor hay una casilla **"Partida de prueba (no entra al ranking)"**. Se desmarca sola en el grupo siguiente.
- **Borrar el ranking:** en el menú, **🗑 BORRAR RANKING** (hay que tocarlo dos veces). Borra la tabla en Firebase y en la notebook. Conviene hacerlo antes de que empiece la noche, para sacar las pruebas.
- Con Firebase, la tabla de la base es la que vale: cada notebook guarda solo una copia por si se corta internet.

## Cómo responde cada terminal

- **◆ Cliente:** debajo de cada símbolo en clave hay un casillero; se completa letra por letra y se verifica solo al llenar todos.
- **★ Diseño:** dos paneles enfrentados, "Lo que pide el cliente" y "Tu botón". Los controles van en el mismo orden que los pedidos. En el nivel 1 aparece ✔ al lado de cada pedido cumplido. El botón **¿Cuántos me faltan?** dice cuántas cosas faltan.
- **● Código:** la respuesta se escribe en la consola del editor (`> ?`). Hay un espacio de "Tus anotaciones" para seguir el valor de x.
- **▲ Calidad:** se tocan las filas con error para marcarlas (también el botón, si corresponde) y después se verifica. Tienen que estar marcadas exactamente las filas con error.

Mientras esperan, cada terminal muestra un brief: qué número tienen que conseguir, de qué se trata su desafío y las reglas de pistas. Al conseguirlo, la pantalla indica a qué casillero del servidor llevar el número. En el servidor, cada casillero se marca cuando su terminal ya consiguió el número, y cuando están los 4 aparece un aviso.

## Cómo se juega

Las cinco notebooks están **conectadas en tiempo real**:

1. Mientras no hay partida, las terminales muestran **«Esperando al servidor»**.
2. El game master toca **Iniciar partida** en el servidor. En ese momento se desbloquean las cuatro terminales a la vez, arranca el timer (también se ve arriba en cada terminal) y la música de tensión.
3. El grupo resuelve las cuatro terminales **al mismo tiempo**. Cada partida tiene **un código nuevo**: el servidor sortea un número para cada terminal y cada desafío se arma con ese número (Cliente y Código: 1 a 9; Diseño: 1 a 9; Calidad: 2 a 5 errores). Además, cada terminal elige al azar uno de sus casos. Cada respuesta correcta muestra un fragmento numerado (1 a 4) con su dígito y un dato corto **«¿Sabías que…?»**. El servidor marca cada terminal como RECUPERADO apenas se resuelve, y en la barra de cada terminal se ve cuáles ya resolvió el resto del equipo.
4. En el servidor se ingresa cada fragmento en su casillero. Hay **3 intentos como máximo**. Cada código incorrecto **descuenta 30 segundos**, muestra ACCESO DENEGADO y avisa en todas las terminales.
5. Si aciertan: **SISTEMA RESTAURADO** con lluvia de píxeles, música, tiempo, intentos y un nivel según el tiempo que sobró. Si usan los 3 intentos: **SISTEMA BLOQUEADO**. Si se termina el tiempo: **SE ACABÓ EL TIEMPO**. El resultado aparece también en las cuatro terminales.
6. **Siguiente grupo** en el servidor reinicia todo: las terminales vuelven a «Esperando al servidor», cada una con un caso nuevo.

Las terminales **no se reinician por separado**: solo cuando el servidor termina una partida y empieza la siguiente (salvo el reinicio manual de emergencia, Ctrl + Alt + R). Si una notebook se recarga en medio de una partida, vuelve al mismo caso (y si ya lo había resuelto, sigue resuelto).

Con 5 minutos restantes aparece un aviso y se suma una melodía; con 1 minuto la música se acelera; con 30 segundos suena una alarma; los últimos 10 segundos tienen cuenta regresiva con pitidos. Cada terminal tiene un recuadro **«Tu misión»** y un botón **Pedir pista**, que descuenta 30 segundos del tiempo de todo el grupo (avisa en todas las pantallas). Después del **segundo intento fallido** aparece gratis una ayuda mucho más obvia. Un código incompleto no gasta intentos.

## Conexión entre notebooks

El juego puede conectarse de dos maneras. Elige solo según la constante `FIREBASE_URL`, al principio del `<script>`:

- **Firebase Realtime Database** (recomendado): si `FIREBASE_URL` tiene la dirección de la base. Gratis y sin límite práctico de mensajes para una noche de stand. Al empezar cada grupo, el servidor borra los mensajes del grupo anterior.
- **ntfy.sh**: si `FIREBASE_URL` está vacía. Servicio público y gratuito, sin cuenta, pero con un límite diario de mensajes compartido por todas las notebooks del mismo wifi. Una partida usa unos 9.

En las dos, todas las notebooks se encuentran en la misma **sala** (`SALA`). Para probar en casa sin mezclarse con otra partida, agregar `?sala=prueba` antes del `#` en todas las notebooks, por ejemplo `…/escape-room/?sala=prueba#calidad`. El menú muestra la sala y por dónde se está conectando.

Cada pantalla muestra **● EN LÍNEA** u **○ SIN CONEXIÓN**. Si se corta, se reconecta sola y manda lo que quedó pendiente.

### Crear la base de Firebase (una sola vez, unos 10 minutos)

1. Entrar a https://console.firebase.google.com con una cuenta de Google y crear un proyecto (por ejemplo `error500-isfdyt125`). Google Analytics no hace falta.
2. En el menú: **Compilación → Realtime Database → Crear base de datos**. Ubicación: Estados Unidos. Modo: **bloqueado**.
3. En la pestaña **Reglas**, reemplazar todo por esto y tocar **Publicar**:

```json
{
  "rules": {
    "salas": {
      "$sala": {
        ".read": true,
        "msgs": { ".write": true, "$m": { ".validate": "newData.hasChildren(['k', 'u'])" } },
        "hora": { ".write": true },
        "ranking": { ".write": true }
      }
    }
  }
}
```

4. En la pestaña **Datos**, copiar la dirección que aparece arriba (termina en `firebaseio.com` o `firebasedatabase.app`) y pegarla en `FIREBASE_URL`. (Ya está cargada: `https://error500-isfdyt125-default-rtdb.firebaseio.com`.)

Es el plan gratuito (Spark): no pide tarjeta.

### Si falla la conexión: reinicio manual

**Ctrl + Alt + R** en cualquier notebook, sin necesidad de internet:

- **En una terminal:** pone un caso nuevo y deja el desafío listo para jugar. Cuando el servidor arranque la próxima partida, la terminal vuelve a seguirlo sola.
- **En el servidor:** corta la partida y queda listo para el próximo grupo.

Las terminales que están esperando y no tienen conexión también muestran un botón **Jugar sin conexión**.

## Para el equipo (no mostrar a los jugadores)

- **El código cambia en cada partida.** El menú (la dirección sin `#`) muestra el de la partida en curso.
- **Código de respaldo: 3 7 5 4.** Si una terminal se juega sin conexión (o con Ctrl + Alt + R), usa su número de respaldo: Cliente 3, Diseño 7, Código 5, Calidad 4. El servidor acepta, casillero por casillero, el número sorteado o el de respaldo, así una terminal sin conexión no traba al grupo.
- **Próximo grupo:** botón **Siguiente grupo** en el servidor. **Cortar una partida a la mitad:** Ctrl + Alt + R en el servidor (o triple clic en su barra rosa).
- **Sonido:** se genera en el navegador, sin archivos. Los navegadores solo permiten audio después de un clic, por eso la música arranca con el botón Iniciar. Subir el volumen de la notebook del servidor.
- Botón **Pantalla completa** abajo a la derecha (o F11).

## Publicar en GitHub Pages

1. Subir `index.html` y este `README.md` al repositorio `escape-room`.
2. Settings → Pages → Source: *Deploy from a branch* → Branch `master`, carpeta `/ (root)` → Save.
3. En un par de minutos queda en `https://camilafrontera.github.io/escape-room/`. Cada PC abre su dirección de la tabla de arriba.
4. Si después de subir un cambio una PC sigue mostrando la versión vieja, recargar con Ctrl + Shift + R.

## Personalizar

Al principio del `<script>`: `DURATION` (segundos de juego), `MAX_TRIES` (intentos), `PENALTY` (segundos que descuenta cada error) y el objeto `PISTAS`: cada pista tiene una función `build(número, azar, caso)` que arma el desafío para el número sorteado. `RANGO` define entre qué números se sortea cada terminal y `FIJO` el código de respaldo.
