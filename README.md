# 수학 계산을 위한 코드를 제공하는 프로젝트
### 개발자 : Kim SeongMin
- **GitHub**
<!--python-->
<img height="32" width="32" src="https://cdn.simpleicons.org/github/181717" />

## 1. calculator.py : '계산기'에 있는 4칙연산 기능들을 제공하는 모듈
- **소스코드**
  <br/>
```py
import tkinter as tk

def press_button(value):
    current = entry_var.get()
    entry_var.set(current + value)

def clear():
    entry_var.set("")

def calculate():
    expression = entry_var.get()

    allowed_chars = "0123456789+-*/(). "
    for ch in expression:
        if ch not in allowed_chars:
            entry_var.set("오류")
            return

    try:
        # eval()을 사용하여 식 계산
        result = str(eval(expression))
        entry_var.set(result)
    except ZeroDivisionError:
        entry_var.set("0으로 나눌 수 없음")
    except:
        entry_var.set("오류")

root = tk.Tk()
root.title("간단 계산기")

entry_var = tk.StringVar()

entry = tk.Entry(
    root,
    textvariable=entry_var,
    font=("Arial", 20),
    bd=10,
    relief="sunken",
    justify="right"
)
entry.grid(row=0, column=0, columnspan=4, ipadx=10, ipady=10)

buttons = [
    ('7', 1, 0), ('8', 1, 1), ('9', 1, 2), ('/', 1, 3),
    ('4', 2, 0), ('5', 2, 1), ('6', 2, 2), ('*', 2, 3),
    ('1', 3, 0), ('2', 3, 1), ('3', 3, 2), ('-', 3, 3),
    ('0', 4, 0), ('.', 4, 1), ('+', 4, 2), ('(', 4, 3),
    (')', 5, 0)
]

for (text, row, col) in buttons:
    tk.Button(
        root,
        text=text,
        width=5,
        height=2,
        font=("Arial", 18),
        command=lambda val=text: press_button(val)
    ).grid(row=row, column=col)

tk.Button(
    root,
    text='C',
    width=5,
    height=2,
    font=("Arial", 18),
    command=clear
).grid(row=5, column=1)

tk.Button(
    root,
    text='=',
    width=11,
    height=2,
    font=("Arial", 18),
    command=calculate
).grid(row=5, column=2, columnspan=2)

root.mainloop()
```

## 2. License : '라이센스'에 따라서 프리미엄 제품이냐 무료제공 제품인지 구분
- **현 제품은 무료버전이므로 간단한 4칙연산만 가능**
- **1년만 사용가능**

## 3. READEME.md : 이 프로그램에 관한 설명

