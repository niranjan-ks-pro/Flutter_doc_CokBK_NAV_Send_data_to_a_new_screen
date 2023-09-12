# Flutter_doc_CokBK_NAV_Send_data_to_a_new_screen
 https://docs.flutter.dev/cookbook/navigation/passing-data
Send data to a new screen
=========================

1.  [Cookbook](https://docs.flutter.dev/cookbook)
2.  [Navigation](https://docs.flutter.dev/cookbook/navigation)
3.  [Send data to a new screen](https://docs.flutter.dev/cookbook/navigation/passing-data)

Often, you not only want to navigate to a new screen, but also pass data to the screen as well. For example, you might want to pass information about the item that's been tapped.

Remember: Screens are just widgets. In this example, create a list of todos. When a todo is tapped, navigate to a new screen (widget) that displays information about the todo. This recipe uses the following steps:

1.  Define a todo class.
2.  Display a list of todos.
3.  Create a detail screen that can display information about a todo.
4.  Navigate and pass data to the detail screen.

[](https://docs.flutter.dev/cookbook/navigation/passing-data#1-define-a-todo-class)1\. Define a todo class
----------------------------------------------------------------------------------------------------------

First, you need a simple way to represent todos. For this example, create a class that contains two pieces of data: the title and description.

content_copy

```
class Todo {
  final String title;
  final String description;

  const Todo(this.title, this.description);
}
```

[](https://docs.flutter.dev/cookbook/navigation/passing-data#2-create-a-list-of-todos)2\. Create a list of todos
----------------------------------------------------------------------------------------------------------------

Second, display a list of todos. In this example, generate 20 todos and show them using a ListView. For more information on working with lists, see the [Use lists](https://docs.flutter.dev/cookbook/lists/basic-list) recipe.

### [](https://docs.flutter.dev/cookbook/navigation/passing-data#generate-the-list-of-todos)Generate the list of todos

content_copy

```
final todos = List.generate(
  20,
  (i) => Todo(
    'Todo $i',
    'A description of what needs to be done for Todo $i',
  ),
);
```

### [](https://docs.flutter.dev/cookbook/navigation/passing-data#display-the-list-of-todos-using-a-listview)Display the list of todos using a ListView

content_copy

```
ListView.builder(
  itemCount: todos.length,
  itemBuilder: (context, index) {
    return ListTile(
      title: Text(todos[index].title),
    );
  },
),
```

So far, so good. This generates 20 todos and displays them in a ListView.

[](https://docs.flutter.dev/cookbook/navigation/passing-data#3-create-a-todo-screen-to-display-the-list)3\. Create a Todo screen to display the list
----------------------------------------------------------------------------------------------------------------------------------------------------

For this, we create a `StatelessWidget`. We call it `TodosScreen`. Since the contents of this page won't change during runtime, we'll have to require the list of todos within the scope of this widget.

We pass in our `ListView.builder` as body of the widget we're returning to `build()`. This'll render the list on to the screen for you to get going!

content_copy

```
class TodosScreen extends StatelessWidget {
  // Requiring the list of todos.
  const TodosScreen({super.key, required this.todos});

  final List<Todo> todos;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Todos'),
      ),
      //passing in the ListView.builder
      body: ListView.builder(
        itemCount: todos.length,
        itemBuilder: (context, index) {
          return ListTile(
            title: Text(todos[index].title),
          );
        },
      ),
    );
  }
}
```

With Flutter's default styling, you're good to go without sweating about things that you'd like to do later on!

[](https://docs.flutter.dev/cookbook/navigation/passing-data#4-create-a-detail-screen-to-display-information-about-a-todo)4\. Create a detail screen to display information about a todo
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Now, create the second screen. The title of the screen contains the title of the todo, and the body of the screen shows the description.

Since the detail screen is a normal `StatelessWidget`, require the user to enter a `Todo` in the UI. Then, build the UI using the given todo.

content_copy

```
class DetailScreen extends StatelessWidget {
  // In the constructor, require a Todo.
  const DetailScreen({super.key, required this.todo});

  // Declare a field that holds the Todo.
  final Todo todo;

  @override
  Widget build(BuildContext context) {
    // Use the Todo to create the UI.
    return Scaffold(
      appBar: AppBar(
        title: Text(todo.title),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Text(todo.description),
      ),
    );
  }
}
```

[](https://docs.flutter.dev/cookbook/navigation/passing-data#5-navigate-and-pass-data-to-the-detail-screen)5\. Navigate and pass data to the detail screen
----------------------------------------------------------------------------------------------------------------------------------------------------------

With a `DetailScreen` in place, you're ready to perform the Navigation. In this example, navigate to the `DetailScreen` when a user taps a todo in the list. Pass the todo to the `DetailScreen`.

To capture the user's tap in the `TodosScreen`, write an [`onTap()`](https://api.flutter.dev/flutter/material/ListTile/onTap.html) callback for the `ListTile` widget. Within the `onTap()` callback, use the [`Navigator.push()`](https://api.flutter.dev/flutter/widgets/Navigator/push.html) method.

content_copy

```
body: ListView.builder(
  itemCount: todos.length,
  itemBuilder: (context, index) {
    return ListTile(
      title: Text(todos[index].title),
      // When a user taps the ListTile, navigate to the DetailScreen.
      // Notice that you're not only creating a DetailScreen, you're
      // also passing the current todo through to it.
      onTap: () {
        Navigator.push(
          context,
          MaterialPageRoute(
            builder: (context) => DetailScreen(todo: todos[index]),
          ),
        );
      },
    );
  },
),
```

### [](https://docs.flutter.dev/cookbook/navigation/passing-data#interactive-example)Interactive example

[](https://docs.flutter.dev/cookbook/navigation/passing-data#alternatively-pass-the-arguments-using-routesettings)Alternatively, pass the arguments using RouteSettings
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

Repeat the first two steps.

### [](https://docs.flutter.dev/cookbook/navigation/passing-data#create-a-detail-screen-to-extract-the-arguments)Create a detail screen to extract the arguments

Next, create a detail screen that extracts and displays the title and description from the `Todo`. To access the `Todo`, use the [`ModalRoute.of()`](https://api.flutter.dev/flutter/widgets/ModalRoute/of.html) method. This method returns the current route with the arguments.

content_copy

```
class DetailScreen extends StatelessWidget {
  const DetailScreen({super.key});

  @override
  Widget build(BuildContext context) {
    final todo = ModalRoute.of(context)!.settings.arguments as Todo;

    // Use the Todo to create the UI.
    return Scaffold(
      appBar: AppBar(
        title: Text(todo.title),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Text(todo.description),
      ),
    );
  }
}
```

### [](https://docs.flutter.dev/cookbook/navigation/passing-data#navigate-and-pass-the-arguments-to-the-detail-screen)Navigate and pass the arguments to the detail screen

Finally, navigate to the `DetailScreen` when a user taps a `ListTile` widget using `Navigator.push()`. Pass the arguments as part of the [`RouteSettings`](https://api.flutter.dev/flutter/widgets/RouteSettings-class.html). The `DetailScreen` extracts these arguments.

content_copy

```
ListView.builder(
  itemCount: todos.length,
  itemBuilder: (context, index) {
    return ListTile(
      title: Text(todos[index].title),
      // When a user taps the ListTile, navigate to the DetailScreen.
      // Notice that you're not only creating a DetailScreen, you're
      // also passing the current todo through to it.
      onTap: () {
        Navigator.push(
          context,
          MaterialPageRoute(
            builder: (context) => const DetailScreen(),
            // Pass the arguments as part of the RouteSettings. The
            // DetailScreen reads the arguments from these settings.
            settings: RouteSettings(
              arguments: todos[index],
            ),
          ),
        );
      },
    );
  },
)
```

### [](https://docs.flutter.dev/cookbook/navigation/passing-data#complete-example)Complete example

content_copy

```
import 'package:flutter/material.dart';

class Todo {
  final String title;
  final String description;

  const Todo(this.title, this.description);
}

void main() {
  runApp(
    MaterialApp(
      title: 'Passing Data',
      home: TodosScreen(
        todos: List.generate(
          20,
          (i) => Todo(
            'Todo $i',
            'A description of what needs to be done for Todo $i',
          ),
        ),
      ),
    ),
  );
}

class TodosScreen extends StatelessWidget {
  const TodosScreen({super.key, required this.todos});

  final List<Todo> todos;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Todos'),
      ),
      body: ListView.builder(
        itemCount: todos.length,
        itemBuilder: (context, index) {
          return ListTile(
            title: Text(todos[index].title),
            // When a user taps the ListTile, navigate to the DetailScreen.
            // Notice that you're not only creating a DetailScreen, you're
            // also passing the current todo through to it.
            onTap: () {
              Navigator.push(
                context,
                MaterialPageRoute(
                  builder: (context) => const DetailScreen(),
                  // Pass the arguments as part of the RouteSettings. The
                  // DetailScreen reads the arguments from these settings.
                  settings: RouteSettings(
                    arguments: todos[index],
                  ),
                ),
              );
            },
          );
        },
      ),
    );
  }
}

class DetailScreen extends StatelessWidget {
  const DetailScreen({super.key});

  @override
  Widget build(BuildContext context) {
    final todo = ModalRoute.of(context)!.settings.arguments as Todo;

    // Use the Todo to create the UI.
    return Scaffold(
      appBar: AppBar(
        title: Text(todo.title),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Text(todo.description),
      ),
    );
  }

```

`\
`
