# Kryptografiverktøy

Et enkelt kryptografiverktøy laget i Svelte. Hver krypteringsmetode har sin egen side, og forsiden og menyen øverst lenker til dem.

Implementert:
- Caesar
- Vigenère
- One-Time Pad

## Sider

- `/` – forside med lenker
- `/caesar` – Caesar
- `/vigenere` – Vigenère
- `/otp` – One-Time Pad

Merk: One-Time Pad bruker Math.random(), som ikke er trygt for ekte nøkler. Det er bare en demonstrasjon.

## Starte prosjektet

```
npm install
npm run dev
```

Åpne linken som kommer opp i terminalen (som regel http://localhost:5173).

## Kilder

- svelte.dev/docs
- Wikipedia: Caesar cipher, Vigenère cipher, One-time pad
