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

## 1. calculator.py : '계산기'에 있는 4칙연산 기능들을 제공하는 모듈
- **소스코드**
  <br/>
```py
# 프리미엄 전용 계산기
import tkinter as tk
import math
from fractions import Fraction

# ================================
# 개선된 UI + 넓은 화면 + 공학용/기본형 전환 계산기
# ================================

# 안전한 eval 네임스페이스
safe_names = {name: getattr(math, name) for name in dir(math) if not name.startswith("__")}
safe_names.update({'pi': math.pi, 'e': math.e, 'Fraction': Fraction})

# -----------------------------
# 계산 기록 저장
# -----------------------------
history = []  # 최근 5개 기록 저장

# -----------------------------
# 계산 함수
# -----------------------------

def press_button(value):
    cur = entry_var.get()
    if value == '^':  # 파이썬 연산자 변환
        entry_var.set(cur + '**')
    else:
        entry_var.set(cur + value)


def clear(): entry_var.set("")
def backspace(): entry_var.set(entry_var.get()[:-1])

def to_fraction():
    try:
        v = entry_var.get()
        val = eval(v, {"__builtins__": None}, safe_names)
        entry_var.set(str(Fraction(val).limit_denominator()))
    except: entry_var.set("오류")


def calculate():
    global history
    try:
        result = eval(entry_var.get(), {"__builtins__": None}, safe_names)
        entry_var.set(str(result))
        # 계산 기록 저장
        expr = v if (v := entry_var.get()) else ''
        history.append(f"{expr} = {result}")
        if len(history) > 5:
            history.pop(0)
    except ZeroDivisionError:
        entry_var.set("0으로 나눌 수 없음")
    except:
        entry_var.set("오류")

# -----------------------------
# GUI 설정
# -----------------------------
root = tk.Tk()
root.title("공학용 계산기 - UI 개선 버전")
root.geometry("720x950")   # 화면 확장
root.configure(bg="#1e1e1e")
root.resizable(True, True)

entry_var = tk.StringVar()

# 상단 입력창 스타일
entry = tk.Entry(
    root, textvariable=entry_var, font=("Arial", 26), bd=0,
    relief='flat', justify='right', bg="#2d2d2d", fg="white",
    insertbackground="white"
)
entry.grid(row=0, column=0, columnspan=8, padx=15, pady=20, sticky='we')

# 키보드 입력 처리

def key_event(event):
    key = event.keysym
    if key in ['Return', 'equal']:
        calculate()
    elif key in ['BackSpace']:
        backspace()
    elif key in ['Escape']:
        clear()
    else:
        char = event.char
        if char and char in '0123456789.+\-*/()':
            press_button(char)

root.bind('<Key>', key_event)

# 모드 표시
mode_label = tk.Label(root, text="모드: 기본형", bg="#1e1e1e", fg="#bbbbbb", font=("Arial", 12))
mode_label.grid(row=1, column=0, sticky='w', padx=20)

# -----------------------------
# 프레임 생성
# -----------------------------
basic_frame = tk.Frame(root, bg="#1e1e1e")
scientific_frame = tk.Frame(root, bg="#1e1e1e")

# -----------------------------
# 토글 버튼
# -----------------------------

def toggle_mode():
    if basic_frame.winfo_ismapped():
        basic_frame.grid_forget()
        scientific_frame.grid(row=3, column=0, columnspan=8, padx=10, pady=10)
        mode_button.config(text="기본형으로 전환")
        mode_label.config(text="모드: 공학용 계산기")
    else:
        scientific_frame.grid_forget()
        basic_frame.grid(row=3, column=0, columnspan=8, padx=10, pady=10)
        mode_button.config(text="공학용으로 전환")
        mode_label.config(text="모드: 기본형")

mode_button = tk.Button(
    root, text="공학용으로 전환", command=toggle_mode,
    bg="#3c3c3c", fg="white", font=("Arial", 12), height=2,
    activebackground="#505050"
)
mode_button.grid(row=1, column=6, columnspan=2, padx=10, sticky='e')

# -----------------------------
# 버튼 스타일 함수
# -----------------------------

def styled_button(master, text, cmd, r, c, w=7, h=3, color="#3c3c3c"):
    btn = tk.Button(
        master, text=text, command=cmd, width=w, height=h,
        bg=color, fg="white", activebackground="#5a5a5a",
        font=("Arial", 14), relief="flat"
    )
    btn.grid(row=r, column=c, padx=5, pady=5)
    return btn

# -----------------------------
# 기본형 버튼
# -----------------------------

basic_buttons = [
    ('7', 0, 0), ('8', 0, 1), ('9', 0, 2), ('/', 0, 3),
    ('4', 1, 0), ('5', 1, 1), ('6', 1, 2), ('*', 1, 3),
    ('1', 2, 0), ('2', 2, 1), ('3', 2, 2), ('-', 2, 3),
    ('0', 3, 0), ('.', 3, 1), ('=', 3, 2), ('+', 3, 3)
]

for txt, r, c in basic_buttons:
    if txt == '=':
        cmd = calculate
    else:
        cmd = lambda v=txt: press_button(v)
    styled_button(basic_frame, txt, cmd, r, c, color="#2e2e2e")

styled_button(basic_frame, 'C', clear, 4, 0)
styled_button(basic_frame, '←', backspace, 4, 1)

basic_frame.grid(row=3, column=0, columnspan=8, padx=10, pady=10)

# -----------------------------
# 공학용 버튼
# -----------------------------

science_buttons = [
    ('sin(', 0, 0), ('cos(', 0, 1), ('tan(', 0, 2), ('asin(', 0, 3),
    ('acos(', 0, 4), ('atan(', 0, 5),
    ('ln(', 1, 0), ('log10(', 1, 1), ('e**', 1, 2), ('^', 1, 3),
    ('sqrt(', 1, 4), ('**2', 1, 5),
    ('pi', 2, 0), ('e', 2, 1), ('(', 2, 2), (')', 2, 3),
    ('.', 2, 4), (',', 2, 5),
    ('7', 3, 0), ('8', 3, 1), ('9', 3, 2), ('/', 3, 3), ('Frac', 3, 4), ('C', 3, 5),
    ('4', 4, 0), ('5', 4, 1), ('6', 4, 2), ('*', 4, 3), ('←', 4, 4), ('=', 4, 5),
    ('1', 5, 0), ('2', 5, 1), ('3', 5, 2), ('-', 5, 3), ('+', 5, 4), ('0', 5, 5)
]

for txt, r, c in science_buttons:
    if txt == 'C': cmd = clear
    elif txt == '←': cmd = backspace
    elif txt == 'Frac': cmd = to_fraction
    elif txt == '=': cmd = calculate
    elif txt == '^': cmd = lambda v='^': press_button(v)
    elif txt == '**2': cmd = lambda: press_button('**2')
    elif txt == 'e**': cmd = lambda: press_button('e**')
    else: cmd = lambda v=txt: press_button(v)

    styled_button(scientific_frame, txt, cmd, r, c, color="#2a2a2a")

# -----------------------------
# 기록 보기 버튼
# -----------------------------

def show_history():
    hist_win = tk.Toplevel(root)
    hist_win.title("계산 기록")
    hist_win.geometry("350x250")
    hist_win.configure(bg="#1e1e1e")

    tk.Label(hist_win, text="최근 계산 5개", font=("Arial", 14), fg="white", bg="#1e1e1e").pack(pady=10)

    if history:
        for item in reversed(history):  # 최근 기록부터 표시
            tk.Label(hist_win, text=item, fg="white", bg="#1e1e1e", font=("Arial", 12)).pack(anchor='w', padx=15)
    else:
        tk.Label(hist_win, text="기록 없음", fg="gray", bg="#1e1e1e", font=("Arial", 12)).pack(pady=20)

# 기록 보기 버튼 추가
history_button = tk.Button(
    root, text="기록 보기", command=show_history,
    bg="#3c3c3c", fg="white", font=("Arial", 12), height=2,
    activebackground="#505050"
)
history_button.grid(row=1, column=4, columnspan=2, padx=10, sticky='e')

# -----------------------------
# 실행
# -----------------------------
root.mainloop()

```

## 2. License : '라이센스'에 따라서 프리미엄 제품이냐 무료제공 제품인지 구분
- **현 제품은 무료버전이므로 간단한 4칙연산만 가능**
- **1년만 사용가능**

## 3. READEME.md : 이 프로그램에 관한 설명

