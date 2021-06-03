# Planning the App

- Title: Roommates' Bill
- Description: An app that gets as input the amount of a bill for a particular period and the days that each of the roommate has to pay. It also generates a PDF Report stating the names of the roommates, the period, and how much each of them had to pay.
- Objects: Let's find a noun
    - Bill    
        - amount
        - period
    - Roommates:
        - name
        - days_in_house
        - pays(bill) - method(formula)
    - PdfReport:
        - filename
        - generate(roommate1, roommate2, bill) - save() - method
    
## Writing the Empty Classes

```python
# main.py
class Bill:
    """
    Object that contains data about a bill, such as total amount and period of the bill
    """

    # Short-Cut - alt + enter
    def __init__(self, amount, period):
        self.amount = amount
        self.period = period

class Roommate:
    """
    Create a roommate person who lives in the flat and pays a share of the bill
    """
    def __init__(self, name, days_in_house):
        self.name = name
        self.days_in_house = days_in_house

    def pays(self, bill):
        return bill.amount / 2

class PdfReport:
    """
    Create a Pdf file that contains data about the roommate such as their name, their due amount,
    and the period of the bill
    """

    def __init__(self, filename):
        self.filename = filename

    def generate(self, roommate1, roommate2, bill):
        pass

the_bill = Bill(amount=120, period="March 2021")
john = Roommate(name="John", days_in_house=20)
marry = Roommate(name="Marry", days_in_house=25)

print(john.pays(bill=the_bill))
```

Previously we used the code below to instantiate classes and call methods:
```python
marry = Flatmate(name="Marry", days_in_house=25)
john.pays(bill=the_bill)
```
Note that we explicitly mentioned the argument names in the calls, but you can get the exact same behavior by directly passing argument values without argument names, like so:

```python
marry = Flatmate("Marry", 25)
john.pays(the_bill)
```
Both ways are correct, and it is up to the programmer's style whether to use one or the other, but the first code has the advantage of being more readable because, for instance, it immediately tells the reader that 25 is the number of days.

It is recommended that, as a beginner, you do include argument names in calls so you can understand your code better.

```
Total = 120

A: 20 == 120 * 0.444 = 53.33
B: 25 == 120 * 0.555 = 66.67

20 / (20 + 25) = 0.444.... = 120 * 0.444 = 53.33
25 / (20 + 25) = 0.555.... = 120 * 0.555 = 66.67

53.33 + 66.67 = 120
```
## the Pays Method
```python
class Bill:
    """
    Object that contains data about a bill, such as total amount and period of the bill
    """

    # Short-Cut - alt + enter
    def __init__(self, amount, period):
        self.amount = amount
        self.period = period

class Roommate:
    """
    Create a roommate person who lives in the flat and pays a share of the bill
    """
    def __init__(self, name, days_in_house):
        self.name = name
        self.days_in_house = days_in_house

    def pays(self, bill, roommate2):
        weight = self.days_in_house / (self.days_in_house + roommate2.days_in_house)
        to_pay = bill.amount * weight
        return to_pay

class PdfReport:
    """
    Create a Pdf file that contains data about the roommate such as their name, their due amount,
    and the period of the bill
    """

    def __init__(self, filename):
        self.filename = filename

    def generate(self, roommate1, roommate2, bill):
        pass

the_bill = Bill(amount=120, period="March 2021")
john = Roommate(name="John", days_in_house=20)
marry = Roommate(name="Marry", days_in_house=25)

print("John pays: ", john.pays(bill=the_bill, roommate2=marry))
print("Marry pays: ", marry.pays(bill=the_bill, roommate2=john))
```

