# 수학 계산을 위한 코드를 제공하는 프로젝트
### 팀원 : 김성민, 박영민, 김성우, 김희선, 장한희

- **김성민**
<a href="https://github.com/seongminkim691-svg" target="_blank">
<img height="32" width="32" src="https://cdn.simpleicons.org/github/181717" />
</a>

- **박영민**
<!--github-->
<a href="https://github.com/pyminno12" target="_blank">
<img height="32" width="32" src="https://cdn.simpleicons.org/github/181717" />
</a>

- **김성우**
<!--github-->
<a href="https://github.com/hapa0151" target="_blank">
<img height="32" width="32" src="https://cdn.simpleicons.org/github/181717" />
</a>

- **김희선**
<!--github-->
<a href="https://github.com/huiseon097" target="_blank">
<img height="32" width="32" src="https://cdn.simpleicons.org/github/181717" />
</a>

- **장한희**
<!--github-->
<a href="https://github.com/hanhuijang588-star" target="_blank">
<img height="32" width="32" src="https://cdn.simpleicons.org/github/181717" />
</a>

## 1. calculator.py : '공학 계산기' 기능들을 제공하는 모듈
- **소스코드**
  <br/>
```py
#프리미엄 사용자를 공학용 계산기 (UI개선버전)
import tkinter as tk
import math

# =============================
# Modern Calculator + Scientific Mode + History
# =============================

root = tk.Tk()
root.title("Modern Calculator")
root.geometry("370x720")  # 화면 크게 조정
root.resizable(False, False)
root.configure(bg="#1e1e1e")

# -----------------------------
# Display
# -----------------------------
display = tk.Entry(root, font=("Arial", 24), bd=0, bg="#000000", fg="#00ff9d",
                    justify="right", insertbackground="white")
display.pack(fill="x", ipady=18, pady=(20, 10), padx=20)

# -----------------------------
# History storage
# -----------------------------
history = []  # 최근 20개 저장

# -----------------------------
# Basic calculator functions
# -----------------------------
def input_value(v):
    display.insert(tk.END, v)

def clear_all():
    display.delete(0, tk.END)

def backspace():
    current = display.get()
    display.delete(len(current) - 1, tk.END)

def calculate():
    try:
        exp = display.get().replace("^", "**")
        result = eval(exp)

        # 히스토리 저장
        history.append(f"{exp} = {result}")
        if len(history) > 20:
            history.pop(0)

        display.delete(0, tk.END)
        display.insert(0, str(result))
    except:
        display.delete(0, tk.END)
        display.insert(0, "Error")

# -----------------------------
# Scientific functions
# -----------------------------
def sci_function(func):
    try:
        value = float(display.get())
        res = None

        if func == "sqrt": res = math.sqrt(value)
        elif func == "sin": res = math.sin(math.radians(value))
        elif func == "cos": res = math.cos(math.radians(value))
        elif func == "tan": res = math.tan(math.radians(value))
        elif func == "log": res = math.log10(value)
        elif func == "ln": res = math.log(value)
        elif func == "exp": res = math.exp(value)
        elif func == "square": res = value ** 2
        elif func == "cube": res = value ** 3
        elif func == "deg": res = math.degrees(value)
        elif func == "rad": res = math.radians(value)
        elif func == "fact": res = math.factorial(int(value))

        display.delete(0, tk.END)
        display.insert(0, str(res))
    except:
        display.delete(0, tk.END)
        display.insert(0, "Error")

# -----------------------------
# History popup window
# -----------------------------
def show_history():
    hist_win = tk.Toplevel(root)
    hist_win.title("계산 기록 (최근 20개)")
    hist_win.geometry("360x500")
    hist_win.configure(bg="#1e1e1e")

    tk.Label(hist_win, text="최근 계산 기록", bg="#1e1e1e", fg="white",
             font=("Arial", 16)).pack(pady=10)

    box = tk.Text(hist_win, bg="#000000", fg="#00ff9d", font=("Arial", 14), height=20)
    box.pack(padx=15, pady=10, fill="both")

    if not history:
        box.insert(tk.END, "기록 없음")
    else:
        for h in reversed(history):  # 최신 순
            box.insert(tk.END, h + "\n")

# -----------------------------
# Button styles
# -----------------------------
BTN = {"font": ("Arial", 16), "width": 5, "height": 2, "bd": 0,
       "fg": "white", "bg": "#333333", "activebackground": "#444444"}
OP = BTN.copy(); OP.update({"bg": "#ff9500", "activebackground": "#ffad33"})
SCI = BTN.copy(); SCI.update({"bg": "#2a2a2a", "fg": "#00e6ff"})

# -----------------------------
# Buttons layout
# -----------------------------
frame = tk.Frame(root, bg="#1e1e1e")
frame.pack(pady=5)

buttons = [
    [("C", clear_all, OP), ("⌫", backspace, OP), ("History", show_history, SCI), ("√", lambda: sci_function("sqrt"), SCI)],
    [("sin", lambda: sci_function("sin"), SCI), ("cos", lambda: sci_function("cos"), SCI), ("tan", lambda: sci_function("tan"), SCI), ("/", lambda: input_value("/"), OP)],
    [("log", lambda: sci_function("log"), SCI), ("ln", lambda: sci_function("ln"), SCI), ("exp", lambda: sci_function("exp"), SCI), ("*", lambda: input_value("*"), OP)],
    [("7", lambda: input_value("7"), BTN), ("8", lambda: input_value("8"), BTN), ("9", lambda: input_value("9"), BTN), ("-", lambda: input_value("-"), OP)],
    [("4", lambda: input_value("4"), BTN), ("5", lambda: input_value("5"), BTN), ("6", lambda: input_value("6"), BTN), ("+", lambda: input_value("+"), OP)],
    [("1", lambda: input_value("1"), BTN), ("2", lambda: input_value("2"), BTN), ("3", lambda: input_value("3"), BTN), ("x²", lambda: sci_function("square"), SCI)],
    [("0", lambda: input_value("0"), BTN), (".", lambda: input_value("."), BTN), ("=", calculate, OP), ("x³", lambda: sci_function("cube"), SCI)],
]

# Button placement
for r, row in enumerate(buttons):
    for c, (txt, cmd, style) in enumerate(row):
        tk.Button(frame, text=txt, command=cmd, **style).grid(
            row=r, column=c, padx=6, pady=8)

root.mainloop()

```

## 2. License : '라이센스'에 따라서 프리미엄 제품이냐 무료제공 제품인지 구분
- **현 제품은 무료버전이므로 간단한 4칙연산만 가능**
- **1년만 사용가능**

## 3. READEME.md : 이 프로그램에 관한 설명

