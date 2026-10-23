<script>
	const A = 65; // tegnkoden til 'A', brukes for å gjøre bokstaver om til 0-25

	let tekst = $state('');
	let nokkel = $state('');
	let resultat = $state('');
	let feil = $state('');

	// Flytter én bokstav "steg" plasser. Negativt steg = andre veien.
	function flyttBokstav(bokstav, steg) {
		let tall = bokstav.charCodeAt(0) - A; // A=0, B=1, ..., Z=25
		let nyttTall = (((tall + steg) % 26) + 26) % 26; // modulo, og +26 for å unngå negative tall
		return String.fromCharCode(nyttTall + A);
	}

	function caesar(tekst, steg) {
		return tekst
			.toUpperCase()
			.split('')
			.map((tegn) => (tegn === ' ' ? tegn : flyttBokstav(tegn, steg)))
			.join('');
	}

	// Sjekker teksten og nøkkelen. Returnerer en feilmelding, eller tom tekst hvis alt er ok.
	function finnFeil() {
		if (tekst.trim() === '') {
			return 'Du må skrive inn en tekst.';
		}
		if (!/^[A-Za-z ]+$/.test(tekst)) {
			return 'Teksten kan bare ha bokstaver (A-Z) og mellomrom. Tall og spesialtegn er ikke lov.';
		}
		if (nokkel.trim() === '') {
			return 'Du må skrive inn en nøkkel.';
		}
		if (!/^[0-9]+$/.test(nokkel.trim()) || Number(nokkel) > 25) {
			return 'Nøkkelen må være et heltall fra 0 til 25.';
		}
		return '';
	}

	// retning: 1 = krypter, -1 = dekrypter
	function kjor(retning) {
		resultat = '';
		feil = finnFeil();
		if (feil) return;

		resultat = caesar(tekst, retning * Number(nokkel));
	}
</script>

<h1>Caesar</h1>

<label for="tekst">Tekst:</label>
<textarea id="tekst" rows="4" bind:value={tekst}></textarea>

<p><label for="nokkel">Nøkkel (tall 0-25):</label></p>
<input id="nokkel" type="text" bind:value={nokkel} />

<button onclick={() => kjor(1)}>Krypter</button>
<button onclick={() => kjor(-1)}>Dekrypter</button>

{#if feil}
	<p class="feil">{feil}</p>
{/if}

{#if resultat}
	<p>Resultat:</p>
	<p class="resultat">{resultat}</p>
{/if}
