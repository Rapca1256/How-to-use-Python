# Python体験編

## 1. はじめに

これはPythonに触れたいけど難しそうなことはやりたくない！という人向けです。3つの作品と少しの課題で楽しく学べると思います。

これを読む前にPythonのすゝめの2.インストールまで読んでインストールを完了させることを強く勧めます。

完成形は体験編フォルダにあるので課題ができたらぜひ同じように動作するか自身の環境に移して試してみてください。

## 2. 電卓を作ろう

早速見ていきましょう。以下のコードは電卓ですが、何かがおかしいです。実行して確かめてみてください。

```python
import tkinter as tk # Tkinterモジュール(アプリの見た目を司るやつ)を読み込む

class Calculator(tk.Tk):
    def __init__(self): # 初期化(おまじない)
        super().__init__()
        self.title("電卓") # アプリの名前を決める
        self.geometry("300x400") # スクリーンのサイズ
        self.result_var = tk.StringVar() # 計算式、結果表示

        self.create_widgets()

    def create_widgets(self):
        # 結果表示エリア
        result_entry = tk.Entry(self, textvariable=self.result_var, font=("Arial", 24), bd=10, relief="ridge")
        result_entry.grid(row=0, column=0, columnspan=4)

        # ボタン配置(''で囲まれている文字が入力される)
        buttons = [
            ('7', 1, 0), ('5', 1, 1), ('9', 1, 2), ('+', 1, 3),
            ('8', 2, 0), ('3', 2, 1), ('6', 2, 2), ('+', 2, 3),
            ('1', 3, 0), ('2', 3, 1), ('4', 3, 2), ('+', 3, 3),
            ('0', 4, 0), ('.', 4, 1), ('=', 4, 2), ('+', 4, 3),
            ('C', 5, 0, 4)
        ]

        for btn in buttons:
            text, row, col = btn[:3]
            colspan = btn[3] if len(btn) == 4 else 1
            button = tk.Button(self, text=text, font=("Arial", 18), padx=20, pady=20, command=lambda t=text: self.on_button_click(t))
            button.grid(row=row, column=col, columnspan=colspan, sticky="news")

        for i in range(4):
            self.grid_columnconfigure(i, weight=1)
        for i in range(6):
            self.grid_rowconfigure(i, weight=1)

    def on_button_click(self, char):
        if char == 'C':
            self.result_var.set('')
        elif char == '=':
            try:
                result = str(eval(self.result_var.get()))
                self.result_var.set(result)
            except Exception as e:
                self.result_var.set("エラー")
        else:
            self.result_var.set(self.result_var.get() + char)

if __name__ == "__main__":
    app = Calculator()
    app.mainloop()
```

わかりましたか？記号(+, -, *(×), /(÷))が全部+になっていてしかも数字がぐちゃぐちゃです。このままでは使い物にならないので直していきましょう。

まず、33~39行目が表示されるボタンに書かれる文字の一覧です。()の中にいくつか書かれていますが、これらは

```
(書かれる文字, 表示される位置(縦), 表示される位置(横))
```

または

```
(書かれる文字, 表示される位置(縦), 表示される位置(横), 横の長さ(デフォルトは1))
```

となっています。4つ目の要素は計算リセットのボタンにだけありますがこれは横に長くするためです。

さて、何をいじればいいかわかりましたか？

今回は書かれている文字がおかしいので1つ目の要素だけを順番に変えてあげればよさそうです。だから、、、

```python
buttons = [
            ('7', 1, 0), ('5', 1, 1), ('9', 1, 2), ('+', 1, 3),
            ('8', 2, 0), ('3', 2, 1), ('6', 2, 2), ('+', 2, 3),
            ('1', 3, 0), ('2', 3, 1), ('4', 3, 2), ('+', 3, 3),
            ('0', 4, 0), ('.', 4, 1), ('=', 4, 2), ('+', 4, 3),
            ('C', 5, 0, 4)
        ]
```

これを、、、

```python
buttons = [
            ('7', 1, 0), ('8', 1, 1), ('9', 1, 2), ('/', 1, 3),
            ('4', 2, 0), ('5', 2, 1), ('6', 2, 2), ('*', 2, 3),
            ('1', 3, 0), ('2', 3, 1), ('3', 3, 2), ('-', 3, 3),
            ('0', 4, 0), ('.', 4, 1), ('=', 4, 2), ('+', 4, 3),
            ('C', 5, 0, 4)
        ]
```
こう！

これでボタンが正しく表示されましたね。

## 3. 手書き文字認識AIを作ろう

やっぱりPythonといえばAIですよね。今回はテストの採点とかで使われている手書き文字の認識をやるAIを作っていきましょう。

以下がコードですが、やっぱり未完成なので直してあげましょう。これの作者はなんで中途半端なものしか渡さないんでしょうね。そのせいで私たちが困る羽目になるんですよ。大体最初から完成品を渡して解説すればいいだけなのにあれのせいで私が苦労するんですよふざけんな。

失礼しました。まず、コマンドプロンプトで次を実行してください。

```cmd
pip install tensorflow matplotlib

```

```python

import tensorflow as tf
import matplotlib
import matplotlib.pyplot as plt

datasets = tf.keras.datases.mnist.load_model()

(X_train, Y_train), (X_test, Y_test) = datasets

plt.imshow(X_train[0], cmap='gray')
plt.show()
```
