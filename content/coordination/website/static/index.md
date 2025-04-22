In the static folder, all files that should be globally accessible to the content files can be placed.
Currently, this only contains images. 
If a file is specifically used for a single content file, it should be bundled with the page, rather than placed in static. 
If is a more general purpose file (like R-ladies logo etc) to be used in multiple files, it is best to place it in static and refer to it by its relative path to the base-url of the page.

```
/images/logo.png  # Looks for the image as placed in static/images/logo.png
images/logo.png   # Looks for the image as relative to the content index.md file
```