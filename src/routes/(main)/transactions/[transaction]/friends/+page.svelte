<script lang="ts">
	import { goto } from '$app/navigation'
	import { page } from '$app/stores'
	import FriendCard from '$lib/components/FriendCard.svelte'
	import { Button } from '$lib/components/ui/button/index'
	import { Input } from '$lib/components/ui/input'
	import { supabase } from '$lib/db'
	import CircleCheck from 'lucide-svelte/icons/circle-check'
	import { fade } from 'svelte/transition'

	console.log("IN FRIENDS PAGE")

	let { friendsData, id } = $page.data.props
	let { data } = $props()
	let transactionId = data.transaction_id
	let prof = data.prof
	// console.log(data.items)
	// data = data.items
	console.log('Data!', friendsData)

	let friend_map: Map<number, {email:string; id:string; name:string}> = new Map()
	let selected_map = $state(new Map())
	for (let i = 0; i < friendsData.length; i++) {
		let pair = friendsData[i]
		let friend
		if (pair.friend_1.id === id) {
			friend = pair.friend_2
		} else {
			friend = pair.friend_1
		}
		// friends.push(friend)
		friend_map.set(friend.id, friend)
		selected_map.set(friend.id, 0)
	}

	const handleClick = (id: string) => {
		console.log(id)
		if (selected_map.get(id) === 1) {
			selected_map.set(id, 0)
		} else {
			console.log('setting as true')
			selected_map.set(id, 1)
		}
		selected_map = new Map(selected_map)
	}

	const submitEmails = async () => {
		console.log('clicked')

		let emails: string[] = []
		selected_map.forEach((v, k) => {
			if (v === 1) {
				emails.push(friend_map.get(k).email)
			}
		})
		emails.push(prof?.email)

		// update the transaction with the selected emails
		const { data, error } = await supabase
			.from('transactions')
			.update({ selected_emails: emails })
			.eq('id', transactionId)
			.select()

		if (error) {
			console.error(error)
			return
		}

		console.log('updated transaction emails', data)

		goto(`/transactions/${transactionId}/itemize`)
	}
</script>

<!-- there's a search at the top -->
<!--  for selected friends, they just are in the friends list -->
<!-- for entered emails, they go near the top -->
<p class="m-5 text-lg">Choose who to split with!</p>
<Input type="text" class="m-5 w-96" placeholder="Search for friends or enter email" />
{#each selected_map.keys() as friend}
	<div class="m-5 flex flex-col">
		<div
			id={friend}
			aria-label="friend cards"
			role="button"
			on:click={() => handleClick(friend)}
			class="flex cursor-pointer flex-row gap-4"
			tabindex="0"
		>
			<div
				class="transition-opacity duration-200 {selected_map.get(friend) === 0 ? '' : 'opacity-50'}"
			>
				<FriendCard name={friend_map.get(friend).name} avatar={friend_map.get(friend).avatar_url} />
			</div>
			{#if selected_map.get(friend) === 1}
				<div transition:fade={{ duration: 200 }} class="my-auto ml-auto">
					<CircleCheck color="#16a34a" />
				</div>
			{/if}
		</div>
	</div>
{/each}
<div class="m-5 flex">
	<Button on:click={(e) => submitEmails()} class="mx-auto">Submit</Button>
</div>