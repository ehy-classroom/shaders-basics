
# Shader-Grundlagen für Webentwickler:innen

Dieses Dokument erklärt die Grundlagen von **Shadern in der Webentwicklung** – kompakt, praxisnah und für Einsteiger:innen mit HTML/CSS/JS-Vorkenntnissen gedacht.

Shader eröffnen ganz neue kreative Möglichkeiten im Browser – von animierten Hintergründen über interaktive Effekte bis hin zu echter 3D-Grafik. Sie ermöglichen Dinge, die mit CSS oder klassischem JavaScript bisher nicht oder nur sehr eingeschränkt möglich waren.

---

## Was sind Shader?

Ein **Shader** ist ein kleines Programm, das direkt auf der **Grafikkarte (GPU)** läuft. Shader bestimmen, wie Inhalte auf dem Bildschirm dargestellt werden – z. B. wie ein Pixel gefärbt wird, wie Licht reagiert oder wie eine Oberfläche sich verformt.

Shader sind essenzieller Bestandteil moderner Grafiksysteme und werden auch im Browser über **WebGL** ausgeführt.

---

## Zwei Haupttypen von Shadern

| Typ               | Aufgabe                                                                 |
|--------------------|------------------------------------------------------------------------|
| **Vertex Shader**   | Verändert die Position von Punkten (Vertices) im 3D-Raum              |
| **Fragment Shader** | Berechnet die Farbe jedes einzelnen Pixels ("Fragmente" = Bildpunkte) |

Im Web beginnt man meist mit **Fragment Shadern**, weil sie sich direkt auf das sichtbare Bild auswirken und leichter verständlich sind.

---

## Shader im Web verwenden

Im Browser nutzt man Shader über die API **WebGL**. Die Arbeit damit erfolgt meist in Kombination mit JavaScript – direkt oder über Hilfsbibliotheken wie:

- **Three.js** – vereinfacht das Arbeiten mit WebGL enorm
- **GLSL (OpenGL Shading Language)** – die Sprache, in der Shader geschrieben werden
- **Canvas-Elemente** – als Ausgabefläche für die GPU

> Hinweis: WebGL ist in allen modernen Browsern verfügbar (Chrome, Firefox, Safari, Edge, ...)

---

## Die Rolle des Canvas-Elements

Das **`<canvas>`-Element** ist der zentrale Ausgabepunkt für Shader im Web. Es dient als Zeichenfläche, auf der die GPU ihre Berechnungen darstellt.

**Ist Canvas immer notwendig?**
- **Ja, für WebGL-Shader** – Canvas ist die einzige Möglichkeit, WebGL-Inhalte im Browser darzustellen
- **Nein, für CSS-Effekte** – CSS hat eigene Shader-ähnliche Eigenschaften (`filter`, `backdrop-filter`)
- **Alternativen**: SVG-Filter können ähnliche Effekte erzielen, aber mit weniger Flexibilität

**Warum Canvas?**
- Canvas bietet direkten Zugriff auf die GPU über WebGL
- Es ermöglicht pixelgenaue Kontrolle über die Darstellung
- Canvas kann interaktiv auf Maus- und Tastatureingaben reagieren
- Die Ausgabe ist performant und in Echtzeit möglich

```html
<canvas id="myCanvas" width="800" height="600"></canvas>
```

Das Canvas-Element wird dann von JavaScript und WebGL "übernommen" und als Ausgabefläche für Shader-Berechnungen verwendet.

---

## 3D-Rotation: Von Grad zu Radiant

Beim Arbeiten mit 3D-Objekten ist die **Rotation** ein zentrales Konzept. WebGL und Three.js verwenden **Radiant** statt Grad für Winkelangaben.

**Die Umrechnung:**
```
Radiant = Grad × (π / 180)
```

**Beispiele:**
- 10° = 10 × (π / 180) = 0.1745 Radiant
- 30° = 30 × (π / 180) = 0.5236 Radiant  
- 90° = 90 × (π / 180) = 1.5708 Radiant
- 180° = 180 × (π / 180) = 3.1416 Radiant (π)

**Warum Radiant?**
- Radiant ist die mathematisch "natürliche" Einheit für Winkel
- Trigonometrische Funktionen (sin, cos) arbeiten mit Radiant
- GPUs rechnen intern immer mit Radiant

**In der Praxis:**
```javascript
// Rotation um 10° in allen Achsen
object.rotation.x = 0.1745;  // 10° in Radiant
object.rotation.y = 0.1745;  // 10° in Radiant
object.rotation.z = 0.1745;  // 10° in Radiant

// Oder mit der Formel:
object.rotation.x = 10 * (Math.PI / 180);
```

---

## Ein einfaches Fragment-Shader-Konzept

Ein Fragment Shader hat Zugriff auf Informationen wie:

- Die Position des aktuellen Pixels (`gl_FragCoord`)
- Die Bildschirmgröße (als `uniform`)
- Die Zeit (`uniform float u_time`) – um Animationen zu ermöglichen

Beispielhafte Logik in GLSL:

```glsl
void main() {
  vec2 st = gl_FragCoord.xy / u_resolution.xy;
  vec3 color = vec3(st.x, st.y, 0.5);
  gl_FragColor = vec4(color, 1.0);
}
````

Bedeutung:

* `st` ist die Normalisierung der Pixelkoordinaten (Werte zwischen 0.0 und 1.0)
* Die Farbe ändert sich basierend auf der Position – links ist rot, oben grün usw.

---

## Was Shader auf dem Web ermöglichen

Mit Shadern lassen sich visuelle Effekte erstellen wie:

* Dynamische Farbverläufe
* Interaktive Hintergründe (z. B. auf Mausbewegung reagierend)
* Simulationen von Wasser, Feuer, Nebel
* Glitch-, Blur- oder Noise-Effekte
* Licht- und Schattenberechnungen
* Echtzeitfilter auf Bildern und Videos

---

## Empfohlene Lernquellen

* [The Book of Shaders](https://thebookofshaders.com/) – interaktives Tutorial (englisch)
* [ShaderToy](https://shadertoy.com) – Plattform zum Testen und Teilen von Shadern
* [Three.js](https://threejs.org) – JavaScript-Framework für WebGL
* [GLSL Language Guide (OpenGL)](https://www.khronos.org/opengl/wiki/OpenGL_Shading_Language)

---

## Begriffe, die du bald kennen wirst

* **uniform** – ein globaler Wert, der vom JS aus an den Shader übergeben wird (z. B. Zeit, Mausposition)
* **vec2 / vec3 / vec4** – Vektortypen für 2D/3D/4D-Werte in GLSL
* **gl\_FragColor** – der endgültige Farbwert, den der Shader für einen Pixel zurückgibt
* **sin / cos / mod / fract** – mathematische Funktionen für kreative Animationen

---

## Warum Shader lernen?

Shader geben dir die Kontrolle über jedes einzelne Pixel und öffnen damit eine neue visuelle Ebene im Webdesign – GPU-beschleunigt, animiert, dynamisch.

Sie sind nicht nur für Spiele oder 3D-Welten interessant, sondern auch für kreative Websites, visuelle Installationen oder datengesteuerte Visualisierungen.

---
