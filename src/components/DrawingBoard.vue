<template>
  <div class="DrawingBoard">
    <h1>Polygon drawing board</h1>
  </div>
</template>

<script>
const THREE = require('three')
export default {
  name: 'DrawingBoard',
  data () {
    return{
      points : [],
      firstPoint : [],
      index : 2,
      radius : window.innerHeight/1.39807692308,
      theta : 0.2,
      polygonColor : Math.random() * 0x42b983 + 0x42b983,
      renderer : new THREE.WebGLRenderer(),
      scene : new THREE.Scene(),
      camera : new THREE.PerspectiveCamera( 70, window.innerWidth/window.innerHeight , 1, 10000 )
    }
  },
  created : function () {   
    this.createCanvas();
  },
  methods: {
    createCanvas: function(){
      this.renderer.setClearColor(0xf9f9f9);
      this.renderer.setSize( window.innerWidth, window.innerHeight);
      document.body.appendChild( this.renderer.domElement );

      document.addEventListener('mousedown', this.onMouseDown, false);
      this.renderer.render( this.scene, this.camera );
    },
    onMouseDown : function(event) {
      event.preventDefault();
      //set point's material and color,
      // material had to be declared here to make different color for each point
      var geometry = new THREE.CircleGeometry( 5, 32);
      var material = new THREE.MeshBasicMaterial( { color: this.polygonColor} );
      
      // Create Point
      var circle = new THREE.Mesh( geometry, material );
      var mouseX = (event.clientX - window.innerWidth/2) ;
      mouseX = mouseX + mouseX/200;
      var mouseY = (event.clientY - window.innerHeight/2);
      circle.position.x = mouseX;
      circle.position.y = -mouseY;
      circle.scale.x = circle.scale.y = 1.5;
      // save point
      this.points.push(circle.position.x,circle.position.y);
      // add point to scene
      this.scene.add( circle );

      //save first point
      if (this.points.length == 2) {
      this.firstPoint.push(circle.position.x);
      this.firstPoint.push(circle.position.y);
      }
    
      if (this.points.length > 2) {
      var geometry2 = new THREE.Geometry();
      var material2 = new THREE.LineBasicMaterial( { color: this.polygonColor, linewidth: 5, linecap: 'round', linejoin:  'round'});
        geometry2.vertices.push(
        new THREE.Vector3( this.points[this.index], this.points[this.index+1]),
        new THREE.Vector3( this.points[this.index-2],  this.points[this.index-1]),
      );
      var line = new THREE.Line( geometry2, material2 );
      this.scene.add( line );
      this.index+=2;

        if (((this.firstPoint[0] - mouseX > -12) && (this.firstPoint[0] - mouseX < 12))&&((this.firstPoint[1] + mouseY > -12) && (this.firstPoint[1] + mouseY < 12))){
          this.points = []; 
          this.firstPoint = [];
          this.index = 2;
          this.polygonColor = Math.random() * 0x42b983 + 0x42b983;
      }

    }

      //this.theta += 0.1;
      this.camera.position.x = this.radius * Math.sin( THREE.Math.degToRad( this.theta ) );
			this.camera.position.y = this.radius * Math.sin( THREE.Math.degToRad( this.theta ) );
			this.camera.position.z = this.radius * Math.cos( THREE.Math.degToRad( this.theta ) );
      this.camera.lookAt( this.scene.position );
      this.renderer.render( this.scene, this.camera );
    },
    
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.DrawingBoard{
  height: 100px;
  position: absolute;
  background: transparent !important;
  width: 100%;
}
h1{
  color: #42b983;
  font-family: sans-serif;
  font-weight: 400;
  text-transform: uppercase;
  margin: 0;
  padding: 20px 0 0;
  text-shadow: 1.5px 1.5px 8px #13c763;
}
canvas{
  background: #f1f1f1;
  bottom: 0;
}
</style>