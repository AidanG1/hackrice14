<script lang="ts">
	import { Separator } from '$lib/components/ui/separator/index.js'
	import * as Menubar from '$lib/components/ui/menubar/index.js'
	import Contact from 'lucide-svelte/icons/contact'
	import House from 'lucide-svelte/icons/house'
	import SquarePlus from 'lucide-svelte/icons/square-plus'
	import Settings from '$lib/components/Settings.svelte'
	import { sesh } from '$lib/auth.svelte'

	import { profile } from '$lib/auth.svelte'
	import PageTransition from '$lib/components/PageTransition.svelte'

	let { data } = $props()
</script>

<div class="max-h-screen overflow-y-auto">
	<PageTransition url={data.pathname}>
		<slot></slot>
	</PageTransition>
</div>
<nav>
	<Menubar.Root
		class="fixed bottom-0 left-0 right-0 flex h-20 justify-between rounded-none border-l-0 border-r-0 border-t-2"
	>
		<div class="container mx-auto flex w-full max-w-[435px] justify-between">
			<a href="/" class="my-auto"><House /></a>
			<Separator orientation="vertical" />
			<Menubar.Menu>
				<Menubar.Trigger class="font-sans"><SquarePlus /></Menubar.Trigger>
				<Menubar.Content>
					<Menubar.Item><a href="/add/manual">Input Manually</a></Menubar.Item>
					<Menubar.Separator />
					<Menubar.Item><a href="/add/photo">Upload from Photo</a></Menubar.Item>
				</Menubar.Content>
			</Menubar.Menu>
			<Separator orientation="vertical" />
			<a href="/friends" class="my-auto"><Contact /></a>
			<Separator orientation="vertical" />
			<!-- open the sheet -->
			<span class="my-auto"><Settings {profile} /></span>
		</div>
	</Menubar.Root>
</nav>
