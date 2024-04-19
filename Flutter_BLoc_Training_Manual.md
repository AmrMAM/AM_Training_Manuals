 ## Table of Content:
1. **Introduction to Bloc and Cubit**
   - What is Bloc
   - What is Cubit
   - When to use Bloc vs. Cubit

2. **Lab 1: Setting Up a Basic Flutter Project**
   - Creating a New Project
   - Setting Up Routing
   - Testing Navigation

3. **Lab 2: Creating Your First Cubit**
   - Creating a Counter Cubit
   - Integrating Cubit with UI
   - Testing the Cubit

4. **Lab 3: Transitioning from Cubit to Bloc**
   - Creating a Counter Bloc
   - Integrating Bloc with UI
   - Testing the Bloc

5. **Lab 4: Working with Bloc for Complex States**
   - Creating an Authentication Bloc
   - Creating the Login UI
   - Testing the Authentication Flow

6. **Lab 5: Bloc and Cubit with API Integration**
   - Creating a Repository for API Calls
   - Creating a Bloc for Data Fetching
   - Integrating Bloc with UI
   - Testing API Integration

7. **Lab 6: Handling Navigation with Bloc/Cubit**
   - Modifying Authentication Bloc for Navigation
   - Setting Up Navigation
   - Testing Navigation Logic

8. **Lab 7: Testing Bloc and Cubit**
   - Setting Up Testing Dependencies
   - Writing Tests for CounterCubit
   - Writing Tests for CounterBloc
   - Running the Tests

9. **Lab 8: Advanced Bloc Patterns**
    - Creating a Multi-Step Form Bloc
    - Creating the Multi-Step Form UI
    - Testing the Multi-Step Form

10. **Lab 9: Using BlocObserver for Logging**
    - Creating a BlocObserver
    - Integrating BlocObserver with Your App
    - Testing the BlocObserver

11. **Lab 10: Bloc-to-Bloc Communication**
    - Creating Two Blocs
    - Integrating Both Blocs in Your UI
    - Testing Bloc-to-Bloc Communication

12. **Lab 11: Integrating Firebase with Bloc**
    - Setting Up Firebase
    - Adding Firebase Dependencies
    - Creating a Firebase Service
    - Creating a Bloc for Firebase Data
    - Integrating Firebase with Your UI
    - Testing Firebase Integration

13. **Lab 12: GraphQL Integration with Bloc**
    - Setting Up GraphQL Dependencies
    - Setting Up GraphQL Client
    - Creating a Bloc for GraphQL Data
    - Integrating GraphQL with Your UI
    - Testing GraphQL Integration

14. **Lab 13: Using BlocListener for Side Effects**
    - Creating a Bloc
    - Creating the UI with BlocListener
    - Testing BlocListener

15. **Lab 14: Using HydratedBloc for State Persistence**
    - Setting Up HydratedBloc
    - Creating a Hydrated Bloc
    - Integrating HydratedBloc with Your App
    - Testing HydratedBloc

16. **Conclusion**

---

### **Introduction to Bloc and Cubit**

**What is Bloc?**
- Bloc (Business Logic Component) is a predictable state management library that helps separate presentation from business logic in Flutter applications. It leverages reactive programming and is designed to manage complex states in a clear and scalable way.

**What is Cubit?**
- Cubit is a lightweight version of Bloc that provides a simpler way to manage state. Unlike Bloc, which uses events and states, Cubit directly emits new states without requiring events.

**When to use Bloc vs. Cubit?**
- **Cubit** is ideal for simpler state management when there's no need for complex event handling.
- **Bloc** is better suited for more complex scenarios where you need to handle different types of events and transitions between states.

---

### **Lab 1: Setting Up a Basic Flutter Project**

**Objective:**  
Create a new Flutter project and set up basic routing between two screens.

**Steps:**

1. **Create a New Project:**
   - Open your terminal and run:
     ```bash
     flutter create bloc_cubit_lab
     ```
   - Navigate to the project directory:
     ```bash
     cd bloc_cubit_lab
     ```

2. **Set Up Routing:**
   - Open `lib/main.dart` and modify it to include a `MaterialApp` with routing:
     ```dart
     import 'package:flutter/material.dart';

     void main() {
       runApp(MyApp());
     }

     class MyApp extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return MaterialApp(
           title: 'Bloc Cubit Lab',
           theme: ThemeData(
             primarySwatch: Colors.blue,
           ),
           initialRoute: '/',
           routes: {
             '/': (context) => HomeScreen(),
             '/details': (context) => DetailsScreen(),
           },
         );
       }
     }

     class HomeScreen extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return Scaffold(
           appBar: AppBar(title: Text('Home')),
           body: Center(
             child: ElevatedButton(
               onPressed: () {
                 Navigator.pushNamed(context, '/details');
               },
               child: Text('Go to Details'),
             ),
           ),
         );
       }
     }

     class DetailsScreen extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return Scaffold(
           appBar: AppBar(title: Text('Details')),
           body: Center(child: Text('Details Screen')),
         );
       }
     }
     ```

3. **Test Navigation:**
   - Run the app using an emulator or a physical device:
     ```bash
     flutter run
     ```
   - Verify that clicking the "Go to Details" button navigates you to the DetailsScreen.

**Output:**  
A basic Flutter project with two screens and functional navigation.

---

### **Lab 2: Creating Your First Cubit**

**Objective:**  
Understand and implement a simple Cubit to manage a counter.

**Steps:**

