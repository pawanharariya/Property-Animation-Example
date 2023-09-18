# Property Animation Example #

The app animates stars on the screen by changing various View properties that control position,
size, rotation, and translucency. This app shows how to create Property Animations, using
ObjectAnimator, which are the basic building blocks of most Android animations. Property animations
are used to animate (or change over time) the value of a property on an object, usually a UI object
like an Android view.

## App Preview ##

<img alt="Property Animation Example 3" src="https://github.com/pawanharariya/Property-Animation-Example/assets/43620548/fb3036c6-6745-4578-b4c7-f4227d80aead" width=200/>
<img alt="Property Animation Example 2" src="https://github.com/pawanharariya/Property-Animation-Example/assets/43620548/31d05f96-4f72-4b40-b6a4-431ed423e263" width=200/>
<img alt="Property Animation Example" src="https://github.com/pawanharariya/Property-Animation-Example/assets/43620548/f5333ac6-2681-41d1-9b15-3b65d295d531" width=200/>
<img alt="Property Animation Example gif" src="https://github.com/pawanharariya/Property-Animation-Example/assets/43620548/da139c24-47fd-45ee-a986-90cc5664f2ce" width="200"/>

## Property Animation ##

Property animation involves changing of property values of the views over time like position,
rotation, scale or transparency.

## Object Animator ##

`ObjectAnimator` provides a simple **set-and-forget** mechanism for animating properties.

```
val animator = ObjectAnimator.ofFloat(star, View.ROTATION, -360f, 0f)
```

ObjectAnimator acts on the target “star” (ImageView). It runs an animation on the ROTATION property
of star with start and end values of -360 to 0 degrees. The default duration of animation is 300
milliseconds.

## Jank Effect ##

The discontinuous motion which is caused when the animation restarts in the middle of current
animation, is called **Jank**. When the next animation is requested (by user click) and the current
animation isn't finished yet, the property resets to original value causing an abrupt motion. To
resolve this we can start the new animation from current value or prevent user from requesting next
animation until current one isn't finished.

## AnimatorListenerAdapter ##

To handle animation states we use `AnimatorListenerAdapter` that has callbacks for an animation
starting, ending, pausing, resuming and repeating. Instead of implementing the
raw `AnimatorListener` the adapter class is used because it provides default implementations of all
the listener methods and we can override only those which are required. We prevent the Jank effect
using AnimatorListenerAdapter.

## Property Values Holder ##

It holds information about a single property, with values that the property animates between.
However, it doesn't specify the target to it is applied. We use them to achieve parallel animations.
An `ObjectAnimator` can hold multiple `PropertyViewHolder` objects and they all animate together, in
parallel. The target to `PropertyViewHolder` is specified by `ObjectAnimator`.

```
val scaleX = PropertyValuesHolder.ofFloat(View.SCALE_X, 4f)
val scaleY = PropertyValuesHolder.ofFloat(View.SCALE_Y, 4f)
val animator = ObjectAnimator.ofPropertyValuesHolder(star, scaleX, scaleY)
```

## Animating arbitrary property ##

ObjectAnimator not only animates view properties like alpha and rotation, but it can animate any
object that has properties that object animator can call during animation. For example, the
background color of a view is not exposed as `android.util.property` object, so we pass a string to
object animator, which then uses setter and getter to set and get the value of that field.
ObjectAnimator takes the string and looks for properties with the same name.

```
var animator = ObjectAnimator.ofArgb(view,"backgroundColor", Color.BLACK, Color.RED).start()
```

## Helpful Tips ##

1. We can use `animator.repeatCount` to simply repeat the animation after initial run. We can
   specify if we want to reverse the animation by specify repeat mode.
   ```
   animator.repeatMode = ObjectAnimator.REVERSE
   ```
2. To animate between colors instead of using `ObjectAnimator.ofInt()` factory, we should
   use `ObjectAnimator.ofArgb()` to properly interpret colors (Requires API level 21 and above).

3. To run multiple animations together we can put them into `AnimatorSet`. It can be used to run
   animations in parallel or sequentially.