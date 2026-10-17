# ERROR 500 · Sala de acertijos

Juego para el stand de 3.º año de la **Tecnicatura Superior en Desarrollo de Software** del ISFDyT N° 125 en la Noche del Instituto.

El sistema del instituto "se cayó" y un grupo de 3 a 6 visitantes tiene **10 minutos** para resolver cuatro acertijos en cuatro terminales, armar el código y restaurarlo desde el servidor conectado al proyector.

## Direcciones para cada PC

| PC | Abrir |
|---|---|
| 1 ▲ Calidad | https://camilafrontera.github.io/escape-room/#calidad |
| 2 ◆ Cliente | https://camilafrontera.github.io/escape-room/#cliente |
| 3 ● Código | https://camilafrontera.github.io/escape-room/#codigo |
| 4 ★ Diseño | https://camilafrontera.github.io/escape-room/#diseno |
| Proyector | https://camilafrontera.github.io/escape-room/#servidor |

## Las terminales

Es una sola página. Cada computadora abre la misma dirección con un final distinto:

| PC | Dirección | Qué muestra |
|---|---|---|
| 1 | `index.html#calidad` | ▲ Pantalla con errores para encontrar (la más sencilla) |
| 2 | `index.html#cliente` | ◆ Mensaje cifrado del cliente |
| 3 | `index.html#codigo` | ● Programa en inglés para leer |
| 4 | `index.html#diseno` | ★ Página sin estilos: cambiar el CSS hasta que aparezca el número |
| Proyector | `index.html#servidor` | ERROR 500, timer de 10 minutos con música y código final |

**Cada desafío tiene varias versiones (casos)** y cada terminal elige una al azar cada vez que se abre o se reinicia, sin repetir la anterior. Todas las versiones de un desafío dan el mismo número, así que el código final no cambia nunca. Arriba a la derecha de cada terminal se ve qué caso tocó.

Sin nada después del `#` se abre un menú con la dirección exacta de cada notebook y el estado de la conexión.

## Cómo se juega

Las cinco notebooks están **conectadas en tiempo real**:

1. Mientras no hay partida, las terminales muestran **«Esperando al servidor»**.
2. El game master toca **Iniciar partida** en el servidor. En ese momento se desbloquean las cuatro terminales a la vez, arranca el timer (también se ve arriba en cada terminal) y la música de tensión.
3. El grupo resuelve las cuatro terminales **al mismo tiempo**. Cada respuesta correcta muestra un fragmento numerado (1 a 4) con su dígito y un dato corto **«¿Sabías que…?»**. El servidor marca cada terminal como RECUPERADO apenas se resuelve, y en la barra de cada terminal se ve cuáles ya resolvió el resto del equipo.
4. En el servidor se ingresa cada fragmento en su casillero. Hay **3 intentos como máximo**. Cada código incorrecto **descuenta 30 segundos**, muestra ACCESO DENEGADO y avisa en todas las terminales.
5. Si aciertan: **SISTEMA RESTAURADO** con lluvia de píxeles, música, tiempo, intentos y un nivel según el tiempo que sobró. Si usan los 3 intentos: **SISTEMA BLOQUEADO**. Si se termina el tiempo: **SE ACABÓ EL TIEMPO**. El resultado aparece también en las cuatro terminales.
6. **Siguiente grupo** en el servidor reinicia todo: las terminales vuelven a «Esperando al servidor», cada una con un caso nuevo.

Las terminales **no se pueden reiniciar por separado**: solo se reinician cuando el servidor termina una partida y empieza la siguiente. Si una notebook se recarga en medio de una partida, vuelve al mismo caso (y si ya lo había resuelto, sigue resuelto).

Con 5 minutos restantes aparece un aviso y se suma una melodía; con 1 minuto la música se acelera; con 30 segundos suena una alarma; los últimos 10 segundos tienen cuenta regresiva con pitidos. Cada terminal tiene arriba un recuadro **«Tu misión»**. Desde el primer intento fallido aparece una ayuda con un ejemplo resuelto. Un código incompleto no gasta intentos.

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
        "hora": { ".write": true }
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

- Soluciones (iguales en todos los casos):
  - 1 ▲ Calidad: siempre hay **4 errores** → **4**. Casos: inscripción, certificado, mesa de examen, biblioteca.
  - 2 ◆ Cliente: el mensaje siempre dice **TRES** → **3**. Casos: César +3 (WUHV), César +1 (USFT), números (20·18·5·19), abecedario invertido (GIVH).
  - 3 ● Código: el programa siempre muestra **5** → **5**. Casos: suma en bucle, restas en orden, bucle con multiplicación, condición if/else, bucle de 0 a 5.
  - 4 ★ Diseño: al elegir los valores correctos aparece el **7** → **7**. Casos: `block` + `navy` + `48px` · `visible` + `1` + `56px` · `inline-block` + `purple` + `1` · `visible` + `darkred` + `64px`. Los colores están en palabras y cada opción tiene su significado en español.
- **Código final: 4 3 5 7.** Está en la constante `CODE` de `index.html`.
- **Próximo grupo:** botón **Siguiente grupo** en el servidor. **Cortar una partida a la mitad:** Ctrl + Alt + R en el servidor (o triple clic en su barra rosa).
- **Sonido:** se genera en el navegador, sin archivos. Los navegadores solo permiten audio después de un clic, por eso la música arranca con el botón Iniciar. Subir el volumen de la PC del proyector.
- Botón **Pantalla completa** abajo a la derecha (o F11).

## Publicar en GitHub Pages

1. Subir `index.html` y este `README.md` al repositorio `escape-room`.
2. Settings → Pages → Source: *Deploy from a branch* → Branch `master`, carpeta `/ (root)` → Save.
3. En un par de minutos queda en `https://camilafrontera.github.io/escape-room/`. Cada PC abre su dirección de la tabla de arriba.
4. Si después de subir un cambio una PC sigue mostrando la versión vieja, recargar con Ctrl + Shift + R.

## Personalizar

Al principio del `<script>`: `CODE` (código final), `DURATION` (segundos de juego), `MAX_TRIES` (intentos), `PENALTY` (segundos que descuenta cada error) y el objeto `PISTAS`: cada pista tiene una lista `variants` con sus casos. Para agregar un caso nuevo, copiá uno existente y cambiá el contenido, cuidando que dé el mismo número.