1. **Create a Counter Cubit:**
   - Create a new file `lib/counter_cubit.dart` and define a simple Cubit to manage the counter state:
     ```dart
     import 'package:bloc/bloc.dart';

     class CounterCubit extends Cubit<int> {
       CounterCubit() : super(0);

       void increment() => emit(state + 1);
       void decrement() => emit(state - 1);
     }
     ```

2. **Integrate Cubit with UI:**
   - Modify `main.dart` to use the `CounterCubit`:
     ```dart
     import 'package:flutter/material.dart';
     import 'package:flutter_bloc/flutter_bloc.dart';
     import 'counter_cubit.dart';

     void main() {
       runApp(MyApp());
     }

     class MyApp extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return MaterialApp(
           title: 'Bloc Cubit Lab',
           theme: ThemeData(
             primarySwatch: Colors.blue,
           ),
           home: BlocProvider(
             create: (context) => CounterCubit(),
             child: HomeScreen(),
           ),
         );
       }
     }

     class HomeScreen extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return Scaffold(
           appBar: AppBar(title: Text('Counter')),
           body: Center(
             child: Column(
               mainAxisAlignment: MainAxisAlignment.center,
               children: <Widget>[
                 BlocBuilder<CounterCubit, int>(
                   builder: (context, count) {
                     return Text('$count', style: Theme.of(context).textTheme.headline4);
                   },
                 ),
                 Row(
                   mainAxisAlignment: MainAxisAlignment.center,
                   children: [
                     IconButton(
                       icon: Icon(Icons.remove),
                       onPressed: () => context.read<CounterCubit>().decrement(),
                     ),
                     IconButton(
                       icon: Icon(Icons.add),
                       onPressed: () => context.read<CounterCubit>().increment(),
                     ),
                   ],
                 ),
               ],
             ),
           ),
         );
       }
     }
     ```

3. **Test the Cubit:**
   - Run the app and test the counter functionality. Ensure that pressing the buttons increments and decrements the counter value.

**Output:**  
A Flutter app with a functional counter using Cubit.

---

### **Lab 3: Transitioning from Cubit to Bloc**

**Objective:**  
Understand the difference between Cubit and Bloc by converting a Cubit into a Bloc.

**Steps:**

1. **Create a Counter Bloc:**
   - Create a new file `lib/counter_bloc.dart` and define a Bloc with events and states:
     ```dart
     import 'package:flutter_bloc/flutter_bloc.dart';

     abstract class CounterEvent {}

     class Increment extends CounterEvent {}

     class Decrement extends CounterEvent {}

     class CounterBloc extends Bloc<CounterEvent, int> {
       CounterBloc() : super(0);

       @override
       Stream<int> mapEventToState(CounterEvent event) async* {
         if (event is Increment) {
           yield state + 1;
         } else if (event is Decrement) {
           yield state - 1;
         }
       }
     }
     ```

2. **Integrate Bloc with UI:**
   - Replace the Cubit in `main.dart` with the Bloc:
     ```dart
     import 'package:flutter/material.dart';
     import 'package:flutter_bloc/flutter_bloc.dart';
     import 'counter_bloc.dart';

     void main() {
       runApp(MyApp());
     }

     class MyApp extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return MaterialApp(
           title: 'Bloc Cubit Lab',
           theme: ThemeData(
             primarySwatch: Colors.blue,
           ),
           home: BlocProvider(
             create: (context) => CounterBloc(),
             child: HomeScreen(),
           ),
         );
       }
     }

     class HomeScreen extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return Scaffold(
           appBar: AppBar(title: Text('Counter')),
           body: Center(
             child: Column(
               mainAxisAlignment: MainAxisAlignment.center,
               children: <Widget>[
                 BlocBuilder<CounterBloc, int>(
                   builder: (context, count) {
                     return Text('$count', style: Theme.of(context).textTheme.headline4);
                   },
                 ),
                 Row(
                   mainAxisAlignment: MainAxisAlignment.center,
                   children: [
                     IconButton(
                       icon: Icon(Icons.remove),
                       onPressed: () => context.read<CounterBloc>().add(Decrement()),
                     ),
                     IconButton(
                       icon: Icon(Icons.add),
                       onPressed: () => context.read<CounterBloc>().add(Increment()),
                     ),
                   ],
                 ),
               ],
             ),
           ),
         );
       }
     }
     ```

3. **Test the Bloc:**
   - Run the app again and verify that the counter still works as expected, but now using Bloc instead of Cubit.

**Output:**  
A Flutter app where the counter is managed using Bloc instead of Cubit.

---

### **Lab 4: Working with Bloc for Complex States**

**Objective:**  
Manage complex states using Bloc by simulating an authentication flow.

**Steps:**

1. **Create an Authentication Bloc:**
   - Create a new file `lib/authentication_bloc.dart`:
     ```dart
     import 'package:flutter_bloc/flutter_bloc.dart';

     abstract class AuthenticationEvent {}

     class Login extends AuthenticationEvent {}

     class Logout extends AuthenticationEvent {}

     abstract class AuthenticationState {}

     class Unauthenticated extends AuthenticationState {}

     class Authenticated extends AuthenticationState {}

     class AuthenticationLoading extends AuthenticationState {}

     class AuthenticationBloc extends Bloc<AuthenticationEvent, AuthenticationState> {
       AuthenticationBloc() : super(Unauthenticated());

       @override
       Stream<AuthenticationState> mapEventToState(AuthenticationEvent event) async* {
         if (event is Login) {
           yield AuthenticationLoading();
           await Future.delayed(Duration(seconds: 2)); // Simulate a network call
           yield Authenticated();
         } else if (event is Logout) {
           yield Unauthenticated();
         }
       }
     }
     ```

