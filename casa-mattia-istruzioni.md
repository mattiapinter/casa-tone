# Istruzioni per Claude Code: Guida Ospiti Casa Mattia

## Contesto progetto

Repository: `mattiapinter/guida-appartamenti`
Branch sviluppo: creare un nuovo branch `claude/update-guest-guide-XXXXX`
File da creare: `casa-mattia/index.html`
Riferimento: `casa-marika/index.html` (copiare come base, poi modificare)

Push: usare sempre PAT (vedi sezione Git in fondo).

---

## Casa Mattia vs Casa Marika: differenze chiave

| Elemento | Casa Marika | Casa Mattia |
|---|---|---|
| Camere | 2 camere | 1 camera |
| Esterno | Terrazza arredata | Giardino |
| Posizione | Via dello Zenzero 16F | stessa via/zona, chiedere indirizzo esatto |

Tutto il resto (zona, spiagge, ristoranti, negozi, contatti, regole) è identico o molto simile.

---

## Struttura tecnica del file HTML

Single-file HTML con CSS inline e JS inline. Nessuna dipendenza esterna tranne:
- Google Fonts (Inter + Fraunces)
- Mapbox GL JS v3.3.0

### Sistema multilingua

Tre lingue: italiano (default), inglese, tedesco.

**Blocchi interi** — nascosti/mostrati con `data-l`:
```html
<div data-l="it" class="on">...</div>
<div data-l="en">...</div>
<div data-l="de">...</div>
```
CSS: `[data-l]{display:none} [data-l].on{display:block}`

**Inline (parole singole)** — con `data-li`:
```html
<span data-li="it" class="on">Copia</span>
<span data-li="en">Copy</span>
<span data-li="de">Kopieren</span>
```
CSS: `[data-li]{display:none} [data-li].on{display:inline}`

JS `setLang(l)` gestisce il toggle.

### Variabili CSS principali
```css
:root {
  --green:#0097a7; --green-dark:#006d79; --green-bg:#e0f7fa;
  --navy:#0c3b5e; --navy-light:#154e7a;
  --bg:#f5f8fa; --surface:#fff; --border:#dde4e8;
  --text:#1a1a1a; --muted:#7a756e;
}
```

### Pattern accordion
```html
<div class="acc-item">
  <button class="acc-trig" onclick="accToggle(this)">
    <span class="acc-left">
      <span style="font-size:18px">EMOJI</span>
      <span data-li="it" class="on">IT label</span>
      <span data-li="en">EN label</span>
      <span data-li="de">DE label</span>
    </span>
    <span class="acc-chev">▼</span>
  </button>
  <div class="acc-body"><div class="acc-inner">
    <div data-l="it" class="on">IT content</div>
    <div data-l="en">EN content</div>
    <div data-l="de">DE content</div>
  </div></div>
</div>
```

### Pattern contatti (ci-exp)
```html
<div class="ci-exp">
  <div class="ci-hd" onclick="ciToggle('ID')">
    <span class="ci-icon">EMOJI</span>
    <div style="flex:1">
      <div class="ci-label">Nome</div>
      <div class="ci-val">+39 xxx xxx xxxx</div>
      <div class="ci-note">...</div>
    </div>
    <span class="ci-arr" id="ID-arr">▼</span>
  </div>
  <div class="ci-actions" id="ID-actions">
    <a class="ci-btn" href="tel:+39...">📞 Chiama</a>
    <a class="ci-btn" href="https://wa.me/39..." target="_blank">💬 WhatsApp</a>
  </div>
</div>
```

### Modalità animali (pet mode)
Attivata via URL: `?pet=1`
```css
.pet-only{display:none}
html.has-pet .pet-only{display:block}
```
JS: `if(new URLSearchParams(window.location.search).has('pet')){ document.documentElement.classList.add('has-pet'); }`

---

## Sezioni della guida (ordine navbar)

