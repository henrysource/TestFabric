<template>
	<div id="workspace" class="workspace">
		<div class="top">
			<canvas id="canvas"></canvas>
		</div>
		<div class="controls">
			<button @click.prevent="toSvg">toSvg</button>
			<input type="text" v-model="zoom" @change="updateZoom">
			<button @click.prevent="zoomIn">Zoom In</button>
			<button @click.prevent="zoomOut">Zoom Out</button
		</div>
	</div>
</template>


<script>
	export default {
		data() {
			return {
				canvas: null,
				zoom: 1
			}
		},
		
		ready() {
			this.initCanvas();
		},

		methods: {

			initCanvas() {

				var vm = this;
				var wWidth = document.getElementById('workspace').offsetWidth;
				var wheight = document.getElementById('workspace').offsetHeight;

				this.canvas = new fabric.Canvas('canvas');
				//this.canvas = new fabric.CanvasWithViewport("canvas");
				this.canvas.setWidth(wWidth - 200);
				this.canvas.setHeight(wheight - 200);
				//this.canvas.setZoom(this.canvas.viewport.zoom*this.zoom);

				var rect = new fabric.Rect({
			      left: 150,
			      top: 200,
			      originX: 'left',
			      originY: 'top',
			      width: 150,
			      height: 120,
			      angle: -10,
			      fill: 'rgba(255,0,0,0.5)',
			      transparentCorners: false
			    });



				var scaleFactor=1;
				fabric.Image.fromURL('http://www.tutorialsscripts.com/free-icons/clip-art-icons/teal-clip-art-icon-128-x-128.png', function(img) {
				    var img1 = img.scale(scaleFactor).set({ left: 0, top: 0 });
				    var text = new fabric.Text('the_text_sample\nand more', {
				                fontFamily: 'Arial',
				                fontSize:30,
				            });
				            //text.set("top",img.height*scaleFactor/2+text.getBoundingRectHeight()/2);
				            //text.set("left",-img.width*scaleFactor/2+text.getBoundingRectWidth()/2);          
				    var group = new fabric.Group([ img1,text ], { left: 200, top: 200 });
				    vm.canvas.add(group);
				});
				
				this.canvas.add(rect);

				this.watchCanvas();
			},

			watchCanvas() {
				var vm = this;
				var c = vm.canvas;
				 c.on('object:selected', function(options){
				 	console.log(options.target.item(1));
                    // switch(options.target.type){
                    //     case 'text': vm.handleTextObj(options.target, true); 
                    //     case 'image': vm.handleImageObj(options.target);
                    // }                  
                });
			},


			updateZoom(){
				console.log('update zoom');
				console.log(this.zoom);
				this.canvas.setZoom(this.canvas.viewport.zoom*this.zoom);
			},

			// zoomIn(){
			// 	alert(this.canvas.viewport.zoom);
			// 	this.canvas.setZoom(this.canvas.viewport.zoom*1.1);
			// },

			// zoomOut(){
			// 	this.canvas.setZoom(this.canvas.viewport.zoom/1.1);

			// },

			zoomIn(){
				SCALE_FACTOR = 1.1;
	    		canvasScale = 1 * SCALE_FACTOR;
    
			    this.canvas.setHeight(this.canvas.getHeight() * SCALE_FACTOR);
			    this.canvas.setWidth(this.canvas.getWidth() * SCALE_FACTOR);
			    
			    var objects = this.canvas.getObjects();
			    for (var i in objects) {
			        var scaleX = objects[i].scaleX;
			        var scaleY = objects[i].scaleY;
			        var left = objects[i].left;
			        var top = objects[i].top;
			        
			        var tempScaleX = scaleX * SCALE_FACTOR;
			        var tempScaleY = scaleY * SCALE_FACTOR;
			        var tempLeft = left * SCALE_FACTOR;
			        var tempTop = top * SCALE_FACTOR;
			        
			        objects[i].scaleX = tempScaleX;
			        objects[i].scaleY = tempScaleY;
			        objects[i].left = tempLeft;
			        objects[i].top = tempTop;
			        
			        objects[i].setCoords();
			    }
			    
			    this.canvas.renderAll();        
			},

			zoomOut(){

				SCALE_FACTOR = 1.1;
	    		canvasScale = 1 * SCALE_FACTOR;

			    this.canvas.setHeight(this.canvas.getHeight() * (1 / SCALE_FACTOR));
    			this.canvas.setWidth(this.canvas.getWidth() * (1 / SCALE_FACTOR));
    
			    var objects = this.canvas.getObjects();
			    for (var i in objects) {
			        var scaleX = objects[i].scaleX;
			        var scaleY = objects[i].scaleY;
			        var left = objects[i].left;
			        var top = objects[i].top;
			    
			        var tempScaleX = scaleX * (1 / SCALE_FACTOR);
			        var tempScaleY = scaleY * (1 / SCALE_FACTOR);
			        var tempLeft = left * (1 / SCALE_FACTOR);
			        var tempTop = top * (1 / SCALE_FACTOR);

			        objects[i].scaleX = tempScaleX;
			        objects[i].scaleY = tempScaleY;
			        objects[i].left = tempLeft;
			        objects[i].top = tempTop;

			        objects[i].setCoords();
			    }
			    
			    this.canvas.renderAll();      
			},

			toSvg() {
				console.log(this.canvas.toSVG());
			}
		}
	}
</script>


<style>
	
	.workspace{
		
		width: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
	}

	#canvas{
		border: 1px solid #000;
	}
</style>