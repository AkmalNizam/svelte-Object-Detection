<script>
	import { onMount } from 'svelte'
	import * as p5 from 'p5'
	import * as ml5 from 'ml5'
  
	let canvas;
	let video;
	let detector;
	let detections = [];
  
	onMount(() => {
	  new p5((p) => {
		p.setup = () => {
		  canvas = p.createCanvas(940, 480);
		  video = p.createCapture(p.VIDEO);
		  video.size(640, 480);
  
		  // Wait for the 'loadeddata' event before setting up the video
		  video.elt.addEventListener('loadeddata', () => {
			video.hide();
  
			// Initialize the object detector after the video has loaded
			detector = ml5.objectDetector("cocossd", modelIfReady);
		  });
		};
  
		p.draw = () => {
		  p.image(video, 0, 0);
		  for (let i = 0; i < detections.length; i++) {
			let object = detections[i];
			p.stroke(0, 255, 0);
			p.strokeWeight(4);
			p.noFill();
			p.rect(object.x, object.y, object.width, object.height);
			p.noStroke();
			p.fill(255);
			p.textSize(24);
			p.text(object.label, object.x + 10, object.y + 24);
		  }
		};
  
		function modelIfReady() {
		  detector.detect(video, yourDetections);
		}
  
		function yourDetections(error, results) {
		  if (error) {
			console.log(error);
		  }
		  detections = results;
		  detector.detect(video, yourDetections);
		}
	  }, canvas);
	});
  </script>

  <h1>test video detections</h1>
  