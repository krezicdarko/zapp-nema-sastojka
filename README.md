# Nema sastojka — Zapp

Restoran jednim klikom gasi prilog (npr. kupus) na **svim jelima odjednom**, po lokalu.
Svako jutro se sve automatski vraća na dostupno.

**Live:** https://krezicdarko.github.io/zapp-nema-sastojka/

## Kako radi

U Zappu „kupus" nije jedan objekat — svako jelo ima svoju kopiju opcije s vlastitim `option_id`.
Zato stranica prolazi kroz sva jela lokala i radi `PUT /a/product/variants/{id}` sa `status:0`.
Kupčev API ne prikazuje `status:0` opcije, pa prilog nestane iz aplikacije. **Ništa se ne briše** —
povratak je isti poziv sa `status:1`. Gase se samo dodaci; jela se ne diraju.

Nema servera: `api.zapp.io` ima otvoren CORS, pa browser piše direktno na API. Restoran se
prijavljuje **svojim** Zapp nalogom i token ostaje u njegovom browseru — nigdje se ne čuva.

## Dva dijela

| Dio | Gdje živi |
|---|---|
| `index.html` — restoran gasi/pali tokom dana | ovaj repo, GitHub Pages |
| `jutarnji-reset.py` — ujutro vrati sve na dostupno | Mac mini, cron 05:00 |

Automatika i puna dokumentacija su interni: `Shared drives/Zapp AI/Alati/nema-sastojka/`.

## Baseline

Prilozi koji su bili ugašeni **prije** uvođenja sistema smatraju se trajno ugašenima i jutarnji
reset ih nikad ne pali. Zato se baseline mora snimiti **prije** nego lokal dobije link.