2. **Create the Login UI:**
   - Modify `main.dart` to use the AuthenticationBloc:
     ```dart
     import 'package:flutter/material.dart';
     import 'package:flutter_bloc/flutter_bloc.dart';
     import 'authentication_bloc.dart';

     void main() {
       runApp(MyApp());
     }

     class MyApp extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return MaterialApp(
           title: 'Bloc Cubit Lab',
           theme: ThemeData(
             primarySwatch: Colors.blue,
           ),
           home: BlocProvider(
             create: (context) => AuthenticationBloc(),
             child: AuthenticationScreen(),
           ),
         );
       }
     }

     class AuthenticationScreen extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return Scaffold(
           appBar: AppBar(title: Text('Authentication')),
           body: BlocBuilder<AuthenticationBloc, AuthenticationState>(
             builder: (context, state) {
               if (state is AuthenticationLoading) {
                 return Center(child: CircularProgressIndicator());
               } else if (state is Authenticated) {
                 return Center(
                   child: Column(
                     mainAxisAlignment: MainAxisAlignment.center,
                     children: [
                       Text('Logged In'),
                       ElevatedButton(
                         onPressed: () => context.read<AuthenticationBloc>().add(Logout()),
                         child: Text('Logout'),
                       ),
                     ],
                   ),
                 );
               } else if (state is Unauthenticated) {
                 return Center(
                   child: ElevatedButton(
                     onPressed: () => context.read<AuthenticationBloc>().add(Login()),
                     child: Text('Login'),
                   ),
                 );
               }
               return Container();
             },
           ),
         );
       }
     }
     ```

3. **Test the Authentication Flow:**
   - Run the app and test the login flow. The app should display a loading indicator before showing the authenticated state.

**Output:**  
A login screen that simulates authentication using Bloc.

---

### **Lab 5: Bloc and Cubit with API Integration**

**Objective:**  
Fetch data from an API and manage the state using Bloc or Cubit.

**Steps:**

1. **Create a Repository for API Calls:**
   - Create a new file `lib/repository.dart`:
     ```dart
     import 'package:http/http.dart' as http;
     import 'dart:convert';

     class DataRepository {
       Future<List<String>> fetchData() async {
         final response = await http.get(Uri.parse('https://jsonplaceholder.typicode.com/posts'));

         if (response.statusCode == 200) {
           List<dynamic> data = json.decode(response.body);
           return data.map((item) => item['title'].toString()).toList();
         } else {
           throw Exception('Failed to load data');
         }
       }
     }
     ```

2. **Create a Bloc for Data Fetching:**
   - Create a new file `lib/data_bloc.dart`:
     ```dart
     import 'package:flutter_bloc/flutter_bloc.dart';
     import 'repository.dart';

     abstract class DataEvent {}

     class FetchData extends DataEvent {}

     abstract class DataState {}

     class DataInitial extends DataState {}

     class DataLoading extends DataState {}

     class DataLoaded extends DataState {
       final List<String> data;
       DataLoaded(this.data);
     }

     class DataError extends DataState {}

     class DataBloc extends Bloc<DataEvent, DataState> {
       final DataRepository repository;

       DataBloc(this.repository) : super(DataInitial());

       @override
       Stream<DataState> mapEventToState(DataEvent event) async* {
         if (event is FetchData) {
           yield DataLoading();
           try {
             final data = await repository.fetchData();
             yield DataLoaded(data);
           } catch (e) {
             yield DataError();
           }
         }
       }
     }
     ```

3. **Integrate Bloc with UI:**
   - Modify `main.dart` to use the `DataBloc`:
     ```dart
     import 'package:flutter/material.dart';
     import 'package:flutter_bloc/flutter_bloc.dart';
     import 'data_bloc.dart';
     import 'repository.dart';

     void main() {
       runApp(MyApp());
     }

     class MyApp extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return MaterialApp(
           title: 'Bloc Cubit Lab',
           theme: ThemeData(
             primarySwatch: Colors.blue,
           ),
           home: BlocProvider(
             create: (context) => DataBloc(DataRepository())..add(FetchData()),
             child: DataScreen(),
           ),
         );
       }
     }

     class DataScreen extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return Scaffold(
           appBar: AppBar(title: Text('Data')),
           body: BlocBuilder<DataBloc, DataState>(
             builder: (context, state) {
               if (state is DataLoading) {
                 return Center(child: CircularProgressIndicator());
               } else if (state is DataLoaded) {
                 return ListView.builder(
                   itemCount: state.data.length,
                   itemBuilder: (context, index) {
                     return ListTile(title: Text(state.data[index]));
                   },
                 );
               } else if (state is DataError) {
                 return Center(child: Text('Failed to fetch data'));
               }
               return Container();
             },
           ),
         );
       }
     }
     ```

4. **Test API Integration:**
   - Run the app and verify that it fetches and displays the data from the API.

**Output:**  
A Flutter app that displays data from an API, handling loading and error states.

---

### **Lab 6: Handling Navigation with Bloc/Cubit**

**Objective:**  
Implement navigation logic using Bloc/Cubit.

**Steps:**

