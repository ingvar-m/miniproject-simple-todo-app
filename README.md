# 📝 Simple Todo App

A weekly task management app built in Flutter, inspired by Tweek.

## Technologies

* **Flutter**
* **Hive** (Local Database)

## Features

* Seamless drag-and-drop task reordering across different days
* Optimistic UI updates for zero-latency interactions
* Focus-based editing (tap to edit, long-press to drag)
* Persistent local data storage using Hive

## The Process

My initial goal was straightforward: practice Flutter UI layout and connect a local database without overcomplicating the architecture. However, implementing the core feature-dragging tasks between days-turned into a deep dive into Flutter's gesture system and list rendering.

The first major hurdle was a gesture conflict. Using a standard ReorderableListView, the TextField inside the task would steal the touch event, preventing the drag action. After experimenting with widget swapping, I solved this by wrapping the text field in an AbsorbPointer controlled by a FocusNode. When idle, dragging works perfectly; a standard tap requests focus and allows text editing.

Scaling this to a full week view introduced new issues. Trying to link multiple lists using Draggable and DragTarget resulted in accidental triggers and a poor scrolling experience. The breakthrough was moving to a Flat List Architecture. I created a common ListItem model and mapped everything (Day Headers, Tasks, Input Fields) into a single, one-dimensional array managed by one global ReorderableListView.

Polishing the experience required solving two tricky edge cases:

**UI Snapping:** Tasks would momentarily jump back to their old position while waiting for Hive to save. I fixed this by implementing optimistic UI updates-updating the interface state immediately and saving to the database in the background.

**Structural Breaks:** Dragging a task could wedge it between a day's input field and the next day's header. I created a custom DaySeparatorItem that visually combines both elements into a single, unbreakable unit.

While the final code has a lot of logic living directly inside the UI classes rather than being strictly separated, this project was an incredible sandbox for learning about advanced list manipulation, gesture handling, and state edge cases.

## Running the Project (Android)

Since this repository contains only the core source code (`lib`) and resources (`assets`), please follow these steps to integrate and run it on an Android device or emulator:

1. Create a new Flutter project.
2. Navigate into your new project directory.
3. Replace the contents of the newly created lib folder with the files from this repository.
4. Open the pubspec.yaml file and add the required dependencies:
```yaml
dependencies:
  flutter:
    sdk: flutter
  hive_ce: ^2.11.0
  hive_ce_flutter: ^1.1.0
  cupertino_icons: ^1.0.8
  google_fonts: ^6.3.2
  intl: ^0.20.2

dev_dependencies:
  flutter_test:
    sdk: flutter
  hive_ce_generator: ^1.8.1
  build_runner: ^2.4.0
  flutter_lints: ^5.0.0
```

5. Install the dependencies.
6. Connect an Android device or start an Android emulator, then run.

## Preview

![App Demo](https://github.com/ingvar-m/miniproject-simple-todo-app/blob/main/simple_todo_app.gif)
