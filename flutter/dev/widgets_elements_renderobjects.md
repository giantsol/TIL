# Widget

Describes the configuration for an [Element].

Widgets are the central class hierarchy in the Flutter framework. A widget is an immutable description of part of a user interface. Widgets can be inflated into elements, which manage the underlying render tree.

A given widget can be included in the tree zero or more times. In particular a given widget can be placed in the tree multiple times. Each time a widget is placed in the tree, it is inflated into an [Element], which means a widget that is incorporated into the tree multiple times will be inflated multiple times.

The [key] property controls how one widget replaces another widget in the tree. If the [runtimeType] and [key] properties of the two widgets are [operator==], respectively, then the new widget replaces the old widget by updating the underlying element (i.e., by calling [Element.update] with the new widget). Otherwise, the old element is removed from the tree, the new widget is inflated into an element, and the new element is inserted into the tree.

In addition, using a [GlobalKey] as the widget's [key] allows the element to be moved around the tree (changing parent) without losing state. When a new widget is found (its key and type do not match a previous widget in the same location), but there was a widget with that same global key elsewhere in the tree in the previous frame, then that widget's element is moved to the new location.

Generally, a widget that is the only child of another widget does not need an explicit key.

If the widgets have no key (their key is null), then they are considered a match if they have the same type, even if their children are completely different.

# StatelessWidget

The [build] method of a stateless widget is typically only called in three situations: the first time the widget is inserted in the tree, when the widget's parent changes its configuration, and when an [InheritedWidget] it depends on changes.

If a widget's parent will regularly change the widget's configuration, or if it depends on inherited widgets that frequently change, then it is important to optimize the performance of the [build] method to maintain a fluid rendering performance.

There are several techniques one can use to minimize the impact of rebuilding a stateless widget:

1. Minimize the number of nodes transitively created by the build method and any widgets it creates. For example, instead of an elaborate arrangement of [Row]s, [Column]s, [Padding]s, and [SizedBox]es to position a single child in a particularly fancy manner, consider using just an [Align] or a [CustomSingleChildLayout]. Instead of an intricate layering of multiple [Container]s and with [Decoration]s to draw just the right graphical effect, consider a single [CustomPaint] widget.
2. Use `const` widgets where possible, and provide a `const` constructor for the widget so that users of the widget can also do so.
3. If the widget is likely to get rebuilt frequently due to the use of [InheritedWidget]s, consider refactoring the stateless widget into multiple widgets, with the parts of the tree that change being pushed to the leaves. For example instead of building a tree with four widgets, the inner-most widget depending on the [Theme], consider factoring out the part of the build function that builds the inner-most widget into its own widget, so that only the inner-most widget needs to be rebuilt when the theme changes.

# StatefulWidget

The framework calls [createState] whenever it inflates a [StatefulWidget], which means that multiple [State] objects might be associated with the same [StatefulWidget] if that widget has been inserted into the tree in multiple places. Similarly, if a [StatefulWidget] is removed from the tree and later inserted in to the tree again, the framework will call [createState] again to create a fresh [State] object, simplifying the lifecycle of [State] objects.

There are two primary categories of [StatefulWidget]s.

The first is one which allocates resources in [State.initState] and disposes of them in [State.dispose], but which does not depend on [InheritedWidget]s or call [State.setState]. Such widgets are commonly used at the root of an application or page, and communicate with subwidgets via [ChangeNotifier]s, [Stream]s, or other such objects. Stateful widgets following such a pattern are relatively cheap (in terms of CPU and GPU cycles), because they are built once then never update. They can, therefore, have somewhat complicated and deep build methods.

The second category is widgets that use [State.setState] or depend on [InheritedWidget]s. These will typically rebuild many times during the application's lifetime, and it is therefore important to minimize the impact of rebuilding such a widget. (They may also use [State.initState] or [State.didChangeDependencies] and allocate resources, but the important part is that they rebuild.)