1. **Modify Authentication Bloc for Navigation:**
   - Update the `AuthenticationBloc` to include navigation logic:
     ```dart
     import 'package:flutter_bloc/flutter_bloc.dart';
     import 'package:flutter/material.dart';

     abstract class AuthenticationEvent {}

     class Login extends AuthenticationEvent {}

     class Logout extends AuthenticationEvent {}

     abstract class AuthenticationState {}

     class Unauthenticated extends AuthenticationState {}

     class Authenticated extends AuthenticationState {}

     class AuthenticationLoading extends AuthenticationState {}

     class AuthenticationBloc extends Bloc<AuthenticationEvent, AuthenticationState> {
       final BuildContext context;

       AuthenticationBloc(this.context) : super(Unauthenticated());

       @override
       Stream<AuthenticationState> mapEventToState(AuthenticationEvent event) async* {
         if (event is Login) {
           yield AuthenticationLoading();
           await Future.delayed(Duration(seconds: 2)); // Simulate a network call
           yield Authenticated();
           Navigator.pushReplacementNamed(context, '/home');
         } else if (event is Logout) {
           yield Unauthenticated();
           Navigator.pushReplacementNamed(context, '/login');
         }
       }
     }
     ```

2. **Set Up Navigation:**
   - Modify `main.dart` to include navigation routes:
     ```dart
     import 'package:flutter/material.dart';
     import 'package:flutter_bloc/flutter_bloc.dart';
     import 'authentication_bloc.dart';

     void main() {
       runApp(MyApp());
     }

     class MyApp extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return MaterialApp(
           title: 'Bloc Cubit Lab',
           theme: ThemeData(
             primarySwatch: Colors.blue,
           ),
           initialRoute: '/login',
           routes: {
             '/login': (context) => BlocProvider(
                   create: (context) => AuthenticationBloc(context),
                   child: AuthenticationScreen(),
                 ),
             '/home': (context) => HomeScreen(),
           },
         );
       }
     }

     class AuthenticationScreen extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return Scaffold(
           appBar: AppBar(title: Text('Login')),
           body: BlocBuilder<AuthenticationBloc, AuthenticationState>(
             builder: (context, state) {
               if (state is AuthenticationLoading) {
                 return Center(child: CircularProgressIndicator());
               } else if (state is Unauthenticated) {
                 return Center(
                   child: ElevatedButton(
                     onPressed: () => context.read<AuthenticationBloc>().add(Login()),
                     child: Text('Login'),
                   ),
                 );
               }
               return Container();
             },
           ),
         );
       }
     }

     class HomeScreen extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return Scaffold(
           appBar: AppBar(title: Text('Home')),
           body: Center(
             child: ElevatedButton(
               onPressed: () => context.read<AuthenticationBloc>().add(Logout()),
               child: Text('Logout'),
             ),
           ),
         );
       }
     }
     ```

3. **Test Navigation Logic:**
   - Run the app and verify that successful login navigates to the home screen, and logging out navigates back to the login screen.

**Output:**  
A Flutter app that uses Bloc for state-based navigation.

---

### **Lab 7: Testing Bloc and Cubit**

**Objective:**  
Write unit tests for Bloc and Cubit to ensure they function correctly.

**Steps:**

1. **Set Up Testing Dependencies:**
   - Add the necessary testing packages to your `pubspec.yaml`:
     ```yaml
     dev_dependencies:
       flutter_test:
         sdk: flutter
       bloc_test: ^9.0.3
       mocktail: ^0.3.0
     ```

2. **Write Tests for CounterCubit:**
   - Create a new file `test/counter_cubit_test.dart`:
     ```dart
     import 'package:flutter_test/flutter_test.dart';
     import 'package:bloc_cubit_lab/counter_cubit.dart';

     void main() {
       group('CounterCubit', () {
         test('initial state is 0', () {
           expect(CounterCubit().state, 0);
         });

         test('emits [1] when increment is called', () {
           final cubit = CounterCubit();
           cubit.increment();
           expect(cubit.state, 1);
         });

         test('emits [-1] when decrement is called', () {
           final cubit = CounterCubit();
           cubit.decrement();
           expect(cubit.state, -1);
         });
       });
     }
     ```

3. **Write Tests for CounterBloc:**
   - Create a new file `test/counter_bloc_test.dart`:
     ```dart
     import 'package:flutter_test/flutter_test.dart';
     import 'package:bloc_cubit_lab/counter_bloc.dart';
     import 'package:bloc_test/bloc_test.dart';

     void main() {
       group('CounterBloc', () {
         blocTest<CounterBloc, int>(
           'emits [1] when Increment is added',
           build: () => CounterBloc(),
           act: (bloc) => bloc.add(Increment()),
           expect: () => [1],
         );

         blocTest<CounterBloc, int>(
           'emits [-1] when Decrement is added',
           build: () => CounterBloc(),
           act: (bloc) => bloc.add(Decrement()),
           expect: () => [-1],
         );
       });
     }
     ```

4. **Run the Tests:**
   - Run your tests using:
     ```bash
     flutter test
     ```

5. **Ensure All Tests Pass:**
   - Verify that all the test cases pass successfully.

**Output:**  
Test cases that ensure the Bloc/Cubit logic works as expected.

---

### **Lab 8: Advanced Bloc Patterns**

**Objective:**  
Explore advanced Bloc patterns and best practices by implementing a multi-step form and Bloc-to-Bloc communication.

**Steps:**

1. **Create a Multi-Step Form Bloc:**
   - Create a new file `lib/multi_step_form_bloc.dart`:
     ```dart
     import 'package:flutter_bloc/flutter_bloc.dart';

     abstract class FormEvent {}

     class NextStep extends FormEvent {}

     class PreviousStep extends FormEvent {}

     class SubmitForm extends FormEvent {}

     class MultiStepFormBloc extends Bloc<FormEvent, int> {
       MultiStepFormBloc() : super(0);

       @override
       Stream<int> mapEventToState(FormEvent event) async* {
         if (event is NextStep && state < 2) {
           yield state + 1;
         } else if (event is PreviousStep && state > 0) {
           yield state - 1;
         } else if (event is SubmitForm) {
           yield 0; // Reset after submission
         }
       }
     }
     ```

