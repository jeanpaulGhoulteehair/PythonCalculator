# PythonCalculator

import sys

from PyQt5.QtWidgets import QApplication, QWidget, QPushButton, QLabel, QGridLayout
from PyQt5.QtCore import Qt
from PyQt5.QtGui import QIcon


class Calculator(QWidget):
    def __init__(self):
        super().__init__()
        self.symbols = ["*", "÷", "-", "+", "."]
        self.button_back = QPushButton("Delete", self)
        self.button_decimal = QPushButton(".", self)
        self.button_1 = QPushButton("1", self)
        self.button_2 = QPushButton("2", self)
        self.button_3 = QPushButton("3", self)
        self.button_4 = QPushButton("4", self)
        self.button_5 = QPushButton("5", self)
        self.button_6 = QPushButton("6", self)
        self.button_7 = QPushButton("7", self)
        self.button_8 = QPushButton("8", self)
        self.button_9 = QPushButton("9", self)
        self.button_0 = QPushButton("0", self)
        self.button_multiply = QPushButton("*", self)
        self.button_divide = QPushButton("÷", self)
        self.button_add = QPushButton("+", self)
        self.button_subtract = QPushButton("-", self)
        self.button_enter = QPushButton("Enter", self)
        self.label_results = QLabel("", self)
        self.current_text = ""
        self.show_result = QLabel(self)
        self.initUI()


    def initUI(self):
        self.setWindowTitle("Calculator program")
        display_icon = QIcon("icon.webp")
        self.setWindowIcon(display_icon)

        self.button_0.setObjectName("button_0")
        self.button_1.setObjectName("button_1")
        self.button_2.setObjectName("button_2")
        self.button_3.setObjectName("button_3")
        self.button_4.setObjectName("button_4")
        self.button_5.setObjectName("button_5")
        self.button_6.setObjectName("button_6")
        self.button_7.setObjectName("button_7")
        self.button_8.setObjectName("button_8")
        self.button_9.setObjectName("button_9")
        self.button_enter.setObjectName("button_enter")
        self.button_decimal.setObjectName("button_decimal")
        self.button_back.setObjectName("button_back")
        self.button_divide.setObjectName("button_divide")
        self.button_add.setObjectName("button_add")
        self.button_subtract.setObjectName("button_subtract")
        self.button_multiply.setObjectName("button_multiply")



        self.setStyleSheet("""
            QPushButton{
            background-color: orange;
            
        }
            QPushButton#button_multiply{
               background-color: black;
               color: orange;
        }
            QPushButton#button_divide{
               background-color: black;
               color: orange;
        }
            QPushButton#button_add{
               background-color: black;
               color: orange;
        }
            QPushButton#button_subtract{
               background-color: black;
               color: orange;
        }
            QPushButton#button_enter{
               background-color: black;
               color: orange;
        }
            QPushButton#button_back{
               background-color: black;
               color: orange;
        }
        
        """)

        grid_numpad = QGridLayout()
        grid_numpad.setAlignment(Qt.AlignCenter)
        grid_numpad.addWidget(self.button_back,5,0,1,2)
        grid_numpad.addWidget(self.button_decimal,4,2)
        grid_numpad.addWidget(self.label_results,0,0,1,4)
        grid_numpad.addWidget(self.button_0,4,0,1, 2)
        grid_numpad.addWidget(self.button_enter,5,2,1,2)
        grid_numpad.addWidget(self.button_1,1,0)
        grid_numpad.addWidget(self.button_2,1,1)
        grid_numpad.addWidget(self.button_3,1,2)
        grid_numpad.addWidget(self.button_4,2,0)
        grid_numpad.addWidget(self.button_5,2,1)
        grid_numpad.addWidget(self.button_6,2,2)
        grid_numpad.addWidget(self.button_7,3,0)
        grid_numpad.addWidget(self.button_8,3,1)
        grid_numpad.addWidget(self.button_9,3,2)
        grid_numpad.addWidget(self.button_add,1,3)
        grid_numpad.addWidget(self.button_subtract,2,3)
        grid_numpad.addWidget(self.button_multiply,3,3)
        grid_numpad.addWidget(self.button_divide,4,3)

        self.setLayout(grid_numpad)

        self.button_0.clicked.connect(self.display_label)
        self.button_1.clicked.connect(self.display_label)
        self.button_2.clicked.connect(self.display_label)
        self.button_3.clicked.connect(self.display_label)
        self.button_4.clicked.connect(self.display_label)
        self.button_5.clicked.connect(self.display_label)
        self.button_6.clicked.connect(self.display_label)
        self.button_7.clicked.connect(self.display_label)
        self.button_8.clicked.connect(self.display_label)
        self.button_9.clicked.connect(self.display_label)
        self.button_subtract.clicked.connect(self.display_label)
        self.button_add.clicked.connect(self.display_label)
        self.button_divide.clicked.connect(self.display_label)
        self.button_multiply.clicked.connect(self.display_label)
        self.button_enter.clicked.connect(self.calculate)
        self.button_decimal.clicked.connect(self.display_label)
        self.button_back.clicked.connect(self.delete_char)


    def display_label(self):
        if self.sender().text().isdigit():
            self.current_text += self.sender().text()
        elif self.sender().text() == "Enter":
            self.calculate()
        else:
            if self.current_text == "" or self.current_text[len(self.current_text) - 1] in self.symbols:
                self.current_text = self.current_text
            else:
                self.current_text += self.sender().text()

        self.label_results.setText(self.current_text)
        self.label_results.setAlignment(Qt.AlignRight)

    def delete_char(self):
        self.current_text = self.current_text[:len(self.current_text) - 1]
        self.label_results.setText(self.current_text)


    def calculate(self):
        is_divide = self.current_text.find("÷")
        is_multiply = self.current_text.find("*")
        is_add = self.current_text.find("+")
        is_subtract = self.current_text.find("-")


        if is_divide != -1:
            self.calc_divide()
        if is_multiply != -1:
            self.calc_multiply()
        if is_add != -1:
            self.calc_addition()
        if is_subtract != -1:
            self.calc_subtract()



