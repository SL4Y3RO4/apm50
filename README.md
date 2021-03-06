<h1>APM50</h1>

<canvas id="canvas" width="720" height="300"></canvas>

<p>
  Noob: 0 - 200 <br>
  Average: 200 - 300 <br>
  Decent: 300 - 350 <br>
  Good: 350 - 400 <br>
  Fast: 400 - 500 <br>
  Pro: 500 - 715 <br>
</p>

<script>
  function getRndInteger(min, max) {
    return Math.floor(Math.random() * (max - min) ) + min;
  }

  const canvas = document.getElementById('canvas');
  const ctx = canvas.getContext('2d');
      
  function drawCircles(arr) {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    if (arr.length == 0) {
      score.end = new Date().getTime();
      ctx.font = "30px Arial";
      ctx.fillStyle = "black";
      ctx.textAlign = "center";
      ctx.textBaseline = 'middle';
      var time = (score.end - score.start);
      var apm = Math.ceil(50 / time * 1e3 * 60 * 1.8)
      ctx.fillText('Time: ' + time/1000 + '\nAPM: ' + apm, canvas.width / 2, canvas.height / 2); 
    }
    
    arr.slice().reverse().forEach(circle => {
      ctx.beginPath();
      ctx.arc(circle.x, circle.y, radius, 0, 2 * Math.PI, false);
      ctx.fillStyle = '#00f2d7';
      ctx.fill();

      ctx.beginPath();
      ctx.arc(circle.x, circle.y, radius - border, 0, 2 * Math.PI, false);
      ctx.fillStyle = '#666666';
      ctx.fill();

      ctx.font = "20px Arial";
      ctx.fillStyle = "white";
      ctx.textAlign = "center";
      ctx.textBaseline = 'middle';
      ctx.fillText(circle.n , circle.x, circle.y); 
    });
  }

  function spawnCircle(numbers) {
    if (numbers.pos > 0) {
      var circle = {
        x : getRndInteger(radius, canvas.width - radius), 
        y : getRndInteger(radius, canvas.height - radius),
        n : numbers.pos
      };
      numbers.arr.push(circle);
      numbers.pos -= 1;
    }
  }

  const radius = 25;
  const border = 2;
  const num_circles = 3;
  var numbers = {
    arr: [],
    pos: 50
  };

  var score = {
    active: false,
    start: null,
    end: null
  }

  for (i = 0; i < num_circles; i++) {
    spawnCircle(numbers);
  }

  drawCircles(numbers.arr);

  function getMousePos(canvas, evt) {
    var rect = canvas.getBoundingClientRect();
    return {
      x: evt.clientX - rect.left,
      y: evt.clientY - rect.top
    };
  }
  
  canvas.addEventListener('mousedown', (e) => {
    const mouse_pos = getMousePos(canvas, e);
    if (Math.abs(mouse_pos.x - numbers.arr[0].x) <= radius && Math.abs(mouse_pos.y - numbers.arr[0].y) <= radius) {
      if (!score.active) {
        score.active = true;
        score.start = new Date().getTime();
      }
      numbers.arr.shift();
      spawnCircle(numbers);
      drawCircles(numbers.arr);
    }
  });
</script>

<style>
  canvas {
    padding-left: 0;
    padding-right: 0;
    margin-left: auto;
    margin-right: auto;
    display: block;
  }
</style>
