<script lang="ts">
  import { debounce } from 'obsidian'
  import { toggleInputComposition } from 'src/globals'
  import { createEventDispatcher, onMount, tick } from 'svelte'

  export let value = ''
  const dispatch = createEventDispatcher()

  let elInput: HTMLInputElement

  onMount(async () => {
    await tick()
    elInput.focus()
    setTimeout(() => {
      // tick() is not working here?
      elInput.select()
    }, 0)
  })

  const debouncedOnInput = debounce(() => {
    dispatch('input', value)
  }, 100)
</script>

<input
  bind:value
  bind:this={elInput}
  on:input={debouncedOnInput}
  on:compositionstart={_ => toggleInputComposition(true)}
  on:compositionend={_ => toggleInputComposition(false)}
  type="text"
  class="prompt-input"
  placeholder="Type to search through your notes"
  spellcheck="false" />
