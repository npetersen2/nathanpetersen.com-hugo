---
title: "CirArrow: Circular Arrow Tool"
tags: ["web", "html", "tool", "arrow", "cad", "design"]
categories: ["Projects"]
description: "Tool which calculates points for defining circular arrows."
cover: "images/cover.jpg"
date: 2019-01-31 14:18:00
---

{{< rawhtml >}}

<style>

img.diagram {
  width: 30em;
  height: auto;
}

canvas {
  border: 1px solid #000;
  max-width: 100%;
  margin: 1em auto;
  display: block;
}

</style>

{{< /rawhtml >}}

Use this tool to calculate shape end-points when modeling circular arrows using CAD software.
The output of this tool is a simple set of points which define the requested parametrized circular arrows. Therefore, this tool should be useful for nearly every CAD program that supports elementary objects and shapes, such as lines and polygons.

_I thank my friend and roommate Sabrina Kabakov for her help in determining the mathematics behind this tool._

## Arrow Parameters

The following completely parametrize a circular arrow about some center point:

{{< rawhtml >}}

<ul>
<li>Center point &mdash; (<i>h</i>, <i>k</i>)</li>
<li>Radius &mdash; <i>r</i></li>
<li>Start angle &mdash; <i>&theta;</i></li>
<li>Tail sweep angle &mdash; <i>&Psi;<sub>t</sub></i></li>
<li>Head length &mdash; <i>d</i></li>
<li>Head width &mdash; <i>w</i></li>
</ul>

{{< /rawhtml >}}

{{< image
  src="images/cirarrow-diagram.png"
  caption="Diagram of parameters relating to circular arrow"
>}}

## Parameter Input

Use the form below to input parameters for a single arrow:

{{< rawhtml >}}

<div style="font-size: 75%">
<div class="form-group">
  <label>Center point &mdash; (<i>h</i>, <i>k</i>)</label>
  <div class="row">
    <div class="col">
     <input id="parm-h" class="form-control form-control-sm" type="text" placeholder="try 500" />
    </div>
    <div class="col">
      <input id="parm-k" class="form-control form-control-sm" type="text" placeholder="try 500" />
    </div>
  </div>
</div>

<div class="form-group">
  <label for="parm-r">Radius &mdash; <i>r</i></label>
  <input id="parm-r" class="form-control form-control-sm" type="text" placeholder="try 400" />
</div>

<div class="form-group">
  <label for="parm-theta">Start angle &mdash; <i>&theta;</i></label>
  <input id="parm-theta" class="form-control form-control-sm" type="text" placeholder="try 0" />
</div>

<div class="form-group">
  <label for="parm-psi-t">Tail sweep angle &mdash; <i>&Psi;<sub>t</sub></i></label>
  <input id="parm-psi-t" class="form-control form-control-sm" type="text" placeholder="try 45" />
</div>

<div class="form-group">
  <label for="parm-d">Head length &mdash; <i>d</i></label>
  <input id="parm-d" class="form-control form-control-sm" type="text" placeholder="try 10" />
</div>

<div class="form-group">
  <label for="parm-d">Head width &mdash; <i>w</i></label>
  <input id="parm-w" class="form-control form-control-sm" type="text" placeholder="try 10" />
</div>


<div class="form-group">
  <label>Arrow head?</label>

  <div class="form-check">
    <input id="parm-head-fwd" type="checkbox" checked />
    <label class="form-check-label" for="parm-head-fwd">Forward</label>
  </div>

  <div class="form-check">
    <input id="parm-head-bkwrd" type="checkbox" checked />
    <label class="form-check-label" for="parm-head-bkwrd">Backward</label>
  </div>
</div>

<div class="form-group">
  <label>Canvas size &mdash; (<i>width</i>, <i>height</i>)</label>
  <div class="row">
    <div class="col">
     <input id="canvas-x" class="form-control form-control-sm" type="text" placeholder="try 1000" />
    </div>
    <div class="col">
      <input id="canvas-y" class="form-control form-control-sm" type="text" placeholder="try 1000" />
    </div>
  </div>
</div>

</div>

<p><button id="submit" type="submit" class="btn btn-primary">Calculate arrow points...</button></p>



<div id="results" style="display:none;">

