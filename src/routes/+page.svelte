<script>
	// ===== normal static import: OK =====

	// OK in dev: inlined
	// OK in build: included
	import './example-import.css';

	// ===== special Vite imports: KO =====

	// KO in dev: should not be inlined
	// OK in build
	import raw from './example-raw.css?raw';
	import('./example-dynamic-raw.css?raw');
	import url from './example-url.css?url'; // raw URL is included in this case 🙃
	import('./example-dynamic-url.css?url');

	// KO in dev: should not be inlined
	// OK in build (not included)
	import.meta.glob('./example-glob-as-raw.css', { as: 'raw' });
	// { './example-glob-1.css': () => import("/src/routes/example-glob-1.css?raw").then(m => m["default"]) }

	// KO in dev: should not be inlined
	// OK in build (not included)
	import.meta.glob('./example-glob-as-url.css', { as: 'url' });
	// { './example-glob-2.css': () => import("/src/routes/example-glob-2.css?url").then(m => m["default"]) }

	// ===== dynamic imports: ❓🤯❓ ======

	// No idea what's the expected behaviour, here 🤔

	// ?? in dev: inlined
	// ?? in build: runtime crash
	{
		import('./example-dynamic.css').then((m) => {
			// it gets the raw file contents
			console.log(m.default);
			// /* example-dynamic.css */
			// body {
			//   font-size: 3rem;
			// }
		});
	}

	// ?? in dev: inlined
	// ?? in build: should it be included? dynamically?
	{
		const glob = import.meta.glob('./example-glob.css');
		// it returns some loaders for the files' raw text content
		glob['./example-glob.css']().then((m) => {
			console.log(m.default);
			// /* example-glob.css */
			// body {
			//   font-weight: bold;
			// }
		});
	}
</script>

<h1>Welcome to SvelteKit</h1>
<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>