# ----------------DIVIDE--------------------



    def calc_divide(self):
        is_divide = self.current_text.find("÷")
        left_find_int = is_divide - 1
        list_div_leftside = []
        list_div_rightside = []

        while left_find_int >= 0 and (
                self.current_text[left_find_int - 1].isdigit() or
                self.current_text[left_find_int - 1] == '.'):
            list_div_leftside.append(self.current_text[left_find_int])
            left_find_int -= 1

        l_range = left_find_int + 1
        right_find_int = is_divide + 1

        while right_find_int < len(self.current_text) and (
                self.current_text[right_find_int].isdigit() or
                self.current_text[right_find_int] == '.'
        ):
            list_div_rightside.append(self.current_text[right_find_int])
            right_find_int += 1
        r_range = right_find_int
        div_leftside = ''.join(reversed(list_div_leftside))
        div_rightside = ''.join(list_div_rightside)
        div_leftside = float(div_leftside)
        div_rightside = float(div_rightside)

        ans = float(div_leftside) / float(div_rightside)
        self.current_text = self.current_text[:l_range] + str(ans) + self.current_text[r_range:]

        self.label_results.setText(self.current_text)



#---------------MULTIPLY-----------------



    def calc_multiply(self):
        is_multiply = self.current_text.find("*")
        list_mult_leftside = []
        list_mult_rightside = []

        left_find_int = is_multiply - 1

        while left_find_int >= 0 and (
                self.current_text[left_find_int - 1].isdigit() or
                self.current_text[left_find_int - 1] == '.'):
            list_mult_leftside.append(self.current_text[left_find_int])
            left_find_int -= 1

        l_range = left_find_int + 1
        right_find_int = is_multiply + 1

        while right_find_int < len(self.current_text) and (
                self.current_text[right_find_int].isdigit() or
                self.current_text[right_find_int] == '.'
        ):
            list_mult_rightside.append(self.current_text[right_find_int])
            right_find_int += 1
        r_range = right_find_int
        mult_leftside = ''.join(reversed(list_mult_leftside))
        mult_rightside = ''.join(list_mult_rightside)
        mult_leftside = float(mult_leftside)
        mult_rightside = float(mult_rightside)

        ans = float(mult_leftside) * float(mult_rightside)
        self.current_text = self.current_text[:l_range] + str(ans) + self.current_text[r_range:]
        is_add = self.current_text.find("+")
        self.label_results.setText(self.current_text)



#---------------ADDITION--------------




    def calc_addition(self):
        is_add = self.current_text.find("+")
        list_add_leftside = []
        list_add_rightside = []

        left_find_int = is_add - 1
        print(left_find_int)
        while left_find_int >= 0 and (
                self.current_text[left_find_int - 1].isdigit() or
                self.current_text[left_find_int - 1] == '.'
        ):
            list_add_leftside.append(self.current_text[left_find_int])
            left_find_int -= 1

        l_range = left_find_int + 1
        right_find_int = is_add + 1

        while right_find_int < len(self.current_text) and (
                self.current_text[right_find_int].isdigit() or
                self.current_text[right_find_int] == '.'
        ):
            list_add_rightside.append(self.current_text[right_find_int])
            right_find_int += 1
        r_range = right_find_int
        print(list_add_leftside)
        print(list_add_rightside)
        add_leftside = ''.join(reversed(list_add_leftside))
        add_rightside = ''.join(list_add_rightside)
        print(add_leftside)
        print(f"1 2 3 4 5{add_rightside}")
        add_leftside = float(add_leftside)
        add_rightside = float(add_rightside)

        ans = float(add_leftside) + float(add_rightside)
        self.current_text = self.current_text[:l_range] + str(ans) + self.current_text[r_range:]
        self.label_results.setText(self.current_text)



#--------------SUBTRACT---------------






    def calc_subtract(self):
        print("gfdgdf")
        is_subtract = self.current_text.find("-")
        list_subtract_leftside = []
        list_subtract_rightside = []

        left_find_int = is_subtract - 1


        while left_find_int >= 0 and (
                self.current_text[left_find_int - 1].isdigit() or
                self.current_text[left_find_int - 1] == '.'
        ):
            list_subtract_leftside.append(self.current_text[left_find_int])
            left_find_int -= 1


        l_range = left_find_int + 1
        right_find_int = is_subtract + 1

        while right_find_int < len(self.current_text) and (
                self.current_text[right_find_int].isdigit() or
                self.current_text[right_find_int] == '.'
        ):
            list_subtract_rightside.append(self.current_text[right_find_int])
            right_find_int += 1
        r_range = right_find_int
        print("fdsfdsd")

        subtract_leftside = ''.join(reversed(list_subtract_leftside))
        subtract_rightside = ''.join(list_subtract_rightside)

        subtract_leftside = float(subtract_leftside)
        subtract_rightside = float(subtract_rightside)

        ans = float(subtract_leftside) - float(subtract_rightside)
        self.current_text = self.current_text[:l_range] + str(ans) + self.current_text[r_range:]
        self.label_results.setText(self.current_text)





if __name__ == '__main__':
    app = QApplication(sys.argv)
    calc = Calculator()
    calc.show()
    sys.exit(app.exec_())