2. **Create the Multi-Step Form UI:**
   - Modify `main.dart` to include the multi-step form:
     ```dart
     import 'package:flutter/material.dart';
     import 'package:flutter_bloc/flutter_bloc.dart';
     import 'multi_step_form_bloc.dart';

     void main() {
       runApp(MyApp());
     }

     class MyApp extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return MaterialApp(
           title: 'Bloc Cubit Lab',
           theme: ThemeData(
             primarySwatch: Colors.blue,
           ),
           home: BlocProvider(
             create: (context) => MultiStepFormBloc(),
             child: MultiStepFormScreen(),
           ),
         );
       }
     }

     class MultiStepFormScreen extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return Scaffold(
           appBar: AppBar(title: Text('Multi-Step Form')),
           body: BlocBuilder<MultiStepFormBloc, int>(
             builder: (context, step) {
               return Column(
                 mainAxisAlignment: MainAxisAlignment.center,
                 children: [
                   if (step == 0) Text('Step 1: Basic Information'),
                   if (step == 1) Text('Step 2: Additional Details'),
                   if (step == 2) Text('Step 3: Review & Submit'),
                   Row(
                     mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                     children: [
                       if (step > 0)
                         ElevatedButton(
                           onPressed: () => context.read<MultiStepFormBloc>().add(PreviousStep()),
                           child: Text('Previous'),
                         ),
                       if (step < 2)
                         ElevatedButton(
                           onPressed: () => context.read<MultiStepFormBloc>().add(NextStep()),
                           child: Text('Next'),
                         ),
                       if (step == 2)
                         ElevatedButton(
                           onPressed: () => context.read<MultiStepFormBloc>().add(SubmitForm()),
                           child: Text('Submit'),
                         ),
                     ],
                   ),
                 ],
               );
             },
           ),
         );
       }
     }
     ```

3. **Test the Multi-Step Form:**
   - Run the app and test the form flow. Ensure that you can navigate between steps and submit the form.

**Output:**  
A complex Flutter app utilizing advanced Bloc patterns with a multi-step form.

---

### **Lab 9: Using BlocObserver for Logging**

**Objective:**  
Implement a `BlocObserver` to log state changes and transitions in your application.

**Steps:**

1. **Create a BlocObserver:**
   - Create a new file `lib/bloc_observer.dart`:
     ```dart
     import 'package:bloc/bloc.dart';

     class AppBlocObserver extends BlocObserver {
       @override
       void onEvent(Bloc bloc, Object? event) {
         super.onEvent(bloc, event);
         print('Event: $event');
       }

       @override
       void onTransition(Bloc bloc, Transition transition) {
         super.onTransition(bloc, transition);
         print('Transition: $transition');
       }

       @override
       void onError(Bloc bloc, Object error, StackTrace stackTrace) {
         super.onError(bloc, error, stackTrace);
         print('Error: $error');
       }
     }
     ```

2. **Integrate BlocObserver with Your App:**
   - Modify `main.dart` to use the `AppBlocObserver`:
     ```dart
     import 'package:flutter/material.dart';
     import 'package:flutter_bloc/flutter_bloc.dart';
     import 'bloc_observer.dart';

     void main() {
       Bloc.observer = AppBlocObserver();
       runApp(MyApp());
     }

     class MyApp extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return MaterialApp(
           title: 'Bloc Observer Lab',
           theme: ThemeData(
             primarySwatch: Colors.blue,
           ),
           home: HomeScreen(),
         );
       }
     }

     class HomeScreen extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return Scaffold(
           appBar: AppBar(title: Text('Home')),
           body: Center(child: Text('Check your console for Bloc logs.')),
         );
       }
     }
     ```

3. **Test the BlocObserver:**
   - Run your app and observe the logs in the console. You should see logs for events, transitions, and errors as they occur.

**Output:**  
A functioning `BlocObserver` that logs Bloc events, transitions, and errors to the console.

---

### **Lab 10: Bloc-to-Bloc Communication**

**Objective:**  
Implement communication between multiple Blocs within an application.

**Steps:**

1. **Create Two Blocs:**
   - Create `lib/user_bloc.dart`:
     ```dart
     import 'package:flutter_bloc/flutter_bloc.dart';

     abstract class UserEvent {}

     class FetchUser extends UserEvent {}

     abstract class UserState {}

     class UserInitial extends UserState {}

     class UserLoading extends UserState {}

     class UserLoaded extends UserState {
       final String userName;
       UserLoaded(this.userName);
     }

     class UserError extends UserState {}

     class UserBloc extends Bloc<UserEvent, UserState> {
       UserBloc() : super(UserInitial());

       @override
       Stream<UserState> mapEventToState(UserEvent event) async* {
         if (event is FetchUser) {
           yield UserLoading();
           await Future.delayed(Duration(seconds: 2)); // Simulate network call
           yield UserLoaded('John Doe');
         }
       }
     }
     ```

   - Create `lib/profile_bloc.dart`:
     ```dart
     import 'package:flutter_bloc/flutter_bloc.dart';
     import 'user_bloc.dart';

     abstract class ProfileEvent {}

     class LoadProfile extends ProfileEvent {}

     abstract class ProfileState {}

     class ProfileInitial extends ProfileState {}

     class ProfileLoaded extends ProfileState {
       final String userName;
       ProfileLoaded(this.userName);
     }

     class ProfileBloc extends Bloc<ProfileEvent, ProfileState> {
       final UserBloc userBloc;

       ProfileBloc(this.userBloc) : super(ProfileInitial()) {
         userBloc.stream.listen((userState) {
           if (userState is UserLoaded) {
             add(LoadProfile());
           }
         });
       }

       @override
       Stream<ProfileState> mapEventToState(ProfileEvent event) async* {
         if (event is LoadProfile) {
           final userName = (userBloc.state as UserLoaded).userName;
           yield ProfileLoaded(userName);
         }
       }
     }
     ```

