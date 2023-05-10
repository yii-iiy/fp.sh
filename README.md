
# fp.sh

~~~ md
😈 Just using basic feature of SHell, and pass the tests on the `ash`. 🥗
~~~

## Thanks for

- [you-dont-need/You-Dont-Need-Loops: Avoid The One-off Problem, Infinite Loops, Statefulness and Hidden intent.](https://github.com/you-dont-need/You-Dont-Need-Loops)

## Playings

### Fibonacci

*stream/lazylist/unfold/iterator style :*

~~~ sh
# use tmp asigns (more quick ... maybe)
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

# or use the Tuple inside the fp (more simple ~)
fib () 
(
    x=0 y=0 z=1 \
    iterator='
        
        test "$x" -eq "'"${n:-13}"'" && break ; 
        fielder=",$IFS" Tuple -- x y z < <(echo "$((x + 1)), $((z)), $((y + z))")
        
        ' fp iterate \
    eval printf "'${fmt:-%d: %d\\n}' ${mapas:-\"\$x\" \"\$y\"}" &&
    
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

### `Tuple`

~~~ sh
Tuple -- x y < <(echo X Y) # then $x will be "X" and $y will be "Y"
fielder=",$IFS" Tuple -- x y < <(echo 'XX, YY') # then $x will be "XX" and $y will be "YY"
~~~

*Just use it inside these fp tools.*

### `trim`

~~~ sh
fp trim $'  \n  U W U \n   \n\n ' $'  \n  U W U \n   \n\n '
~~~

~~~
U W U



  U W U
~~~

### `iterate`

*see the Fibonacci ...*

### `per`

~~~ sh
seq 2 2 8 | cat -n | f='echo "$x -> $y"' fp per x y
~~~

~~~
1 -> 2
2 -> 4
3 -> 6
4 -> 8
~~~

~~~ sh
echo a,b,c:d,e,f: | fielder=, f='echo "$z ~ $x -> $y"' fp per -d : -- y x z
~~~

~~~
c ~ b -> a
f ~ e -> d
~~~

### `reduce`

~~~ sh
seq 7 | acc=3 f='echo $((x + acc))' fp reduce -- x # 31
echo a,b,c:d,e,f: | fielder=, acc='' f='echo "$y .. $z .. $x ~ $acc"' fp reduce -d : -- x y z # e .. f .. d ~ b .. c .. a ~
~~~

*or see the Fibonacci ...*

### `map`

~~~ sh
seq 2 2 8 | cat -n | f='"$x -> $y"' fp map x y
~~~

~~~
1 -> 2
2 -> 4
3 -> 6
4 -> 8
~~~

~~~ sh
echo a,b,c:d,e,f: | fielder=, f='"$z ~ $x -> $y"' fp map -d : -- y x z
~~~

~~~

c ~ b -> a
f ~ e -> d

~~~

### `formatf`

~~~ sh
formatter=' _%s' fp formatf $(seq 12)
~~~

~~~
 _1 _2 _3 _4 _5 _6 _7 _8 _9 _10 _11 _12
~~~

*equal with :*

~~~ sh
f='printf \ _%s "$x"' fp per x < <(seq 12)
~~~

### `repeat`

~~~ sh
repeater=21 fp repeat AA BBB CCCC
~~~

~~~
AA AA AA AA AA AA AA AA AA AA AA AA AA AA AA AA AA AA AA AA AA
BBB BBB BBB BBB BBB BBB BBB BBB BBB BBB BBB BBB BBB BBB BBB BBB BBB BBB BBB BBB BBB
CCCC CCCC CCCC CCCC CCCC CCCC CCCC CCCC CCCC CCCC CCCC CCCC CCCC CCCC CCCC CCCC CCCC CCCC CCCC CCCC CCCC
~~~

~~~ sh
ofs='!?::' repeater=5 fp repeat AA BBB CCCC
~~~

~~~
!?::AA!?::AA!?::AA!?::AA!?::AA!?::
!?::BBB!?::BBB!?::BBB!?::BBB!?::BBB!?::
!?::CCCC!?::CCCC!?::CCCC!?::CCCC!?::CCCC!?::
~~~

~~~ sh
ofs=' ' concater=$'\n:;;:' repeater=3 fp repeat AA BBB CCCC
~~~

~~~
:;;:AA AA AA
:;;:BBB BBB BBB
:;;:CCCC CCCC CCCC
:;;:
~~~





