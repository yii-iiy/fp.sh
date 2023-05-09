
# fp.sh

~~~ md
😈 Just using basic feature of SHell, and pass the tests on the `ash`. 🥗
~~~

## Thanks for

- [you-dont-need/You-Dont-Need-Loops: Avoid The One-off Problem, Infinite Loops, Statefulness and Hidden intent.](https://github.com/you-dont-need/You-Dont-Need-Loops)

## Playings

### Fib

*stream/lazylist/unfold/iterator style :*

~~~ sh
# use tmp asigns
fib ()
(
    _x=0 _y=0 _z=1 \
    iterator='
        
        test "$x" -eq "'"${n:-13}"'" && break ;
        _x="$((x + 1))" _y="$((z))" _z="$((y + z))"
        
        ' fp iterate \
    eval '
        
        x="$_x" y="$_y" z="$_z" ;
        printf '"'${fmt:-%d: %d\\n}' ${mapas:-\"\$x\" \"\$y\"}" &&
    
    : ) ;

# or use the _tuple inside the fp
fib ()
(
    x=0 y=0 z=1 \
    iterator='
        
        test "$x" -eq "'"${n:-13}"'" && break ;
        fielder=",$IFS" _tuple -- x y z < <(echo "$((x + 1)), $((z)), $((y + z))")
        
        ' fp iterate \
    eval printf "'${fmt:-%d: %d\\n}'" "${mapas:-\"\$x\" \"\$y\"}" &&
    
    : ) ;
~~~        

~~~ sh
fmt='%i %i, ' n=13 fib
# 0 0, 1 1, 2 1, 3 2, 4 3, 5 5, 6 8, 7 13, 8 21, 9 34, 10 55, 11 89, 12 144, 13 233, 
~~~

*reduce style :*

~~~ sh
fib ()
(
    acc="0${ofs:-, }0${ofs:-, }1${ofs:-, }_" \
    f='
        
        echo "$(
            
            fielder="${ofs:-,}$IFS" _tuple -d "${concater:-${IFS: -1}}" -- x y z _ < <(echo "$acc") ;
            echo "$((x + 1))${ofs:-, }$((z))${ofs:-, }$((y + z))${ofs:-, }${q}"
            
            )${concater:-${IFS: -1}}${acc}"
        
        ' fp reduce q < <(seq "$@") &&
    
    : ) ;
~~~

~~~ sh
ofs=' ' fib 13 | f='"$x $y"' fp map -- x y _ _ | tac | f='printf "%s, " "$x $y"' fp per -- x y
# 0 0, 1 1, 2 1, 3 2, 4 3, 5 5, 6 8, 7 13, 8 21, 9 34, 10 55, 11 89, 12 144, 13 233, 
~~~

## Funcs

### `_tuple`

~~~ sh
_tuple -- x y < <(echo X Y) # then $x will be "X" and $y will be "Y"
fielder=",$IFS" _tuple -- x y < <(echo 'XX, YY') # then $x will be "XX" and $y will be "YY"
~~~

### `iterate`



