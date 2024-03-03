<script>
	import {
		Card,
		Button,
		Avatar,
		Listgroup,
		Table,
		TableBodyCell,
		TableBody,
		TableBodyRow,
		ButtonGroup,
		Label,
		NumberInput,
		Search,
		Select,
		Modal,
		Badge,
		Indicator,
		GradientButton
	} from 'flowbite-svelte';
	import { ArrowRightOutline, ArrowLeftOutline, EnvelopeSolid } from 'flowbite-svelte-icons';
	import { onMount } from 'svelte';
	import { ExclamationCircleOutline, ChartLineUpSolid } from 'flowbite-svelte-icons';
	import { sineIn } from 'svelte/easing';
	import Marquee from 'svelte-fast-marquee';
	import poster from '../../assets/bid_page_logo.jpeg';

	let bid_url = 'http://65.2.31.208/api';

	let hidden10 = false;
	let activateClickOutside = false;
	let backdrop = false;
	let transitionParams = {
		x: 320,
		duration: 200,
		easing: sineIn
	};

	let current_player_value = 100;
	let current_team = 0;
	let role = '';
	let clickOutsideModal = false;
	let showpage = true;

	let role_list = [
		{ value: 'Batsman', name: 'Batsman' },
		{ value: 'Wicketkeeper', name: 'Wicket-Keeper' },
		{ value: 'Bowler', name: 'Bowler' },
		{ value: 'Bowling-All-Rounder', name: 'Bowling All-Rounder' },
		{ value: 'Batting-All-Rounder', name: 'Batting All-Rounder' }
	];

	let team_list = [
		{ value: 2, name: 'INDIA' },
		{ value: 3, name: 'ENGLAND' },
		{ value: 4, name: 'NEW ZEALAND' },
		{ value: 5, name: 'SCOTLAND' },
		{ value: 6, name: 'AUSTRALIA' },
		{ value: 7, name: 'NEPAL' },
		{ value: 8, name: 'ZIMBABWE' },
		{ value: 9, name: 'IRELAND' },
		{ value: 10, name: 'SOUTH AFRICA' },
		{ value: 11, name: 'WEST INDIES' },
		{ value: 12, name: 'NETHERLANDS' },
		{ value: 13, name: 'LAST TEAM' }
		// { value: 2, name: 'knights' }
	];

	let player_data = {};

	let allteamData = [];
	// let allplayer = [];
	let key = 'all';
	let search_player_id = null;
	let records = {};
	// let allteamData = [
	// 	{
	// 		team_name: 'Mumbai Indians',
	// 		pot: 1000000,
	// 		players: 25
	// 	},
	// 	{
	// 		team_name: 'Chennai Super Kings',
	// 		pot: 900000,
	// 		players: 22
	// 	},
	// 	{
	// 		team_name: 'Royal Challengers Bangalore',
	// 		pot: 800000,
	// 		players: 20
	// 	},
	// 	{
	// 		team_name: 'Kolkata Knight Riders',
	// 		pot: 750000,
	// 		players: 18
	// 	},
	// 	{
	// 		team_name: 'Delhi Capitals',
	// 		pot: 700000,
	// 		players: 23
	// 	},
	// 	{
	// 		team_name: 'Punjab Kings',
	// 		pot: 650000,
	// 		players: 21
	// 	},
	// 	{
	// 		team_name: 'Rajasthan Royals',
	// 		pot: 600000,
	// 		players: 19
	// 	},
	// 	{
	// 		team_name: 'Sunrisers Hyderabad',
	// 		pot: 550000,
	// 		players: 24
	// 	},
	// 	{
	// 		team_name: 'Royal Challengers Bangalore',
	// 		pot: 500000,
	// 		players: 20
	// 	},
	// 	{
	// 		team_name: 'Chennai Super Kings',
	// 		pot: 450000,
	// 		players: 22
	// 	},
	// 	{
	// 		team_name: 'Mumbai Indians',
	// 		pot: 400000,
	// 		players: 18
	// 	},
	// 	{
	// 		team_name: 'Delhi Capitals',
	// 		pot: 350000,
	// 		players: 23
	// 	}
	// ];

	function handleinc() {
		if (current_player_value < 1000) {
			current_player_value += 50;
		} else if (current_player_value >= 1000 && current_player_value < 2000) {
			current_player_value += 100;
		} else if (current_player_value >= 2000 && current_player_value < 3000) {
			current_player_value += 150;
		} else if (current_player_value >= 3000 && current_player_value < 4000) {
			current_player_value += 200;
		} else {
			current_player_value += 250;
		}
		console.log(current_player_value);
		current_player_value = current_player_value;
	}
	function handledec() {
		current_player_value -= 1;
		console.log(current_player_value);
		current_player_value = current_player_value;
	}
	// $: curr_plval = current_player_value;

	// function handlevalue(event) {}

	async function fetchallPlayerData() {
		try {
			const response = await fetch(`${bid_url}/getallplayers?key=${key}`);
			const data = await response.json();

			allplayer = data;
		} catch (error) {
			console.error('Error fetching data:', error);
		}
	}

	async function fetchPlayer() {
		try {
			const response = await fetch(`${bid_url}/getnextplayer?role=${role}`);
			const data = await response.json();
			player_data = data;
			current_player_value = 100;
			console.log(player_data);
		} catch (error) {
			console.error(error);
		}
	}

	async function fetchAllTeamData() {
		try {
			const response = await fetch(`${bid_url}/getallteam`);
			allteamData = await response.json();
		} catch (error) {
			console.error(error);
		}
	}

	async function fetchPlayerById() {
		try {
			const response = await fetch(`${bid_url}/getplayerbyid?player_id=${search_player_id}`);
			player_data = await response.json();
		} catch (error) {
			console.log(error);
		}
	}
	async function doTransferPlayer() {
		try {
			const response = await fetch(`${bid_url}/transferplayer`, {
				method: 'POST',
				headers: {
					'Content-Type': 'application/json'
				},
				body: JSON.stringify({
					player_id: player_data.id,
					team_id: current_team,
					player_value: current_player_value
				})
			});
			await fetchAllTeamData();
			await fetchRecords();
			console.log(response.data);
		} catch (error) {
			console.log(error);
		}
	}

	async function fetchRecords() {
		try {
			const response = await fetch(`${bid_url}/records`);
			records = await response.json();
			console.log(records);
		} catch (error) {
			console.log(error);
		}
	}

	onMount(() => {
		// fetchPlayer();
		fetchAllTeamData();
		fetchRecords();
		// fetchallPlayerData();
	});