<h2 id="Calculated-Points">Calculated Points</h2>

<table id="output">
  <tr>
    <th>Parameters &mdash; <i>(h,k), r, &theta;, &Psi;<sub>t</sub>, d, w</i></th>
    <th>Points</th>
  </tr>
</table>

<h2 id="Visualization">Visualization of Arrows</h2>

<canvas id="canvas" width="0" height="0"></canvas>

</div>

<script>

var ARROWS = [];
var CANVAS_X = 0;
var CANVAS_Y = 0;


function toRadians(angle) {
  return angle * (Math.PI / 180);
}

function calcArrow() {
  // Gather input from form
  var h = document.getElementById("parm-h").value;
  var k = document.getElementById("parm-k").value;
  var r = document.getElementById("parm-r").value;
  var theta = document.getElementById("parm-theta").value;
  var psi_t = document.getElementById("parm-psi-t").value;
  var d = document.getElementById("parm-d").value;
  var w = document.getElementById("parm-w").value;

  var head_fwd = document.getElementById("parm-head-fwd").checked;
  var head_bkwrd = document.getElementById("parm-head-bkwrd").checked;

  CANVAS_X = document.getElementById("canvas-x").value;
  CANVAS_Y = document.getElementById("canvas-y").value;

  var invalid_input = (	h === "" ||
			k === "" ||
			r === "" ||
			theta === "" ||
			psi_t === "" ||
			d === "" ||
			w === "" ||
			CANVAS_X == 0 ||
			CANVAS_Y == 0
			);

  if (invalid_input) {
    alert("BAD INPUT!");
    return;
  }

  var canvas = document.getElementById("canvas");

  if (canvas.width == 0 || canvas.height == 0) {
    // Only do these actions once, at the beginning

    document.getElementById("canvas-x").disabled = true;
    document.getElementById("canvas-y").disabled = true;

    canvas.width = CANVAS_X;
    canvas.height = CANVAS_Y;

    document.getElementById("results").style.display = "block";

  }

  // Parse inputs as numbers
  h = Number(h);
  k = Number(k);
  r = Number(r);
  theta = Number(theta);
  psi_t = Number(psi_t);
  d = Number(d);
  w = Number(w);

  // Create vector to store points
  var x = [0,0,0,0,0,0,0,0];
  var y = [0,0,0,0,0,0,0,0];

  // Point 1
  x[0] = h + r * Math.cos(toRadians(theta));
  y[0] = k + r * Math.sin(toRadians(theta));

  // Point 2
  x[1] = h + r * Math.cos(toRadians(theta + psi_t));
  y[1] = k + r * Math.sin(toRadians(theta + psi_t));

  // Point 3...

  // Calculate point that is 0.000001 degree before point 2
  xx1 = h + r * Math.cos(toRadians(theta + psi_t - 0.000001));
  yy1 = k + r * Math.sin(toRadians(theta + psi_t - 0.000001));

  // Find slope between xx1 and x[1], yy1 and y[1];
  var slope1 = (y[1] - yy1) / (x[1] - xx1);

  var PSI1 = Math.atan(1 / slope1);

  var angle1 = (theta + psi_t) % 360;
  if (angle1 > 90 && angle1 < 270) {
    x[2] = x[1] - d * Math.sin(PSI1);
    y[2] = y[1] - d * Math.cos(PSI1);
  } else {
    x[2] = x[1] + d * Math.sin(PSI1);
    y[2] = y[1] + d * Math.cos(PSI1);
  }


  // Point 4
  x[3] = h + (r - w) * Math.cos(toRadians(theta + psi_t));
  y[3] = k + (r - w) * Math.sin(toRadians(theta + psi_t));

  // Point 5
  x[4] = h + (r + w) * Math.cos(toRadians(theta + psi_t));
  y[4] = k + (r + w) * Math.sin(toRadians(theta + psi_t));


  // Point 6
  // Calculate point that is 0.000001 degree after point 1
  xx5 = h + r * Math.cos(toRadians(theta + 0.000001));
  yy5 = k + r * Math.sin(toRadians(theta + 0.000001));

  // Find slope between xx5 and x[0], yy5 and y[0];
  var slope2 = (yy5 - y[0]) / (xx5 - x[0]);

  var PSI2 = Math.atan(1 / slope2);

  var angle2 = theta % 360;
  if (angle2 > 90 && angle2 < 270) {
    x[5] = x[0] + d * Math.sin(PSI2);
    y[5] = y[0] + d * Math.cos(PSI2);
  } else {
    x[5] = x[0] - d * Math.sin(PSI2);
    y[5] = y[0] - d * Math.cos(PSI2);
  }



  // Point 7
  x[6] = h + (r - w) * Math.cos(toRadians(theta));
  y[6] = k + (r - w) * Math.sin(toRadians(theta));

  // Point 8
  x[7] = h + (r + w) * Math.cos(toRadians(theta));
  y[7] = k + (r + w) * Math.sin(toRadians(theta));



  // Create list for output to table
  var ol = document.createElement("ol");

  var points = [];
  for (var i = 0; i < 8; i++) {
    var X = "x";
    var Y = "y";
    var point = {};
    point[X] = parseFloat(x[i].toFixed(5));
    point[Y] = parseFloat(y[i].toFixed(5));

    var li = document.createElement("li");
    var t = document.createTextNode("(" + point[X] + ", " + point[Y] + ")");
    li.appendChild(t);
    ol.appendChild(li);

    points.push(point);
  }

  ARROWS.push(points);

  // Append results to output table
  var table = document.getElementById("output");
  var row = table.insertRow(-1);
  var cell1 = row.insertCell(0);
  var cell2 = row.insertCell(1);
  cell1.innerHTML = "(" + h + "," + k + "), " + r + ", " + theta + ", " + psi_t + ", " + d + ", " + w;
  cell2.appendChild(ol);

  console.log(ARROWS);

  drawArrow(h, k, r, theta, psi_t, d, w, points, head_fwd, head_bkwrd);
}

