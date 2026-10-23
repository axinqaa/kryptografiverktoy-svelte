<script>
	const A = 65;

	let tekst = $state('');
	let nokkel = $state(''); // brukes bare ved dekryptering
	let resultat = $state('');
	let feil = $state('');
	let lagetNokkel = $state(''); // nøkkelen som lages automatisk ved kryptering

	function flyttBokstav(bokstav, steg) {
		let tall = bokstav.charCodeAt(0) - A;
		let nyttTall = (((tall + steg) % 26) + 26) % 26;
		return String.fromCharCode(nyttTall + A);
	}

	// Mellomrom telles ikke
	function antallBokstaver(tekst) {
		return tekst.replaceAll(' ', '').length;
	}

	// Lager like mange tilfeldige bokstaver som det er bokstaver i teksten.
	// OBS: Math.random() er ikke trygt nok til ekte kryptografi, det er bare for å vise hvordan det virker.
	function lagNokkel(tekst) {
		let svar = '';
		for (let i = 0; i < antallBokstaver(tekst); i++) {
			svar += String.fromCharCode(A + Math.floor(Math.random() * 26));
		}
		return svar;
	}

	// Samme utregning som Vigenère, men nøkkelen er like lang som teksten og gjentas aldri.
	// retning: 1 = krypter, -1 = dekrypter
	function otp(tekst, nokkel, retning) {
		let nokkelPlass = 0;
		let svar = '';

		for (let tegn of tekst.toUpperCase()) {
			if (tegn === ' ') {
				svar += ' ';
				continue;
			}
			let nokkelTall = nokkel.toUpperCase().charCodeAt(nokkelPlass) - A;
			svar += flyttBokstav(tegn, nokkelTall * retning);
			nokkelPlass++;
		}
		return svar;
	}

	function sjekkTekst() {
		if (tekst.trim() === '') {
			return 'Du må skrive inn en tekst.';
		}
		if (!/^[A-Za-z ]+$/.test(tekst)) {
			return 'Teksten kan bare ha bokstaver (A-Z) og mellomrom. Tall og spesialtegn er ikke lov.';
		}
		return '';
	}

	function sjekkNokkel() {
		if (nokkel === '') {
			return 'Du må skrive inn nøkkelen.';
		}
		if (!/^[A-Za-z]+$/.test(nokkel)) {
			return 'Nøkkelen kan bare ha bokstaver fra A til Z (ingen tall, mellomrom eller spesialtegn).';
		}
		if (nokkel.length !== antallBokstaver(tekst)) {
			return `Nøkkelen har feil lengde: den har ${nokkel.length} bokstaver, men teksten har ${antallBokstaver(tekst)} bokstaver (mellomrom telles ikke).`;
		}
		return '';
	}

	function krypter() {
		resultat = '';
		lagetNokkel = '';
		feil = sjekkTekst();
		if (feil) return;

		lagetNokkel = lagNokkel(tekst);
		resultat = otp(tekst, lagetNokkel, 1);
	}

	function dekrypter() {
		resultat = '';
		lagetNokkel = '';
		feil = sjekkTekst() || sjekkNokkel();
		if (feil) return;

		resultat = otp(tekst, nokkel, -1);
	}
</script>

<h1>One-Time Pad</h1>

<label for="tekst">Tekst:</label>
<textarea id="tekst" rows="4" bind:value={tekst}></textarea>

<p>
	<label for="nokkel">Nøkkel (bare ved dekryptering, like mange bokstaver som teksten):</label>
</p>
<input id="nokkel" type="text" bind:value={nokkel} />

<button onclick={krypter}>Krypter (lager nøkkel)</button>
<button onclick={dekrypter}>Dekrypter</button>

{#if feil}
	<p class="feil">{feil}</p>
{/if}

{#if resultat}
	<p>Resultat:</p>
	<p class="resultat">{resultat}</p>
{/if}

{#if lagetNokkel}
	<p>Nøkkelen som ble laget (ta vare på den, du trenger den for å dekryptere):</p>
	<p class="resultat nokkel">{lagetNokkel}</p>
	<p>
		<small>
			Math.random() brukes bare for å vise hvordan det virker. Det er ikke trygt nok til ekte
			kryptografi.
		</small>
	</p>
{/if}
