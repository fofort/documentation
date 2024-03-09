
reference 
https://developer.mozilla.org/en-US/docs/Web/API/Fullscreen_API

Code example : 

```
<html>
<head>
<script language='javascript'>
document.addEventListener(
  "keydown",
  (e) => {
    if (e.key === "Enter") {
      toggleFullScreen();
    }
  },
  false,
);
function toggleFullScreen() {
  if (!document.fullscreenElement) {
    document.documentElement.requestFullscreen();
  } else if (document.exitFullscreen) {
    document.exitFullscreen();
  }
}

</script>
</head>
<body>

</body>
</html>
```
