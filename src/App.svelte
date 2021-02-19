<script>
  import { onMount } from 'svelte';
  
  import * as THREE from 'three';
  import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';
  import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
  import { TrackballControls } from 'three/examples/jsm/controls/TrackballControls.js';
  import { FlyControls } from 'three/examples/jsm/controls/FlyControls.js';
  
  let camera, scene, renderer;
  let geometry, material, mesh;
  let controls;
  
  const onWindowResize = (ev) => {
    // console.log('resize', ev, window.innerWidth, window.innerHeight)
    renderer.setSize(window.innerWidth, window.innerHeight)
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
  };

    onMount( async () => {
      init();
    });

  function init() {
    scene = new THREE.Scene();
    camera = new THREE.PerspectiveCamera(60, 1, 1, 1000);
    camera.position.set(5, 5, 5);
    // camera.position.set(0, 0, 1);
    //camera.position.set(100, 0, 500);
    renderer = new THREE.WebGLRenderer({
      antialias: true
    });
    renderer.setClearColor(0x808080);
    renderer.setSize(window.innerWidth, window.innerHeight)
    var canvas = renderer.domElement
    document.body.appendChild(canvas);
    
    // var controls = new OrbitControls(camera, renderer.domElement);
    controls = new TrackballControls(camera, renderer.domElement);
    controls.rotateSpeed = 10.0;
    controls.zoomSpeed = 1.2;
    controls.panSpeed = 0.8;

    /*
                var controls = new FlyControls( camera, renderer.domElement );
                //camera control properties
                controls.movementSpeed = 1;
                controls.domElement = renderer.domElement;
                controls.rollSpeed = 0.01;
                controls.autoForward = false;
                controls.dragToLook = true;
     */
    var light = new THREE.HemisphereLight( 0xf0f080, 0x080820, 1 );
    scene.add( light );

    var loader = new GLTFLoader();
    // var loader = new THREE.GLTFLoader();

    // loader.load( 'https://threejs.org/examples/models/gltf/Horse.glb', function ( gltf ) {
    loader.load( './freecad.glb', function ( gltf ) {
      // loader.load( './Duck.glb', function ( gltf ) {
      console.log(gltf);
      scene.add( gltf.scene );

    }, undefined, function ( error ) {

      console.error( error );

    } );




    render();

    function render() {
      if (resize(renderer)) {
        camera.aspect = canvas.clientWidth / canvas.clientHeight;
        camera.updateProjectionMatrix();
      }
      renderer.render(scene, camera);
      requestAnimationFrame(render);
    }

    function resize(renderer) {
      const canvas = renderer.domElement;
      const width = canvas.clientWidth;
      const height = canvas.clientHeight;
      const needResize = canvas.width !== width || canvas.height !== height;
      if (needResize) {
        renderer.setSize(width, height, false);
      }
      return needResize;
    }
    window.addEventListener('resize', onWindowResize);
    animate();
    function animate() {
      requestAnimationFrame( animate );
      controls.update();
      render();
    }

    function render() {
      renderer.render( scene, camera );
    }
  }

</script>
<!--
<canvas id="canvas"></canvas>
-->

