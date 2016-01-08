# Keeping state

We created a little application that display the current mouse x coordinate. But in most web applications we want to keep some state around as the user interacts with our application. 

Let's create an application that tracks clicks.

```elm
import Html
import Mouse

view : Int -> Html.Html
view count =
  Html.text (toString count)

countSignal : Signal Int
countSignal =
  Signal.map (always 1) Mouse.clicks

main: Signal.Signal Html.Html
main =
  Signal.map view countSignal
```

To introduce things gradually here we have an application that just shows `1` all the time. The `view` function displays the given count, in this case it is always 1.

The `countSignal` function produces a signal of integers, it takes the `Mouse.clicks` signal and maps it to always `1` for now.

`main` takes this `countSignal` and maps it through the `view` function.

## Signal.foldp

`Signal.foldp` for 'fold past' is a signal that takes past state and combines it with a current input. It is quite similar `inject` in JavaScript arrays.

Here is the application using `foldp`:

```elm
import Html
import Mouse

view : Int -> Html.Html
view count =
  Html.text (toString count)

countSignal : Signal Int
countSignal =
  Signal.foldp (\_ acc -> acc + 1) 0 Mouse.clicks

main: Signal.Signal Html.Html
main =
  Signal.map view countSignal
```

Let's see what is happening in the `Signal.foldp` line.

Signal.foldp takes three arguments:

- An accumulation function: `(\_ acc -> acc + 1)`
- The initial state, in this case `0`
- And the source signal: `Mouse.clicks`

The accumulation function 