1. **Mappa** — hero Mapbox con filtri per categoria (casa, spiagge, ristoranti, negozi, supermercato, attività)
2. **Meteo** — strip meteo con OpenMeteo API
3. **Dove siamo** (`sec-dove`) — indirizzo, parcheggio, link navigazione Google Maps
4. **Check-in/out** (`sec-checkin`) — orari, Debora per chiavi, punto raccolta rifiuti, nota €20
5. **Regole della casa** (`sec-rules`) — accordion rifiuti (primo), poi lista regole, pet card collapsible
6. **Wi-Fi** (`sec-wifi`) — rete + password con copy button
7. **Info appartamento** (`sec-casa`) — card acqua rubinetto, card aria condizionata
8. **Spiagge vicine** (`sec-beaches`) — highlight spiaggesardegna.eu + 4 spiagge card + Tavolara
9. **Contatti** (`sec-contacts`) — Mattia (👦 primo), Debora (👩 check-in/out), Others collapsible (👨 Andrea + 👩‍🦱 Paola)
10. **Lascia una recensione** (`sec-review`) — testo Airbnb/Booking
11. **Footer** — Appartamenti Brigadoi

---

## Contenuti Casa Mattia (dati reali da Notion)

### Indirizzo e coordinate
- Indirizzo: Via Maltineddu 19C, Murta Maria (Olbia, SS)
- Coordinate mappa casa: lat 40.8865387, lng 9.5890972
- Google Maps link: https://www.google.com/maps/dir//40.8865387,9.5890972
- Parcheggio: sotto la tettoia all'interno della proprietà

### Check-in/out
- Check-in: dalle 15:00
- Check-out: entro le 10:00
- Debora chiavi: +39 340 352 1832
- Raccolta rifiuti al check-out: rotonda Murta Maria, 8:00-13:00 (DIVERSO da Marika che era 6:00-24:00)
- Nessun codice fiscale richiesto (differenza da Marika)
- Testo checkout Notion: "portare la spazzatura al punto di raccolta presso la rotonda di Murta Maria. Se uscite senza incontrarci mandateci un messaggio."

### Wi-Fi
- Rete: Bilo
- Password: casamattia

### Acqua (tono diverso da Casa Marika)
- L'acqua viene dall'acquedotto, sicura per uso quotidiano (doccia, denti, piatti)
- Per bere e cucinare, consigliata acqua in bottiglia
- NON usare "non consigliata per bere" come a Marika, qui il tono è più positivo

### Giardino
- Perfetto per mangiare all'aperto con il sole
- Gli ospiti lo adorano
- Nessun riferimento a tramonti (quelli erano dalla terrazza di Marika)