function drawArrow(h, k, r, theta, psi_t, d, w, points, head_fwd, head_bkwrd) {
  var canvas = document.getElementById("canvas");
  var ctx = canvas.getContext("2d");

  // Draw tail
  ctx.beginPath();
  ctx.arc(h, CANVAS_Y - k, r, -toRadians(theta), -toRadians(theta + psi_t), true);
  ctx.stroke();
  ctx.closePath();

  // Draw head fwd
  if (head_fwd) { 
    ctx.beginPath();
    ctx.moveTo(points[2]["x"], CANVAS_Y - points[2]["y"]);
    ctx.lineTo(points[3]["x"], CANVAS_Y - points[3]["y"]);
    ctx.lineTo(points[4]["x"], CANVAS_Y - points[4]["y"]);
    ctx.lineTo(points[2]["x"], CANVAS_Y - points[2]["y"]);
    ctx.stroke();
    ctx.closePath();

    // Draw all the points as little circles
    for (var i = 2; i <= 4; i++) {
      ctx.beginPath();
      ctx.arc(points[i]["x"], CANVAS_Y - points[i]["y"], 2, 0, 2 * Math.PI, true);
      ctx.fill();
      ctx.closePath();
    }
  }

  // Draw head bkwrd
  if (head_bkwrd) {
    ctx.beginPath();
    ctx.moveTo(points[5]["x"], CANVAS_Y - points[5]["y"]);
    ctx.lineTo(points[6]["x"], CANVAS_Y - points[6]["y"]);
    ctx.lineTo(points[7]["x"], CANVAS_Y - points[7]["y"]);
    ctx.lineTo(points[5]["x"], CANVAS_Y - points[5]["y"]);
    ctx.stroke();
    ctx.closePath();

    // Draw all the points as little circles
    for (var i = 5; i <= 7; i++) {
      ctx.beginPath();
      ctx.arc(points[i]["x"], CANVAS_Y - points[i]["y"], 2, 0, 2 * Math.PI, true);
      ctx.fill();
      ctx.closePath();
    }
  }

  // Draw all the points as little circles
  for (var i = 0; i <= 1; i++) {
    ctx.beginPath();
    ctx.arc(points[i]["x"], CANVAS_Y - points[i]["y"], 2, 0, 2 * Math.PI, true);
    ctx.fill();
    ctx.closePath();
  }
}


document.getElementById("submit").addEventListener("click", calcArrow);

</script>

{{< /rawhtml >}}