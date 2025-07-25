import tkinter as tk
import math

# Main window
win = tk.Tk()
win.title("📱 Mobile Scientific Calculator")
win.geometry("360x550")
win.configure(bg="#222831")

expression = ""
input_text = tk.StringVar()

# Input field
input_frame = tk.Frame(win, bg="#222831")
input_frame.pack(pady=10)

input_box = tk.Entry(
    input_frame, font=('Arial', 20),
    textvariable=input_text, width=18, bd=8, bg="#393e46",
    fg="#00fff7", justify="right"
)
input_box.grid(row=0, column=0)
input_box.pack()

def press(num):
    global expression
    expression += str(num)
    input_text.set(expression)

def clear():
    global expression
    expression = ""
    input_text.set("")

def back():
    global expression
    expression = expression[:-1]
    input_text.set(expression)

def equal():
    global expression
    try:
        result = str(eval(expression))
        input_text.set(result)
        expression = result
    except:
        input_text.set("Error")
        expression = ""

def sci_func(func):
    global expression
    try:
        val = eval(expression)
        if func == "sqrt":
            result = math.sqrt(val)
        elif func == "log":
            result = math.log10(val)
        elif func == "ln":
            result = math.log(val)
        elif func == "sin":
            result = math.sin(math.radians(val))
        elif func == "cos":
            result = math.cos(math.radians(val))
        elif func == "tan":
            result = math.tan(math.radians(val))
        input_text.set(str(result))
        expression = str(result)
    except:
        input_text.set("Error")
        expression = ""

# Button Layout
btns = [
    ['sin', 'cos', 'tan', 'sqrt'],
    ['log', 'ln', '^', '/'],
    ['7', '8', '9', '*'],
    ['4', '5', '6', '-'],
    ['1', '2', '3', '+'],
    ['0', '.', '=', 'C'],
    ['π', 'e', '⌫', '()']
]

# Button Frame
btn_frame = tk.Frame(win, bg="#222831")
btn_frame.pack()

for i in range(len(btns)):
    for j in range(len(btns[i])):
        text = btns[i][j]
        def cmd(x=text):
            if x == "=":
                return equal()
            elif x == "C":
                return clear()
            elif x == "⌫":
                return back()
            elif x == "π":
                return press(str(math.pi))
            elif x == "e":
                return press(str(math.e))
            elif x in ['sin', 'cos', 'tan', 'log', 'ln', 'sqrt']:
                return sci_func(x)
            elif x == '^':
                return press("**")
            elif x == '()':
                return press("()")
            else:
                return press(x)

        btn = tk.Button(
            btn_frame, text=text, width=6, height=2,
            font=('Arial', 14), bg="#00adb5", fg="#eeeeee",
            activebackground="#ff5722", command=lambda t=text: cmd(t)
        )
        btn.grid(row=i, column=j, padx=3, pady=3)

win.mainloop()