## the Generate Method
```python
from fpdf import FPDF

class Bill:
    """
    Object that contains data about a bill, such as total amount and period of the bill
    """

    # Short-Cut - alt + enter
    def __init__(self, amount, period):
        self.amount = amount
        self.period = period

class Roommate:
    """
    Create a roommate person who lives in the flat and pays a share of the bill
    """
    def __init__(self, name, days_in_house):
        self.name = name
        self.days_in_house = days_in_house

    def pays(self, bill, roommate2):
        weight = self.days_in_house / (self.days_in_house + roommate2.days_in_house)
        to_pay = bill.amount * weight
        return to_pay

class PdfReport:
    """
    Create a Pdf file that contains data about the roommate such as their name, their due amount,
    and the period of the bill
    """

    def __init__(self, filename):
        self.filename = filename

    def generate(self, roommate1, roommate2, bill):

        pdf = FPDF(orientation="P", unit="pt", format="A4")
        pdf.add_page()

        # Insert Title
        pdf.set_font(family="Times", size=24, style="B")
        pdf.cell(w=0, h=80, txt="Roommates Bill", border=1, align='C', ln=1)

        # Insert Period Label and value
        pdf.cell(w=100, h=40, txt="Period:", border=1)
        pdf.cell(w=150, h=40, txt=bill.period, border=1, ln=1)

        # Insert Name and Due Amount of the first roommate
        pdf.cell(w=100, h=40, txt=roommate1.name, border=1)
        pdf.cell(w=150, h=40, txt=str(roommate1.pays(bill, roommate2)), border=1, ln=1)

        pdf.output(self.filename)

the_bill = Bill(amount=120, period="April 2021")
john = Roommate(name="John", days_in_house=20)
marry = Roommate(name="Marry", days_in_house=25)

print("John pays: ", john.pays(bill=the_bill, roommate2=marry))
print("Marry pays: ", marry.pays(bill=the_bill, roommate2=john))

pdf_report = PdfReport(filename='Report1.pdf')
pdf_report.generate(roommate1=john, roommate2=marry, bill=the_bill)
```

## Polishing the Code
```python
from fpdf import FPDF

class Bill:
    """
    Object that contains data about a bill, such as total amount and period of the bill
    """

    # Short-Cut - alt + enter
    def __init__(self, amount, period):
        self.amount = amount
        self.period = period

class Roommate:
    """
    Create a roommate person who lives in the flat and pays a share of the bill
    """
    def __init__(self, name, days_in_house):
        self.name = name
        self.days_in_house = days_in_house

    def pays(self, bill, roommate2):
        weight = self.days_in_house / (self.days_in_house + roommate2.days_in_house)
        to_pay = bill.amount * weight
        return to_pay

class PdfReport:
    """
    Create a Pdf file that contains data about the roommate such as their name, their due amount,
    and the period of the bill
    """

    def __init__(self, filename):
        self.filename = filename

    def generate(self, roommate1, roommate2, bill):

        roommate1_pay = str(round(roommate1.pays(bill, roommate2), 2))
        roommate2_pay = str(round(roommate2.pays(bill, roommate1), 2))

        pdf = FPDF(orientation="P", unit="pt", format="A4")
        pdf.add_page()

        # Insert Title
        pdf.set_font(family="Times", size=24, style="B")
        pdf.cell(w=0, h=80, txt="Roommates Bill", border=1, align='C', ln=1)

        # Insert Period Label and value
        pdf.cell(w=100, h=40, txt="Period:", border=1)
        pdf.cell(w=150, h=40, txt=bill.period, border=1, ln=1)

        # Insert Name and Due Amount of the first roommate
        pdf.cell(w=100, h=40, txt=roommate1.name, border=1)
        pdf.cell(w=150, h=40, txt=roommate1_pay, border=1, ln=1)

        # Insert Name and Due Amount of the second roommate
        pdf.cell(w=100, h=40, txt=roommate2.name, border=1)
        pdf.cell(w=150, h=40, txt=roommate2_pay, border=1, ln=1)

        pdf.output(self.filename)

the_bill = Bill(amount=120, period="April 2021")
john = Roommate(name="John", days_in_house=20)
marry = Roommate(name="Marry", days_in_house=25)

print("John pays: ", john.pays(bill=the_bill, roommate2=marry))
print("Marry pays: ", marry.pays(bill=the_bill, roommate2=john))

pdf_report = PdfReport(filename='Report1.pdf')
pdf_report.generate(roommate1=john, roommate2=marry, bill=the_bill)
```

## Adding an Image to the PDF Document
```python
pdf.image("house.jpg", w=30, h=30)
```

### Changing the PDF Text Font - Automatically View a PDF File
```python
# mac or linux
# open('file://' + os.path.realpath(self.filename)
webbrowser.open(self.filename)
```


