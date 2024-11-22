import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: SecretBoxesPage(),
    );
  }
}

class SecretBoxesPage extends StatelessWidget {
  final List<String> secrets = [
    'Secret 1: Flutter Rocks!',
    'Secret 2: Dart is awesome!',
    'Secret 3: Mobile Development is Fun!',
    'Secret 4: Keep Learning!',
    'Secret 5: Flutter is Cross-Platform!',
    'Secret 6: Always Code!',
    'Secret 7: Stay Motivated!',
    'Secret 8: Never Stop Exploring!',
    'Secret 9: Write Clean Code!'
  ];

  void copyToClipboard(String secret) {
    Clipboard.setData(ClipboardData(text: secret));
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Click a Box for a Secret')),
      body: GridView.builder(
        gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
          crossAxisCount: 3, // 3 boxes in each row
          crossAxisSpacing: 10,
          mainAxisSpacing: 10,
        ),
        itemCount: secrets.length,
        itemBuilder: (context, index) {
          return GestureDetector(
            onTap: () {
              copyToClipboard(secrets[index]);
              ScaffoldMessenger.of(context).showSnackBar(SnackBar(
                content: Text('Copied: ${secrets[index]}'),
              ));
            },
            child: Container(
              alignment: Alignment.center,
              color: Colors.blueAccent,
              child: Text(
                'Box ${index + 1}',
                style: TextStyle(color: Colors.white, fontSize: 18),
              ),
            ),
          );
        },
      ),
    );
  }
}