2. **Integrate Both Blocs in Your UI:**
   - Modify `main.dart`:
     ```dart
     import 'package:flutter/material.dart';
     import 'package:flutter_bloc/flutter_bloc.dart';
     import 'user_bloc.dart';
     import 'profile_bloc.dart';

     void main() {
       runApp(MyApp());
     }

     class MyApp extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return MaterialApp(
           title: 'Bloc-to-Bloc Communication',
           theme: ThemeData(
             primarySwatch: Colors.blue,
           ),
           home: MultiBlocProvider(
             providers: [
               BlocProvider(create: (context) => UserBloc()),
               BlocProvider(
                 create: (context) => ProfileBloc(context.read<UserBloc>())
                   ..add(LoadProfile()),
               ),
             ],
             child: ProfileScreen(),
           ),
         );
       }
     }

     class ProfileScreen extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return Scaffold(
           appBar: AppBar(title: Text('Profile')),
           body: BlocBuilder<ProfileBloc, ProfileState>(
             builder: (context, state) {
               if (state is ProfileLoaded) {
                 return Center(child: Text('User Name: ${state.userName}'));
               }
               return Center(child: Text('Loading...'));
             },
           ),
           floatingActionButton: FloatingActionButton(
             onPressed: () => context.read<UserBloc>().add(FetchUser()),
             child: Icon(Icons.refresh),
           ),
         );
       }
     }
     ```

3. **Test Bloc-to-Bloc Communication:**
   - Run the app and verify that the `ProfileScreen` displays the user name once the `UserBloc` fetches the user data.

**Output:**  
An app where two Blocs communicate, with the `ProfileBloc` reacting to state changes in the `UserBloc`.

---

### **Lab 11: Integrating Firebase with Bloc**

**Objective:**  
Fetch and manage data from Firebase using Bloc.

**Steps:**

