---
layout: ~/layouts/MainLayout.astro
title: Konfigurations&shy;referenz
setup: |
  import Since from '../../../components/Since.astro';
---

Die folgenden Referenzen beschreiben alle unterstützten Konfigurationsoptionen in Astro. Wenn du mehr über die Konfiguration von Astro erfahren möchtest, lies unsere Anleitung [Astro konfigurieren](/de/guides/configuring-astro/).

```js
// astro.config.mjs
import { defineConfig } from 'astro/config'

export default defineConfig({
  // Deine Konfigurationsoptionen hier...
})
```
## Top-Level-Optionen

### root

<p>

**Typ:** `string`<br>
**CLI:** `--root`<br>
**Standard:** `"."` (Aktuelles Arbeitsverzeichnis)
</p>

Du solltest diese Option nur angeben, wenn du die `astro`-CLI-Befehle in einem anderen Verzeichnis als dem Stammverzeichnis deines Projekts ausführst. Normalerweise wird diese Option über die Kommandozeile statt über die [Astro-Konfigurationsdatei](/de/guides/configuring-astro/#supported-config-file-types) bereitgestellt, da Astro dein Projektstammverzeichnis kennen muss, bevor es deine Konfigurationsdatei finden kann.

Wenn du einen relativen Pfad angibst (z.B.: `--root: './mein-projekt'`), löst Astro diesen gegen das aktuelle Arbeitsverzeichnis auf.

#### Beispiele

```js
{
  root: './mein-projektverzeichnis'
}
```
```bash
$ astro build --root ./mein-projektverzeichnis
```


### srcDir

<p>

**Typ:** `string`<br>
**Standard:** `"./src"`
</p>

Legt das Verzeichnis fest, aus dem Astro deine Website lesen soll.

Der Wert kann entweder ein absoluter Dateisystempfad oder ein Pfad relativ zum Projektstamm sein.

```js
{
  srcDir: './www'
}
```


### publicDir

<p>

**Typ:** `string`<br>
**Standard:** `"./public"`
</p>

Legt das Verzeichnis für deine statischen Assets fest. Die Dateien in diesem Verzeichnis werden während der Entwicklung unter `/` bereitgestellt, und während des Build-Prozesses in dein Build-Verzeichnis kopiert. Diese Dateien werden immer unverändert bereitgestellt oder kopiert, ohne Transformation oder Bündelung.

Der Wert kann entweder ein absoluter Dateisystempfad oder ein Pfad relativ zum Projektstamm sein.

```js
{
  publicDir: './mein-eigenes-publicDir-verzeichnis'
}
```


### outDir

<p>

**Typ:** `string`<br>
**Standard:** `"./dist"`
</p>

Legt das Verzeichnis fest, in das `astro build` deinen endgültigen Build schreibt.

Der Wert kann entweder ein absoluter Dateisystempfad oder ein Pfad relativ zum Projektstamm sein.

```js
{
  outDir: './mein-eigenes-build-verzeichnis'
}
```


### site

<p>

**Typ:** `string`
</p>

Die endgültige URL deiner Seite bei deinem Hosting-Anbieter. Astro verwendet diese vollständige URL, um deine Sitemap und kanonische URLs in deinem endgültigen Build zu generieren. Es wird ausdrücklich empfohlen, diese Konfigurationsoption festzulegen, um Astro optimal nutzen zu können.

```js
{
  site: 'https://www.meine-website.dev'
}
```


### base

<p>

**Typ:** `string`
</p>

Der Basispfad, unter dem du deine Website hosten möchtest. Astro wird diesen Pfadnamen auch während der Entwicklung verwenden, damit deine Entwicklungsumgebung so gut wie möglich mit deiner Build-Umgebung übereinstimmt. Im folgenden Beispiel wird dein Server mit `astro dev` unter `/dokumentation` gestartet.

```js
{
  base: '/dokumentation'
}
```


### trailingSlash

<p>

**Typ:** `'always' | 'never' | 'ignore'`<br>
**Standard:** `'ignore'`
</p>

Legt das Verhalten des Entwicklungsservers beim Zuordnen von URLs zu Seiten im Dateisystem fest. Wähle aus den folgenden Optionen:
  - `'always'` - Nur URLs mit abschließendem Schrägstrich zuordnen (z. B. "/foo/")
  - `'never'` - Nur URLs ohne abschließenden Schrägstrich zuordnen (z. B. "/foo")
  - `'ignore'` - URLs unabhängig von der Präsenz eines abschließenden "/" zuordnen

Verwende diese Konfigurationsoption, wenn dein Hosting-Anbieter strikte Regeln dafür hat, wie abschließende Schrägstriche funktionieren oder nicht funktionieren.

Du kannst diese Einstellung auch vornehmen, wenn du dir selbst striktere Vorgaben machen willst, dass URLs mit oder ohne abschließende Schrägstriche während der Entwicklung nicht funktionieren.

```js
{
  // Beispiel: Erfordere abschließende Schrägstriche
  // in Seiten-URLs während der Entwicklung
  trailingSlash: 'always'
}
```
**Siehe auch:**
- build.format


## Build-Optionen

### build.format

<p>

**Typ:** `('file' | 'directory')`<br>
**Standard:** `'directory'`
</p>

Bestimmt das Format, nach dem die Ausgabedatei jeder Seite benannt werden soll:
  - Bei "file" erzeugt Astro für jede Seite eine HTML-Datei (z. B. "/foo.html").
  - Bei "directory" erzeugt Astro für jede Seite ein Verzeichnis mit einer darin enthaltenen `index.html`-Datei (z. B.: "/foo/index.html").

```js
{
  build: {
    // Beispiel: Erzeuge `page.html` statt `page/index.html`
    // während des Build-Prozesses.
    format: 'file'
  }
}
```


## Server-Optionen

Passe den Astro-Entwicklungsserver an, der sowohl von `astro dev` als auch von `astro preview` verwendet wird.

```js
{
  server: { port: 1234, host: true }
}
```

Um unterschiedliche Konfigurationen auf der Grundlage des ausgeführten Befehls ("dev", "preview") festzulegen, kann dieser Konfigurationsoption auch eine Funktion übergeben werden.

```js
{
  // Beispiel: Verwende die Funktionssyntax, um Anpassungen
  // auf der Grundlage des ausgeführten Befehls vorzunehmen
  server: (command) => ({ port: command === 'dev' ? 3000 : 4000 })
}
```

### server.host

<p>

**Typ:** `string | boolean`<br>
**Standard:** `false`<br>
<Since v="0.24.0" />
</p>

Legt fest, unter welchen Netzwerk-IP-Adressen der Entwicklungsserver erreichbar sein soll (d. h. IPs außerhalb von `localhost`).
- `false` - Keine Erreichbarkeit über Netzwerk-IP-Adressen
- `true` - Erreichbarkeit über alle Adressen, inkl. LAN- und Internet-Adressen
- `[eigene-adresse]` - Erreichbarkeit über die IP-Adresse `[eigene-adresse]` (z. B. `192.168.0.1`)


### server.port

<p>

**Typ:** `number`<br>
**Standard:** `3000`
</p>

Legt fest, unter welchem Port der Entwicklungsserver erreichbar sein soll.

Wenn der angegebene Port bereits belegt ist, versucht Astro automatisch den nächsten verfügbaren Port zu verwenden.

```js
{
  server: { port: 8080 }
}
```


## Markdown-Optionen

### markdown.drafts

<p>

**Typ:** `boolean`<br>
**Standard:** `false`
</p>

Legt fest, ob Markdown-Entwurfsseiten in den Build aufgenommen werden sollen.

Eine Markdown-Seite wird als Entwurf betrachtet, wenn sie die Frontmatter-Eigenschaft `draft: true` enthält. Entwurfsseiten sind während der Entwicklung (`astro dev`) immer enthalten und sichtbar, werden aber standardmäßig nicht in den endgültigen Build aufgenommen.

```js
{
  markdown: {
    // Beispiel: Alle Entwürfe in den endgültigen Build einbeziehen
    drafts: true,
  }
}
```


### markdown.mode

<p>

**Typ:** `'md' | 'mdx'`<br>
**Standard:** `mdx`
</p>

Legt fest, ob Markdown-Inhalte mit oder ohne MDX verarbeitet werden sollen.

Die MDX-Verarbeitung ermöglicht dir die Verwendung von JSX innerhalb deiner Markdown-Dateien. Falls du diese Funktionalität nicht benötigst und eine "normale" Markdown-Verarbeitung bevorzugst, kannst du diese Einstellung verwenden, um das gewünschte Verhalten festzulegen.

```js
{
  markdown: {
    // Beispiel: Verarbeite Markdown-Dateien ohne MDX
    mode: 'md',
  }
}
```


### markdown.shikiConfig

<p>

**Typ:** `Partial<ShikiConfig>`
</p>

Shiki-Konfigurationsoptionen. Siehe unsere [Markdown-Dokumentation](/de/guides/markdown-content/#shiki-configuration).


### markdown.syntaxHighlight

<p>

**Typ:** `'shiki' | 'prism' | false`<br>
**Standard:** `shiki`
</p>

Legt fest, welche Syntaxhervorhebung verwendet werden soll (wenn überhaupt):
- `shiki` - Verwendet [Shiki](https://github.com/shikijs/shiki) zur Syntaxhervorhebung
- `prism` - Verwendet [Prism](https://prismjs.com/) zur Syntaxhervorhebung
- `false` - Deaktiviert die Syntaxhervorhebung

```js
{
  markdown: {
    // Beispiel: Ändere die Syntaxhervorhebung in Markdown auf Prism
    syntaxHighlight: 'prism',
  }
}
```


### markdown.remarkPlugins

<p>

**Typ:** `RemarkPlugins`
</p>

Übergibt benutzerdefinierte [Remark](https://github.com/remarkjs/remark)-Plugins, um die Verarbeitung deiner Markdown-Inhalte anzupassen.

**Anmerkung:** Die Verwendung der Konfigurationsoptionen `remarkPlugins` oder `rehypePlugins` entfernt Astros native Unterstützung für [Markdown im GitHub-Stil](https://github.github.com/gfm/) und [Smartypants](https://github.com/silvenon/remark-smartypants). Du musst diese Plugins explizit in deine Datei `astro.config.mjs` aufnehmen, falls gewünscht.

```js
{
  markdown: {
    // Beispiel: Die von Astro standardmäßig verwendeten Remark-Plugins
    remarkPlugins: ['remark-gfm', 'remark-smartypants'],
  },
};
```


### markdown.rehypePlugins

<p>

**Typ:** `RehypePlugins`
</p>

Übergibt benutzerdefinierte [Rehype](https://github.com/remarkjs/remark-rehype)-Plugins, um die Verarbeitung deiner Markdown-Inhalte anzupassen.

**Anmerkung:** Die Verwendung der Konfigurationsoptionen `remarkPlugins` oder `rehypePlugins` entfernt Astros native Unterstützung für [Markdown im GitHub-Stil](https://github.github.com/gfm/) und [Smartypants](https://github.com/silvenon/remark-smartypants). Du musst diese Plugins explizit in deine Datei `astro.config.mjs` aufnehmen, falls gewünscht.

```js
{
  markdown: {
    // Beispiel: Die von Astro standardmäßig verwendeten Rehype-Plugins
    rehypePlugins: [],
  },
};
```

## Adapter

Build-Adapter ermöglichen dir die Veröffentlichung auf deinem Lieblings-Webserver, serverlos oder auf Edge-Servern. Importiere einen der von uns gepflegten Adapter für [Netlify](/de/guides/deploy/netlify/#adapter-for-ssredge), [Vercel](/de/guides/deploy/vercel/#adapter-for-ssr) oder andere Anbieter, um serverseitiges Rendern (SSR) in Astro zu aktivieren.

Lies unsere [Anleitung zum serverseitigen Rendern](/de/guides/server-side-rendering/), um mehr über SSR zu erfahren, und [unsere Veröffentlichungs-Anleitungen](/de/guides/deploy/) für eine vollständige Liste der unterstützten Anbieter.

```js
import netlify from '@astrojs/netlify/functions';
{
  // Beispiel: Erzeuge einen Build für die
  // serverlose Veröffentlichung auf Netlify
  adapter: netlify(),
}
```

## Integrationen

Erweitere Astro mit benutzerdefinierten Integrationen. Integrationen sind deine zentrale Anlaufstelle für das Hinzufügen von UI-Framework-Unterstützungen (wie Solid.js), neuen Funktionen (wie Sitemaps) und neuen Bibliotheken (wie Partytown und Turbolinks).

Lies unseren [Integrations-Leitfaden](/de/guides/integrations-guide/), um deine ersten Schritte mit Astro-Integrationen zu machen.

```js
import react from '@astrojs/react';
import tailwind from '@astrojs/tailwind';
{
  // Beispiel: Füge React- & Tailwind-Unterstützung zu Astro hinzu
  integrations: [react(), tailwind()]
}
```

## Vite

Übergibt zusätzliche Konfigurationsoptionen an Vite. Nützlich, wenn Astro eine fortgeschrittene Konfiguration nicht unterstützt, die du vielleicht benötigst.

Sieh dir die vollständige Dokumentation zum Konfigurationsobjekt `vite` auf [vitejs.dev](https://vitejs.dev/config/) an.

#### Beispiele

```js
{
  vite: {
    ssr: {
      // Beispiel: Erzwinge das Überspringen eines defekten Pakets
      // bei der SSR-Verarbeitung, falls erforderlich
      external: ['defektes-npm-paket'],
    }
  }
}
```

```js
{
  vite: {
    // Beispiel: Füge benutzerdefinierte Vite-Plugins
    // direkt zu deinem Astro-Projekt hinzu
    plugins: [meinPlugin()],
  }
}
```

