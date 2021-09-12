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

### Buttons

Two equivalent way of putting a button:

```swift
		var remove: some View {
        Button(action: {
            if emojiCount < emojis.count
            {
                emojiCount-=1
            }
        }, label: { Image(systemName: "minus") })
    }
    
    var add: some View {
        Button{
            if emojiCount > 1
            {
                emojiCount-=1
            }
        } label: { Image(systemName: "plus") }
    }

// and these views can be add directly to the major big view.

HStack{
  	add
    Spacer()
    remove
  }.padding(.horizontal)
```

### LazyVGrid

To display our cards in grid style, we use this type of `View Layout and Presentation`.

#### Overview - LazyVGrid

The grid is “lazy,” in that the grid view does not create items until they are needed.

In the following example, a [`ScrollView`](doc://com.apple.documentation/documentation/swiftui/scrollview?language=swift) contains a `LazyVGrid`consisting of a two-column grid of [`Text`](doc://com.apple.documentation/documentation/swiftui/text?language=swift) views, showing Unicode code points from the “Smileys” group and their corresponding emoji:

```swift
 var columns: [GridItem] =
         Array(repeating: .init(.flexible()), count: 2)
 ScrollView {
     LazyVGrid(columns: columns) {
         ForEach((0...79), id: \.self) {
             let codepoint = $0 + 0x1f600
             let codepointString = String(format: "%02X", codepoint)
             Text("\(codepointString)")
             let emoji = String(Character(UnicodeScalar(codepoint)!))
             Text("\(emoji)")
         }
     }.font(.largeTitle)
 }
```

The `HStack` uses **all the space it can**. But the `LazyVGrid` distributes the space quite differently. It uses all the space horizontally but the least possible space vertically.



## Letcure 3 - MVVM and the Swift type system

### MVVM

*Model-View-ViewModel*

**Model**: UI independent. Pure data and logic. Backbend of the app. Single source of "Truth".

**View**: Reflects the model. Stateless, so ~~@State~~ is not a regular solution to logic in the apps. `@State` only facilitating transient movements in the UI. View is immutable. **Declared**: It performs exactly what we declared in the `body var`. Imperative. 

Data flows from Model to View (i.e. read-only). Any change inthe model will cause the affected parts of the view to be rebuilt.

**ViewModel**: Binds View to Model. Sort of a **Interpreter**. A **Gatekeeper**. The View will always get data from the Model from The ViewModel. Constantly noticing changes in the Model, the publishes "something changes" to those interested in such changes. The View automatically observes publications and pulls data and rebuilds.

For the VM, there are keywords like... `ObservableObject`, `@Published`, `objectWillChange.send()`...

For the View, there are `@ObservedObejct`, `@Binding`, `.onReceive`, `@EnvironmentObject`, `.environmentObject()`...

**But what about the other direction?**

After the user interacts with the View, the View calls Intent Function to the VM. The VM processes Intent and modifies the Model. This is often seen in *Store Data* process. Make sure to leave database access statements in the VM.