# ComponentElement

An [Element] that composes other [Element]s.

Rather than creating a [RenderObject] directly, a [ComponentElement] creates
[RenderObject]s indirectly by creating other [Element]s.

Contrast with [RenderObjectElement].

# RenderObjectElement

An [Element] that uses a [RenderObjectWidget] as its configuration.

[RenderObjectElement] objects have an associated [RenderObject] widget in the render tree, which handles concrete operations like laying out, painting, and hit testing.

There are three common child models used by most [RenderObject]s:

* Leaf render objects, with no children: The [LeafRenderObjectElement] class handles this case.

* A single child: The [SingleChildRenderObjectElement] class handles this case.

* A linked list of children: The [MultiChildRenderObjectElement] class handles this case.

Sometimes, however, a render object's child model is more complicated. Maybe it has a two-dimensional array of children. Maybe it constructs children on demand. Maybe it features multiple lists. In such situations, the corresponding [Element] for the [Widget] that configures that [RenderObject] will be a new subclass of [RenderObjectElement].

[RenderObjectElement] objects spend much of their time acting as intermediaries between their [widget] and their [renderObject]. To make this more tractable, most [RenderObjectElement] subclasses override these getters so that they return the specific type that the element expects, e.g.:

```dart
class FooElement extends RenderObjectElement {

  @override
  Foo get widget => super.widget;

  @override
  RenderFoo get renderObject => super.renderObject;

  // ...
}
```

# RenderObject

An object in the render tree.

The [RenderObject] class hierarchy is the core of the rendering library's reason for being.

The [RenderObject] class also implements the basic layout and paint protocols.

The [RenderObject] class, however, does not define a child model (e.g. whether a node has zero, one, or more children). It also doesn't define a coordinate system (e.g. whether children are positioned in Cartesian coordinates, in polar coordinates, etc) or a specific layout protocol (e.g. whether the layout is width-in-height-out, or constraint-in-size-out, or whether the parent sets the size and position of the child before or after the child lays out, etc; or indeed whether the children are allowed to read their parent's [parentData] slot).

The [RenderBox] subclass introduces the opinion that the layout system uses Cartesian coordinates.

In most cases, subclassing [RenderObject] itself is overkill, and [RenderBox] would be a better starting point. However, if a render object doesn't want to use a Cartesian coordinate system, then it should indeed inherit from [RenderObject] directly. This allows it to define its own layout protocol by using a new subclass of [Constraints] rather than using [BoxConstraints], and by potentially using an entirely new set of objects and values to represent the result of the output rather than just a [Size]. This increased flexibility comes at the cost of not being able to rely on the features of [RenderBox]. For example, [RenderBox] implements an intrinsic sizing protocol that allows you to measure a child without fully laying it out, in such a way that if that child changes size, the parent will be laid out again (to take into account the new dimensions of the child). This is a subtle and bug-prone feature to get right.

The [performLayout] method should take the [constraints], and apply them. The output of the layout algorithm is fields set on the object that describe the geometry of the object for the purposes of the parent's layout. For example, with [RenderBox] the output is the [RenderBox.size] field. This output should only be read by the parent if the parent specified `parentUsesSize` as true when calling [layout] on the child.

Anytime anything changes on a render object that would affect the layout of that object, it should call [markNeedsLayout].

In general, the root of a Flutter render object tree is a [RenderView]. This object has a single child, which must be a [RenderBox]. Thus, if you want to have a custom [RenderObject] subclass in the render tree, you have two choices: you either need to replace the [RenderView] itself, or you need to have a [RenderBox] that has your class as its child. (The latter is the much more common case.)

In general, the layout of a render object should only depend on the output of its child's layout, and then only if `parentUsesSize` is set to true in the [layout] call. Furthermore, if it is set to true, the parent must call the child's [layout] if the child is to be rendered, because otherwise the parent will not be notified when the child changes its layout outputs.
