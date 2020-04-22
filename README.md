# CairoMakie

The Cairo Backend for Makie

[![Build Status](https://travis-ci.org/JuliaPlots/CairoMakie.jl.svg?branch=master)](https://travis-ci.org/JuliaPlots/CairoMakie.jl) ![](https://github.com/JuliaPlots/CairoMakie.jl/workflows/CI/badge.svg)

[![codecov](https://codecov.io/gh/JuliaPlots/CairoMakie.jl/branch/master/graph/badge.svg)](https://codecov.io/gh/JuliaPlots/CairoMakie.jl)

## Usage

To add CairoMakie to your environment, simply run the following in the REPL:
```julia
]add CairoMakie
```

If you are using CairoMakie and GLMakie together, you can use each backend's `activate!` function to switch between them.

## Limitations

As of now, CairoMakie only supports 2D scenes.  It is also noticeably slower than GLMakie.

## Using CairoMakie with Gtk.jl

You can render onto a GtkCanvas using Gtk, and use that as a display for your scenes.

```julia
using Gtk, CairoMakie, AbstractPlotting

canvas = @GtkCanvas()
window = GtkWindow(canvas, "Makie", 500, 500)

function drawonto(canvas, scene)
    @guarded draw(canvas) do _
       resize!(scene, Gtk.width(canvas), Gtk.height(canvas))
       screen = CairoMakie.CairoScreen(scene, Gtk.cairo_surface(canvas), getgc(canvas), nothing)
       CairoMakie.cairo_draw(screen, scene)
    end
end

scene = heatmap(rand(50, 50)) # or something

drawonto(canvas, scene)
show(canvas); # trigger rendering
```
