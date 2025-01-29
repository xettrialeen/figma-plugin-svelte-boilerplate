<script lang="ts">
	import Counter from "./Counter.svelte";
	import { get_random_number } from "$lib";
	import Resizeable from "./Resizeable.svelte";
	import{ Button } from "$lib/components/ui/button"

	let counts = $state(0);  // This is automatically reactive
  
  function increment() {


	
    counts += 1;   // Updates trigger re-render
	
  }
	

	/**
	 * Sends a message to plugin's `code.ts` file.
	 * We wrap `parent.postMessage` to get the type safety.
	 *
	 * @param message - The message to be sent.
	 */
	function postMessage(message: UiMessage, targetOrigin: string = "*") {
		parent.postMessage({ pluginMessage: message }, targetOrigin);
	}

	onmessage = async (event: MessageEvent<{ pluginMessage: PluginMessage }>) => {
		console.log("🚀 ~ onmessage= ~ event:", event);
		const data = event.data.pluginMessage;

		switch (data.type) {
			case "count":
				console.log("count", data.count);
				count = data.count;
				break;
			case "a-thing-happened":
				console.log("a-thing-happened");
				data;
				break;
		}
	};

	function button_click() {
		postMessage({ type: "button-click" });
	}

	const initial_count = get_random_number();

	let count = $state(initial_count);
	let text = $state("");
</script>

<Resizeable />

<Button onclick={increment} variant="outline" > Hello world {counts} </Button>
<main
	class="prose max-w-none h-full bg-gray-100 dark:bg-gray-900 flex flex-col items-center justify-center py-8 px-4 text-center"
>

</main>