### Zanzariere
- Presenti su tutte le finestre (a Marika erano state rimosse dall'accordion, qui possono restare come FAQ)

### Regole (da Notion)
1. Raccolta differenziata
2. No fumo all'interno
3. Spegnere luci e aria condizionata se non in casa
4. Avvisare subito danni/rotture
5. Svuotare frigo e cassetti alla partenza
6. Lasciare in ordine (altrimenti tariffe pulizia extra)

### Regole rifiuti (adattare da Marika)
- Guida differenziata: verificare se c'è cartello sul frigo anche a Casa Mattia (chiedere a Mattia)
- Porta a porta: stesso calendario presumibilmente (Martedi plastica/vetro/umido, Giovedi carta, Sabato secco/umido) — verificare
- Punto raccolta: rotonda Murta Maria, 8:00-13:00

### Contatti
| Persona | Tel | Emoji | Note |
|---|---|---|---|
| Mattia | +39 349 136 8343 | 👦 | Principale, call+WA |
| Debora | +39 340 352 1832 | 👩 | Solo check-in/out, call+WA |
| Andrea (papà) | +39 348 760 0318 | 👨 | In "Others", solo WA in EN/DE |
| Paola (mamma) | +39 349 284 1605 | 👩‍🦱 | In "Others", solo WA in EN/DE |

EN/DE: Andrea e Paola mostrano solo WA (no pulsante Chiama). IT mostra anche Chiama.

### Mappa PLACES (array JS)
Categorie e relativi colori marker:
- `casa` → rosso `#e74c3c`
- `spiagge` → teal `#0097a7`
- `ristoranti` → arancione `#e67e22`
- `negozi` → viola `#8e44ad`
- `supermercato` → teal scuro `#00838f`
- `attivita` → verde `#16a085`

Ogni place ha: `lat`, `lng`, `cat`, `ssurl` (solo spiagge), `it/en/de: {name, desc}`.

Spiagge con link spiaggesardegna.eu (slug pattern: `spiaggia-[nome]`).

---

## Adattamenti specifici per Casa Mattia

### Differenze da applicare rispetto a Casa Marika

1. **Titolo**: "Casa Marika" diventa "Casa Mattia" ovunque nel file
2. **Favicon/brand**: stessa ancora, stessa famiglia Brigadoi
3. **Camere**: 1 camera invece di 2
4. **Giardino**: sostituire terrazza con giardino. Testo suggerito: "Il giardino e perfetto per mangiare all'aperto. Gli ospiti lo adorano."
5. **Indirizzo**: Via Maltineddu 19C, Murta Maria (Olbia, SS)
6. **Coordinate**: lat 40.8865387, lng 9.5890972
7. **Parcheggio**: sotto la tettoia all'interno della proprieta
8. **Wi-Fi**: Rete Bilo / Password casamattia
9. **Raccolta rifiuti orari**: 8:00-13:00 (non 6:00-24:00 come Marika). Nessun codice fiscale.
10. **Acqua**: tono positivo (diverso da Marika). Sicura per uso quotidiano, bottiglia solo per bere/cucinare
11. **Spiagge/ristoranti/mappa**: identici, stessa zona, stessi PLACES

### Tone of voice
- Caldo, diretto, familiare
- No em dash (mai, in nessuna lingua)
- Frasi corte con punto finale
- No jargon alberghiero

---

## Git workflow

```bash
# Setup branch
git checkout -b claude/update-guest-guide-XXXXX

# Push con PAT (sostituire <PAT> con token attivo)
git remote set-url origin https://x-access-token:<PAT>@github.com/mattiapinter/guida-appartamenti.git
git push origin claude/update-guest-guide-XXXXX:main
git push origin claude/update-guest-guide-XXXXX
git remote set-url origin https://github.com/mattiapinter/guida-appartamenti.git
```

PAT: chiedere a Mattia il token GitHub (Settings → Developer settings → Personal access tokens → scope: all repos).

---

## Changelog guida Casa Marika (sessioni precedenti)

### Contenuto aggiornato
- Acqua: solo "non consigliata per bere, usa bottiglia, per il resto ok"
- Aria condizionata: "condizionatore" (non "split"), ogni stanza, spegnere all'uscita
- Tramonti terrazza: "spettacolari"
- Raccolta rifiuti: punto di raccolta rotonda Murta Maria + calendario porta a porta
- Cisterna 2.000L: menzione rimossa dalla sezione acqua (era confusionaria)

### Struttura modificata
- Sezione "Vivere la casa" → "Info appartamento": rimossi accordion zanzariere e terrazza, ora due card dirette
- Regole: accordion ♻️ come prima regola (no duplicato), rimosse le regole pet dal rlist
- Pet: card ambra collapsible sopra le regole con tono accogliente
- Regole pet: no salire su divani/letti, no stoviglie cucina, non lasciare solo

### Contatti
- Ordine: Mattia primo, Debora secondo, Andrea+Paola in "Others" collapsible
- Emoji: Mattia 👦, Debora 👩, Andrea 👨, Paola 👩‍🦱
- EN/DE: Andrea e Paola solo WhatsApp (no tasto chiama)

### Mappa
- Beach popups: link diretto spiaggesardegna.eu per ogni spiaggia
- "Spiagge vicine a me" button in sezione spiagge
- Lu Tricu: solo panificio (pane fresco, carasau, guttiau, panini da spiaggia)

### Fix generali
- Rimosse tutte le em dash dal file
- Punti finali aggiunti a tutte le regole
- Nota €20: "Se dovessimo occuparcene noi, è previsto un costo extra di 20€ per il servizio di smaltimento, da saldare direttamente a Debora"
