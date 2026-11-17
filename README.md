# Mijn To-Do App

## 1. Beschrijving

Voor deze opdracht heb ik een To-Do App gebouwd met Vue.js en Tailwind CSS.

Met deze applicatie kan een gebruiker taken toevoegen, bekijken, afvinken en verwijderen. Ook kan de gebruiker prioriteiten instellen en taken filteren op basis van hun status.

De applicatie bewaart de taken automatisch in Local Storage. Hierdoor blijven de taken bestaan wanneer de pagina wordt vernieuwd.

## 2. Functionaliteiten

De applicatie bevat de volgende functionaliteiten:

- Taken toevoegen via een invoerveld.
- Een prioriteit kiezen voor iedere taak: Hoog, Middel of Laag.
- Taken weergeven in een overzichtelijke lijst.
- Taken afvinken wanneer ze zijn afgerond.
- Taken verwijderen.
- Taken filteren op Alles, Open en Afgerond.
- Het aantal totale, open en afgeronde taken weergeven.
- Taken automatisch opslaan in Local Storage.

## 3. Gebruikte technieken

Voor het bouwen van deze applicatie heb ik gebruikgemaakt van:

- **Vue.js:** voor de structuur en functionaliteit van de applicatie.
- **JavaScript:** voor de functies en logica.
- **Tailwind CSS:** voor de vormgeving en lay-out.
- **Vite:** voor het opzetten en starten van het project.
- **Local Storage:** voor het bewaren van taken in de browser.

## 4. Projectstructuur

De belangrijkste bestanden binnen het project zijn:

```text
my-project/
│
├── src/
│   ├── components/
│   │   └── TodoItem.vue
│   │
│   ├── App.vue
│   ├── main.js
│   └── style.css
│
├── index.html
├── package.json
├── package-lock.json
├── vite.config.js
└── README.md
```

### Uitleg bestanden

- **App.vue:** bevat de hoofdfunctionaliteiten, zoals taken toevoegen, verwijderen, filteren en opslaan.
- **TodoItem.vue:** is een eigen component waarmee iedere taak wordt weergegeven en afgevinkt.
- **main.js:** start de Vue-applicatie.
- **style.css:** bevat de basisopmaak en importeert Tailwind CSS.
- **vite.config.js:** bevat de configuratie van Vite en de Vue- en Tailwind-plugins.
- **package.json:** bevat de projectinformatie en benodigde afhankelijkheden.

## 5. Installatie en starten

Om het project lokaal te starten, zijn Node.js en npm nodig.

Volg de onderstaande stappen:

**Stap 1.** Download of clone het GitHub-repository.

**Stap 2.** Open de projectmap in Visual Studio Code.

**Stap 3.** Open de terminal in de hoofdmap van het project.

**Stap 4.** Installeer de benodigde dependencies:

```bash
npm install
```

**Stap 5.** Start de ontwikkelserver:

```bash
npm run dev
```

**Stap 6.** Open de lokale URL die in de terminal verschijnt, meestal:

```text
http://localhost:5173/
```

De applicatie is nu klaar voor gebruik.

## 6. Opslag van taken

De applicatie gebruikt Local Storage om de taken op te slaan in de browser.

Wanneer een gebruiker een taak toevoegt, verwijdert of afvinkt, worden de wijzigingen automatisch opgeslagen. Bij het opnieuw openen of vernieuwen van de pagina worden de opgeslagen taken opnieuw geladen.
