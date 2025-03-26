# All About Robots

Oh wow! Now you can learn all about robots, with our latest web service, All About Robots!!

-----

In `robots.txt`:

```
User-agent: *
Disallow: /open_the_pod_bay_doors_hal_and_give_me_the_flag.html
```

![[Pasted image 20240524113223.png]]

```
flag{3f19b983c1de42bd49af1a237d7e57b9}
```

```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Celebration</title>
<link href="[https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css](view-source:https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css)" rel="stylesheet">
<script src="[https://cdn.jsdelivr.net/npm/canvas-confetti](view-source:https://cdn.jsdelivr.net/npm/canvas-confetti)"></script>
<style>
  html, body {
    height: 100%;
    margin: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    background: #f8f9fa;
    position: relative;
    overflow: hidden;
  }
  canvas {
    position: absolute;
    top: 0;
    left: 0;
    pointer-events: none; /* Ensures click events pass through to elements below */
    z-index: -1; /* Lower z-index so it's behind other content */
  }
  .celebration-text {
    font-size: 2rem;
    font-weight: bold;
    color: #343a40;
    position: relative; /* Ensures it is positioned relative to allow z-index */
    z-index: 2; /* Higher z-index to be above the confetti */
    padding: 20px; /* Optional: for better visibility */
    background-color: rgba(255, 255, 255, 0.85); /* Semi-transparent white background */
    border-radius: 10px; /* Rounded corners for aesthetic */
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Soft shadow for 3D effect */
  }
</style>

</head>
<body>
<div class="celebration-text"><code>flag{3f19b983c1de42bd49af1a237d7e57b9}</code></div>

<script>
  function fireConfetti() {
    var end = Date.now() + (5 * 1000); // Duration of the confetti effect
    var colors = ['#bb0000', '#ffffff', '#3333ff', '#ff6600', '#00ff00', '#00c3ff'];
    (function frame() {
      // Configuration for confetti streams from various origins
      confetti({
        particleCount: 7,
        angle: 60,
        spread: 100,
        origin: { x: 0 },
        colors: colors
      });
      confetti({
        particleCount: 7,
        angle: 120,
        spread: 100,
        origin: { x: 1 },
        colors: colors
      });
      confetti({
        particleCount: 7,
        angle: 90,
        spread: 100,
        origin: { x: 0.5, y: 0 },
        colors: colors
      });
      confetti({
        particleCount: 7,
        angle: 90,
        spread: 100,
        origin: { x: 0.1, y: 0 },
        colors: colors
      });
      confetti({
        particleCount: 7,
        angle: 90,
        spread: 100,
        origin: { x: 0.9, y: 0 },
        colors: colors
      });
      // Repeat if the end time has not been reached
      if (Date.now() < end) {
        requestAnimationFrame(frame);
      }
    }());
  }
  fireConfetti();
</script>
</body>
</html>
```