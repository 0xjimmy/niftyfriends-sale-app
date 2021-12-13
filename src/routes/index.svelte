<script lang="ts">
	import { connectMetamask, connectWalletConnect, walletAddress } from '$lib/stores/provider';
	import Logo from '$lib/assets/meta_citizens_logo_white.png';
	import NFT1 from '$lib/assets/example1.png';
	import NFT2 from '$lib/assets/example2.png';
	import NFT3 from '$lib/assets/example3.png';
	let buyAmount: number = 0;
</script>

<section
	class="flex flex-col p-10 md:p-20 w-screen h-screen items-center gap-5 font-mono text-white mb-10 "
>
	<img src={Logo} alt="MetaCitizens" class="w-3/4" />
	<h1 class="text-5xl font-bold">Twitter / Discord /</h1>
	<h2 class="text-2xl font-semibold">
		Wallet Address: {$walletAddress ? $walletAddress : 'Not Connected'}
	</h2>
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
		<h3 class="text-xl font-medium">1 / 10,000 Minted</h3>
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
					on:click={connectWalletConnect}
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
	<div class="prose md:prose-2xl">
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
</section>
