<script lang="ts">
	import { BigNumber, Contract } from 'ethers';
	import {
		connectMetamask,
		connectWalletConnect,
		walletAddress,
		provider
	} from '$lib/stores/provider';
	import ABI from '$lib/nftabi.json';
	import Logo from '$lib/assets/meta_citizens_logo_white.png';
	import NFT1 from '$lib/assets/example1.png';
	import NFT2 from '$lib/assets/example2.png';
	import NFT3 from '$lib/assets/example3.png';
	import { onMount } from 'svelte';
	let buyAmount: number = 0;

	let minted = '0';
	let contract: Contract;

	onMount(() => {
		contract = new Contract('0x25Ca3D9871dd26683CE37275487Ee49309eeD9B2', ABI, $provider);
		contract.totalSupply().then((result: BigNumber) => {
			minted = result.toString();
		});
	});

	const mint = () => {
		const amount = Math.floor(Number(buyAmount));
		contract = new Contract(
			'0x25Ca3D9871dd26683CE37275487Ee49309eeD9B2',
			ABI,
			// @ts-ignore
			$provider.getSigner()
		);
		contract.mint(BigNumber.from(amount));
	};
</script>

<section
	class="flex flex-col p-10 md:p-20 w-screen h-screen items-center gap-5 font-mono text-white mb-10 "
