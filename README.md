import 'package:flutter/material.dart';
import 'package:flutter_fortune_wheel/flutter_fortune_wheel.dart';
import 'dart:math';

void main() {
  runApp(SpinWheelApp());
}

class SpinWheelApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Spin Wheel App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SpinWheelPage(),
    );
  }
}

class SpinWheelPage extends StatefulWidget {
  @override
  _SpinWheelPageState createState() => _SpinWheelPageState();
}

class _SpinWheelPageState extends State<SpinWheelPage> {
  // Spin Wheel සදහා Item List එකක් නිර්මාණය කරන්න
  final List<String> items = [
    'Prize 1',
    'Prize 2',
    'Prize 3',
    'Prize 4',
    'Prize 5',
    'Prize 6',
  ];

  // Randomly එකක් තෝරාගැනීම සදහා
  int selected = 0;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Spin Wheel App'),
      ),
      body: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          FortuneWheel(
            selected: Stream.value(selected),
            items: [
              for (var item in items)
                FortuneItem(
                  child: Text(item),
                ),
            ],
            onAnimationEnd: () {
              // Spin Wheel එක අවසන් වූ විට විමසන්න
              ScaffoldMessenger.of(context).showSnackBar(
                SnackBar(content: Text('Congratulations! You won: ${items[selected]}')),
              );
            },
          ),
          SizedBox(height: 20),
          ElevatedButton(
            onPressed: () {
              // Spin Wheel එකක් තවත් වරක් හරවන්න
              setState(() {
                selected = Random().nextInt(items.length);
              });
            },
            child: Text('Spin'),
          ),
        ],
      ),
    );
  }
}