1. **Set Up Firebase:**
   - Follow the [Firebase setup guide for Flutter](https://firebase.flutter.dev/docs/overview) to add Firebase to your Flutter project.

2. **Add Firebase Dependencies:**
   - Update `pubspec.yaml`:
     ```yaml
     dependencies:
       flutter:
         sdk: flutter
       flutter_bloc: ^8.0.0
       firebase_core: ^2.7.0
       cloud_firestore: ^5.0.0
     ```

3. **Create a Firebase Service:**
   - Create `lib/firebase_service.dart`:
     ```dart
     import 'package:cloud_firestore/cloud_firestore.dart';

     class FirebaseService {
       final FirebaseFirestore _firestore = FirebaseFirestore.instance;

       Future<List<String>> fetchItems() async {
         final snapshot = await _firestore.collection('items').get();
         return snapshot.docs.map((doc) => doc['name'] as String).toList();
       }
     }
     ```

4. **Create a Bloc for Firebase Data:**
   - Create `lib/firebase_bloc.dart`:
     ```dart
     import 'package:flutter_bloc/flutter_bloc.dart';
     import 'firebase_service.dart';

     abstract class FirebaseEvent {}

     class LoadItems extends FirebaseEvent {}

     abstract class FirebaseState {}

     class FirebaseInitial extends FirebaseState {}

     class FirebaseLoading extends FirebaseState {}

     class FirebaseLoaded extends FirebaseState {
       final List<String> items;
       FirebaseLoaded(this.items);
     }

     class FirebaseError extends FirebaseState {}

     class FirebaseBloc extends Bloc<FirebaseEvent, FirebaseState> {
       final FirebaseService firebaseService;

       FirebaseBloc(this.firebaseService) : super(FirebaseInitial());

       @override
       Stream<FirebaseState> mapEventToState(FirebaseEvent event) async* {
         if (event is LoadItems) {
           yield FirebaseLoading();
           try {
             final items = await firebaseService.fetchItems();
             yield FirebaseLoaded(items);
           } catch (e) {
             yield FirebaseError();
           }
         }
       }
     }
     ```

5. **Integrate Firebase with Your UI:**
   - Modify `main.dart`:
     ```dart
     import 'package:flutter/material.dart';
     import 'package:flutter_bloc/flutter_bloc.dart';
     import 'firebase_service.dart';
     import 'firebase_bloc.dart';
     import 'package:firebase_core/firebase_core.dart';

     void main() async {
       WidgetsFlutterBinding.ensureInitialized();
       await Firebase.initializeApp();
       runApp(MyApp());
     }

     class MyApp extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return MaterialApp(
           title: 'Firebase Bloc Integration',
           theme: ThemeData(
             primarySwatch: Colors.blue,
           ),
           home: BlocProvider(
             create: (context) => FirebaseBloc(FirebaseService())..add(LoadItems()),
             child: ItemsScreen(),
           ),
         );
       }
     }

     class ItemsScreen extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return Scaffold(
           appBar: AppBar(title: Text('Items')),
           body: BlocBuilder<FirebaseBloc, FirebaseState>(
             builder: (context, state) {
               if (state is FirebaseLoading) {
                 return Center(child

: CircularProgressIndicator());
               } else if (state is FirebaseLoaded) {
                 return ListView.builder(
                   itemCount: state.items.length,
                   itemBuilder: (context, index) {
                     return ListTile(title: Text(state.items[index]));
                   },
                 );
               } else if (state is FirebaseError) {
                 return Center(child: Text('Failed to load items'));
               }
               return Container();
             },
           ),
         );
       }
     }
     ```

6. **Test Firebase Integration:**
   - Run the app and verify that it correctly fetches and displays data from Firebase.

**Output:**  
A Flutter app that integrates with Firebase to fetch and display data using Bloc.

---

### **Lab 12: GraphQL Integration with Bloc**

**Objective:**  
Fetch and manage data from a GraphQL API using Bloc.

**Steps:**

1. **Set Up GraphQL Dependencies:**
   - Add dependencies to `pubspec.yaml`:
     ```yaml
     dependencies:
       flutter:
         sdk: flutter
       flutter_bloc: ^8.0.0
       graphql_flutter: ^6.0.0
     ```

2. **Set Up GraphQL Client:**
   - Create `lib/graphql_client.dart`:
     ```dart
     import 'package:graphql_flutter/graphql_flutter.dart';

     class GraphQLClientService {
       final GraphQLClient _client;

       GraphQLClientService()
           : _client = GraphQLClient(
             link: HttpLink('https://example.com/graphql'),
             cache: GraphQLCache(),
           );

       Future<List<String>> fetchItems() async {
         const String query = '''
           query {
             items {
               name
             }
           }
         ''';

         final result = await _client.query(QueryOptions(document: gql(query)));

         if (result.hasException) {
           throw Exception('Failed to load data');
         }

         return (result.data!['items'] as List)
             .map((item) => item['name'] as String)
             .toList();
       }
     }
     ```

3. **Create a Bloc for GraphQL Data:**
   - Create `lib/graphql_bloc.dart`:
     ```dart
     import 'package:flutter_bloc/flutter_bloc.dart';
     import 'graphql_client.dart';

     abstract class GraphQLEvent {}

     class LoadItems extends GraphQLEvent {}

     abstract class GraphQLState {}

     class GraphQLInitial extends GraphQLState {}

     class GraphQLLoading extends GraphQLState {}

     class GraphQLLoaded extends GraphQLState {
       final List<String> items;
       GraphQLLoaded(this.items);
     }

     class GraphQLError extends GraphQLState {}

     class GraphQLBloc extends Bloc<GraphQLEvent, GraphQLState> {
       final GraphQLClientService graphQLClientService;

       GraphQLBloc(this.graphQLClientService) : super(GraphQLInitial());

       @override
       Stream<GraphQLState> mapEventToState(GraphQLEvent event) async* {
         if (event is LoadItems) {
           yield GraphQLLoading();
           try {
             final items = await graphQLClientService.fetchItems();
             yield GraphQLLoaded(items);
           } catch (e) {
             yield GraphQLError();
           }
         }
       }
     }
     ```

4. **Integrate GraphQL with Your UI:**
   - Modify `main.dart`:
     ```dart
     import 'package:flutter/material.dart';
     import 'package:flutter_bloc/flutter_bloc.dart';
     import 'graphql_client.dart';
     import 'graphql_bloc.dart';

     void main() {
       runApp(MyApp());
     }

     class MyApp extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return MaterialApp(
           title: 'GraphQL Bloc Integration',
           theme: ThemeData(
             primarySwatch: Colors.blue,
           ),
           home: BlocProvider(
             create: (context) => GraphQLBloc(GraphQLClientService())..add(LoadItems()),
             child: ItemsScreen(),
           ),
         );
       }
     }

     class ItemsScreen extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return Scaffold(
           appBar: AppBar(title: Text('Items')),
           body: BlocBuilder<GraphQLBloc, GraphQLState>(
             builder: (context, state) {
               if (state is GraphQLLoading) {
                 return Center(child: CircularProgressIndicator());
               } else if (state is GraphQLLoaded) {
                 return ListView.builder(
                   itemCount: state.items.length,
                   itemBuilder: (context, index) {
                     return ListTile(title: Text(state.items[index]));
                   },
                 );
               } else if (state is GraphQLError) {
                 return Center(child: Text('Failed to load items'));
               }
               return Container();
             },
           ),
         );
       }
     }
     ```

5. **Test GraphQL Integration:**
   - Run the app and ensure that it fetches and displays data from the GraphQL API.

**Output:**  
A Flutter app that integrates with a GraphQL API to fetch and display data using Bloc.

---

### **Lab 13: Using BlocListener for Side Effects**

**Objective:**  
Implement `BlocListener` to handle side effects in response to state changes, such as showing snackbars or navigating between screens.

**Steps:**

1. **Create a Bloc:**
   - Create `lib/auth_bloc.dart`:
     ```dart
     import 'package:flutter_bloc/flutter_bloc.dart';

     abstract class AuthEvent {}

     class Login extends AuthEvent {}

     class Logout extends AuthEvent {}

     abstract class AuthState {}

     class AuthInitial extends AuthState {}

     class Authenticated extends AuthState {}

     class Unauthenticated extends AuthState {}

     class AuthBloc extends Bloc<AuthEvent, AuthState> {
       AuthBloc() : super(AuthInitial());

       @override
       Stream<AuthState> mapEventToState(AuthEvent event) async* {
         if (event is Login) {
           yield Authenticated();
         } else if (event is Logout) {
           yield Unauthenticated();
         }
       }
     }
     ```

2. **Create the UI with BlocListener:**
   - Modify `main.dart`:
     ```dart
     import 'package:flutter/material.dart';
     import 'package:flutter_bloc/flutter_bloc.dart';
     import 'auth_bloc.dart';

     void main() {
       runApp(MyApp());
     }

     class MyApp extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return MaterialApp(
           title: 'BlocListener Lab',
           theme: ThemeData(
             primarySwatch: Colors.blue,
           ),
           home: BlocProvider(
             create: (context) => AuthBloc(),
             child: HomeScreen(),
           ),
         );
       }
     }

     class HomeScreen extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return Scaffold(
           appBar: AppBar(title: Text('Home')),
           body: BlocListener<AuthBloc, AuthState>(
             listener: (context, state) {
               if (state is Authenticated) {
                 ScaffoldMessenger.of(context).showSnackBar(
                   SnackBar(content: Text('User authenticated')),
                 );
               } else if (state is Unauthenticated) {
                 ScaffoldMessenger.of(context).showSnackBar(
                   SnackBar(content: Text('User logged out')),
                 );
               }
             },
             child: Center(
               child: Column(
                 mainAxisAlignment: MainAxisAlignment.center,
                 children: [
                   ElevatedButton(
                     onPressed: () => context.read<AuthBloc>().add(Login()),
                     child: Text('Login'),
                   ),
                   ElevatedButton(
                     onPressed: () => context.read<AuthBloc>().add(Logout()),
                     child: Text('Logout'),
                   ),
                 ],
               ),
             ),
           ),
         );
       }
     }
     ```

3. **Test BlocListener:**
   - Run the app and interact with the login and logout buttons. Observe that `BlocListener` shows snackbars based on state changes.

**Output:**  
An application where `BlocListener` responds to state changes by showing appropriate snackbars.

---

### **Lab 14: Using HydratedBloc for State Persistence**

**Objective:**  
Implement `HydratedBloc` to persist and restore state across app restarts.

**Steps:**

1. **Set Up HydratedBloc:**
   - Add dependencies to `pubspec.yaml`:
     ```yaml
     dependencies:
       flutter:
         sdk: flutter
       flutter_bloc: ^8.0.0
       hydrated_bloc: ^10.0.0
       path_provider: ^2.0.8
     ```

2. **Create a Hydrated Bloc:**
   - Create `lib/counter_hydrated_bloc.dart`:
     ```dart
     import 'package:flutter_bloc/flutter_bloc.dart';
     import 'package:hydrated_bloc/hydrated_bloc.dart';

     class CounterEvent {}

     class Increment extends CounterEvent {}

     class Decrement extends CounterEvent {}

     class CounterState {
       final int count;
       CounterState(this.count);

       factory CounterState.fromJson(Map<String, dynamic> json) {
         return CounterState(json['count'] as int);
       }

       Map<String, dynamic> toJson() {
         return {'count': count};
       }
     }

     class CounterHydratedBloc extends HydratedBloc<CounterEvent, CounterState> {
       CounterHydratedBloc() : super(CounterState(0));

       @override
       Stream<CounterState> mapEventToState(CounterEvent event) async* {
         if (event is Increment) {
           yield CounterState(state.count + 1);
         } else if (event is Decrement) {
           yield CounterState(state.count - 1);
         }
       }

       @override
       CounterState? fromJson(Map<String, dynamic> json) {
         return CounterState.fromJson(json);
       }

       @override
       Map<String, dynamic>? toJson(CounterState state) {
         return state.toJson();
       }
     }
     ```

3. **Integrate HydratedBloc with Your App:**
   - Modify `main.dart`:
     ```dart
     import 'package:flutter/material.dart';
     import 'package:flutter_bloc/flutter_bloc.dart';
     import 'package:hydrated_bloc/hydrated_bloc.dart';
     import 'package:path_provider/path_provider.dart';
     import 'counter_hydrated_bloc.dart';

     Future<void> main() async {
       WidgetsFlutterBinding.ensureInitialized();
       final storage = await HydratedStorage.build(
         storageDirectory: await getApplicationDocumentsDirectory(),
       );
       HydratedBloc.storage = storage;
       runApp(MyApp());
     }

     class MyApp extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return MaterialApp(
           title: 'Hydrated Bloc Lab',
           theme: ThemeData(
             primarySwatch: Colors.blue,
           ),
           home: BlocProvider(
             create: (context) => CounterHydratedBloc(),
             child: CounterScreen(),
           ),
         );
       }
     }

     class CounterScreen extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return Scaffold(
           appBar: AppBar(title: Text('Counter')),
           body: Center(
             child: BlocBuilder<CounterHydratedBloc, CounterState>(
               builder: (context, state) {
                 return Column(
                   mainAxisAlignment: MainAxisAlignment.center,
                   children: [
                     Text('Count: ${state.count}', style: TextStyle(fontSize: 24)),
                     Row(
                       mainAxisAlignment: MainAxisAlignment.center,
                       children: [
                         ElevatedButton(
                           onPressed: () => context.read<CounterHydratedBloc>().add(Increment()),
                           child: Text('Increment'),
                         ),
                         SizedBox(width: 16),
                         ElevatedButton(
                           onPressed: () => context.read<CounterHydratedBloc>().add(Decrement()),
                           child: Text('Decrement'),
                         ),
                       ],
                     ),
                   ],
                 );
               },
             ),
           ),
         );
       }
     }
     ```

4. **Test HydratedBloc:**
   - Run the app, increment or decrement the counter, and then restart the app. Verify that the counter value persists after the app is restarted.

**Output:**  
An app that uses `HydratedBloc` to persist and restore counter state across app restarts.

---

### **Conclusion**

By completing these labs, you've gained hands-on experience with both Cubit and Bloc in Flutter, from simple state management to more complex patterns like API integration, navigation, and advanced state management techniques.

---
https://github.com/AmrMAM

Amr_MAM