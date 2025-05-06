import 'package:flutter/material.dart';

void main() {
  runApp(BMICalculatorApp());
}

class BMICalculatorApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: BMICalculator(),
    );
  }
}

class BMICalculator extends StatefulWidget {
  @override
  _BMICalculatorState createState() => _BMICalculatorState();
}

class _BMICalculatorState extends State<BMICalculator> {
  final heightController = TextEditingController();
  final weightController = TextEditingController();
  double? _bmi;

  // Function object for BMI calculation
  double Function(double, double) calculateBMI = (weight, height) {
    return weight / (height * height);
  };

  void _calculate() {
    final height = double.tryParse(heightController.text);
    final weight = double.tryParse(weightController.text);

    if (height != null && weight != null) {
      setState(() {
        _bmi = calculateBMI(weight, height); // use function object
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('BMI Calculator')),
      body: Padding(
        padding: EdgeInsets.all(16),
        child: Column(
          children: [
            TextField(
              controller: heightController,
              decoration: InputDecoration(labelText: 'Height (m)'),
              keyboardType: TextInputType.number,
            ),
            TextField(
              controller: weightController,
              decoration: InputDecoration(labelText: 'Weight (kg)'),
              keyboardType: TextInputType.number,
            ),
            ElevatedButton(
              onPressed: _calculate,
              child: Text('Calculate'),
            ),
            if (_bmi != null) Text('Your BMI is ${_bmi!.toStringAsFixed(2)}'),
          ],
        ),
      ),
    );
  }
}