>
	<img src={Logo} alt="MetaCitizens" class="w-3/4 max-w-md" />
	<div class="flex flex-row flex-wrap max-w-90 items-center justify-center my-10">
		<img
			src={NFT1}
			class="rounded-3xl drop-shadow-md h-60 transition transition-300 -rotate-6 hover:rotate-3 hover:scale-[1.05]"
			alt="MetaCitizens"
		/>
		<img
			src={NFT3}
			class="rounded-3xl drop-shadow-md h-60 transition transition-300 rotate-12 hover:rotate-3 hover:scale-[1.05]"
			alt="MetaCitizens"
		/>
	</div>
	<div
		class="flex flex-col outline outline-4 outline-[#227694] p-3 rounded-2xl items-center gap-2 text-center"
	>
		<h2 class="text-2xl font-semibold">Mint Your Meta Citizen</h2>
		<h3 class="text-xl font-medium">{minted} / 10,000 Minted</h3>
		<br />

		{#if !$walletAddress}
			<button
				on:click={connectMetamask}
				class="p-3 rounded-2xl text-sm md:text-xl w-fit bg-slate-900 text-white font-semibold hover:scale-[1.05] transition transition-200"
				>Connect via MetaMask</button
			>
			<button
				on:click={connectWalletConnect}
				class="p-3 rounded-2xl text-sm md:text-xl w-fit bg-slate-900 text-white font-semibold hover:scale-[1.05] transition transition-200"
				>Connect via WalletConnect</button
			>
		{:else}
			<div class="flex flex-row items-center align-center gap-2">
				<input
					type="number"
					placeholder="0"
					bind:value={buyAmount}
					class="p-3 rounded-2xl text-sm md:text-xl w-20 bg-slate-700 text-white font-semibold hover:scale-[1.05] transition transition-200"
				/>
				<button
					disabled={buyAmount > 5}
					on:click={mint}
					class="p-3 rounded-2xl text-sm md:text-xl w-fit bg-slate-900 text-white font-semibold hover:scale-[1.05] transition transition-200"
					>Mint</button
				>
			</div>

			<p class="text-sm font-regular text-grey-600">Max mint 5 per user</p>
			<p class="text-sm font-regular text-grey-600">
				Logged in as {`${$walletAddress.substr(0, 5)}...${$walletAddress.substr(-3)}`}
			</p>
		{/if}
	</div>
	<div class="font-regular text-xl my-16 flex flex-col gap-5 md:w-full max-w-screen-md">
		<p>
			Meta Citizens consists of 10,000 NFTs, a mixture of both male and female genders, residing on
			the Ethereum Blockchain. Each Citizen is Unique in Appearance and Personality. The purpose of
			project is to be a community-driven be a Free-to-Mint project with no gimmicks. The
			development and direction of the project is decided upon by its community members with
			guidance from the original creator, <a
				href="https://twitter.com/Metamantra"
				target="_blank"
				class="font-bold underline decoration-sky-500">@Metamantra</a
			>.
		</p>
		<img
			src={NFT2}
			class="rounded-3xl drop-shadow-md h-80 transition transition-300 hover:rotate-3 hover:scale-[1.05] mx-auto"
			alt="MetaCitizens"
		/>
		<p>
			Owners of a Meta Citizens NFT have full IP rights to the character(s) they own. As the artist
			and creator of the project, I just ask that you do not engage in any malicious or illegal
			behavior while utilizing the NFT as we would like to keep positive vibes around our community.
		</p>
		<p>
			This is a community driven project. Any and all support to help grow the project would be much
			appreciated.
		</p>
	</div>
	<footer class="flex flex-row gap-5 items-center align-center pb-20 font-semibold text-xl">
		<a href="https://twitter.com/MetaCitizens" target="_blank" class="underline decoration-sky-500"
			><svg
				aria-hidden="true"
				focusable="false"
				data-prefix="fab"
				data-icon="twitter"
				class="svg-inline--fa fa-twitter fa-w-16 h-10 w-10 inline-block m-2"
				role="img"
				xmlns="http://www.w3.org/2000/svg"
				viewBox="0 0 512 512"
				><path
					fill="currentColor"
					d="M459.37 151.716c.325 4.548.325 9.097.325 13.645 0 138.72-105.583 298.558-298.558 298.558-59.452 0-114.68-17.219-161.137-47.106 8.447.974 16.568 1.299 25.34 1.299 49.055 0 94.213-16.568 130.274-44.832-46.132-.975-84.792-31.188-98.112-72.772 6.498.974 12.995 1.624 19.818 1.624 9.421 0 18.843-1.3 27.614-3.573-48.081-9.747-84.143-51.98-84.143-102.985v-1.299c13.969 7.797 30.214 12.67 47.431 13.319-28.264-18.843-46.781-51.005-46.781-87.391 0-19.492 5.197-37.36 14.294-52.954 51.655 63.675 129.3 105.258 216.365 109.807-1.624-7.797-2.599-15.918-2.599-24.04 0-57.828 46.782-104.934 104.934-104.934 30.213 0 57.502 12.67 76.67 33.137 23.715-4.548 46.456-13.32 66.599-25.34-7.798 24.366-24.366 44.833-46.132 57.827 21.117-2.273 41.584-8.122 60.426-16.243-14.292 20.791-32.161 39.308-52.628 54.253z"
				/></svg
			>Twitter</a
		>
		<a href="https://discord.gg/jjwKucEJ5Y" target="_blank" class="underline decoration-sky-500"
			><svg
				aria-hidden="true"
				focusable="false"
				data-prefix="fab"
				data-icon="discord"
				class="svg-inline--fa fa-discord fa-w-20 h-10 w-10 inline-block m-2"
				role="img"
				xmlns="http://www.w3.org/2000/svg"
				viewBox="0 0 640 512"
				><path
					fill="currentColor"
					d="M524.531,69.836a1.5,1.5,0,0,0-.764-.7A485.065,485.065,0,0,0,404.081,32.03a1.816,1.816,0,0,0-1.923.91,337.461,337.461,0,0,0-14.9,30.6,447.848,447.848,0,0,0-134.426,0,309.541,309.541,0,0,0-15.135-30.6,1.89,1.89,0,0,0-1.924-.91A483.689,483.689,0,0,0,116.085,69.137a1.712,1.712,0,0,0-.788.676C39.068,183.651,18.186,294.69,28.43,404.354a2.016,2.016,0,0,0,.765,1.375A487.666,487.666,0,0,0,176.02,479.918a1.9,1.9,0,0,0,2.063-.676A348.2,348.2,0,0,0,208.12,430.4a1.86,1.86,0,0,0-1.019-2.588,321.173,321.173,0,0,1-45.868-21.853,1.885,1.885,0,0,1-.185-3.126c3.082-2.309,6.166-4.711,9.109-7.137a1.819,1.819,0,0,1,1.9-.256c96.229,43.917,200.41,43.917,295.5,0a1.812,1.812,0,0,1,1.924.233c2.944,2.426,6.027,4.851,9.132,7.16a1.884,1.884,0,0,1-.162,3.126,301.407,301.407,0,0,1-45.89,21.83,1.875,1.875,0,0,0-1,2.611,391.055,391.055,0,0,0,30.014,48.815,1.864,1.864,0,0,0,2.063.7A486.048,486.048,0,0,0,610.7,405.729a1.882,1.882,0,0,0,.765-1.352C623.729,277.594,590.933,167.465,524.531,69.836ZM222.491,337.58c-28.972,0-52.844-26.587-52.844-59.239S193.056,219.1,222.491,219.1c29.665,0,53.306,26.82,52.843,59.239C275.334,310.993,251.924,337.58,222.491,337.58Zm195.38,0c-28.971,0-52.843-26.587-52.843-59.239S388.437,219.1,417.871,219.1c29.667,0,53.307,26.82,52.844,59.239C470.715,310.993,447.538,337.58,417.871,337.58Z"
				/></svg
			>Discord</a
		><a
			href="https://opensea.io/collection/meta-citizens"
			target="_blank"
			class="underline decoration-sky-500"
		>
			<svg
				xmlns="http://www.w3.org/2000/svg"
				width="100"
				class="h-10 w-10 inline-block m-2"
				height="100"
				fill="none"
				viewBox="0 0 100 100"
				><path
					fill="#fff"
					d="M50 0C22.39 0 0 22.39 0 50C0 77.61 22.39 100 50 100C77.61 100 100 77.61 100 50C100 22.39 77.62 0 50 0ZM24.67 51.68L24.88 51.34L37.89 30.99C38.08 30.7 38.53 30.73 38.67 31.05C40.84 35.92 42.72 41.98 41.84 45.75C41.47 47.3 40.44 49.4 39.28 51.34C39.13 51.62 38.97 51.9 38.79 52.17C38.71 52.29 38.57 52.36 38.42 52.36H25.05C24.69 52.36 24.48 51.97 24.67 51.68ZM82.64 58.68C82.64 58.87 82.53 59.03 82.37 59.1C81.36 59.53 77.91 61.12 76.48 63.11C72.82 68.2 70.03 75.48 63.78 75.48H37.72C28.48 75.48 21 67.97 21 58.7V58.4C21 58.16 21.2 57.96 21.45 57.96H35.97C36.26 57.96 36.47 58.22 36.45 58.51C36.34 59.45 36.52 60.42 36.97 61.3C37.83 63.05 39.62 64.14 41.55 64.14H48.74V58.53H41.63C41.27 58.53 41.05 58.11 41.26 57.81C41.34 57.69 41.42 57.57 41.52 57.43C42.19 56.47 43.15 54.99 44.11 53.3C44.76 52.16 45.39 50.94 45.9 49.72C46 49.5 46.08 49.27 46.17 49.05C46.31 48.66 46.45 48.29 46.55 47.93C46.65 47.62 46.74 47.3 46.82 47C47.06 45.96 47.16 44.86 47.16 43.72C47.16 43.27 47.14 42.8 47.1 42.36C47.08 41.87 47.02 41.38 46.96 40.89C46.92 40.46 46.84 40.03 46.76 39.59C46.65 38.94 46.51 38.29 46.35 37.64L46.29 37.39C46.17 36.94 46.06 36.52 45.92 36.07C45.51 34.67 45.05 33.3 44.55 32.02C44.37 31.51 44.17 31.02 43.96 30.54C43.66 29.8 43.35 29.13 43.07 28.5C42.92 28.21 42.8 27.95 42.68 27.68C42.54 27.38 42.4 27.08 42.25 26.79C42.15 26.57 42.03 26.36 41.95 26.16L41.07 24.54C40.95 24.32 41.15 24.05 41.39 24.12L46.89 25.61H46.91C46.92 25.61 46.92 25.61 46.93 25.61L47.65 25.82L48.45 26.04L48.74 26.12V22.86C48.74 21.28 50 20 51.57 20C52.35 20 53.06 20.32 53.56 20.84C54.07 21.36 54.39 22.07 54.39 22.86V27.71L54.98 27.87C55.02 27.89 55.07 27.91 55.11 27.94C55.25 28.04 55.46 28.2 55.72 28.4C55.93 28.56 56.15 28.76 56.41 28.97C56.94 29.4 57.58 29.95 58.27 30.58C58.45 30.74 58.63 30.9 58.8 31.07C59.69 31.9 60.69 32.87 61.65 33.95C61.92 34.26 62.18 34.56 62.45 34.89C62.71 35.22 63 35.54 63.24 35.86C63.57 36.29 63.91 36.74 64.22 37.21C64.36 37.43 64.53 37.66 64.66 37.88C65.06 38.47 65.4 39.08 65.73 39.69C65.87 39.97 66.01 40.28 66.13 40.58C66.5 41.4 66.79 42.23 66.97 43.07C67.03 43.25 67.07 43.44 67.09 43.62V43.66C67.15 43.9 67.17 44.16 67.19 44.43C67.27 45.28 67.23 46.14 67.05 47C66.97 47.36 66.87 47.7 66.75 48.07C66.62 48.42 66.5 48.78 66.34 49.13C66.03 49.84 65.67 50.56 65.24 51.22C65.1 51.47 64.93 51.73 64.77 51.98C64.59 52.24 64.4 52.49 64.24 52.73C64.01 53.04 63.77 53.36 63.52 53.65C63.3 53.95 63.08 54.25 62.83 54.52C62.49 54.93 62.16 55.31 61.81 55.68C61.61 55.92 61.39 56.17 61.16 56.39C60.94 56.64 60.71 56.86 60.51 57.06C60.16 57.41 59.88 57.67 59.64 57.9L59.07 58.41C58.99 58.49 58.88 58.53 58.77 58.53H54.39V64.14H59.9C61.13 64.14 62.3 63.71 63.25 62.9C63.57 62.62 64.98 61.4 66.65 59.56C66.71 59.49 66.78 59.45 66.86 59.43L82.07 55.03C82.36 54.95 82.64 55.16 82.64 55.46V58.68V58.68Z"
				/></svg
			>OpenSea</a
		>
	</footer>
</section>
