# PlutoHTML.jl

Convert Pluto notebooks to pure HTML (without Javascript).

The functionality provided by this package is quite simple.
It would probably be better to move this into Pluto.jl at some point if the maintainers agree.
For now, I'm just trying this out a bit to see how well it works.

An example output from a Pluto notebook is visible at <https://rikhuijzer.github.io/PlutoHTML.jl/dev/>.

## Documenter.jl

To see how to embed output from a Pluto notebook in Documenter.jl, checkout "make.jl" in this repository.

## Franklin.jl

In `utils.jl` define:

    """
        lx_pluto(com, _)

    Embed a Pluto notebook via:
    https://github.com/rikhuijzer/PlutoHTML.jl
    """
    function lx_pluto(com, _)
        file = string(Franklin.content(com.braces[1]))::String
        path = joinpath("posts", "notebooks", "$file.jl")
        return """
            ```julia:pluto
            # hideall

            using PlutoHTML: notebook2html

            path = "$path"
            @assert isfile(path)
            println("→ evaluating Pluto notebook at (\$path)")
            html = notebook2html(path)
            println("~~~\n\$html\n~~~\n")
            ```
            \\textoutput{pluto}
            """
    end

Next, the Pluto notebook at "/posts/notebooks/analysis.jl" can be included in a Franklin webpage via:

```
\pluto{analysis}
```

