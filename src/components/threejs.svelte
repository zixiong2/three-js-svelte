<script>
    import * as THREE from 'three';
    import data from "../data/data.json";
    import { DragControls } from '../../node_modules/three/examples/jsm/controls/DragControls';
    import { OrbitControls } from '../../node_modules/three/examples/jsm/controls/OrbitControls';
    import { TrackballControls } from '../../node_modules/three/examples/jsm/controls/TrackballControls';
    
    
    function randomColorHex() {
    return '#' + Math.floor(Math.random() * 16777215).toString(16);
    }

    const prepareMaterial = (data) => {
        const materialPool = {};
        const groups = new Set(data.nodes.map((it) => it.group));
        groups.forEach((it) => {
            materialPool[it] = new THREE.MeshBasicMaterial({
                color: randomColorHex(),
            });
        })

        console.log(materialPool);
        return materialPool;
    }


    //export const main = () => {
    const scene = new THREE.Scene();

    const renderer = new THREE.WebGLRenderer();
    renderer.setSize( window.innerWidth, window.innerHeight );
    document.body.appendChild( renderer.domElement );

    scene.background = new THREE.Color(0x1f1f1f);
    const camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );


    const controls = new OrbitControls( camera, renderer.domElement );

    const materialPool = prepareMaterial(data);

    const geometry = new THREE.SphereGeometry(.5);

    const spherePool = {};

    for (const node of data.nodes) {
        const sphere = new THREE.Mesh(geometry, materialPool[node.group]);
        sphere.position.x = Math.random() * 100 - 50;
        sphere.position.y = Math.random() * 100 - 50;
        sphere.position.z = Math.random() * 100 - 50;
        scene.add(sphere);
        // sphere.lineAsSource = [];
        // sphere.lineAsTarget= [];
        sphere.userData = {
            'id': node.id,
            'lineAsSource': [],
            'lineAsTarget': []
        }
        spherePool[node.id] = sphere;
    }

    const dragControl = new DragControls(Object.values(spherePool), camera, renderer.domElement);

    const lineMaterial = new THREE.LineBasicMaterial( { color: 0xffffff } );

    for (const link of data.links) {
        const sphere1 = spherePool[link.source];
        const sphere2 = spherePool[link.target];
        const lineGeometry = new THREE.BufferGeometry();
        lineMaterial.opacity = link.value * 0.1;
        lineGeometry.setFromPoints([sphere1.position, sphere2.position]);
        const line = new THREE.Line(lineGeometry, lineMaterial);
        sphere1.userData.lineAsSource.push([line, sphere2]);
        sphere2.userData.lineAsTarget.push([line, sphere1]);

        scene.add(line);
    }

    camera.position.z = 5;



    dragControl.addEventListener('dragstart', (e) => {
        controls.enabled = false;
    });

    dragControl.addEventListener('drag', (e) => {
        for (const [line, sphere] of e.object.userData.lineAsSource) {
            line.geometry.setFromPoints([e.object.position, sphere.position]);
        }

        for (const [line, sphere] of e.object.userData.lineAsTarget) {
            line.geometry.setFromPoints([sphere.position, e.object.position]);
        }
    });


    dragControl.addEventListener('dragend', (e) => {
        controls.enabled = true;
    });


    function animate() {
        requestAnimationFrame( animate );
        renderer.render( scene, camera );
    };

    animate();

    //}

</script>


