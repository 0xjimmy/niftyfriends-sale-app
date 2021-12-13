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
			Meta Citizens is to be a Free-to-Mint project with no gimmicks or promises of gain.
		</p>
		<img
			src={NFT2}
			class="rounded-3xl drop-shadow-md h-80 transition transition-300 hover:rotate-3 hover:scale-[1.05] mx-auto"
			alt="MetaCitizens"
		/>
		<p>
			Owners of a Meta Citizens NFT have full IP rights to the character(s) they own. As the artist
			and creator of the project, I just that you do not engage in any malicious or illegal behavior
			while utilizing the NFT as we would like to keep positive vibes around our community.
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
		>
	</footer>
</section>
