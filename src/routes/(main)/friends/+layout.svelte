<!-- <script lang="ts">
	import * as Command from '$lib/components/ui/command/index.js'
	import * as Tabs from '$lib/components/ui/tabs/index.js'

	// let {data, error} = await supabase
	// .from('friends')
	// .select("*")
	// .eq()
	// let { data } = $props()

	// const friends = data.
	

	// const requests = {}

</script>

<Command.Root class="mx-5 mb-0 mt-5 w-96 rounded-md border">
	<Command.Input placeholder="Search for new friends" />
</Command.Root>
<Tabs.Root value="friends">
	<Tabs.List class="mx-5 mt-3 grid w-96 grid-cols-2">
		<Tabs.Trigger value="friends"><a href="/friends">Friends</a></Tabs.Trigger>
		<Tabs.Trigger value="requests"><a href="/friends/requests">Requests</a></Tabs.Trigger>
	</Tabs.List>
	<Tabs.Content value="friends">
		
	</Tabs.Content>
</Tabs.Root> -->

<slot></slot>