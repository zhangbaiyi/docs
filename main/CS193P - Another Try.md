# CS193P @Stanford - Another Try

## Lecture 2 - Learning more about SwiftUI

### Preview Generator

```swift
struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
            .preferredColorScheme(.light)
        ContentView()
            .preferredColorScheme(.dark)
    }

}
```

This brings both light and dark mode on the right side of the screen.

### Initialize Variables

::red_circle:`Missing argument for parameter 'XXX' in call`

If not, probably the error message above appears.

You can initialize in the declaration or calling.

`CardView(isFaceUp: true)`

**`var body: some View`**

This is a `var` because the value of `body` is determined by executing the `body` function.'

### The View Modifier

> When something or events happened, the entire UI is being **constantly rebuilt**. New views would be created to reflect the changes.

So you can't assign gesture action to a variable **inside a View object** 

::red_circle:`Cannot assign to property: 'self' is immutable`

Solution: **`@State`**

`@State var isFaceUp: Bool = true`

In this case the `isFaceUp` is no longer a `Boolean`, but a **pointer**. So though the value change but the pointer points the same place.

**BUT** this is not commonly used. Mostly the logic will cover changes. 

### A Reference Trick

`option` + `click` gives you document on certain functions or variables.

### A Bag of Lego

So an array is a bag of Lego. Now I want to create a CardView for each Lego in the bag. It's `ForEach` statement's job basically.

Apple's Official Doc for `ForEach`:



```swift
ForEach(emojis, content:{ emoji 
//err: Referencing initializer 'init(_:content)' on 'ForEach' requires that 'String' conform to 'Identifiable'
    CardView(content: emoji)
})
```



