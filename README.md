
<html lang="es">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>
    <!-- LIBRERÍAS EXTERNAS -->

    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet"><!-- ESTILOS CSS -->

    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600&family=Playfair+Display:ital,wght@1,400&display=swap');

        :root {
            --light-pink: #F2C0DD; 
            --deep-purple: #25092E; 
            --purple-mid: #8c6595; 
            --shine: #FFF0F8; 
        }

        body {
            font-family: 'Outfit', sans-serif;
            color: var(--light-pink);
            min-height: 100vh;
            /* Soporte para altura dinámica en móviles modernos */
            min-height: 100dvh; 
            cursor: auto; /* Cursor normal por defecto (móvil) */
            overflow-x: hidden;
            background-color: var(--deep-purple);
            /* Evita el rebote elástico en iOS */
            overscroll-behavior-y: none;
            -webkit-user-select: none;
            -moz-user-select: none;
            -ms-user-select: none;
            user-select: none;
            /* Suavizado de fuentes */
            -webkit-font-smoothing: antialiased;
        }

        /* Solo ocultar el cursor nativo en PC */
        @media (hover: hover) {
            body {
                cursor: none;
            }
        }

        /* VIDEO DE FONDO */
        #video-bg {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            object-fit: cover; z-index: -5;
            /* Optimización de renderizado */
            will-change: transform;
        }
        #video-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(37, 9, 46, 0.75);
            z-index: -5;
        }

        /* EFECTOS */
        #stars-container, #ambient-glows {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            pointer-events: none; z-index: -4;
        }
        .star {
            position: absolute; background: white; border-radius: 50%;
            animation: twinkle var(--duration) ease-in-out infinite; animation-delay: var(--delay);
        }
        @keyframes twinkle {
            0%, 100% { opacity: 0; transform: scale(0.5); }
            50% { opacity: var(--max-opacity); transform: scale(1); box-shadow: 0 0 4px var(--light-pink); }
        }

        /* CURSOR Y PARTÍCULAS (Estrictamente ocultos en móvil) */
        .cursor-particle {
            position: fixed; pointer-events: none; border-radius: 50%; z-index: 9998;
            animation: particleFade 1s forwards;
            display: none; /* Oculto por defecto */
        }
        
        #cursor-glow {
            position: fixed; width: 20px; height: 20px;
            background: rgba(242, 192, 221, 0.5);
            border: 1px solid var(--light-pink); border-radius: 50%;
            pointer-events: none; z-index: 9999;
            transform: translate(-50%, -50%);
            transition: width 0.2s, height 0.2s, background 0.3s;
            mix-blend-mode: difference; 
            display: none; /* Oculto por defecto */
        }

        /* Solo mostrar cursor y partículas si el dispositivo tiene puntero preciso (PC) */
        @media (hover: hover) and (pointer: fine) {
            #cursor-glow { display: block; }
            .cursor-particle { display: block; }
        }

        @keyframes particleFade {
            0% { transform: translate(-50%, -50%) scale(1); opacity: 1; }
            100% { transform: translate(var(--tx), var(--ty)) scale(0); opacity: 0; }
        }

        /* UI */
        .glass-card {
            background: rgba(37, 9, 46, 0.65); 
            backdrop-filter: blur(24px); -webkit-backdrop-filter: blur(24px);
            border: 1px solid rgba(242, 192, 221, 0.15); border-top: 1px solid rgba(242, 192, 221, 0.3);
            box-shadow: 0 20px 50px -10px rgba(0, 0, 0, 0.8);
        }
        .title-glow {
            font-family: 'Playfair Display', serif;
            background: linear-gradient(110deg, var(--light-pink) 10%, #fff 30%, var(--purple-mid) 50%, #fff 70%, var(--light-pink) 90%);
            background-size: 200% auto; -webkit-background-clip: text; -webkit-text-fill-color: transparent;
            animation: shineText 8s linear infinite; filter: drop-shadow(0 0 10px rgba(242, 192, 221, 0.3));
        }
        @keyframes shineText { to { background-position: 200% center; } }

        .btn-luxury {
            background: linear-gradient(135deg, var(--deep-purple) 0%, var(--purple-mid) 100%);
            position: relative; overflow: hidden; transition: all 0.4s ease;
            border: 1px solid rgba(242, 192, 221, 0.3); color: var(--light-pink);
            -webkit-tap-highlight-color: transparent;
        }
        .btn-luxury:hover { box-shadow: 0 0 25px rgba(242, 192, 221, 0.4); transform: translateY(-2px); border-color: var(--light-pink); }
        .btn-luxury:active { transform: scale(0.95); }
        .btn-luxury:disabled { opacity: 0.7; cursor: not-allowed; filter: grayscale(1); }

        .gallery-scroll::-webkit-scrollbar { display: none; }
        .gallery-scroll { -ms-overflow-style: none; scrollbar-width: none; }
        .gallery-item { scroll-snap-align: center; }

        input[type=range] { accent-color: var(--purple-mid); cursor: pointer; }
        input[type=range]::-webkit-slider-thumb {
            -webkit-appearance: none; height: 14px; width: 14px; border-radius: 50%;
            background: var(--deep-purple); border: 2px solid var(--light-pink); cursor: pointer; margin-top: -5px;
        }
        .equalizer-bar { background: var(--light-pink); width: 3px; border-radius: 2px; animation: equalize 1s infinite ease-in-out; }
        .equalizer-bar:nth-child(even) { background: var(--purple-mid); }
        @keyframes equalize { 0%, 100% { height: 4px; opacity: 0.5; } 50% { height: 24px; opacity: 1; } }

        #start-screen { background: radial-gradient(circle at center, var(--deep-purple) 0%, #000 100%); }
        ::-webkit-scrollbar { width: 4px; }
        ::-webkit-scrollbar-track { background: #000; }
        ::-webkit-scrollbar-thumb { background: var(--light-pink); border-radius: 10px; }
        
        @keyframes pump {
            0% { transform: scale(1); }
            50% { transform: scale(1.2); color: white; text-shadow: 0 0 15px var(--light-pink); }
            100% { transform: scale(1); }
        }
        .pump-anim { animation: pump 0.3s ease-in-out; }

        @keyframes floatUpLike {
            0% { transform: translate(0, 0) scale(0.5); opacity: 1; }
            50% { opacity: 1; }
            100% { transform: translate(var(--tx), var(--ty)) scale(1.5); opacity: 0; }
        }
        .like-particle {
            position: fixed; pointer-events: none; z-index: 10000;
            color: var(--light-pink); font-size: 1.2rem;
            animation: floatUpLike 1s ease-out forwards;
            text-shadow: 0 0 5px rgba(255, 255, 255, 0.5);
        }
    </style>
    <style>
    *, ::before, ::after{--tw-border-spacing-x:0;--tw-border-spacing-y:0;--tw-translate-x:0;--tw-translate-y:0;--tw-rotate:0;--tw-skew-x:0;--tw-skew-y:0;--tw-scale-x:1;--tw-scale-y:1;--tw-pan-x: ;--tw-pan-y: ;--tw-pinch-zoom: ;--tw-scroll-snap-strictness:proximity;--tw-gradient-from-position: ;--tw-gradient-via-position: ;--tw-gradient-to-position: ;--tw-ordinal: ;--tw-slashed-zero: ;--tw-numeric-figure: ;--tw-numeric-spacing: ;--tw-numeric-fraction: ;--tw-ring-inset: ;--tw-ring-offset-width:0px;--tw-ring-offset-color:#fff;--tw-ring-color:rgb(59 130 246 / 0.5);--tw-ring-offset-shadow:0 0 #0000;--tw-ring-shadow:0 0 #0000;--tw-shadow:0 0 #0000;--tw-shadow-colored:0 0 #0000;--tw-blur: ;--tw-brightness: ;--tw-contrast: ;--tw-grayscale: ;--tw-hue-rotate: ;--tw-invert: ;--tw-saturate: ;--tw-sepia: ;--tw-drop-shadow: ;--tw-backdrop-blur: ;--tw-backdrop-brightness: ;--tw-backdrop-contrast: ;--tw-backdrop-grayscale: ;--tw-backdrop-hue-rotate: ;--tw-backdrop-invert: ;--tw-backdrop-opacity: ;--tw-backdrop-saturate: ;--tw-backdrop-sepia: ;--tw-contain-size: ;--tw-contain-layout: ;--tw-contain-paint: ;--tw-contain-style: }::backdrop{--tw-border-spacing-x:0;--tw-border-spacing-y:0;--tw-translate-x:0;--tw-translate-y:0;--tw-rotate:0;--tw-skew-x:0;--tw-skew-y:0;--tw-scale-x:1;--tw-scale-y:1;--tw-pan-x: ;--tw-pan-y: ;--tw-pinch-zoom: ;--tw-scroll-snap-strictness:proximity;--tw-gradient-from-position: ;--tw-gradient-via-position: ;--tw-gradient-to-position: ;--tw-ordinal: ;--tw-slashed-zero: ;--tw-numeric-figure: ;--tw-numeric-spacing: ;--tw-numeric-fraction: ;--tw-ring-inset: ;--tw-ring-offset-width:0px;--tw-ring-offset-color:#fff;--tw-ring-color:rgb(59 130 246 / 0.5);--tw-ring-offset-shadow:0 0 #0000;--tw-ring-shadow:0 0 #0000;--tw-shadow:0 0 #0000;--tw-shadow-colored:0 0 #0000;--tw-blur: ;--tw-brightness: ;--tw-contrast: ;--tw-grayscale: ;--tw-hue-rotate: ;--tw-invert: ;--tw-saturate: ;--tw-sepia: ;--tw-drop-shadow: ;--tw-backdrop-blur: ;--tw-backdrop-brightness: ;--tw-backdrop-contrast: ;--tw-backdrop-grayscale: ;--tw-backdrop-hue-rotate: ;--tw-backdrop-invert: ;--tw-backdrop-opacity: ;--tw-backdrop-saturate: ;--tw-backdrop-sepia: ;--tw-contain-size: ;--tw-contain-layout: ;--tw-contain-paint: ;--tw-contain-style: }/* ! tailwindcss v3.4.17 | MIT License | https://tailwindcss.com */*,::after,::before{box-sizing:border-box;border-width:0;border-style:solid;border-color:#e5e7eb}::after,::before{--tw-content:''}:host,html{line-height:1.5;-webkit-text-size-adjust:100%;-moz-tab-size:4;tab-size:4;font-family:ui-sans-serif, system-ui, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji";font-feature-settings:normal;font-variation-settings:normal;-webkit-tap-highlight-color:transparent}body{margin:0;line-height:inherit}hr{height:0;color:inherit;border-top-width:1px}abbr:where([title]){-webkit-text-decoration:underline dotted;text-decoration:underline dotted}h1,h2,h3,h4,h5,h6{font-size:inherit;font-weight:inherit}a{color:inherit;text-decoration:inherit}b,strong{font-weight:bolder}code,kbd,pre,samp{font-family:ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace;font-feature-settings:normal;font-variation-settings:normal;font-size:1em}small{font-size:80%}sub,sup{font-size:75%;line-height:0;position:relative;vertical-align:baseline}sub{bottom:-.25em}sup{top:-.5em}table{text-indent:0;border-color:inherit;border-collapse:collapse}button,input,optgroup,select,textarea{font-family:inherit;font-feature-settings:inherit;font-variation-settings:inherit;font-size:100%;font-weight:inherit;line-height:inherit;letter-spacing:inherit;color:inherit;margin:0;padding:0}button,select{text-transform:none}button,input:where([type=button]),input:where([type=reset]),input:where([type=submit]){-webkit-appearance:button;background-color:transparent;background-image:none}:-moz-focusring{outline:auto}:-moz-ui-invalid{box-shadow:none}progress{vertical-align:baseline}::-webkit-inner-spin-button,::-webkit-outer-spin-button{height:auto}[type=search]{-webkit-appearance:textfield;outline-offset:-2px}::-webkit-search-decoration{-webkit-appearance:none}::-webkit-file-upload-button{-webkit-appearance:button;font:inherit}summary{display:list-item}blockquote,dd,dl,figure,h1,h2,h3,h4,h5,h6,hr,p,pre{margin:0}fieldset{margin:0;padding:0}legend{padding:0}menu,ol,ul{list-style:none;margin:0;padding:0}dialog{padding:0}textarea{resize:vertical}input::placeholder,textarea::placeholder{opacity:1;color:#9ca3af}[role=button],button{cursor:pointer}:disabled{cursor:default}audio,canvas,embed,iframe,img,object,svg,video{display:block;vertical-align:middle}img,video{max-width:100%;height:auto}[hidden]:where(:not([hidden=until-found])){display:none}.fixed{position:fixed}.absolute{position:absolute}.relative{position:relative}.inset-0{inset:0px}.inset-3{inset:0.75rem}.inset-4{inset:1rem}.bottom-3{bottom:0.75rem}.left-0{left:0px}.left-1\/2{left:50%}.left-2{left:0.5rem}.right-2{right:0.5rem}.right-3{right:0.75rem}.top-0{top:0px}.top-1\/2{top:50%}.z-10{z-index:10}.z-20{z-index:20}.z-50{z-index:50}.-mx-2{margin-left:-0.5rem;margin-right:-0.5rem}.mx-auto{margin-left:auto;margin-right:auto}.my-auto{margin-top:auto;margin-bottom:auto}.mb-1{margin-bottom:0.25rem}.mb-2{margin-bottom:0.5rem}.mb-3{margin-bottom:0.75rem}.mb-4{margin-bottom:1rem}.mb-6{margin-bottom:1.5rem}.ml-1{margin-left:0.25rem}.mr-2{margin-right:0.5rem}.mt-0\.5{margin-top:0.125rem}.mt-1{margin-top:0.25rem}.mt-2{margin-top:0.5rem}.mt-3{margin-top:0.75rem}.mt-4{margin-top:1rem}.block{display:block}.inline-block{display:inline-block}.flex{display:flex}.grid{display:grid}.hidden{display:none}.aspect-\[4\/3\]{aspect-ratio:4/3}.aspect-\[4\/5\]{aspect-ratio:4/5}.h-1{height:0.25rem}.h-1\.5{height:0.375rem}.h-10{height:2.5rem}.h-12{height:3rem}.h-28{height:7rem}.h-32{height:8rem}.h-4{height:1rem}.h-6{height:1.5rem}.h-8{height:2rem}.h-\[1px\]{height:1px}.h-full{height:100%}.min-h-screen{min-height:100vh}.w-0{width:0px}.w-1\.5{width:0.375rem}.w-10{width:2.5rem}.w-12{width:3rem}.w-24{width:6rem}.w-28{width:7rem}.w-32{width:8rem}.w-4{width:1rem}.w-8{width:2rem}.w-\[85\%\]{width:85%}.w-full{width:100%}.min-w-0{min-width:0px}.max-w-4xl{max-width:56rem}.flex-1{flex:1 1 0%}.flex-shrink-0{flex-shrink:0}.shrink-0{flex-shrink:0}.-translate-x-1\/2{--tw-translate-x:-50%;transform:translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y))}.-translate-y-1\/2{--tw-translate-y:-50%;transform:translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y))}.scale-110{--tw-scale-x:1.1;--tw-scale-y:1.1;transform:translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y))}.scale-95{--tw-scale-x:.95;--tw-scale-y:.95;transform:translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y))}.transform{transform:translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y))}@keyframes spin{to{transform:rotate(360deg)}}.animate-\[spin_12s_linear_infinite\]{animation:spin 12s linear infinite}@keyframes spin{to{transform:rotate(360deg)}}.animate-\[spin_4s_linear_infinite\]{animation:spin 4s linear infinite}@keyframes spin{to{transform:rotate(360deg)}}.animate-\[spin_6s_linear_infinite\]{animation:spin 6s linear infinite}@keyframes pulse{50%{opacity:.5}}.animate-pulse{animation:pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite}.cursor-pointer{cursor:pointer}.select-none{-webkit-user-select:none;user-select:none}.snap-x{scroll-snap-type:x var(--tw-scroll-snap-strictness)}.snap-mandatory{--tw-scroll-snap-strictness:mandatory}.grid-cols-1{grid-template-columns:repeat(1, minmax(0, 1fr))}.flex-col{flex-direction:column}.items-start{align-items:flex-start}.items-end{align-items:flex-end}.items-center{align-items:center}.justify-center{justify-content:center}.justify-between{justify-content:space-between}.gap-1{gap:0.25rem}.gap-1\.5{gap:0.375rem}.gap-2{gap:0.5rem}.gap-3{gap:0.75rem}.gap-4{gap:1rem}.gap-6{gap:1.5rem}.overflow-hidden{overflow:hidden}.overflow-visible{overflow:visible}.overflow-x-auto{overflow-x:auto}.scroll-smooth{scroll-behavior:smooth}.truncate{overflow:hidden;text-overflow:ellipsis;white-space:nowrap}.rounded-2xl{border-radius:1rem}.rounded-3xl{border-radius:1.5rem}.rounded-\[2px\]{border-radius:2px}.rounded-full{border-radius:9999px}.rounded-lg{border-radius:0.5rem}.rounded-xl{border-radius:0.75rem}.border{border-width:1px}.border-b{border-bottom-width:1px}.border-l-2{border-left-width:2px}.border-t{border-top-width:1px}.border-\[var\(--light-pink\)\]{border-color:var(--light-pink)}.border-transparent{border-color:transparent}.border-white\/10{border-color:rgb(255 255 255 / 0.1)}.border-b-\[var\(--light-pink\)\]{border-bottom-color:var(--light-pink)}.border-l-\[var\(--light-pink\)\]{border-left-color:var(--light-pink)}.border-l-\[var\(--purple-mid\)\]{border-left-color:var(--purple-mid)}.border-r-\[var\(--purple-mid\)\]{border-right-color:var(--purple-mid)}.border-t-\[var\(--light-pink\)\]{border-top-color:var(--light-pink)}.bg-\[\#25092E\]{--tw-bg-opacity:1;background-color:rgb(37 9 46 / var(--tw-bg-opacity, 1))}.bg-\[var\(--light-pink\)\]{background-color:var(--light-pink)}.bg-\[var\(--purple-mid\)\]{background-color:var(--purple-mid)}.bg-black{--tw-bg-opacity:1;background-color:rgb(0 0 0 / var(--tw-bg-opacity, 1))}.bg-black\/10{background-color:rgb(0 0 0 / 0.1)}.bg-black\/20{background-color:rgb(0 0 0 / 0.2)}.bg-black\/50{background-color:rgb(0 0 0 / 0.5)}.bg-gradient-to-r{background-image:linear-gradient(to right, var(--tw-gradient-stops))}.from-\[var\(--purple-mid\)\]{--tw-gradient-from:var(--purple-mid) var(--tw-gradient-from-position);--tw-gradient-to:rgb(255 255 255 / 0) var(--tw-gradient-to-position);--tw-gradient-stops:var(--tw-gradient-from), var(--tw-gradient-to)}.from-transparent{--tw-gradient-from:transparent var(--tw-gradient-from-position);--tw-gradient-to:rgb(0 0 0 / 0) var(--tw-gradient-to-position);--tw-gradient-stops:var(--tw-gradient-from), var(--tw-gradient-to)}.from-white{--tw-gradient-from:#fff var(--tw-gradient-from-position);--tw-gradient-to:rgb(255 255 255 / 0) var(--tw-gradient-to-position);--tw-gradient-stops:var(--tw-gradient-from), var(--tw-gradient-to)}.via-\[var\(--light-pink\)\]{--tw-gradient-to:rgb(255 255 255 / 0)  var(--tw-gradient-to-position);--tw-gradient-stops:var(--tw-gradient-from), var(--light-pink) var(--tw-gradient-via-position), var(--tw-gradient-to)}.to-\[var\(--light-pink\)\]{--tw-gradient-to:var(--light-pink) var(--tw-gradient-to-position)}.to-transparent{--tw-gradient-to:transparent var(--tw-gradient-to-position)}.to-white{--tw-gradient-to:#fff var(--tw-gradient-to-position)}.bg-clip-text{-webkit-background-clip:text;background-clip:text}.object-cover{object-fit:cover}.p-0\.5{padding:0.125rem}.p-1{padding:0.25rem}.p-4{padding:1rem}.p-5{padding:1.25rem}.p-6{padding:1.5rem}.px-4{padding-left:1rem;padding-right:1rem}.px-1{padding-left:0.25rem;padding-right:0.25rem}.px-2{padding-left:0.5rem;padding-right:0.5rem}.py-1{padding-top:0.25rem;padding-bottom:0.25rem}.py-2{padding-top:0.5rem;padding-bottom:0.5rem}.py-2\.5{padding-top:0.625rem;padding-bottom:0.625rem}.py-4{padding-top:1rem;padding-bottom:1rem}.pb-6{padding-bottom:1.5rem}.pt-\[65px\]{padding-top:65px}.pb-2{padding-bottom:0.5rem}.pb-3{padding-bottom:0.75rem}.pb-4{padding-bottom:1rem}.pt-2{padding-top:0.5rem}.text-center{text-align:center}.font-mono{font-family:ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace}.font-serif{font-family:ui-serif, Georgia, Cambria, "Times New Roman", Times, serif}.text-2xl{font-size:1.5rem;line-height:2rem}.text-3xl{font-size:1.875rem;line-height:2.25rem}.text-4xl{font-size:2.25rem;line-height:2.5rem}.text-\[10px\]{font-size:10px}.text-\[9px\]{font-size:9px}.text-base{font-size:1rem;line-height:1.5rem}.text-lg{font-size:1.125rem;line-height:1.75rem}.text-xl{font-size:1.25rem;line-height:1.75rem}.text-xs{font-size:0.75rem;line-height:1rem}.font-bold{font-weight:700}.font-medium{font-weight:500}.font-semibold{font-weight:600}.uppercase{text-transform:uppercase}.italic{font-style:italic}.leading-relaxed{line-height:1.625}.tracking-\[0\.2em\]{letter-spacing:0.2em}.tracking-\[0\.3em\]{letter-spacing:0.3em}.tracking-wider{letter-spacing:0.05em}.tracking-widest{letter-spacing:0.1em}.text-\[\#a47ba8\]{--tw-text-opacity:1;color:rgb(164 123 168 / var(--tw-text-opacity, 1))}.text-\[var\(--light-pink\)\]{color:var(--light-pink)}.text-\[var\(--purple-mid\)\]{color:var(--purple-mid)}.text-gray-400{--tw-text-opacity:1;color:rgb(156 163 175 / var(--tw-text-opacity, 1))}.text-gray-500{--tw-text-opacity:1;color:rgb(107 114 128 / var(--tw-text-opacity, 1))}.text-transparent{color:transparent}.text-white{--tw-text-opacity:1;color:rgb(255 255 255 / var(--tw-text-opacity, 1))}.text-white\/90{color:rgb(255 255 255 / 0.9)}.opacity-0{opacity:0}.opacity-50{opacity:0.5}.opacity-60{opacity:0.6}.opacity-80{opacity:0.8}.shadow-2xl{--tw-shadow:0 25px 50px -12px rgb(0 0 0 / 0.25);--tw-shadow-colored:0 25px 50px -12px var(--tw-shadow-color);box-shadow:var(--tw-ring-offset-shadow, 0 0 #0000), var(--tw-ring-shadow, 0 0 #0000), var(--tw-shadow)}.shadow-lg{--tw-shadow:0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);--tw-shadow-colored:0 10px 15px -3px var(--tw-shadow-color), 0 4px 6px -4px var(--tw-shadow-color);box-shadow:var(--tw-ring-offset-shadow, 0 0 #0000), var(--tw-ring-shadow, 0 0 #0000), var(--tw-shadow)}.shadow-xl{--tw-shadow:0 20px 25px -5px rgb(0 0 0 / 0.1), 0 8px 10px -6px rgb(0 0 0 / 0.1);--tw-shadow-colored:0 20px 25px -5px var(--tw-shadow-color), 0 8px 10px -6px var(--tw-shadow-color);box-shadow:var(--tw-ring-offset-shadow, 0 0 #0000), var(--tw-ring-shadow, 0 0 #0000), var(--tw-shadow)}.outline-none{outline:2px solid transparent;outline-offset:2px}.blur-sm{--tw-blur:blur(4px);filter:var(--tw-blur) var(--tw-brightness) var(--tw-contrast) var(--tw-grayscale) var(--tw-hue-rotate) var(--tw-invert) var(--tw-saturate) var(--tw-sepia) var(--tw-drop-shadow)}.drop-shadow-\[0_0_10px_var\(--light-pink\)\]{--tw-drop-shadow:drop-shadow(0 0 10px var(--light-pink));filter:var(--tw-blur) var(--tw-brightness) var(--tw-contrast) var(--tw-grayscale) var(--tw-hue-rotate) var(--tw-invert) var(--tw-saturate) var(--tw-sepia) var(--tw-drop-shadow)}.filter{filter:var(--tw-blur) var(--tw-brightness) var(--tw-contrast) var(--tw-grayscale) var(--tw-hue-rotate) var(--tw-invert) var(--tw-saturate) var(--tw-sepia) var(--tw-drop-shadow)}.backdrop-blur-md{--tw-backdrop-blur:blur(12px);-webkit-backdrop-filter:var(--tw-backdrop-blur) var(--tw-backdrop-brightness) var(--tw-backdrop-contrast) var(--tw-backdrop-grayscale) var(--tw-backdrop-hue-rotate) var(--tw-backdrop-invert) var(--tw-backdrop-opacity) var(--tw-backdrop-saturate) var(--tw-backdrop-sepia);backdrop-filter:var(--tw-backdrop-blur) var(--tw-backdrop-brightness) var(--tw-backdrop-contrast) var(--tw-backdrop-grayscale) var(--tw-backdrop-hue-rotate) var(--tw-backdrop-invert) var(--tw-backdrop-opacity) var(--tw-backdrop-saturate) var(--tw-backdrop-sepia)}.backdrop-blur-sm{--tw-backdrop-blur:blur(4px);-webkit-backdrop-filter:var(--tw-backdrop-blur) var(--tw-backdrop-brightness) var(--tw-backdrop-contrast) var(--tw-backdrop-grayscale) var(--tw-backdrop-hue-rotate) var(--tw-backdrop-invert) var(--tw-backdrop-opacity) var(--tw-backdrop-saturate) var(--tw-backdrop-sepia);backdrop-filter:var(--tw-backdrop-blur) var(--tw-backdrop-brightness) var(--tw-backdrop-contrast) var(--tw-backdrop-grayscale) var(--tw-backdrop-hue-rotate) var(--tw-backdrop-invert) var(--tw-backdrop-opacity) var(--tw-backdrop-saturate) var(--tw-backdrop-sepia)}.transition-all{transition-property:all;transition-timing-function:cubic-bezier(0.4, 0, 0.2, 1);transition-duration:150ms}.transition-colors{transition-property:color, background-color, border-color, fill, stroke, -webkit-text-decoration-color;transition-property:color, background-color, border-color, text-decoration-color, fill, stroke;transition-property:color, background-color, border-color, text-decoration-color, fill, stroke, -webkit-text-decoration-color;transition-timing-function:cubic-bezier(0.4, 0, 0.2, 1);transition-duration:150ms}.transition-opacity{transition-property:opacity;transition-timing-function:cubic-bezier(0.4, 0, 0.2, 1);transition-duration:150ms}.transition-transform{transition-property:transform;transition-timing-function:cubic-bezier(0.4, 0, 0.2, 1);transition-duration:150ms}.duration-1000{transition-duration:1000ms}.duration-200{transition-duration:200ms}.duration-300{transition-duration:300ms}.duration-500{transition-duration:500ms}.duration-700{transition-duration:700ms}.ease-in-out{transition-timing-function:cubic-bezier(0.4, 0, 0.2, 1)}.ease-out{transition-timing-function:cubic-bezier(0, 0, 0.2, 1)}.selection\:bg-\[\#F2C0DD\] *::selection{--tw-bg-opacity:1;background-color:rgb(242 192 221 / var(--tw-bg-opacity, 1))}.selection\:text-\[\#25092E\] *::selection{--tw-text-opacity:1;color:rgb(37 9 46 / var(--tw-text-opacity, 1))}.selection\:bg-\[\#F2C0DD\]::selection{--tw-bg-opacity:1;background-color:rgb(242 192 221 / var(--tw-bg-opacity, 1))}.selection\:text-\[\#25092E\]::selection{--tw-text-opacity:1;color:rgb(37 9 46 / var(--tw-text-opacity, 1))}.hover\:scale-105:hover{--tw-scale-x:1.05;--tw-scale-y:1.05;transform:translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y))}.hover\:bg-\[var\(--light-pink\)\]:hover{background-color:var(--light-pink)}.hover\:text-\[var\(--light-pink\)\]:hover{color:var(--light-pink)}.hover\:text-black:hover{--tw-text-opacity:1;color:rgb(0 0 0 / var(--tw-text-opacity, 1))}.hover\:shadow-\[0_0_10px_var\(--light-pink\)\]:hover{--tw-shadow:0 0 10px var(--light-pink);--tw-shadow-colored:0 0 10px var(--tw-shadow-color);box-shadow:var(--tw-ring-offset-shadow, 0 0 #0000), var(--tw-ring-shadow, 0 0 #0000), var(--tw-shadow)}.active\:scale-95:active{--tw-scale-x:.95;--tw-scale-y:.95;transform:translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y))}.group:hover .group-hover\:h-1\.5{height:0.375rem}.group\/img:hover .group-hover\/img\:scale-110{--tw-scale-x:1.1;--tw-scale-y:1.1;transform:translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y))}.group:hover .group-hover\:scale-100{--tw-scale-x:1;--tw-scale-y:1;transform:translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y))}.group:hover .group-hover\:scale-105{--tw-scale-x:1.05;--tw-scale-y:1.05;transform:translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y))}.group:hover .group-hover\:scale-110{--tw-scale-x:1.1;--tw-scale-y:1.1;transform:translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y))}.group:hover .group-hover\:border-\[var\(--light-pink\)\]{border-color:var(--light-pink)}.group:hover .group-hover\:bg-transparent{background-color:transparent}.group\/btn:hover .group-hover\/btn\:text-white{--tw-text-opacity:1;color:rgb(255 255 255 / var(--tw-text-opacity, 1))}.group\/slider:hover .group-hover\/slider\:opacity-100{opacity:1}.group:hover .group-hover\:opacity-100{opacity:1}@media (min-width: 640px){.sm\:mx-0{margin-left:0px;margin-right:0px}.sm\:mt-0{margin-top:0px}.sm\:w-\[48\%\]{width:48%}.sm\:w-auto{width:auto}.sm\:flex-row{flex-direction:row}.sm\:text-left{text-align:left}}@media (min-width: 768px){.md\:col-span-5{grid-column:span 5 / span 5}.md\:col-span-7{grid-column:span 7 / span 7}.md\:flex{display:flex}.md\:h-12{height:3rem}.md\:h-36{height:9rem}.md\:h-40{height:10rem}.md\:h-8{height:2rem}.md\:w-12{width:3rem}.md\:w-36{width:9rem}.md\:w-40{width:10rem}.md\:grid-cols-12{grid-template-columns:repeat(12, minmax(0, 1fr))}.md\:gap-3{gap:0.75rem}.md\:gap-4{gap:1rem}.md\:gap-6{gap:1.5rem}.md\:gap-8{gap:2rem}.md\:p-5{padding:1.25rem}.md\:p-6{padding:1.5rem}.md\:p-8{padding:2rem}.md\:px-6{padding-left:1.5rem;padding-right:1.5rem}.md\:py-3{padding-top:0.75rem;padding-bottom:0.75rem}.md\:pb-6{padding-bottom:1.5rem}.md\:text-2xl{font-size:1.5rem;line-height:2rem}.md\:text-4xl{font-size:2.25rem;line-height:2.5rem}.md\:text-5xl{font-size:3rem;line-height:1}.md\:text-lg{font-size:1.125rem;line-height:1.75rem}.md\:text-sm{font-size:0.875rem;line-height:1.25rem}.md\:text-xl{font-size:1.25rem;line-height:1.75rem}.md\:text-xs{font-size:0.75rem;line-height:1rem}}
    </style>
    <style>
    *, ::before, ::after{--tw-border-spacing-x:0;--tw-border-spacing-y:0;--tw-translate-x:0;--tw-translate-y:0;--tw-rotate:0;--tw-skew-x:0;--tw-skew-y:0;--tw-scale-x:1;--tw-scale-y:1;--tw-pan-x: ;--tw-pan-y: ;--tw-pinch-zoom: ;--tw-scroll-snap-strictness:proximity;--tw-gradient-from-position: ;--tw-gradient-via-position: ;--tw-gradient-to-position: ;--tw-ordinal: ;--tw-slashed-zero: ;--tw-numeric-figure: ;--tw-numeric-spacing: ;--tw-numeric-fraction: ;--tw-ring-inset: ;--tw-ring-offset-width:0px;--tw-ring-offset-color:#fff;--tw-ring-color:rgb(59 130 246 / 0.5);--tw-ring-offset-shadow:0 0 #0000;--tw-ring-shadow:0 0 #0000;--tw-shadow:0 0 #0000;--tw-shadow-colored:0 0 #0000;--tw-blur: ;--tw-brightness: ;--tw-contrast: ;--tw-grayscale: ;--tw-hue-rotate: ;--tw-invert: ;--tw-saturate: ;--tw-sepia: ;--tw-drop-shadow: ;--tw-backdrop-blur: ;--tw-backdrop-brightness: ;--tw-backdrop-contrast: ;--tw-backdrop-grayscale: ;--tw-backdrop-hue-rotate: ;--tw-backdrop-invert: ;--tw-backdrop-opacity: ;--tw-backdrop-saturate: ;--tw-backdrop-sepia: ;--tw-contain-size: ;--tw-contain-layout: ;--tw-contain-paint: ;--tw-contain-style: }::backdrop{--tw-border-spacing-x:0;--tw-border-spacing-y:0;--tw-translate-x:0;--tw-translate-y:0;--tw-rotate:0;--tw-skew-x:0;--tw-skew-y:0;--tw-scale-x:1;--tw-scale-y:1;--tw-pan-x: ;--tw-pan-y: ;--tw-pinch-zoom: ;--tw-scroll-snap-strictness:proximity;--tw-gradient-from-position: ;--tw-gradient-via-position: ;--tw-gradient-to-position: ;--tw-ordinal: ;--tw-slashed-zero: ;--tw-numeric-figure: ;--tw-numeric-spacing: ;--tw-numeric-fraction: ;--tw-ring-inset: ;--tw-ring-offset-width:0px;--tw-ring-offset-color:#fff;--tw-ring-color:rgb(59 130 246 / 0.5);--tw-ring-offset-shadow:0 0 #0000;--tw-ring-shadow:0 0 #0000;--tw-shadow:0 0 #0000;--tw-shadow-colored:0 0 #0000;--tw-blur: ;--tw-brightness: ;--tw-contrast: ;--tw-grayscale: ;--tw-hue-rotate: ;--tw-invert: ;--tw-saturate: ;--tw-sepia: ;--tw-drop-shadow: ;--tw-backdrop-blur: ;--tw-backdrop-brightness: ;--tw-backdrop-contrast: ;--tw-backdrop-grayscale: ;--tw-backdrop-hue-rotate: ;--tw-backdrop-invert: ;--tw-backdrop-opacity: ;--tw-backdrop-saturate: ;--tw-backdrop-sepia: ;--tw-contain-size: ;--tw-contain-layout: ;--tw-contain-paint: ;--tw-contain-style: }/* ! tailwindcss v3.4.17 | MIT License | https://tailwindcss.com */*,::after,::before{box-sizing:border-box;border-width:0;border-style:solid;border-color:#e5e7eb}::after,::before{--tw-content:''}:host,html{line-height:1.5;-webkit-text-size-adjust:100%;-moz-tab-size:4;tab-size:4;font-family:ui-sans-serif, system-ui, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji";font-feature-settings:normal;font-variation-settings:normal;-webkit-tap-highlight-color:transparent}body{margin:0;line-height:inherit}hr{height:0;color:inherit;border-top-width:1px}abbr:where([title]){-webkit-text-decoration:underline dotted;text-decoration:underline dotted}h1,h2,h3,h4,h5,h6{font-size:inherit;font-weight:inherit}a{color:inherit;text-decoration:inherit}b,strong{font-weight:bolder}code,kbd,pre,samp{font-family:ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace;font-feature-settings:normal;font-variation-settings:normal;font-size:1em}small{font-size:80%}sub,sup{font-size:75%;line-height:0;position:relative;vertical-align:baseline}sub{bottom:-.25em}sup{top:-.5em}table{text-indent:0;border-color:inherit;border-collapse:collapse}button,input,optgroup,select,textarea{font-family:inherit;font-feature-settings:inherit;font-variation-settings:inherit;font-size:100%;font-weight:inherit;line-height:inherit;letter-spacing:inherit;color:inherit;margin:0;padding:0}button,select{text-transform:none}button,input:where([type=button]),input:where([type=reset]),input:where([type=submit]){-webkit-appearance:button;background-color:transparent;background-image:none}:-moz-focusring{outline:auto}:-moz-ui-invalid{box-shadow:none}progress{vertical-align:baseline}::-webkit-inner-spin-button,::-webkit-outer-spin-button{height:auto}[type=search]{-webkit-appearance:textfield;outline-offset:-2px}::-webkit-search-decoration{-webkit-appearance:none}::-webkit-file-upload-button{-webkit-appearance:button;font:inherit}summary{display:list-item}blockquote,dd,dl,figure,h1,h2,h3,h4,h5,h6,hr,p,pre{margin:0}fieldset{margin:0;padding:0}legend{padding:0}menu,ol,ul{list-style:none;margin:0;padding:0}dialog{padding:0}textarea{resize:vertical}input::placeholder,textarea::placeholder{opacity:1;color:#9ca3af}[role=button],button{cursor:pointer}:disabled{cursor:default}audio,canvas,embed,iframe,img,object,svg,video{display:block;vertical-align:middle}img,video{max-width:100%;height:auto}[hidden]:where(:not([hidden=until-found])){display:none}.fixed{position:fixed}.absolute{position:absolute}.relative{position:relative}.inset-0{inset:0px}.inset-3{inset:0.75rem}.inset-4{inset:1rem}.bottom-3{bottom:0.75rem}.left-0{left:0px}.left-1\/2{left:50%}.left-2{left:0.5rem}.right-2{right:0.5rem}.right-3{right:0.75rem}.top-0{top:0px}.top-1\/2{top:50%}.z-10{z-index:10}.z-20{z-index:20}.z-50{z-index:50}.-mx-2{margin-left:-0.5rem;margin-right:-0.5rem}.mx-auto{margin-left:auto;margin-right:auto}.my-auto{margin-top:auto;margin-bottom:auto}.mb-1{margin-bottom:0.25rem}.mb-2{margin-bottom:0.5rem}.mb-3{margin-bottom:0.75rem}.mb-4{margin-bottom:1rem}.mb-6{margin-bottom:1.5rem}.ml-1{margin-left:0.25rem}.mr-2{margin-right:0.5rem}.mt-0\.5{margin-top:0.125rem}.mt-1{margin-top:0.25rem}.mt-2{margin-top:0.5rem}.mt-3{margin-top:0.75rem}.mt-4{margin-top:1rem}.block{display:block}.inline-block{display:inline-block}.flex{display:flex}.grid{display:grid}.hidden{display:none}.aspect-\[4\/3\]{aspect-ratio:4/3}.aspect-\[4\/5\]{aspect-ratio:4/5}.h-1{height:0.25rem}.h-1\.5{height:0.375rem}.h-10{height:2.5rem}.h-12{height:3rem}.h-28{height:7rem}.h-32{height:8rem}.h-4{height:1rem}.h-6{height:1.5rem}.h-8{height:2rem}.h-\[1px\]{height:1px}.h-full{height:100%}.min-h-screen{min-height:100vh}.w-0{width:0px}.w-1\.5{width:0.375rem}.w-10{width:2.5rem}.w-12{width:3rem}.w-24{width:6rem}.w-28{width:7rem}.w-32{width:8rem}.w-4{width:1rem}.w-8{width:2rem}.w-\[85\%\]{width:85%}.w-full{width:100%}.min-w-0{min-width:0px}.max-w-4xl{max-width:56rem}.flex-1{flex:1 1 0%}.flex-shrink-0{flex-shrink:0}.shrink-0{flex-shrink:0}.-translate-x-1\/2{--tw-translate-x:-50%;transform:translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y))}.-translate-y-1\/2{--tw-translate-y:-50%;transform:translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y))}.scale-110{--tw-scale-x:1.1;--tw-scale-y:1.1;transform:translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y))}.scale-95{--tw-scale-x:.95;--tw-scale-y:.95;transform:translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y))}.transform{transform:translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y))}@keyframes spin{to{transform:rotate(360deg)}}.animate-\[spin_12s_linear_infinite\]{animation:spin 12s linear infinite}@keyframes spin{to{transform:rotate(360deg)}}.animate-\[spin_4s_linear_infinite\]{animation:spin 4s linear infinite}@keyframes spin{to{transform:rotate(360deg)}}.animate-\[spin_6s_linear_infinite\]{animation:spin 6s linear infinite}@keyframes pulse{50%{opacity:.5}}.animate-pulse{animation:pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite}.cursor-pointer{cursor:pointer}.select-none{-webkit-user-select:none;user-select:none}.snap-x{scroll-snap-type:x var(--tw-scroll-snap-strictness)}.snap-mandatory{--tw-scroll-snap-strictness:mandatory}.grid-cols-1{grid-template-columns:repeat(1, minmax(0, 1fr))}.flex-col{flex-direction:column}.items-start{align-items:flex-start}.items-end{align-items:flex-end}.items-center{align-items:center}.justify-center{justify-content:center}.justify-between{justify-content:space-between}.gap-1{gap:0.25rem}.gap-1\.5{gap:0.375rem}.gap-2{gap:0.5rem}.gap-3{gap:0.75rem}.gap-4{gap:1rem}.gap-6{gap:1.5rem}.overflow-hidden{overflow:hidden}.overflow-visible{overflow:visible}.overflow-x-auto{overflow-x:auto}.scroll-smooth{scroll-behavior:smooth}.truncate{overflow:hidden;text-overflow:ellipsis;white-space:nowrap}.rounded-2xl{border-radius:1rem}.rounded-3xl{border-radius:1.5rem}.rounded-\[2px\]{border-radius:2px}.rounded-full{border-radius:9999px}.rounded-lg{border-radius:0.5rem}.rounded-xl{border-radius:0.75rem}.border{border-width:1px}.border-b{border-bottom-width:1px}.border-l-2{border-left-width:2px}.border-t{border-top-width:1px}.border-\[var\(--light-pink\)\]{border-color:var(--light-pink)}.border-transparent{border-color:transparent}.border-white\/10{border-color:rgb(255 255 255 / 0.1)}.border-b-\[var\(--light-pink\)\]{border-bottom-color:var(--light-pink)}.border-l-\[var\(--light-pink\)\]{border-left-color:var(--light-pink)}.border-l-\[var\(--purple-mid\)\]{border-left-color:var(--purple-mid)}.border-r-\[var\(--purple-mid\)\]{border-right-color:var(--purple-mid)}.border-t-\[var\(--light-pink\)\]{border-top-color:var(--light-pink)}.bg-\[\#25092E\]{--tw-bg-opacity:1;background-color:rgb(37 9 46 / var(--tw-bg-opacity, 1))}.bg-\[var\(--light-pink\)\]{background-color:var(--light-pink)}.bg-\[var\(--purple-mid\)\]{background-color:var(--purple-mid)}.bg-black{--tw-bg-opacity:1;background-color:rgb(0 0 0 / var(--tw-bg-opacity, 1))}.bg-black\/10{background-color:rgb(0 0 0 / 0.1)}.bg-black\/20{background-color:rgb(0 0 0 / 0.2)}.bg-black\/50{background-color:rgb(0 0 0 / 0.5)}.bg-gradient-to-r{background-image:linear-gradient(to right, var(--tw-gradient-stops))}.from-\[var\(--purple-mid\)\]{--tw-gradient-from:var(--purple-mid) var(--tw-gradient-from-position);--tw-gradient-to:rgb(255 255 255 / 0) var(--tw-gradient-to-position);--tw-gradient-stops:var(--tw-gradient-from), var(--tw-gradient-to)}.from-transparent{--tw-gradient-from:transparent var(--tw-gradient-from-position);--tw-gradient-to:rgb(0 0 0 / 0) var(--tw-gradient-to-position);--tw-gradient-stops:var(--tw-gradient-from), var(--tw-gradient-to)}.from-white{--tw-gradient-from:#fff var(--tw-gradient-from-position);--tw-gradient-to:rgb(255 255 255 / 0) var(--tw-gradient-to-position);--tw-gradient-stops:var(--tw-gradient-from), var(--tw-gradient-to)}.via-\[var\(--light-pink\)\]{--tw-gradient-to:rgb(255 255 255 / 0)  var(--tw-gradient-to-position);--tw-gradient-stops:var(--tw-gradient-from), var(--light-pink) var(--tw-gradient-via-position), var(--tw-gradient-to)}.to-\[var\(--light-pink\)\]{--tw-gradient-to:var(--light-pink) var(--tw-gradient-to-position)}.to-transparent{--tw-gradient-to:transparent var(--tw-gradient-to-position)}.to-white{--tw-gradient-to:#fff var(--tw-gradient-to-position)}.bg-clip-text{-webkit-background-clip:text;background-clip:text}.object-cover{object-fit:cover}.p-0\.5{padding:0.125rem}.p-1{padding:0.25rem}.p-4{padding:1rem}.p-5{padding:1.25rem}.p-6{padding:1.5rem}.px-1{padding-left:0.25rem;padding-right:0.25rem}.px-2{padding-left:0.5rem;padding-right:0.5rem}.px-4{padding-left:1rem;padding-right:1rem}.py-1{padding-top:0.25rem;padding-bottom:0.25rem}.py-2{padding-top:0.5rem;padding-bottom:0.5rem}.py-2\.5{padding-top:0.625rem;padding-bottom:0.625rem}.py-4{padding-top:1rem;padding-bottom:1rem}.pb-2{padding-bottom:0.5rem}.pb-3{padding-bottom:0.75rem}.pb-4{padding-bottom:1rem}.pb-6{padding-bottom:1.5rem}.pt-2{padding-top:0.5rem}.pt-\[65px\]{padding-top:65px}.text-center{text-align:center}.font-mono{font-family:ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace}.font-serif{font-family:ui-serif, Georgia, Cambria, "Times New Roman", Times, serif}.text-2xl{font-size:1.5rem;line-height:2rem}.text-3xl{font-size:1.875rem;line-height:2.25rem}.text-4xl{font-size:2.25rem;line-height:2.5rem}.text-\[10px\]{font-size:10px}.text-\[9px\]{font-size:9px}.text-base{font-size:1rem;line-height:1.5rem}.text-lg{font-size:1.125rem;line-height:1.75rem}.text-xl{font-size:1.25rem;line-height:1.75rem}.text-xs{font-size:0.75rem;line-height:1rem}.font-bold{font-weight:700}.font-medium{font-weight:500}.font-semibold{font-weight:600}.uppercase{text-transform:uppercase}.italic{font-style:italic}.leading-relaxed{line-height:1.625}.tracking-\[0\.2em\]{letter-spacing:0.2em}.tracking-\[0\.3em\]{letter-spacing:0.3em}.tracking-wider{letter-spacing:0.05em}.tracking-widest{letter-spacing:0.1em}.text-\[\#a47ba8\]{--tw-text-opacity:1;color:rgb(164 123 168 / var(--tw-text-opacity, 1))}.text-\[var\(--light-pink\)\]{color:var(--light-pink)}.text-\[var\(--purple-mid\)\]{color:var(--purple-mid)}.text-gray-400{--tw-text-opacity:1;color:rgb(156 163 175 / var(--tw-text-opacity, 1))}.text-gray-500{--tw-text-opacity:1;color:rgb(107 114 128 / var(--tw-text-opacity, 1))}.text-transparent{color:transparent}.text-white{--tw-text-opacity:1;color:rgb(255 255 255 / var(--tw-text-opacity, 1))}.text-white\/90{color:rgb(255 255 255 / 0.9)}.opacity-0{opacity:0}.opacity-50{opacity:0.5}.opacity-60{opacity:0.6}.opacity-80{opacity:0.8}.shadow-2xl{--tw-shadow:0 25px 50px -12px rgb(0 0 0 / 0.25);--tw-shadow-colored:0 25px 50px -12px var(--tw-shadow-color);box-shadow:var(--tw-ring-offset-shadow, 0 0 #0000), var(--tw-ring-shadow, 0 0 #0000), var(--tw-shadow)}.shadow-lg{--tw-shadow:0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);--tw-shadow-colored:0 10px 15px -3px var(--tw-shadow-color), 0 4px 6px -4px var(--tw-shadow-color);box-shadow:var(--tw-ring-offset-shadow, 0 0 #0000), var(--tw-ring-shadow, 0 0 #0000), var(--tw-shadow)}.shadow-xl{--tw-shadow:0 20px 25px -5px rgb(0 0 0 / 0.1), 0 8px 10px -6px rgb(0 0 0 / 0.1);--tw-shadow-colored:0 20px 25px -5px var(--tw-shadow-color), 0 8px 10px -6px var(--tw-shadow-color);box-shadow:var(--tw-ring-offset-shadow, 0 0 #0000), var(--tw-ring-shadow, 0 0 #0000), var(--tw-shadow)}.outline-none{outline:2px solid transparent;outline-offset:2px}.blur-sm{--tw-blur:blur(4px);filter:var(--tw-blur) var(--tw-brightness) var(--tw-contrast) var(--tw-grayscale) var(--tw-hue-rotate) var(--tw-invert) var(--tw-saturate) var(--tw-sepia) var(--tw-drop-shadow)}.drop-shadow-\[0_0_10px_var\(--light-pink\)\]{--tw-drop-shadow:drop-shadow(0 0 10px var(--light-pink));filter:var(--tw-blur) var(--tw-brightness) var(--tw-contrast) var(--tw-grayscale) var(--tw-hue-rotate) var(--tw-invert) var(--tw-saturate) var(--tw-sepia) var(--tw-drop-shadow)}.filter{filter:var(--tw-blur) var(--tw-brightness) var(--tw-contrast) var(--tw-grayscale) var(--tw-hue-rotate) var(--tw-invert) var(--tw-saturate) var(--tw-sepia) var(--tw-drop-shadow)}.backdrop-blur-md{--tw-backdrop-blur:blur(12px);-webkit-backdrop-filter:var(--tw-backdrop-blur) var(--tw-backdrop-brightness) var(--tw-backdrop-contrast) var(--tw-backdrop-grayscale) var(--tw-backdrop-hue-rotate) var(--tw-backdrop-invert) var(--tw-backdrop-opacity) var(--tw-backdrop-saturate) var(--tw-backdrop-sepia);backdrop-filter:var(--tw-backdrop-blur) var(--tw-backdrop-brightness) var(--tw-backdrop-contrast) var(--tw-backdrop-grayscale) var(--tw-backdrop-hue-rotate) var(--tw-backdrop-invert) var(--tw-backdrop-opacity) var(--tw-backdrop-saturate) var(--tw-backdrop-sepia)}.backdrop-blur-sm{--tw-backdrop-blur:blur(4px);-webkit-backdrop-filter:var(--tw-backdrop-blur) var(--tw-backdrop-brightness) var(--tw-backdrop-contrast) var(--tw-backdrop-grayscale) var(--tw-backdrop-hue-rotate) var(--tw-backdrop-invert) var(--tw-backdrop-opacity) var(--tw-backdrop-saturate) var(--tw-backdrop-sepia);backdrop-filter:var(--tw-backdrop-blur) var(--tw-backdrop-brightness) var(--tw-backdrop-contrast) var(--tw-backdrop-grayscale) var(--tw-backdrop-hue-rotate) var(--tw-backdrop-invert) var(--tw-backdrop-opacity) var(--tw-backdrop-saturate) var(--tw-backdrop-sepia)}.transition-all{transition-property:all;transition-timing-function:cubic-bezier(0.4, 0, 0.2, 1);transition-duration:150ms}.transition-colors{transition-property:color, background-color, border-color, fill, stroke, -webkit-text-decoration-color;transition-property:color, background-color, border-color, text-decoration-color, fill, stroke;transition-property:color, background-color, border-color, text-decoration-color, fill, stroke, -webkit-text-decoration-color;transition-timing-function:cubic-bezier(0.4, 0, 0.2, 1);transition-duration:150ms}.transition-opacity{transition-property:opacity;transition-timing-function:cubic-bezier(0.4, 0, 0.2, 1);transition-duration:150ms}.transition-transform{transition-property:transform;transition-timing-function:cubic-bezier(0.4, 0, 0.2, 1);transition-duration:150ms}.duration-1000{transition-duration:1000ms}.duration-200{transition-duration:200ms}.duration-300{transition-duration:300ms}.duration-500{transition-duration:500ms}.duration-700{transition-duration:700ms}.ease-in-out{transition-timing-function:cubic-bezier(0.4, 0, 0.2, 1)}.ease-out{transition-timing-function:cubic-bezier(0, 0, 0.2, 1)}.selection\:bg-\[\#F2C0DD\] *::selection{--tw-bg-opacity:1;background-color:rgb(242 192 221 / var(--tw-bg-opacity, 1))}.selection\:text-\[\#25092E\] *::selection{--tw-text-opacity:1;color:rgb(37 9 46 / var(--tw-text-opacity, 1))}.selection\:bg-\[\#F2C0DD\]::selection{--tw-bg-opacity:1;background-color:rgb(242 192 221 / var(--tw-bg-opacity, 1))}.selection\:text-\[\#25092E\]::selection{--tw-text-opacity:1;color:rgb(37 9 46 / var(--tw-text-opacity, 1))}.hover\:scale-105:hover{--tw-scale-x:1.05;--tw-scale-y:1.05;transform:translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y))}.hover\:bg-\[var\(--light-pink\)\]:hover{background-color:var(--light-pink)}.hover\:text-\[var\(--light-pink\)\]:hover{color:var(--light-pink)}.hover\:text-black:hover{--tw-text-opacity:1;color:rgb(0 0 0 / var(--tw-text-opacity, 1))}.hover\:shadow-\[0_0_10px_var\(--light-pink\)\]:hover{--tw-shadow:0 0 10px var(--light-pink);--tw-shadow-colored:0 0 10px var(--tw-shadow-color);box-shadow:var(--tw-ring-offset-shadow, 0 0 #0000), var(--tw-ring-shadow, 0 0 #0000), var(--tw-shadow)}.active\:scale-95:active{--tw-scale-x:.95;--tw-scale-y:.95;transform:translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y))}.group:hover .group-hover\:h-1\.5{height:0.375rem}.group\/img:hover .group-hover\/img\:scale-110{--tw-scale-x:1.1;--tw-scale-y:1.1;transform:translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y))}.group:hover .group-hover\:scale-100{--tw-scale-x:1;--tw-scale-y:1;transform:translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y))}.group:hover .group-hover\:scale-105{--tw-scale-x:1.05;--tw-scale-y:1.05;transform:translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y))}.group:hover .group-hover\:scale-110{--tw-scale-x:1.1;--tw-scale-y:1.1;transform:translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y))}.group:hover .group-hover\:border-\[var\(--light-pink\)\]{border-color:var(--light-pink)}.group:hover .group-hover\:bg-transparent{background-color:transparent}.group\/btn:hover .group-hover\/btn\:text-white{--tw-text-opacity:1;color:rgb(255 255 255 / var(--tw-text-opacity, 1))}.group\/slider:hover .group-hover\/slider\:opacity-100{opacity:1}.group:hover .group-hover\:opacity-100{opacity:1}@media (min-width: 640px){.sm\:mx-0{margin-left:0px;margin-right:0px}.sm\:mt-0{margin-top:0px}.sm\:w-\[48\%\]{width:48%}.sm\:w-auto{width:auto}.sm\:flex-row{flex-direction:row}.sm\:text-left{text-align:left}}@media (min-width: 768px){.md\:col-span-5{grid-column:span 5 / span 5}.md\:col-span-7{grid-column:span 7 / span 7}.md\:flex{display:flex}.md\:h-12{height:3rem}.md\:h-36{height:9rem}.md\:h-40{height:10rem}.md\:h-8{height:2rem}.md\:w-12{width:3rem}.md\:w-36{width:9rem}.md\:w-40{width:10rem}.md\:grid-cols-12{grid-template-columns:repeat(12, minmax(0, 1fr))}.md\:gap-3{gap:0.75rem}.md\:gap-4{gap:1rem}.md\:gap-6{gap:1.5rem}.md\:gap-8{gap:2rem}.md\:p-5{padding:1.25rem}.md\:p-6{padding:1.5rem}.md\:p-8{padding:2rem}.md\:px-6{padding-left:1.5rem;padding-right:1.5rem}.md\:py-3{padding-top:0.75rem;padding-bottom:0.75rem}.md\:pb-6{padding-bottom:1.5rem}.md\:text-2xl{font-size:1.5rem;line-height:2rem}.md\:text-4xl{font-size:2.25rem;line-height:2.5rem}.md\:text-5xl{font-size:3rem;line-height:1}.md\:text-lg{font-size:1.125rem;line-height:1.75rem}.md\:text-sm{font-size:0.875rem;line-height:1.25rem}.md\:text-xl{font-size:1.25rem;line-height:1.75rem}.md\:text-xs{font-size:0.75rem;line-height:1rem}}
    </style>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  </head>
  <body class="flex flex-col items-center min-h-screen px-4 pb-6 md:px-6 md:pb-6 pt-[65px] selection:bg-[#F2C0DD] selection:text-[#25092E]">
    <!-- VIDEO DE FONDO -->
     <video autoplay="" muted loop="" playsinline id="video-bg"><source src="fondo.mp4" type="video/mp4"> <source src="https://i.imgur.com/5X5Yczn.mp4" type="video/mp4"></video>
    <div id="video-overlay"></div>
    <div id="stars-container">
      <div class="star" style="width: 1.22302px; height: 1.22302px; left: 87.1631%; top: 14.5738%; --duration: 4.730942276397562s; --delay: 4.167913256974449s; --max-opacity: 0.30656751032228075; background-color: var(--light-pink); box-shadow: 0 0 6px var(--light-pink);"></div>
      <div class="star" style="width: 2.62515px; height: 2.62515px; left: 53.3158%; top: 76.1334%; --duration: 3.4354117599810117s; --delay: 4.468624806765476s; --max-opacity: 0.41183689814945384;"></div>
      <div class="star" style="width: 1.45295px; height: 1.45295px; left: 73.4726%; top: 69.7068%; --duration: 2.1344659399865176s; --delay: 1.1804852017210943s; --max-opacity: 0.6202098704188402;"></div>
      <div class="star" style="width: 2.60891px; height: 2.60891px; left: 11.1849%; top: 32.6248%; --duration: 4.67994225154014s; --delay: 4.111573336919435s; --max-opacity: 0.37178486842484826;"></div>
      <div class="star" style="width: 1.68095px; height: 1.68095px; left: 53.8021%; top: 37.1953%; --duration: 2.4513714090406404s; --delay: 1.9091916654370777s; --max-opacity: 0.3904019689222227;"></div>
      <div class="star" style="width: 1.06805px; height: 1.06805px; left: 45.1943%; top: 46.3804%; --duration: 2.3484504568046574s; --delay: 3.5366176769181146s; --max-opacity: 0.9071563749809284;"></div>
      <div class="star" style="width: 2.42773px; height: 2.42773px; left: 26.9265%; top: 11.532%; --duration: 4.998250359714685s; --delay: 3.0340638594153346s; --max-opacity: 0.36046712558215754;"></div>
      <div class="star" style="width: 1.60624px; height: 1.60624px; left: 84.7579%; top: 36.829%; --duration: 4.67269482087102s; --delay: 1.5122693572273127s; --max-opacity: 0.5850027475943052;"></div>
      <div class="star" style="width: 1.39856px; height: 1.39856px; left: 76.2129%; top: 46.1163%; --duration: 3.405839357835144s; --delay: 4.763054815480513s; --max-opacity: 0.4439544537961084;"></div>
      <div class="star" style="width: 2.31042px; height: 2.31042px; left: 8.89402%; top: 20.5054%; --duration: 2.711470645922981s; --delay: 0.5125076020133246s; --max-opacity: 0.4063999654119196;"></div>
      <div class="star" style="width: 1.58545px; height: 1.58545px; left: 17.4738%; top: 65.9792%; --duration: 3.3734076178211803s; --delay: 4.269773977050066s; --max-opacity: 0.7831104498825313; background-color: var(--light-pink); box-shadow: 0 0 6px var(--light-pink);"></div>
      <div class="star" style="width: 2.90133px; height: 2.90133px; left: 42.4734%; top: 95.4155%; --duration: 3.615938687388707s; --delay: 0.8375420218197382s; --max-opacity: 0.7557512540200202;"></div>
      <div class="star" style="width: 2.25958px; height: 2.25958px; left: 89.2653%; top: 10.051%; --duration: 3.505303438703609s; --delay: 2.4814664730142963s; --max-opacity: 0.572427514666644;"></div>
      <div class="star" style="width: 2.24553px; height: 2.24553px; left: 22.8956%; top: 11.517%; --duration: 4.693285021287437s; --delay: 0.9309946536335612s; --max-opacity: 0.7263726023555662;"></div>
      <div class="star" style="width: 2.989px; height: 2.989px; left: 90.8827%; top: 58.0366%; --duration: 2.668711639704399s; --delay: 3.7656876009639575s; --max-opacity: 0.5407119205617863;"></div>
      <div class="star" style="width: 2.0384px; height: 2.0384px; left: 38.6571%; top: 2.13756%; --duration: 2.677898419560937s; --delay: 3.2665560292598195s; --max-opacity: 0.8551132005289739;"></div>
      <div class="star" style="width: 2.00526px; height: 2.00526px; left: 3.01057%; top: 22.461%; --duration: 4.721696795604434s; --delay: 2.663991063939574s; --max-opacity: 0.5152410897427715;"></div>
      <div class="star" style="width: 1.80332px; height: 1.80332px; left: 49.8472%; top: 36.808%; --duration: 3.0690821929923526s; --delay: 1.5282447898056428s; --max-opacity: 0.8310579524550727;"></div>
      <div class="star" style="width: 1.12176px; height: 1.12176px; left: 69.8029%; top: 86.097%; --duration: 4.896003461413236s; --delay: 3.178264223848525s; --max-opacity: 0.3486748679719994; background-color: var(--light-pink); box-shadow: 0 0 6px var(--light-pink);"></div>
      <div class="star" style="width: 2.79923px; height: 2.79923px; left: 35.5004%; top: 58.1468%; --duration: 4.772107645239589s; --delay: 1.6466996809206746s; --max-opacity: 0.913676541145704;"></div>
      <div class="star" style="width: 1.04541px; height: 1.04541px; left: 27.4968%; top: 34.2411%; --duration: 4.5717140884369165s; --delay: 1.3456504238844658s; --max-opacity: 0.5958516438257118;"></div>
      <div class="star" style="width: 1.61921px; height: 1.61921px; left: 89.654%; top: 13.5165%; --duration: 2.931235623219237s; --delay: 1.9945735017472166s; --max-opacity: 0.7935709275955701;"></div>
      <div class="star" style="width: 1.29521px; height: 1.29521px; left: 49.3101%; top: 57.381%; --duration: 3.416302053286194s; --delay: 3.251815615812146s; --max-opacity: 0.3042849672234596;"></div>
      <div class="star" style="width: 2.47542px; height: 2.47542px; left: 21.4523%; top: 77.5741%; --duration: 3.674213228698666s; --delay: 4.4491789758390325s; --max-opacity: 0.6543434675644528;"></div>
      <div class="star" style="width: 1.46345px; height: 1.46345px; left: 42.0182%; top: 29.2521%; --duration: 2.737743421733529s; --delay: 0.825619917189282s; --max-opacity: 0.6077474801658074;"></div>
      <div class="star" style="width: 1.33894px; height: 1.33894px; left: 25.9294%; top: 69.2802%; --duration: 4.005208138585931s; --delay: 2.884538494901715s; --max-opacity: 0.9656890341603015; background-color: var(--light-pink); box-shadow: 0 0 6px var(--light-pink);"></div>
      <div class="star" style="width: 2.56755px; height: 2.56755px; left: 46.8664%; top: 34.5829%; --duration: 3.3609469020468206s; --delay: 1.2694772620061878s; --max-opacity: 0.6240231254350803;"></div>
      <div class="star" style="width: 2.52288px; height: 2.52288px; left: 88.7597%; top: 68.7382%; --duration: 4.96910012739001s; --delay: 4.539602237724352s; --max-opacity: 0.8986755690656949;"></div>
      <div class="star" style="width: 1.72604px; height: 1.72604px; left: 88.9107%; top: 58.0234%; --duration: 4.5166364784869275s; --delay: 1.377001930650893s; --max-opacity: 0.4167918698021424;"></div>
      <div class="star" style="width: 1.95462px; height: 1.95462px; left: 13.202%; top: 21.6522%; --duration: 3.8054837761923244s; --delay: 1.030031800654726s; --max-opacity: 0.6615636964392151;"></div>
      <div class="star" style="width: 2.93012px; height: 2.93012px; left: 89.948%; top: 74.774%; --duration: 4.379931051958217s; --delay: 1.270240639866963s; --max-opacity: 0.30757681118552077;"></div>
      <div class="star" style="width: 2.66637px; height: 2.66637px; left: 12.2926%; top: 52.2575%; --duration: 2.2555459265187956s; --delay: 0.09847251025302539s; --max-opacity: 0.41425709683424844;"></div>
      <div class="star" style="width: 1.18921px; height: 1.18921px; left: 17.7567%; top: 42.1173%; --duration: 3.13989379952842s; --delay: 4.740937345731598s; --max-opacity: 0.39395288257045774;"></div>
      <div class="star" style="width: 2.79541px; height: 2.79541px; left: 51.3065%; top: 36.6691%; --duration: 3.478837600798966s; --delay: 4.408173120168698s; --max-opacity: 0.3558982510400043;"></div>
      <div class="star" style="width: 1.18401px; height: 1.18401px; left: 71.0731%; top: 0.683121%; --duration: 4.147187185310136s; --delay: 0.6272188629544589s; --max-opacity: 0.3078625505027134; background-color: var(--light-pink); box-shadow: 0 0 6px var(--light-pink);"></div>
      <div class="star" style="width: 1.89222px; height: 1.89222px; left: 76.5461%; top: 30.603%; --duration: 3.0518549746532697s; --delay: 0.39946021230362283s; --max-opacity: 0.765826598328875;"></div>
      <div class="star" style="width: 2.04911px; height: 2.04911px; left: 27.7652%; top: 49.9457%; --duration: 2.536512271034291s; --delay: 4.741594351589211s; --max-opacity: 0.49392888017231956; background-color: var(--light-pink); box-shadow: 0 0 6px var(--light-pink);"></div>
      <div class="star" style="width: 2.79033px; height: 2.79033px; left: 86.3281%; top: 11.2361%; --duration: 3.9248316290475236s; --delay: 4.75518674612853s; --max-opacity: 0.899755430429801; background-color: var(--light-pink); box-shadow: 0 0 6px var(--light-pink);"></div>
      <div class="star" style="width: 1.60545px; height: 1.60545px; left: 81.4806%; top: 89.0132%; --duration: 4.828674393925848s; --delay: 2.5780477898515652s; --max-opacity: 0.6921789537315206; background-color: var(--light-pink); box-shadow: 0 0 6px var(--light-pink);"></div>
      <div class="star" style="width: 1.86759px; height: 1.86759px; left: 5.33269%; top: 30.8856%; --duration: 2.352167656117033s; --delay: 4.605004868551883s; --max-opacity: 0.35903763654905885;"></div>
      <div class="star" style="width: 1.29546px; height: 1.29546px; left: 39.682%; top: 55.2174%; --duration: 2.8168081461735928s; --delay: 1.6225138975691633s; --max-opacity: 0.4518468727068513;"></div>
      <div class="star" style="width: 1.95608px; height: 1.95608px; left: 16.8601%; top: 45.5193%; --duration: 3.560976887349279s; --delay: 4.629725281499789s; --max-opacity: 0.6832719733255543;"></div>
      <div class="star" style="width: 2.34145px; height: 2.34145px; left: 3.17848%; top: 85.6645%; --duration: 2.6780737414956466s; --delay: 2.9960496930993665s; --max-opacity: 0.9117910007715668;"></div>
      <div class="star" style="width: 2.02174px; height: 2.02174px; left: 48.0847%; top: 32.615%; --duration: 2.418979014239731s; --delay: 0.48171087773552723s; --max-opacity: 0.910053091174875;"></div>
      <div class="star" style="width: 1.68667px; height: 1.68667px; left: 92.1171%; top: 52.3724%; --duration: 2.5100301113957633s; --delay: 1.2130171627307966s; --max-opacity: 0.9207981443105668;"></div>
      <div class="star" style="width: 2.42216px; height: 2.42216px; left: 11.4935%; top: 8.85031%; --duration: 2.766450222688835s; --delay: 0.9664704074608566s; --max-opacity: 0.8656702501061244;"></div>
      <div class="star" style="width: 2.38597px; height: 2.38597px; left: 62.5545%; top: 54.9766%; --duration: 2.0463649878698043s; --delay: 4.91187126318621s; --max-opacity: 0.7091742812139239; background-color: var(--light-pink); box-shadow: 0 0 6px var(--light-pink);"></div>
      <div class="star" style="width: 1.02834px; height: 1.02834px; left: 70.0403%; top: 33.8466%; --duration: 3.7360727272177394s; --delay: 1.6203565023048816s; --max-opacity: 0.9796610200936495;"></div>
      <div class="star" style="width: 2.54213px; height: 2.54213px; left: 0.862804%; top: 51.4917%; --duration: 2.957261733774906s; --delay: 4.766607745995298s; --max-opacity: 0.5960232288994739;"></div>
      <div class="star" style="width: 1.00301px; height: 1.00301px; left: 66.9894%; top: 56.1917%; --duration: 3.7098734487635756s; --delay: 2.227853201320893s; --max-opacity: 0.898280209187347;"></div>
      <div class="star" style="width: 2.81144px; height: 2.81144px; left: 73.7051%; top: 50.5284%; --duration: 3.504770633389021s; --delay: 4.614463497243584s; --max-opacity: 0.8848954503279673;"></div>
      <div class="star" style="width: 1.11724px; height: 1.11724px; left: 51.4518%; top: 34.2253%; --duration: 3.8107445672816898s; --delay: 3.0000018173037897s; --max-opacity: 0.92638917946847;"></div>
      <div class="star" style="width: 1.31738px; height: 1.31738px; left: 61.8905%; top: 53.3101%; --duration: 2.713926923617646s; --delay: 1.8676751041026591s; --max-opacity: 0.4131697501037003;"></div>
      <div class="star" style="width: 2.61035px; height: 2.61035px; left: 82.5023%; top: 5.95173%; --duration: 3.7076214945113275s; --delay: 4.713344381089233s; --max-opacity: 0.7466781358585006;"></div>
      <div class="star" style="width: 1.43947px; height: 1.43947px; left: 71.9623%; top: 99.981%; --duration: 2.9719459859063293s; --delay: 0.16577278612861568s; --max-opacity: 0.547141268339633;"></div>
      <div class="star" style="width: 1.91249px; height: 1.91249px; left: 50.4792%; top: 0.982023%; --duration: 4.0941086508219335s; --delay: 1.054137881600703s; --max-opacity: 0.7510297037193191;"></div>
      <div class="star" style="width: 1.2933px; height: 1.2933px; left: 31.7533%; top: 74.2325%; --duration: 2.9561871693211415s; --delay: 4.9021521037654905s; --max-opacity: 0.6896562915500498;"></div>
      <div class="star" style="width: 2.80344px; height: 2.80344px; left: 88.9331%; top: 80.0086%; --duration: 3.079262445277684s; --delay: 4.903741059716236s; --max-opacity: 0.4838155656891238; background-color: var(--light-pink); box-shadow: 0 0 6px var(--light-pink);"></div>
      <div class="star" style="width: 2.08662px; height: 2.08662px; left: 67.8788%; top: 46.663%; --duration: 3.1094823412231216s; --delay: 4.989743326495188s; --max-opacity: 0.48008083819227737;"></div>
      <div class="star" style="width: 1.04199px; height: 1.04199px; left: 50.4997%; top: 53.9767%; --duration: 2.751666051028499s; --delay: 0.41800637049310085s; --max-opacity: 0.8658763519912529; background-color: var(--light-pink); box-shadow: 0 0 6px var(--light-pink);"></div>
      <div class="star" style="width: 2.38479px; height: 2.38479px; left: 53.7402%; top: 94.2134%; --duration: 4.048949428745662s; --delay: 3.4120875246795506s; --max-opacity: 0.7667118416597518;"></div>
      <div class="star" style="width: 1.57583px; height: 1.57583px; left: 15.1637%; top: 32.5544%; --duration: 3.8815933671241174s; --delay: 0.6312514356152571s; --max-opacity: 0.7058361346537072;"></div>
      <div class="star" style="width: 2.12862px; height: 2.12862px; left: 26.5733%; top: 11.9304%; --duration: 2.1531302835075277s; --delay: 0.7114560296142952s; --max-opacity: 0.5206528483984098;"></div>
      <div class="star" style="width: 2.21316px; height: 2.21316px; left: 22.154%; top: 51.4325%; --duration: 2.3865983104715442s; --delay: 4.914884194044593s; --max-opacity: 0.38935883154893014; background-color: var(--light-pink); box-shadow: 0 0 6px var(--light-pink);"></div>
      <div class="star" style="width: 2.60627px; height: 2.60627px; left: 97.1171%; top: 36.6311%; --duration: 4.208885190249273s; --delay: 0.25345790815642255s; --max-opacity: 0.7617346720517506;"></div>
      <div class="star" style="width: 1.9301px; height: 1.9301px; left: 78.3924%; top: 69.2727%; --duration: 4.355508133910195s; --delay: 3.6747233884579638s; --max-opacity: 0.8775908752960861;"></div>
      <div class="star" style="width: 2.64755px; height: 2.64755px; left: 42.9191%; top: 49.3684%; --duration: 2.5788728647688117s; --delay: 4.698726677598935s; --max-opacity: 0.8773031687522719;"></div>
      <div class="star" style="width: 2.06864px; height: 2.06864px; left: 63.1773%; top: 36.3741%; --duration: 3.589377256490287s; --delay: 1.2693226582386612s; --max-opacity: 0.6214721244722555;"></div>
      <div class="star" style="width: 2.98821px; height: 2.98821px; left: 8.65317%; top: 68.7974%; --duration: 3.0220824739694816s; --delay: 2.509012693592652s; --max-opacity: 0.6339432762989545;"></div>
      <div class="star" style="width: 1.5726px; height: 1.5726px; left: 22.0722%; top: 6.88924%; --duration: 2.3046580807674752s; --delay: 1.9348926497735552s; --max-opacity: 0.4628878074462246;"></div>
      <div class="star" style="width: 2.19319px; height: 2.19319px; left: 98.2944%; top: 67.3426%; --duration: 2.3348328302744856s; --delay: 2.1495807120357284s; --max-opacity: 0.3471506326547392;"></div>
      <div class="star" style="width: 1.43992px; height: 1.43992px; left: 6.49434%; top: 39.271%; --duration: 2.390946597999522s; --delay: 0.6235968208602766s; --max-opacity: 0.4432365093479329;"></div>
      <div class="star" style="width: 2.36898px; height: 2.36898px; left: 68.9465%; top: 10.0206%; --duration: 4.652093383063173s; --delay: 4.603914398034191s; --max-opacity: 0.32332438210290576;"></div>
      <div class="star" style="width: 1.43393px; height: 1.43393px; left: 4.33264%; top: 40.966%; --duration: 2.3801291863330087s; --delay: 4.616827545241185s; --max-opacity: 0.3580406592483192;"></div>
      <div class="star" style="width: 2.1818px; height: 2.1818px; left: 20.9479%; top: 51.6257%; --duration: 4.173723581990813s; --delay: 2.6951309115137363s; --max-opacity: 0.7930062876181251;"></div>
      <div class="star" style="width: 2.63651px; height: 2.63651px; left: 27.832%; top: 14.0014%; --duration: 3.3365685198457693s; --delay: 3.132187915537239s; --max-opacity: 0.7047318015514447; background-color: var(--light-pink); box-shadow: 0 0 6px var(--light-pink);"></div>
      <div class="star" style="width: 1.00133px; height: 1.00133px; left: 4.44725%; top: 65.8143%; --duration: 4.972207442670016s; --delay: 2.7680228853193594s; --max-opacity: 0.9797782813939628;"></div>
      <div class="star" style="width: 2.33151px; height: 2.33151px; left: 62.7742%; top: 80.8872%; --duration: 4.149812909423412s; --delay: 1.7553809599573382s; --max-opacity: 0.48422682556524377;"></div>
      <div class="star" style="width: 1.17125px; height: 1.17125px; left: 78.1561%; top: 91.4763%; --duration: 3.279479404397988s; --delay: 0.23877593946453168s; --max-opacity: 0.5512880636373889; background-color: var(--light-pink); box-shadow: 0 0 6px var(--light-pink);"></div>
      <div class="star" style="width: 2.81101px; height: 2.81101px; left: 17.6329%; top: 15.2317%; --duration: 4.505560098284317s; --delay: 1.1178712766929628s; --max-opacity: 0.39495325360374844;"></div>
      <div class="star" style="width: 1.67624px; height: 1.67624px; left: 31.8666%; top: 52.1403%; --duration: 4.954032378984295s; --delay: 4.835302529752777s; --max-opacity: 0.5445776421624798; background-color: var(--light-pink); box-shadow: 0 0 6px var(--light-pink);"></div>
      <div class="star" style="width: 2.44986px; height: 2.44986px; left: 19.6135%; top: 22.4284%; --duration: 4.840316344574664s; --delay: 0.9313109668940994s; --max-opacity: 0.785236201654609;"></div>
      <div class="star" style="width: 1.57598px; height: 1.57598px; left: 2.08944%; top: 50.2881%; --duration: 3.8683777543577977s; --delay: 0.07377735114018513s; --max-opacity: 0.32279778400454945;"></div>
      <div class="star" style="width: 2.9111px; height: 2.9111px; left: 15.4444%; top: 18.7151%; --duration: 3.0283032589947467s; --delay: 2.2487970189911097s; --max-opacity: 0.4682716373595239;"></div>
      <div class="star" style="width: 1.32763px; height: 1.32763px; left: 93.7666%; top: 71.3673%; --duration: 2.9374947379634824s; --delay: 0.37836873420967976s; --max-opacity: 0.42348316964785704;"></div>
      <div class="star" style="width: 2.7041px; height: 2.7041px; left: 56.9513%; top: 75.1454%; --duration: 3.8737007567245256s; --delay: 3.758153242840633s; --max-opacity: 0.6969084137428112;"></div>
      <div class="star" style="width: 1.02452px; height: 1.02452px; left: 6.44248%; top: 11.63%; --duration: 4.680703435808879s; --delay: 3.352127897432097s; --max-opacity: 0.7557324037093639; background-color: var(--light-pink); box-shadow: 0 0 6px var(--light-pink);"></div>
      <div class="star" style="width: 2.21706px; height: 2.21706px; left: 27.8152%; top: 36.7973%; --duration: 4.290565970077674s; --delay: 2.1093154028927352s; --max-opacity: 0.30259312862083987;"></div>
      <div class="star" style="width: 1.48631px; height: 1.48631px; left: 32.6705%; top: 47.5085%; --duration: 3.566040858066098s; --delay: 0.4750955161526532s; --max-opacity: 0.3433480801565443;"></div>
      <div class="star" style="width: 2.583px; height: 2.583px; left: 92.0364%; top: 31.1622%; --duration: 2.1014785540728704s; --delay: 2.6289684729291825s; --max-opacity: 0.572929409037457;"></div>
      <div class="star" style="width: 1.51576px; height: 1.51576px; left: 91.8536%; top: 1.25967%; --duration: 4.004027244091178s; --delay: 0.2438237176017788s; --max-opacity: 0.5027718384849452;"></div>
      <div class="star" style="width: 2.13291px; height: 2.13291px; left: 47.6496%; top: 53.5437%; --duration: 3.843936241799352s; --delay: 4.623438213467215s; --max-opacity: 0.8224113591645967; background-color: var(--light-pink); box-shadow: 0 0 6px var(--light-pink);"></div>
      <div class="star" style="width: 1.27282px; height: 1.27282px; left: 10.7951%; top: 13.0392%; --duration: 2.6777972406560275s; --delay: 3.537232799336616s; --max-opacity: 0.3262156834885524;"></div>
      <div class="star" style="width: 1.21425px; height: 1.21425px; left: 95.6638%; top: 42.6689%; --duration: 2.853289477654985s; --delay: 3.5913509970625457s; --max-opacity: 0.8065132859400701;"></div>
      <div class="star" style="width: 2.09756px; height: 2.09756px; left: 83.8091%; top: 47.3251%; --duration: 2.873578186805051s; --delay: 3.4622792157109084s; --max-opacity: 0.3994678771735398;"></div>
      <div class="star" style="width: 1.06105px; height: 1.06105px; left: 82.4582%; top: 79.8044%; --duration: 3.448122306703782s; --delay: 0.9702852307965654s; --max-opacity: 0.41461298254736956;"></div>
      <div class="star" style="width: 2.22914px; height: 2.22914px; left: 51.25%; top: 6.71577%; --duration: 2.655451497914459s; --delay: 0.6601447358033347s; --max-opacity: 0.9842064300717448; background-color: var(--light-pink); box-shadow: 0 0 6px var(--light-pink);"></div>
      <div class="star" style="width: 2.06391px; height: 2.06391px; left: 25.153%; top: 79.2941%; --duration: 3.6511932662696363s; --delay: 0.7814796969845444s; --max-opacity: 0.7650116603517452;"></div>
      <div class="star" style="width: 1.08085px; height: 1.08085px; left: 7.92945%; top: 16.3362%; --duration: 4.075682744566066s; --delay: 0.3715399368206851s; --max-opacity: 0.6133970838294434;"></div>
      <div class="star" style="width: 1.86714px; height: 1.86714px; left: 23.2148%; top: 20.9415%; --duration: 2.2888415015354338s; --delay: 4.010105603877332s; --max-opacity: 0.9072335382041827;"></div>
      <div class="star" style="width: 1.80228px; height: 1.80228px; left: 94.519%; top: 51.7248%; --duration: 2.9128287672391506s; --delay: 4.901906360159973s; --max-opacity: 0.37867891719959795;"></div>
      <div class="star" style="width: 2.50285px; height: 2.50285px; left: 62.5664%; top: 92.9643%; --duration: 2.847353829272632s; --delay: 1.530249667779875s; --max-opacity: 0.38975732068404967;"></div>
      <div class="star" style="width: 1.48593px; height: 1.48593px; left: 53.4087%; top: 42.2258%; --duration: 4.440880470502814s; --delay: 4.537427191677978s; --max-opacity: 0.6867023989290004;"></div>
      <div class="star" style="width: 1.80107px; height: 1.80107px; left: 27.1873%; top: 48.1251%; --duration: 2.8414229429769406s; --delay: 1.027405057096638s; --max-opacity: 0.4602623715871178;"></div>
      <div class="star" style="width: 1.76209px; height: 1.76209px; left: 18.3817%; top: 23.0244%; --duration: 2.859176027959669s; --delay: 1.4697577178358827s; --max-opacity: 0.6738690818621;"></div>
      <div class="star" style="width: 2.19264px; height: 2.19264px; left: 56.9646%; top: 46.5881%; --duration: 2.627675019379576s; --delay: 0.829759937915957s; --max-opacity: 0.3473778068474365;"></div>
      <div class="star" style="width: 1.75343px; height: 1.75343px; left: 69.2117%; top: 0.426646%; --duration: 4.283197721957184s; --delay: 2.8218315139043737s; --max-opacity: 0.4782968394503575;"></div>
      <div class="star" style="width: 2.94705px; height: 2.94705px; left: 72.3023%; top: 70.0674%; --duration: 3.5870021140402946s; --delay: 3.099828486658103s; --max-opacity: 0.7667602177197861;"></div>
      <div class="star" style="width: 1.14142px; height: 1.14142px; left: 23.0251%; top: 12.8829%; --duration: 4.307867361823535s; --delay: 1.574068846215329s; --max-opacity: 0.7677342428648104;"></div>
      <div class="star" style="width: 1.64219px; height: 1.64219px; left: 25.4091%; top: 64.7681%; --duration: 2.854743094864928s; --delay: 3.026101644374872s; --max-opacity: 0.4703305746513874;"></div>
    </div>
    <div id="ambient-glows"></div>
    <div id="cursor-glow" style="left: 0px; top: 0px;"></div><!-- PANTALLA DE INICIO -->
    <div id="start-screen" class="fixed inset-0 z-50 flex flex-col items-center justify-center transition-opacity duration-1000 ease-in-out px-4 text-center">
      <div class="relative w-32 h-32 md:w-40 md:h-40">
        <div class="absolute inset-0 rounded-full border border-t-[var(--light-pink)] border-r-[var(--purple-mid)] border-b-[var(--light-pink)] border-l-[var(--purple-mid)] animate-[spin_6s_linear_infinite] opacity-80"></div>
        <div class="absolute inset-3 rounded-full border border-white/10"></div>
        <div class="absolute inset-4 rounded-full overflow-hidden flex items-center justify-center bg-black">
          <button onclick="enterSite()" class="relative z-10 group flex flex-col items-center gap-6 md:gap-8 cursor-hover outline-none tap-highlight-transparent"><img src="https://xatimg.com/image/dCvg0UkAcsd1.gif?auto=format&amp;fit=crop&amp;w=400&amp;q=80" onerror="this.src='https://xatimg.com/image/dCvg0UkAcsd1.gif?auto=format&amp;fit=crop&amp;w=400&amp;q=80'" class="w-full h-full object-cover opacity-60 group-hover:opacity-100 transition-opacity duration-500 scale-110 group-hover:scale-100"></button>
          <div class="absolute inset-0 flex items-center justify-center bg-black/20 group-hover:bg-transparent transition-all"></div>
        </div>
      </div><button onclick="enterSite()" class="relative z-10 group flex flex-col items-center gap-6 md:gap-8 cursor-hover outline-none tap-highlight-transparent"><span class="text-lg md:text-xl font-serif italic text-[var(--light-pink)] tracking-[0.2em] uppercase border-b border-transparent group-hover:border-[var(--light-pink)] pb-2 transition-all duration-500">Entrar</span></button>
    </div><!-- CONTENIDO PRINCIPAL -->
    <main id="main-content" class="w-full max-w-4xl grid grid-cols-1 md:grid-cols-12 gap-4 md:gap-6 opacity-0 scale-95 transition-all duration-1000 ease-out filter blur-sm my-auto pb-6">
      <section class="md:col-span-5 flex flex-col gap-4 md:gap-6">
        <div class="glass-card rounded-3xl p-5 md:p-6 relative overflow-hidden group cursor-hover">
          <div class="absolute top-0 left-0 w-full h-[1px] bg-gradient-to-r from-transparent via-[var(--light-pink)] to-transparent opacity-50"></div>
          <div class="text-center mb-6 mt-2">
            <h1 class="text-4xl md:text-5xl font-bold py-2 title-glow select-none">
              ⸄ 𝓝𝓪𝓻𝓪 ๋ ࣭ ⭑
            </h1>
            <p class="text-[var(--light-pink)] text-[10px] md:text-xs tracking-[0.3em] uppercase mt-2 font-semibold border-b border-[var(--light-pink)]/20 pb-3 inline-block">
              Nara (252987266)
            </p>
          </div>
          <div class="relative w-full aspect-[4/5] rounded-2xl overflow-hidden mb-6 border border-[var(--light-pink)]/20 shadow-2xl">
            <img src="https://xatimg.com/image/dCvg0UkAcsd1.gif?auto=format&amp;fit=crop&amp;w=800&amp;q=80" onerror="this.src='https://xatimg.com/image/dCvg0UkAcsd1.gif?auto=format&amp;fit=crop&amp;w=800&amp;q=80'" class="w-full h-full object-cover transition-transform duration-1000 group-hover:scale-105">
            <div class="absolute bottom-3 right-3 bg-[var(--deep-purple)]/80 backdrop-blur-md px-2 py-1 rounded-full border border-[var(--light-pink)]/30 flex items-center gap-1.5">
              <span class="text-[9px] font-bold text-white uppercase tracking-wider">Online</span>
            </div>
          </div>
          <div class="flex justify-center pt-2 pb-4">
            <div class="group/stat text-center">
              <span id="like-counter" class="block text-4xl font-bold text-white drop-shadow-[0_0_10px_var(--light-pink)] transition-transform duration-200">143</span> <span class="text-[10px] text-gray-400 uppercase tracking-widest mt-1 block">Likes</span>
            </div>
          </div>
          <div class="flex gap-2 md:gap-3">
            <button id="like-btn" onclick="addLike(event)" class="cursor-hover flex-1 btn-luxury py-2.5 md:py-3 rounded-xl text-xs font-bold tracking-widest uppercase shadow-lg active:scale-95 transition-transform flex items-center justify-center gap-2 relative overflow-visible">Like</button> <a href="https://www.instagram.com" target="_blank" rel="noopener noreferrer" class="cursor-hover w-10 h-10 md:w-12 md:h-12 glass-card rounded-xl flex items-center justify-center hover:bg-[var(--light-pink)]/10 transition-colors border border-[var(--light-pink)]/20 group/btn"></a> <a href="https://www.tiktok.com" target="_blank" rel="noopener noreferrer" class="cursor-hover w-10 h-10 md:w-12 md:h-12 glass-card rounded-xl flex items-center justify-center hover:bg-[var(--light-pink)]/10 transition-colors border border-[var(--light-pink)]/20 group/btn"></a>
          </div>
        </div>
        <div class="glass-card rounded-3xl p-4 md:p-5 flex items-center gap-3 md:gap-4 cursor-hover border-l-2 border-l-[var(--light-pink)]">
          <div class="w-10 h-10 md:w-12 md:h-12 rounded-full overflow-hidden border border-[var(--light-pink)] p-0.5 shrink-0">
            <img src="https://xatimg.com/image/3Q617uMtIrf3.png?auto=format&amp;fit=crop&amp;w=200&amp;q=80" onerror="this.src='https://xatimg.com/image/3Q617uMtIrf3.png?auto=format&amp;fit=crop&amp;w=200&amp;q=80'" class="w-full h-full object-cover rounded-full">
          </div>
          <div>
            <h3 class="text-xs md:text-sm font-bold text-white flex items-center gap-2">
              Nara<img src="https://flagpedia.net/data/flags/emoji/twitter/256x256/br.png" srcset="https://flagpedia.net/data/flags/emoji/twitter/256x256/br.png 2x" width="16" height="12" alt="México" class="rounded-[2px] opacity-80">
            </h3>
            <p class="text-[10px] md:text-xs text-gray-400 mt-0.5">
              ~/: <span class="text-[var(--light-pink)]">Oque me Acalma</span> & <span> class="text-[#a47ba8]"> ^.^ Trás Paz</span>.
            </p>
          </div>
        </div>
      </section>
      <section class="md:col-span-7 flex flex-col gap-4 md:gap-6">
        <div class="glass-card rounded-3xl p-6 md:p-8 relative overflow-hidden">
          <div class="flex items-start justify-between mb-6 relative z-10">
            <div>
              <div class="flex items-center gap-2 mb-1">
                <h2 class="text-[10px] md:text-xs font-bold text-[var(--light-pink)] uppercase tracking-[0.2em]">
                  Playlist
                </h2>
              </div>
              <p class="text-xl md:text-2xl font-serif italic text-white/90">
                 Vibes Fofuxa
              </p>
            </div>
            <div class="flex items-end gap-1 h-6 md:h-8 paused" id="visualizer">
              <div class="equalizer-bar"></div>
              <div class="equalizer-bar"></div>
              <div class="equalizer-bar"></div>
              <div class="equalizer-bar"></div>
            </div>
          </div>
          <div class="flex flex-col sm:flex-row gap-6 items-center relative z-10 text-center sm:text-left">
            <div class="w-28 h-28 md:w-36 md:h-36 rounded-full p-1 border border-[var(--light-pink)]/30 relative flex-shrink-0 mx-auto sm:mx-0">
              <div class="w-full h-full rounded-full overflow-hidden relative">
                <img id="album-art" src="https://xatimg.com/image/3Q617uMtIrf3.png?auto=format&amp;fit=crop&amp;w=300&amp;q=80" class="w-full h-full object-cover animate-[spin_12s_linear_infinite]" style="animation-play-state: paused;">
                <div class="absolute inset-0 bg-black/10"></div>
                <div class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 w-4 h-4 bg-[#25092E] rounded-full border border-[var(--light-pink)]"></div>
              </div>
            </div>
            <div class="flex-1 w-full min-w-0">
              <div class="mb-4">
                <h3 id="song-title" class="text-2xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-white via-[var(--light-pink)] to-white truncate">
                  Stressed Out
                </h3>
                <p id="artist-name" class="text-xs text-[var(--purple-mid)] font-medium mt-1">
                  twenty one pilots
                </p>
              </div>
              <div class="w-full bg-[var(--deep-purple)]/50 rounded-full h-1 mb-2 cursor-pointer group tap-area border border-[var(--light-pink)]/10" id="progress-container">
                <div id="progress-bar" class="bg-gradient-to-r from-[var(--purple-mid)] to-[var(--light-pink)] h-1 rounded-full w-0 relative group-hover:h-1.5 transition-all duration-300"></div>
              </div>
              <div class="flex justify-between text-[9px] text-gray-400 mb-4 font-mono">
                <span id="current-time">0:00</span><span id="duration">0:00</span>
              </div>
              <div class="flex flex-col sm:flex-row items-center justify-between gap-4">
                <div class="flex items-center gap-6">
                  <button onclick="togglePlay()" class="cursor-hover w-12 h-12 rounded-full btn-luxury flex items-center justify-center text-[var(--light-pink)] shadow-xl hover:scale-105 active:scale-95 transition-transform"><i id="play-icon" class="fa-solid fa-play ml-1"></i></button>
                </div>
                <div class="flex items-center gap-3 w-full sm:w-auto justify-center mt-3 sm:mt-0">
                  <input type="range" id="volume-slider" min="0" max="1" step="0.1" value="0.7" class="w-24">
                </div>
              </div>
            </div>
          </div><audio id="audio-player" ontimeupdate="updateProgress()" onended="nextSong()" src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3"></audio>
        </div>
        <div class="glass-card rounded-2xl p-4 relative group/slider border border-[var(--light-pink)]/10">
          <div class="flex items-center justify-between mb-3 px-1">
            <h3 class="text-[10px] font-bold text-[var(--light-pink)] uppercase tracking-widest">
              Galería
            </h3>
          </div>
          <div id="gallery-slider" class="gallery-scroll flex overflow-x-auto gap-4 snap-x snap-mandatory scroll-smooth pb-2 -mx-2 px-2">
            <div class="gallery-item relative shrink-0 w-[85%] sm:w-[48%] aspect-[4/3] rounded-xl overflow-hidden cursor-pointer group/img">
              <img src="https://xatimg.com/image/p8kdi79qj3e5.jpg?auto=format&amp;fit=crop&amp;w=600&amp;q=80" onerror="this.src='https://images.unsplash.com/photo-1618331835717-801e976710b2?auto=format&amp;fit=crop&amp;w=600&amp;q=80'" class="w-full h-full object-cover transition-transform duration-700 group-hover/img:scale-110">
            </div>
            <div class="gallery-item relative shrink-0 w-[85%] sm:w-[48%] aspect-[4/3] rounded-xl overflow-hidden cursor-pointer group/img">
              <img src="https://xatimg.com/image/uFrdfATblYeM.jpg?auto=format&amp;fit=crop&amp;w=600&amp;q=80" onerror="this.src='https://images.unsplash.com/photo-1614613535308-eb5fbd3d2c17?auto=format&amp;fit=crop&amp;w=600&amp;q=80'" class="w-full h-full object-cover transition-transform duration-700 group-hover/img:scale-110">
            </div>
            <div class="gallery-item relative shrink-0 w-[85%] sm:w-[48%] aspect-[4/3] rounded-xl overflow-hidden cursor-pointer group/img">
              <img src="imagem?auto=format&amp;fit=crop&amp;w=600&amp;q=80" onerror="this.src='https://images.unsplash.com/photo-1550684848-fac1c5b4e853?auto=format&amp;fit=crop&amp;w=600&amp;q=80'" class="w-full h-full object-cover transition-transform duration-700 group-hover/img:scale-110">
            </div>
            <div class="gallery-item relative shrink-0 w-[85%] sm:w-[48%] aspect-[4/3] rounded-xl overflow-hidden cursor-pointer group/img">
              <img src="imagem?auto=format&amp;fit=crop&amp;w=600&amp;q=80" onerror="this.src='https://images.unsplash.com/photo-1517457210348-703079e57d4b?auto=format&amp;fit=crop&amp;w=600&amp;q=80'" class="w-full h-full object-cover transition-transform duration-700 group-hover/img:scale-110">
            </div>
            <div class="gallery-item relative shrink-0 w-[85%] sm:w-[48%] aspect-[4/3] rounded-xl overflow-hidden cursor-pointer group/img">
              <img src="https://xatimg.com/image/XpZfU3uRE3Qi.png?auto=format&amp;fit=crop&amp;w=600&amp;q=80" onerror="this.src='https://images.unsplash.com/photo-1500462918059-b1a0cb512f1d?auto=format&amp;fit=crop&amp;w=600&amp;q=80'" class="w-full h-full object-cover transition-transform duration-700 group-hover/img:scale-110">
            </div>
          </div>
        </div>
        <div class="glass-card rounded-2xl p-6 md:p-8 text-center border-t border-[var(--light-pink)]/20">
          <p class="font-serif italic text-gray-400 text-base md:text-lg leading-relaxed">
            "Dentro de Entre risos, silêncios e lembranças, a vida revela suas cores mais 
            intensas quando nos permitimos sentir de verdade.."<br>
            <span class="text-[var(--light-pink)] font-bold">Fofuxa.</span> (Nara)"
          </p>
          <div class="mt-4 flex justify-center gap-3"></div>
        </div>
      </section>
    </main>
    <footer class="w-full text-center py-4">
      <a href="https://xat.me/iiCarvaozim" target="_blank" class="text-[var(--light-pink)]/50 text-[13px] font-serif tracking-widest uppercase hover:text-[var(--light-pink)] hover:shadow-[0_0_10px_var(--light-pink)] hover:scale-105 transition-all duration-300 cursor-pointer inline-block p-1 rounded-lg">©₂₀₂₆ 𝘋𝘦𝘴𝘪𝘨𝘯 𝘪𝘪𝘊𝘢𝘳𝘷ã𝘰𝘻𝘪𝘮</a>
    </footer>
    <script>
        document.addEventListener('contextmenu', event => event.preventDefault());
        document.onkeydown = function(e) {
            if(e.keyCode == 123) return false; 
            if(e.ctrlKey && e.shiftKey && e.keyCode == 'I'.charCodeAt(0)) return false; 
            if(e.ctrlKey && e.shiftKey && e.keyCode == 'J'.charCodeAt(0)) return false; 
            if(e.ctrlKey && e.keyCode == 'U'.charCodeAt(0)) return false; 
        }

        let likes = 143; 
        let hasLiked = false; 

        function addLike(event) {
            if (hasLiked) return; 

            hasLiked = true;
            likes++;
            
            const counter = document.getElementById('like-counter');
            counter.innerText = likes.toLocaleString();
            
            counter.classList.add('pump-anim');
            setTimeout(() => counter.classList.remove('pump-anim'), 300);

            const btn = event.currentTarget;
            btn.style.opacity = '0.7';
            btn.style.cursor = 'default';

            const rect = btn.getBoundingClientRect();
            const centerX = rect.left + rect.width / 2;
            const centerY = rect.top + rect.height / 2;
            for(let i=0; i<15; i++) { createLikeParticle(centerX, centerY); }
        }

        function createLikeParticle(x, y) {
            const particle = document.createElement('i');
            particle.classList.add('fa-solid', 'fa-heart', 'like-particle');
            document.body.appendChild(particle);

            const angle = Math.random() * Math.PI * 2;
            const velocity = Math.random() * 80 + 40;
            const tx = Math.cos(angle) * velocity;
            const ty = Math.sin(angle) * velocity - 60;

            particle.style.left = `${x}px`;
            particle.style.top = `${y}px`;
            particle.style.setProperty('--tx', `${tx}px`);
            particle.style.setProperty('--ty', `${ty}px`);
            
            particle.style.color = Math.random() > 0.5 ? 'var(--light-pink)' : '#ffffff';
            const scale = Math.random() * 0.8 + 0.8;
            particle.style.fontSize = `${scale}rem`;

            particle.addEventListener('animationend', () => particle.remove());
        }

        function slideGallery(direction) {
            const slider = document.getElementById('gallery-slider');
            const scrollAmount = slider.clientWidth * 0.6;
            slider.scrollBy({ left: direction * scrollAmount, behavior: 'smooth' });
        }

        function createStars() {
            const container = document.getElementById('stars-container');
            // Optimización: Menos estrellas en móvil para rendimiento
            const isMobile = window.innerWidth < 768;
            const count = isMobile ? 30 : 80; 
            
            for (let i = 0; i < count; i++) {
                const star = document.createElement('div');
                star.classList.add('star');
                const size = Math.random() * 2 + 1;
                star.style.width = `${size}px`; star.style.height = `${size}px`;
                star.style.left = `${Math.random() * 100}%`; star.style.top = `${Math.random() * 100}%`;
                star.style.setProperty('--duration', `${Math.random() * 3 + 2}s`);
                star.style.setProperty('--delay', `${Math.random() * 5}s`);
                star.style.setProperty('--max-opacity', Math.random() * 0.7 + 0.3);
                if (Math.random() > 0.8) { star.style.backgroundColor = 'var(--light-pink)'; star.style.boxShadow = '0 0 6px var(--light-pink)'; }
                container.appendChild(star);
            }
        }
        createStars();

        const cursor = document.getElementById('cursor-glow');
        let mouseX = 0, mouseY = 0, cursorX = 0, cursorY = 0, lastParticleTime = 0;
        
        document.addEventListener('mousemove', (e) => { 
            // Optimización: No ejecutar lógica de cursor en dispositivos táctiles
            if (window.matchMedia("(hover: none)").matches) return;

            mouseX = e.clientX; 
            mouseY = e.clientY; 
            const now = Date.now(); 
            if (now - lastParticleTime > 30) { 
                createParticle(mouseX, mouseY); 
                lastParticleTime = now; 
            } 
        });
        
        function createParticle(x, y) {
            const particle = document.createElement('div');
            particle.classList.add('cursor-particle');
            document.body.appendChild(particle);
            const size = Math.random() * 3 + 2; 
            particle.style.width = `${size}px`; particle.style.height = `${size}px`;
            const colors = ['var(--light-pink)', 'var(--purple-mid)', '#ffffff'];
            particle.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
            particle.style.boxShadow = `0 0 ${size*2}px ${particle.style.backgroundColor}`;
            particle.style.left = `${x}px`; particle.style.top = `${y}px`;
            particle.style.setProperty('--tx', `${(Math.random()-0.5)*50}px`);
            particle.style.setProperty('--ty', `${(Math.random()-0.5)*50}px`);
            particle.addEventListener('animationend', () => particle.remove());
        }

        function animateCursor() {
            // Optimización: Detener bucle en móvil
            if (window.matchMedia("(hover: none)").matches) return;

            cursorX += (mouseX - cursorX) * 0.15; cursorY += (mouseY - cursorY) * 0.15;
            // Doble chequeo para asegurar que no se mueva el elemento oculto
            if(window.matchMedia("(hover: hover)").matches) { 
                cursor.style.left = cursorX+'px'; 
                cursor.style.top = cursorY+'px'; 
            }
            requestAnimationFrame(animateCursor);
        }
        animateCursor();

        const playlist = [
            { 
                title: "Stressed Out", artist: "twenty one pilots", 
                cover: "portada1.jpg", src: "cancion1.mp3" 
            }
        ];
        const onlineFallback = [
            { src: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-15.mp3", cover: "https://xatimg.com/image/AqZ2buQJOoBL.png?auto=format&fit=crop&w=300&q=80" }
        ];

        let currentSongIndex = 0, isPlaying = false;
        const audio = document.getElementById('audio-player'), playIcon = document.getElementById('play-icon'), albumArt = document.getElementById('album-art'), visualizer = document.getElementById('visualizer'), progressBar = document.getElementById('progress-bar'), volumeSlider = document.getElementById('volume-slider'), progressContainer = document.getElementById('progress-container');

        function enterSite() {
            document.getElementById('start-screen').style.opacity = '0';
            setTimeout(() => { document.getElementById('start-screen').style.display = 'none'; document.getElementById('main-content').classList.remove('opacity-0', 'scale-95', 'filter', 'blur-sm'); }, 600);
            playSong();
        }
        function loadSong(index) { 
            const song = playlist[index];
            document.getElementById('song-title').innerText = song.title; 
            document.getElementById('artist-name').innerText = song.artist; 
            
            albumArt.src = song.cover;
            albumArt.onerror = function() { this.src = onlineFallback[index].cover; };
            
            audio.src = song.src;
            audio.onerror = function() { this.src = onlineFallback[index].src; }; 
        }
        loadSong(0);

        function togglePlay() { isPlaying ? pauseSong() : playSong(); }
        function playSong() { isPlaying = true; audio.play().then(() => { playIcon.classList.replace('fa-play', 'fa-pause'); albumArt.style.animationPlayState = 'running'; visualizer.classList.remove('paused'); }).catch(() => isPlaying = false); }
        function pauseSong() { isPlaying = false; audio.pause(); playIcon.classList.replace('fa-pause', 'fa-play'); albumArt.style.animationPlayState = 'paused'; visualizer.classList.add('paused'); }
        function nextSong() { currentSongIndex = (currentSongIndex + 1) % playlist.length; loadSong(currentSongIndex); playSong(); }
        function prevSong() { currentSongIndex = (currentSongIndex - 1 + playlist.length) % playlist.length; loadSong(currentSongIndex); if(isPlaying) playSong(); }
        function updateProgress() { if(!isNaN(audio.duration)) { progressBar.style.width = `${(audio.currentTime/audio.duration)*100}%`; document.getElementById('current-time').innerText = fmt(audio.currentTime); document.getElementById('duration').innerText = fmt(audio.duration); } }
        function fmt(s) { return `${Math.floor(s/60)}:${Math.floor(s%60).toString().padStart(2,'0')}`; }
        progressContainer.addEventListener('click', (e) => { audio.currentTime = ((e.clientX - progressContainer.getBoundingClientRect().left) / progressContainer.getBoundingClientRect().width) * audio.duration; });
        volumeSlider.addEventListener('input', (e) => audio.volume = e.target.value);
    </script>
  </body>
</html>
