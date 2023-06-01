#julia


Jupyter Notebook で Julia が使えるらしい。

```bash
$ julia
> ENV["JUPYTER"]=Sys.which("jupyter")
> using Pkg; Pkg.add("IJulia")
...
> using IJulia; notebook()
```

![](image-knduscaj.png)

ラズパイでカメラライブラリあるらしい。

早速、julia で new note

![](image-kndusrwe.png)

エラーのときはこうすると。

![](image-kndut0ya.png)

```bash
function cnt(z, c)
    k::UInt8 = 0
    while(k < 255)
        z = z^2 + c
        abs2(z) > 4. && return k
        k += 1
    end
    return k
end
```

```bash
function jlset(M==Int, N==Int, c)
    grid = Array{UInt8}(undef, M, N)
    xs = range(-2, 2, length = N)
    ys = range(-2, 2, length = M)
    grid = @. cnt(complex(xs', ys),c)
    return grid
end
```

```bash
c = complex(-0.38, 0.6)
M=N=500
heatmap(1:N, 1:M, jlset(M,N,c), aspect_ratio=:equal)
```

このoutput がなんと！

![](image-knduto3n.png)

```bash
$ julia
> using Pkg
> Pkg.add("OhMyREPL")
> using OhMyREPL
コードにシンタックスハイライトが付く。
```

add して、その後は使いたい度に using で指定するのか。

Pkg をスタート ]

```bash
$ julia
> ]
pkg> 
```

