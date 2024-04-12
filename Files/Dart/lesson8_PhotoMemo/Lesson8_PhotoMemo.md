# Photomemo

<!--Add Description of project-->

1. Create new folder for lesson 8
2. Copy lesson 7 to lesson 8 folder and rename
3. Link new github repository to project.
   * [Remove](../../Cheat_Sheets/github.md#remove-remote-repo) git repository if needed
   * [Add](../../Cheat_Sheets/github.md#add-remote-repo) new git repository

## Create Detail View Screen
<!--Add Description-->

From user homescreen as we click on the photomemo nvigate to new screen to view all the details about the photomemo. Eventually we would like to edit the information.

1. In view folder create file name `detailview_screen.dart`

   ```Dart
   import 'package:flutter/material.dart';

   class DetailViewScreen extends StatefulWidget {
     const DetailViewScreen({super.key});

     static const routeName = '/detailViewScreen';

     @override
     State<StatefulWidget> createState() {
       return DetailViewState();
     }
   }

   class DetailViewState extends State<DetailViewScreen> {
     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(
           title: const Text('Detail View'),
         ),
         body: const Text('Detail View'),
       );
     }
   }
   ```

2. Navigate to `main.dart` file.\
   Add route information for new view under `routes:{}` in main file

   ``` Dart
   DetailViewScreen.routeName: (context) => const DetailViewScreen(),
   ```

## General Purpose Error Screen

When we pass values from one screen to another we need to check the validity of the values. If the values is not valid we need to show the error on the screen.

1. In the view folder create a new file `error_screen`

   ``` Dart
   import 'package:flutter/material.dart';

   class ErrorScreen extends StatelessWidget {
     final String error;
     const ErrorScreen(this.error, {super.key});

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(
           title: const Text('Internal Error'),
         ),
         body: Text(
           'Restart the app, please!\n$error',
           style: const TextStyle(
             color: Colors.red,
             fontSize: 18.0,
           ),
         ),
       );
     }
   }
   ```

## Add onTap

Adding onTap function to photos located on the homescreen to preform and action when they are tapped. Navigate to detail view of the photo

onTap takes one corresponding object as a function without any parameters. We need to pass the index from PhotoMemoList tile. Pass index by calling a function.

1. Add onTap to `Widget showPhotoMemoList`

   ``` Dart
   onTap: () => con.onTap(index)
   ```

2. Create new function that will be called by onTap in `home_controller.dart`\
   *Since `photoMemoList` could be null in `home_model.dart` need to add an exclamation to the function. To say not null which is guaranteed because the user have already signed in.*

   ``` Dart
   void onTap(int index) {
      Navigator.pushNamed(state.context,
      DetailViewScreen.routeName,
      arguments: state.model.photoMemoList![index],
      );
   }
   ```

   *The information is passed to the view through the mail call to DetailViewScreen, this needs to be modified to pass in a body to the function.*

3. In `main.dart` add body to detail view route.
   <!-- Diff -->
   ```diff
   - DetailViewScreen.routeName:(context) => const DetailViewScreen(),
   + DetailViewScreen.routeName:(context) {
   +     Object? args = ModalRoute.of(context)?.settings.arguments;
   +     const DetailViewScreen();
   +  },
   ```
