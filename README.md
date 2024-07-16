import 'dart:async';
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Stopwatch',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Color.fromARGB(255, 12, 1, 2)),
        useMaterial3: true,
      ),
      home: const MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key});

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  late Stopwatch stopwatch;
  late Timer t;
  bool isRunning = false;

  void handleStartStop() {
    if (isRunning) {
      stopwatch.stop();
      isRunning = false;
    } else {
      stopwatch.start();
      isRunning = true;
    }
  }

  void handleReset() {
    stopwatch.reset();
  }

  String returnFormattedText() {
    var milli = stopwatch.elapsed.inMilliseconds;

    String milliseconds = (milli % 1000)
        .toString()
        .padLeft(3, "0");
    String seconds = ((milli ~/ 1000) % 60)
        .toString()
        .padLeft(2, "0"); 
    String minutes = ((milli ~/ 1000) ~/ 60)
        .toString()
        .padLeft(2, "0");

    return "$minutes:$seconds:$milliseconds";
  }

  @override
  void initState() {
    super.initState();
    stopwatch = Stopwatch();

    t = Timer.periodic(Duration(milliseconds: 30), (timer) {
      setState(() {});
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SafeArea(
        child: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Container(
                padding: EdgeInsets.all(16),
                child: Text(
                  'Stopwatch', 
                  style: TextStyle(
                    fontSize: 24,
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ),
              Stack(
                alignment: Alignment.center,
                children: [
                  Text(
                    returnFormattedText(),
                    style: TextStyle(
                      color: Colors.black,
                      fontSize: 40,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                ],
              ),
              SizedBox(height: 15,),
              Row(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Padding(
                    padding: const EdgeInsets.only(right: 16.0), // Adjust the padding as needed
                    child: ElevatedButton(
                      onPressed: handleStartStop,
                      child: Text(isRunning ? 'Stop' : 'Start'),
                    ),
                  ),
                  ElevatedButton(
                    onPressed: handleReset,
                    child: Text('Reset'),
                  ),
                ],
              ),
              SizedBox(height: 15,), // Space between buttons and text
              Align(
                alignment: Alignment.bottomCenter,
                child: Padding(
                  padding: const EdgeInsets.only(bottom: 16.0), // Adjust the padding as needed
                  child: Text(
                    'PRASUNET INTERNSHIP PROJECT BY - SEETHALAKSHMI A', 
                    style: TextStyle(
                      fontSize: 16,
                      color: Colors.grey, 
                    ),
                  ),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  } 
}
