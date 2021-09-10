# CS193P - Another Try

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

