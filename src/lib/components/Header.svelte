<script>
    import { PrismicLink } from "@prismicio/svelte";
    import WorldMark from "./WordMark.svelte"
    import ButtonLink from "./ButtonLink.svelte"
	import clsx from "clsx";

    import IconClose from '~icons/ph/x-bold'
    import IconMenu from '~icons/ph/list-bold'
	import { asLink } from "@prismicio/client";
	import { page } from "$app/stores";

    /** @type {import("@prismicio/client").Content.SettingsDocument} */
    export let settings;

    let isOpen = false;
    const toggleOpen = () => (isOpen = !isOpen)
    const close = () => (isOpen = false)

    /** @param {import('@prismicio/client').LinkField} link*/
    const isActive = (link) => {
        const path = asLink(link)

        return path && $page.url.pathname.includes(path)
    }
</script>

<header class="py-4 md:p-6">
    <nav class="mx-auto flex max-w-6xl flex-col justify-between py-2 font font-medium text-white md:flex-row md:items-center" 
    aria-label="Main">

    <div class="flex items-center justify-between">
        <a href="/" on:click={close} class="z-50">
        <WorldMark />
        <span class="sr-only">{settings.data.site_title} home page</span>
        </a>
        
        <button type="button" class="block p-2 text-3xl text-white md:hidden" on:click={toggleOpen}>
            <IconMenu />
        </button>
    </div>

         <!--- Mobile nav -->
        <div class={clsx("fixed inset-0 z-40 flex flex-col items-end bg-gray-950 pr-4 pt-14 transition-transform duration-300 ease-in-out md:hidden", isOpen? "translate-x-0" : "translate-x-[100%]")}>

        <button aria-expanded={isOpen} type="button" class="block p-2 text-3xl text-white md:hidden" on:click={toggleOpen}>
             <IconClose />
         </button>

         <ul class="grid justify-items-end gap-8">
            {#each settings.data.navigation as item (item.label)}
         <li>
            {#if item.cta_button}
                <ButtonLink field={item.link} on:click={close} aria-current={isActive(item.link) ? 'page' : undefined}>
                    {item.label}
                </ButtonLink>
            {:else}
                <PrismicLink field={item.link} on:click={close} class="min-h-11 block px-3 text-3xl first:mt-8" aria-current={isActive(item.link) ? 'page' : undefined}>
                    {item.label}
                </PrismicLink>
            {/if}
        </li>
         {/each}
        </ul>
    </div>

        <!--- Desktop nav -->
        <ul class=" hidden gap-6 md:flex">
            {#each settings.data.navigation as item (item.label)}
         <li>
            {#if item.cta_button}
                <ButtonLink field={item.link} on:click={close} aria-current={isActive(item.link) ? 'page' : undefined}>
                    {item.label}
                </ButtonLink>
            {:else}
                <PrismicLink field={item.link} on:click={close} class="inline-flex min-h-11 items-center" aria-current={isActive(item.link) ? 'page' : undefined}>
                    {item.label}
                </PrismicLink>
            {/if}
        </li>
         {/each}
        </ul>
    </nav>
</header>