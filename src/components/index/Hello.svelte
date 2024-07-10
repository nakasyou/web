<script lang="ts">
  import { onMount } from 'svelte'

  const strs: string[] = [
    'Hello, I\'m',
    'こんにちは、私は',
    '안녕하세요, 난', 
    '你好, 我是',
    'cowsay',
    'Hola, soy'
  ]
  let hellosDiv: HTMLDivElement

  let focusedStr = 0
  onMount(() => {
    const next = () => {
      focusedStr ++
      focusedStr = focusedStr % strs.length

      const rect = hellosDiv.children[focusedStr].getBoundingClientRect()
      const hellosDivRect = hellosDiv.getBoundingClientRect()
      
      const left = rect.left - hellosDivRect.left + hellosDiv.scrollLeft
      hellosDiv.scroll({
        left: left,
        behavior: 'smooth'
      })
      setTimeout(next, 2000)
    }
    next()
  })
</script>
<div bind:this={hellosDiv} class="hidden-scrollbar text-4xl md:text-5xl font-bold flex gap-5 max-w-72 md:max-w-96 overflow-x-scroll break-keep text-nowrap whitespace-nowrap sleect-none touch-none">
  {#each strs as str, i}
    <div id={`hello-im-${i}`} class="transition-opacity duration-200" class:opacity-10={focusedStr !== i}>{str}</div>
  {/each}
  <div class="min-w-96 text-transparent"></div>
</div>
<style>
.hidden-scrollbar {
  scrollbar-width: none;
}
.hidden-scrollbar::-webkit-scrollbar {
  display: none;
}
</style>
