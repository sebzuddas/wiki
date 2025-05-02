
# Introduction


-----
# Common Functions

## Impulse Function

```tikz

\usepackage{pgfplots} 
\begin{document} 
\begin{tikzpicture} 

\begin{axis}[ axis lines=middle, xlabel={$x$}, ylabel={$f(x)$}, ymin=-0.5, ymax=2.5, xmin=-1, xmax=4, xtick={-1,0,1,2,3}, ytick={0,1,2}, extra x ticks={0}, extra x tick labels={$t_0$}, extra x tick style={tick label style={yshift=-0.5em}} ] 

% Add the impulse function 
\addplot[mark=*, only marks, mark size=3pt] coordinates {(0,2)}; 
% Position of impulse 

\draw [thick, -stealth] (axis cs:0,0) -- (axis cs:0,2); 
% Arrow representing the impulse magnitude 

\end{axis} 
\end{tikzpicture} 
\end{document}


```


## Step Function
```tikz

\begin{document} 
\begin{tikzpicture} 
\begin{axis}[ axis lines=middle, xlabel={$x$}, ylabel={$f(x)$}, ymin=-0.5, ymax=2.5, xmin=-1, xmax=4, xtick={-1,0,1,2,3}, ytick={0,1,2}, extra x ticks={2}, extra x tick labels={$a$}, extra x tick style={tick label style={yshift=-0.5em}} ] \addplot[domain=-1:0, thick] {0}; 
% The first step (0) 
\addplot[domain=0:2, thick] {1}; 
% The second step (1)
\addplot[domain=2:4, thick] {2}; 
% The third step (2) % Add vertical lines to mark the steps 

\draw [thick] (axis cs:0,0) -- (axis cs:0,1); 
\draw [thick] (axis cs:2,1) -- (axis cs:2,2); 
\end{axis} 
\end{tikzpicture} 
\end{document}
```


^8d8915

-----