</script>

{#if showpage}
	{#if allteamData}
		<Marquee pauseOnHover={true} speed={75}>
			<TableBodyRow>
				{#each allteamData as teamdata}
					<TableBodyCell class="m-auto text-black">
						<div class="flex flex-col rounded-lg p-2 shadow-lg shadow-purple-700">
							<div class="flex flex-row space-x-1 font-mono font-bold text-gray-500">
								Team:
								<div class="text-balance font-mono font-bold text-black">
									{teamdata.team_name}
								</div>
							</div>
							<div class="flex items-center justify-between space-x-4 font-mono font-bold">
								<div class="flex flex-row font-mono font-bold text-gray-500">
									purse:
									<div class="font-mono font-bold text-green-500">
										{teamdata.pot}
									</div>
								</div>
								<div class="flex flex-row font-mono font-bold text-gray-500">
									rem:
									<div class="font-mono font-bold text-red-700">
										{teamdata.players.length}/12
									</div>
								</div>
							</div>
						</div>
					</TableBodyCell>
				{/each}
			</TableBodyRow>
		</Marquee>
	{/if}

	<div class="min-w-screen flex min-h-screen items-center justify-center bg-gray-100">
		<div>
			<div class="rounded-lg bg-black p-14 shadow-lg shadow-lime-600">
				<div class="mb-4 flex justify-center font-mono text-3xl font-bold text-red-200">
					-:CURRENTLY BIDDING FOR:-
				</div>
				<div class="mt-4 h-auto w-auto rounded-2xl bg-white shadow-lg shadow-indigo-500">
					<div class="flex flex-col">
						<div class="flex flex-row">
							<div>
								<Avatar size="xl" class="h-96 w-96" rounded src={player_data.user_image} />
							</div>
							<Card class="shadow-lg shadow-green-400">
								<div>
									<h5 class="mb-2 text-4xl font-bold tracking-tight text-gray-900 dark:text-white">
										{player_data.firstname}
									</h5>
									<h4 class="mb-2 text-4xl font-bold tracking-tight text-gray-900 dark:text-white">
										{player_data.lastname}
									</h4>
									<div class="flex items-center space-x-2">
										<EnvelopeSolid class="text-black dark:text-white" />
										<div class="">{player_data.email}</div>
									</div>
									<div class="mt-4 space-y-3 rounded-xl bg-white">
										<div class="">
											<span class="flex items-center"
												><Indicator size="sm" color="red" class="me-1.5" />Playing Role :
											</span>
											<Badge border large color="red" class="ml-1 mt-1 text-xl"
												>{player_data.player_position}</Badge
											>
										</div>
										<div class="mt-2 flex">
											<span class="flex items-center"
												><Indicator size="sm" color="yellow" class="me-1.5" />Year :</span
											>
											<Badge border color="yellow" class="ml-2 text-sm">{player_data.year}</Badge>
										</div>
										<div class="">
											<span class="flex items-center"
												><Indicator size="sm" color="purple" class="me-1.5" />Department :</span
											>
											<Badge border large color="purple" class="ml-1 mt-2 text-xl">CSE - AIML</Badge
											>
										</div>
									</div>
								</div>
							</Card>
						</div>
					</div>
				</div>
			</div>
			<div class="mt-4 flex h-auto w-auto justify-center space-x-4 bg-gray-100">
				<div class="space-x-2 rounded-lg bg-black p-2 shadow-lg shadow-indigo-500">
					<!-- <div class="flex items-center bg-red-400 rounded-xl p-4 text-white">current bidding price -</div> -->

					<Badge border color="green">
						<div class="font-mono text-8xl font-extrabold">
							{current_player_value}
						</div>
					</Badge>
				</div>
			</div>
		</div>

		<div class="absolute left-0 flex h-screen w-96 items-center bg-white">
			<div class="absolute top-0 flex flex-col justify-center self-center p-3">
				<div class="flex min-h-full flex-col rounded-lg shadow-lg shadow-green-300">
					<div class="m-2 flex h-2/4 flex-col justify-center">
						<Card class="h-4/5 shadow-md shadow-yellow-400">
							<h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">
								Current Record Values
							</h5>
							{#if records && records.max_batsman}
								<div class="flex items-center space-x-4 p-2 rtl:space-x-reverse">
									<Avatar src={records.max_batsman.user_image} rounded />
									<div class="min-w-full space-y-1 font-medium text-black dark:text-white">
										<div>{records.max_batsman.firstname + ' ' + records.max_batsman.lastname}</div>
										<div class="flex flex-row space-x-2">
											<div class="text-sm text-gray-500 dark:text-gray-400">
												{records.max_batsman.player_position}
											</div>
											<div class="text-sm text-gray-500 dark:text-gray-400">
												| {records.max_batsman.player_value}
											</div>
										</div>
									</div>
								</div>
							{/if}
							{#if records && records.max_bowler}
								<div class="flex items-center space-x-4 p-2 rtl:space-x-reverse">
									<Avatar src={records.max_bowler.user_image} rounded />
									<div class="min-w-full space-y-1 font-medium text-black dark:text-white">
										<div>{records.max_bowler.firstname + ' ' + records.max_bowler.lastname}</div>
										<div class="flex flex-row space-x-2">
											<div class="text-sm text-gray-500 dark:text-gray-400">
												{records.max_bowler.player_position}
											</div>
											<div class="text-sm text-gray-500 dark:text-gray-400">
												| {records.max_bowler.player_value}
											</div>
										</div>
									</div>
								</div>
							{/if}
							{#if records && records.max_wk}
								<div class="flex items-center space-x-4 p-2 rtl:space-x-reverse">
									<Avatar src={records.max_wk.user_image} rounded />
									<div class="min-w-full space-y-1 font-medium text-black dark:text-white">
										<div>{records.max_wk.firstname + ' ' + records.max_wk.lastname}</div>
										<div class="flex flex-row space-x-2">
											<div class="text-sm text-gray-500 dark:text-gray-400">
												{records.max_wk.player_position}
											</div>
											<div class="text-sm text-gray-500 dark:text-gray-400">
												| {records.max_wk.player_value}
											</div>
										</div>
									</div>
								</div>
							{/if}
							{#if records && records.max_bla}
								<div class="flex items-center space-x-4 p-2 rtl:space-x-reverse">
									<Avatar src={records.max_bla.user_image} rounded />
									<div class="min-w-full space-y-1 font-medium text-black dark:text-white">
										<div>{records.max_bla.firstname + ' ' + records.max_bla.lastname}</div>
										<div class="flex flex-row space-x-2">
											<div class="text-sm text-gray-500 dark:text-gray-400">
												{records.max_bla.player_position}
											</div>
											<div class="text-sm text-gray-500 dark:text-gray-400">
												| {records.max_bla.player_value}
											</div>
										</div>
									</div>
								</div>
							{/if}
							{#if records && records.max_bta}
								<div class="flex items-center space-x-4 p-2 rtl:space-x-reverse">
									<Avatar src={records.max_bta.user_image} rounded />
									<div class="min-w-full space-y-1 font-medium text-black dark:text-white">
										<div>{records.max_bta.firstname + ' ' + records.max_bta.lastname}</div>
										<div class="flex flex-row space-x-2">
											<div class="text-sm text-gray-500 dark:text-gray-400">
												{records.max_bta.player_position}
											</div>
											<div class="text-sm text-gray-500 dark:text-gray-400">
												| {records.max_bta.player_value}
											</div>
										</div>
									</div>
								</div>
							{/if}
						</Card>
					</div>
					<card>
						<div class="m-5 ml-8 w-4/5">
							<img src={poster} alt="icl2024" />
						</div>
					</card>
				</div>
			</div>
		</div>

		<div class="absolute right-0 h-screen w-96 bg-white">
			<div class="flex justify-center">
				<div
					class="absolute flex items-center justify-center rounded-lg p-10 shadow-lg shadow-green-300"
				>
					<div class="flex flex-col justify-center">
						<Label class="mb-4 space-y-2 ">
							<span>Current Bid Price</span>
							<NumberInput bind:value={current_player_value} class="min-w-full" />
						</Label>
						<div class="flex justify-center">
							<ButtonGroup>
								<Button
									class="bg-black text-white"
									on:click={() => {
										handledec();
									}}>Decrement</Button
								>
								<Button
									on:click={() => {
										handleinc();
									}}
									class="bg-black text-white">Increment</Button
								>
							</ButtonGroup>
						</div>

						<div class="mt-6 flex flex-col justify-center">
							<Label class="space-y-2">
								<span>Search Player by Id</span>
								<Search size="md" bind:value={search_player_id} />
							</Label>
							<Button class="mt-2" on:click={fetchPlayerById}>Search Player</Button>
						</div>
						<div class="justify- mb-4 mt-6 flex flex-col space-y-4">
							<Label>
								Select an option
								<Select class="mt-2" items={role_list} bind:value={role} />
							</Label>
							<div class=" flex justify-center">
								<ButtonGroup>
									<Button class="bg-black text-white" on:click={fetchPlayer}>Skip Player</Button>
									<Button class="bg-black text-white" on:click={fetchPlayer}>Next Player</Button>
								</ButtonGroup>
							</div>
						</div>
						<div class="mt-6 flex min-w-full flex-col justify-center">
							<Label>
								Select Team to Transfer Player
								<Select class="mt-2" items={team_list} bind:value={current_team} />
							</Label>
							<!-- <GradientButton
								on:click={() => (clickOutsideModal = true)}
								class="mt-6 min-w-full"
								color="redToPink">Transfer Player</GradientButton
							> -->
							<Button class="mt-6 min-w-full bg-red-600" on:click={() => (clickOutsideModal = true)}
								>Confirm Transfer
							</Button>

							<div
								class="mt-6 min-w-full rounded-lg p-2 pl-4 font-mono text-4xl font-bold text-yellow-100 shadow-lg shadow-yellow-400"
							>
								Intra Cricket League-2024
							</div>

							<Modal bind:open={clickOutsideModal} size="xs" autoclose>
								<div class="text-center">
									<ExclamationCircleOutline
										class="mx-auto mb-4 h-12 w-12 text-gray-400 dark:text-gray-200"
									/>
									<h3 class="mb-5 text-lg font-normal text-black dark:text-gray-400">
										Transfering {player_data.firstname + ' ' + player_data.lastname} to {current_team}
										for {current_player_value}
									</h3>
									<Button
										class="me-2 bg-green-500"
										on:click={() => {
											doTransferPlayer();
										}}>Confirm Transfer</Button
									>
									<Button color="alternative" class="bg-red-600 text-white">Cancel Transfer</Button>
								</div>
							</Modal>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
{:else}
	<div>user not authorised!!!</div>
{/if}
