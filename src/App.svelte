<script>
	import * as THREE from 'three';
	import * as SC from 'svelte-cubed';

    let width = 1;
    let height = 1;
    let depth = 1;

    let spin = 0;

    let x = 0;
    let y = 0;
    let z = 0;

    let events = {
        'a': false,
        'd': false,
        'w': false,
        's': false,
    }

    window && window.addEventListener('keydown', (e) => {
        if (events[e.key] === false) { 
            events[e.key] = true; 
        }
        console.clear();
        console.log(events);
    });

    window && window.addEventListener('keyup', (e) => {
        if (events[e.key] === true) { 
            events[e.key] = false; 
        }
        console.clear();
        console.log(events);
    });

    SC.onFrame(() => {
        spin += 0.01;
    });
</script>

<SC.Canvas background={new THREE.Color('skyblue')} shadows fog={new THREE.FogExp2('skyblue', 0.04)}>
	<SC.Mesh
        geometry={new THREE.BoxGeometry()} 
        material={new THREE.MeshStandardMaterial({ color: 0xff3e00 })}
        scale={[width, height, depth]}
        position={[x, y, z]}
        rotation={[0, spin, 0]}
        castShadow
    />

    <SC.Group position={[0, -height / 2, 0]}>
        <SC.Mesh
            geometry={new THREE.PlaneGeometry(50,50)}
            material={new THREE.MeshStandardMaterial({color: 'burlywood'})}
            rotation={[Math.PI / 2, 0, 0]}
            position={[0, -0.001, 0]}
            receiveShadow
        />

        <SC.Primitive
            object={new THREE.GridHelper(100, 100, 0x444444, 0x555555)}
            position={[0, 0.001, 0]}
        />
    </SC.Group>

	<SC.PerspectiveCamera position={[1, 1, 3]} />

    <SC.OrbitControls />

    <SC.AmbientLight intensity={0.6} />
    <SC.DirectionalLight intensity={0.6} position={[-2, 3, 2]} shadow={{mapSize: [2048, 2048]}} />
</SC.Canvas>

<div class="controls">
    <label><input type="range" bind:value={width} min={0.1} max={3} step={0.1} />width</label>
    <label><input type="range" bind:value={height} min={0.1} max={3} step={0.1} />height</label>
    <label><input type="range" bind:value={depth} min={0.1} max={3} step={0.1} />depth</label>
</div>

<style>
    .controls {
        position: absolute;
        left: 1em;
        top: 1em;
    }

    label {
        display: flex;
        width: 60px;
        gap: 0.5em;
        align-items: center;
    }

    input {
        width: 80px;
        margin: 0;
    }
</style>