<script>
	const A = 65;

	let tekst = $state('');
	let nokkel = $state('');
	let resultat = $state('');
	let feil = $state('');

	function flyttBokstav(bokstav, steg) {
		let tall = bokstav.charCodeAt(0) - A;
		let nyttTall = (((tall + steg) % 26) + 26) % 26;
		return String.fromCharCode(nyttTall + A);
	}

	// retning: 1 = krypter, -1 = dekrypter
	function vigenere(tekst, nokkel, retning) {
		// gjør nøkkelen om til en liste med tall, f.eks. KEY -> [10, 4, 24]
		let nokkelTall = nokkel
			.toUpperCase()
			.split('')
			.map((bokstav) => bokstav.charCodeAt(0) - A);

		let nokkelPlass = 0; // hvilken nøkkelbokstav vi er på (mellomrom teller ikke)
		let svar = '';

		for (let tegn of tekst.toUpperCase()) {
			if (tegn === ' ') {
				svar += ' ';
				continue;
			}
			// % lengden gjør at nøkkelen gjentas når den er brukt opp
			let steg = nokkelTall[nokkelPlass % nokkelTall.length] * retning;
			svar += flyttBokstav(tegn, steg);
			nokkelPlass++;
		}
		return svar;
	}

	function finnFeil() {
		if (tekst.trim() === '') {
			return 'Du må skrive inn en tekst.';
		}
		if (!/^[A-Za-z ]+$/.test(tekst)) {
			return 'Teksten kan bare ha bokstaver (A-Z) og mellomrom. Tall og spesialtegn er ikke lov.';
		}
		if (nokkel === '') {
			return 'Du må skrive inn en nøkkel.';
		}
		if (!/^[A-Za-z]+$/.test(nokkel)) {
			return 'Nøkkelen kan bare ha bokstaver fra A til Z (ingen tall, mellomrom eller spesialtegn).';
		}
		return '';
	}

	function kjor(retning) {
		resultat = '';
		feil = finnFeil();
		if (feil) return;

		resultat = vigenere(tekst, nokkel, retning);
	}
</script>

<h1>Vigenère</h1>

<label for="tekst">Tekst:</label>
<textarea id="tekst" rows="4" bind:value={tekst}></textarea>

<p><label for="nokkel">Nøkkel (bokstaver A-Z):</label></p>
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
