<script>
	import MutantBatz from "./MutantBatz.json";
	import {ethers} from "ethers";

	let currentAccount;
	const bats = [];

	const connectWallet = async () => {
		try {
			const accounts = await window.ethereum.request({method: "eth_requestAccounts"});
			currentAccount = accounts[0];

		} catch (err) {
			console.log(err);
		}

	}

	async function isPresaleActive() {
		if (typeof window.ethereum !== "undefined") {
			try {

				const provider = new ethers.providers.Web3Provider(window.ethereum);
				const signer = provider.getSigner();
				const contract = new ethers.Contract("0xe3bf8660426becbcbc66bf3b6d255aae6203d5be", MutantBatz.abi, signer);
				let ids = [];
				for(let i = 0; i < 9666; i++){
					ids.push(i+1);
				}
				const canBatsBite = await contract.canBatsBite(ids);
				for(let i = 0; i < 9666; i++){
					bats[i] = ({id:`${i+1}`, canBite: canBatsBite[i]});
				}

			} catch (err) {
				console.log(err);
				return false;
			}
		}
	}
</script>

<main>
	<h1>Can bats bite</h1>
	<h5>made by hiri#9048</h5>
	<button class="btn btn-primary" on:click={connectWallet}>Connect</button>
	<button class="btn btn-primary" on:click={isPresaleActive}>GetBats</button>
	<table class="table">
		<thead>

		<tr>
			<th scope="col">ID</th>
			<th scope="col">Can bite</th>
		</tr>
		</thead>
		<tbody>
		{#each bats as bat}
		<tr>
			<th scope="row">{bat.id}</th>
			<td>{bat.canBite}</td>

		</tr>
			{/each}
		</tbody>
	</table>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>