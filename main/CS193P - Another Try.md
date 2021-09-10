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

#### Overview - ForEach

Use `ForEach` to provide views based on a [`RandomAccessCollection`](doc://com.apple.documentation/documentation/swift/randomaccesscollection?language=swift) of some data type. Either the collection’s elements must conform to [`Identifiable`](doc://com.apple.documentation/documentation/swift/identifiable?language=swift) or you need to provide an `id`parameter to the `ForEach` initializer.

The following example creates a `NamedFont` type that conforms to [`Identifiable`](doc://com.apple.documentation/documentation/swift/identifiable?language=swift), and an array of this type called `namedFonts`. A `ForEach` instance iterates over the array, producing new [`Text`](doc://com.apple.documentation/documentation/swiftui/text?language=swift)instances that display examples of each SwiftUI [`Font`](doc://com.apple.documentation/documentation/swiftui/font?language=swift) style provided in the array.

```swift
private struct NamedFont: Identifiable 
{    
  let name: String    
  let font: Font    
  var id: String { name }
}

private let namedFonts: [NamedFont] = [    
  NamedFont(name: "Large Title", font: .largeTitle),    
  NamedFont(name: "Title", font: .title),    
  NamedFont(name: "Headline", font: .headline),    
  NamedFont(name: "Body", font: .body),    
  NamedFont(name: "Caption", font: .caption)]

var body: some View {    
  ForEach(namedFonts) { namedFont in        
         Text(namedFont.name)            
               .font(namedFont.font)    
                      }
}
```

So this explains the error below.

```swift
ForEach(emojis, content:{ emoji in
//err: Referencing initializer 'init(_:content)' on 'ForEach' requires that 'String' conform to 'Identifiable'
    CardView(content: emoji)
})
```

For now, we only want the view to generate different emojis. So we do this:

```swift
ForEach(emojis, id: \.self, content:{ emoji 
    CardView(content: emoji)
})
```

> When we use `\.self` as an identifier, we mean “the whole object”, but in practice that doesn’t mean much – a struct is a struct, so it doesn’t have any sort of specific identifying information other than its contents. So what actually happens is that Swift computes the *hash value* of the struct, which is a way of representing complex data in fixed-size values, then uses that hash as an identifier.















