<script lang="ts">
	import { goto } from '$app/navigation';
	import { tv } from 'tailwind-variants';
	import type { HTMLAnchorAttributes, HTMLButtonAttributes } from 'svelte/elements';

	const BUTTONS_VARIANTS = {
		base: 'cursor-pointer focus-visible:ring-ring inline-flex items-center justify-center whitespace-nowrap rounded-md text-sm font-medium transition-colors focus-visible:outline-none focus-visible:ring-1 disabled:pointer-events-none disabled:opacity-50',
		variants: {
			variant: {
				default: 'bg-primary text-primary-foreground hover:bg-primary/90 shadow',
				destructive: 'bg-destructive text-destructive-foreground hover:bg-destructive/90 shadow-sm',
				outlined:
					'border-input bg-background hover:bg-accent hover:text-accent-foreground border shadow-sm',
				secondary: 'bg-secondary text-secondary-foreground hover:bg-secondary/80 shadow-sm',
				ghost: 'hover:bg-accent hover:text-accent-foreground',
				link: 'text-primary underline-offset-4 hover:underline'
			},
			size: {
				default: 'h-9 px-4 py-2',
				sm: 'h-8 rounded-md px-3 text-xs',
				lg: 'h-10 rounded-md px-8',
				icon: 'h-9 w-9'
			}
		},
		defaultVariants: {
			variant: 'default',
			size: 'default'
		}
	} as const;

	const buttonVariants = tv(BUTTONS_VARIANTS);

	const {
		children,
		goto: _goto = '',
		variant = 'default',
		size = 'default',
		loading = false,
		class: _class,
		...props
	}: HTMLAnchorAttributes & {
		goto?: string;
		variant?: keyof typeof BUTTONS_VARIANTS.variants.variant;
		size?: keyof typeof BUTTONS_VARIANTS.variants.size;
		loading?: boolean;
	} = $props();

	let _disabled = $derived(loading);
</script>

<a
	{...props}
	class={`button ${buttonVariants({ variant, size })} ${_disabled ? 'pointer-events-none opacity-75' : ''} ${_class} ${variant}`}
	{..._goto
		? {
				href: _goto,
				onclick: (event) => {
					event.preventDefault();
					goto(_goto);
					return false;
				}
			}
		: {}}
>
	{#if loading}
		Cargando...
	{:else}
		{@render children?.()}
	{/if}
</a>
