# codesoft-expense_tracker_app

import 'package:flutter/material.dart';
import 'package:intl/intl.dart';

void main() => runApp(MyApp());

class Transaction {
  final String id;
  final String title;
  final double amount;
  final DateTime date;
  final String category;

  Transaction({
    required this.id,
    required this.title,
    required this.amount,
    required this.date,
    required this.category,
  });
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'EXPENSE TRACKER APP',
      theme: ThemeData(
        primaryColor: Color.fromARGB(255, 118, 2, 74),
        visualDensity: VisualDensity.adaptivePlatformDensity,
      ),
      home: SplashScreen(),
      debugShowCheckedModeBanner: false,
    );
  }
}

class SplashScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Container(
        decoration: BoxDecoration(
          gradient: LinearGradient(
            colors: [Color.fromARGB(255, 118, 2, 74), Color.fromARGB(255, 31, 2, 36)],
            begin: Alignment.topCenter,
            end: Alignment.bottomCenter,
          ),
        ),
        child: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Text(
                'EXPENSE TRACKER APP',
                style: TextStyle(
                  fontSize: 40,
                  fontWeight: FontWeight.bold,
                  color: Colors.white,
                ),
              ),
              SizedBox(height: 150),
             ElevatedButton(
  onPressed: () {
    Navigator.push(
      context,
      MaterialPageRoute(builder: (context) => ExpenseTrackerPage()),
    );
  },
  style: ElevatedButton.styleFrom(
    primary: Color.fromARGB(255, 164, 1, 101),
    fixedSize: Size(130, 50),
    shape: RoundedRectangleBorder(
      borderRadius: BorderRadius.circular(90.0), // Adjust the radius as needed
    ),
  ),
  child: Text(
    'Start Tracking',
    style: TextStyle(color: Colors.white),
  ),
),


            ],
          ),
        ),
      ),
    );
  }
}

class ExpenseTrackerPage extends StatefulWidget {
  @override
  _ExpenseTrackerPageState createState() => _ExpenseTrackerPageState();
}

class _ExpenseTrackerPageState extends State<ExpenseTrackerPage> {
  final List<Transaction> _transactions = [];

