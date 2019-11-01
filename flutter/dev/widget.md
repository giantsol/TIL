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
