# plotToPrint
A workflow for converting ggplot visualizations to 3D print files.

## Generate plot using dark mode

```r
## Code to produce dark themed GGplot visualizations
dark_theme_gray() +
  theme(
        axis.text.y=element_blank(),
        axis.text.x=element_blank(),
        axis.ticks.x=element_blank(),
        axis.ticks.y=element_blank(),
        panel.grid.major = element_blank(), 
        panel.grid.minor = element_blank()
  )
```

When creating data matrices, elevation is processed used dark to light. If you do not indicate lower parts of the visualization is darker colors, they will be processed incorrectly.

Furthermore, most color schemes in the Viridis package produce awkward indentation on the .stl file. To remedy this, I use the green-yellow theme that fits within the standard processing of raster data into elevation data.

```r
## Implementing correct color theme for GGplot raster
scale_fill_continuous_sequential(palette="Green-Yellow", na.value="lightgrey")
```

## Converting visualization to raster image

The visualizations must first be converted to raster images. To do this, use this code:

```r
## Converting vis to PNG format
plot(yourPlot)

dev.copy(png,'yourPlot.png')
dev.off()
```

## Converting raster image to height matrix

Raster image is converted to data matrix that can be used to generate 3D print file using:

```r
## Converting raster image to data matrix
yourPlot_Raster <- raster_to_matrix('yourPlot.png')
```

## Converting matrix to STL file

The STL file is generated from the raster matrix:

```r
## generating an STL file from matrix DF
yourPlot_Raster %>%
  sphere_shade() %>%
  plot_3d(yourPlot_Raster,zscale=50)
save_3dprint("yourPlot.stl", maxwidth = 150, unit = "mm")
```