  Widget _buildSummary() {
    return Card(
      elevation: 6,
      margin: EdgeInsets.all(10),
      child: Padding(
        padding: EdgeInsets.all(10),
        child: Row(
          mainAxisAlignment: MainAxisAlignment.spaceAround,
          children: <Widget>[
            Text(
              'Total Spending: \$XXXX',
              style: TextStyle(fontSize: 16),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildTransactionList() {
  return Expanded(
    child: ListView.builder(
      itemCount: _transactions.length,
      itemBuilder: (context, index) {
        return Card(
          child: ListTile(
            leading: CircleAvatar(
              radius: 30,
              backgroundColor: Color.fromARGB(255, 118, 2, 74), // Purple color
              child: Padding(
                padding: EdgeInsets.all(6),
                child: FittedBox(
                  child: Text('\$${_transactions[index].amount.toStringAsFixed(2)}'),
                ),
              ),
            ),
            title: Text(_transactions[index].title),
            subtitle: Text(DateFormat.yMMMd().format(_transactions[index].date)),
          ),
        );
      },
    ),
  );
}


  void _addTransaction(String title, double amount, DateTime date, String category) {
    final newTransaction = Transaction(
      id: DateTime.now().toString(),
      title: title,
      amount: amount,
      date: date,
      category: category,
    );

    setState(() {
      _transactions.add(newTransaction);
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Expense Tracker'),
        backgroundColor: Color.fromARGB(255, 118, 2, 74),
        actions: [
          IconButton(
            onPressed: () {},
            icon: Icon(Icons.filter_list),
          ),
        ],
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          Navigator.push(
            context,
            MaterialPageRoute(builder: (context) => AddTransactionPage(_addTransaction)),
          );
        },
        backgroundColor: Color.fromARGB(255, 118, 2, 74),
        child: Icon(Icons.add),
      ),
      body: Container(
        decoration: BoxDecoration(
          gradient: LinearGradient(
            colors: [Color.fromARGB(255, 118, 2, 74), Color.fromARGB(255, 31, 2, 36)],
            begin: Alignment.topCenter,
            end: Alignment.bottomCenter,
          ),
        ),
        child: Column(
          children: <Widget>[
            _buildSummary(),
            _buildTransactionList(),
          ],
        ),
      ),
    );
  }
}

class AddTransactionPage extends StatefulWidget {
  final Function(String, double, DateTime, String) addTransaction;

  AddTransactionPage(this.addTransaction);

  @override
  _AddTransactionPageState createState() => _AddTransactionPageState();
}

class _AddTransactionPageState extends State<AddTransactionPage> {
  final TextEditingController _titleController = TextEditingController();
  final TextEditingController _amountController = TextEditingController();
  DateTime _selectedDate = DateTime.now();
  String _selectedCategory = 'General';

  void _submitData() {
    final enteredTitle = _titleController.text;
    final enteredAmount = double.tryParse(_amountController.text) ?? 0.0;

    if (enteredTitle.isEmpty || enteredAmount <= 0 || _selectedCategory.isEmpty) {
      return;
    }

    widget.addTransaction(enteredTitle, enteredAmount, _selectedDate, _selectedCategory);

    Navigator.of(context).pop();
  }

  void _presentDatePicker() {
    showDatePicker(
      context: context,
      initialDate: DateTime.now(),
      firstDate: DateTime(2022),
      lastDate: DateTime.now(),
    ).then((pickedDate) {
      if (pickedDate != null) {
        setState(() {
          _selectedDate = pickedDate;
        });
      }
    });
  }

 @override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: Text('Add Transaction'),
      backgroundColor: Color.fromARGB(255, 118, 2, 74),
    ),
    body: Container(
      decoration: BoxDecoration(
        gradient: LinearGradient(
          colors: [Color.fromARGB(255, 118, 2, 74), Color.fromARGB(255, 31, 2, 36)],
          begin: Alignment.topCenter,
          end: Alignment.bottomCenter,
        ),
      ),
      child: SingleChildScrollView(
        child: Card(
          elevation: 5,
          child: Container(
            padding: EdgeInsets.all(10),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.end,
              children: <Widget>[
                TextField(
                  decoration: InputDecoration(labelText: 'Title'),
                  controller: _titleController,
                  onSubmitted: (_) => _submitData(),
                ),
                TextField(
                  decoration: InputDecoration(labelText: 'Amount'),
                  controller: _amountController,
                  keyboardType: TextInputType.numberWithOptions(decimal: true),
                  onSubmitted: (_) => _submitData(),
                ),
                Row(
                  children: <Widget>[
                    Expanded(
                      child: Text(
                        _selectedDate == null
                            ? 'No Date Chosen!'
                            : 'Picked Date: ${DateFormat.yMMMd().format(_selectedDate)}',
                      ),
                    ),
                    TextButton(
                      onPressed: _presentDatePicker,
                      child: Text(
                        'Choose Date',
                        style: TextStyle(color: Color.fromARGB(255, 118, 2, 74)),
                      ),
                    ),
                  ],
                ),
                Row(
                  children: [
                    Text('Category: '),
                    DropdownButton<String>(
                      value: _selectedCategory,
                      onChanged: (String? value) {
                        setState(() {
                          _selectedCategory = value!;
                        });
                      },
                      items: <String>['General', 'Food', 'Utilities', 'Entertainment', 'Others']
                          .map<DropdownMenuItem<String>>((String value) {
                        return DropdownMenuItem<String>(
                          value: value,
                          child: Text(value),
                        );
                      }).toList(),
                    ),
                  ],
                ),
                ElevatedButton(
                  onPressed: _submitData,
                  style: ElevatedButton.styleFrom(
                    primary: Color.fromARGB(255, 118, 2, 74),
                  ),
                  child: Text('Add Transaction'),
                ),
              ],
            ),
          ),
        ),
      ),
    )
    );

  }
}
