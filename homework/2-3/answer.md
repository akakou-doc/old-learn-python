# 2. 3. 複合課題　解答

## 課題1
2つの整数を引数として受け取り、その和を返り値として返す関数を作成せよ。

### 解答例
```py
def func(hoge, fuga):
    # hogeとfugaを引数として受け取り
    # その和を返り値として返す
    return hoge + fuga

# => 7
print(func(2, 5))

# => 13
print(func(4, 9))
```

## 課題2
2行の文字列を入力しその値が以下の文字列と比較し、
一致すればTrueを、それ以外はFalseを表示するプログラムを作成せよ。

### ヒント
文字列としての改行は`\n`で表せる

### 比較する文字列
```
Python ha
iizo ~
```

### 解答例
```py
# 2つの文字列（どちらも1行）を受け取る
hoge = input()
fuga = input()

# 2つの文字列を足す
piyo = hoge + '\n' + fuga

# 比較する値
hogera = 'Python ha\niizo ~'

# または
# hogera = '''Python ha
# iizo ~'''


# 比較した値を表示
print(piyo == hogera)
```

### 実行例
```
hoge
fuga
False
```

```
Python ha
iizo ~
True
```

## 課題3
2つの小数または整数を入力し、その和を表示するプログラムを作成せよ。

### 解答例
```py
hoge = input()
fuga = input()

hoge = float(hoge)
fuga = float(fuga)

print(hoge + fuga)
```

### 実行例
```
2
4
6.0
```

```
1.3
2.5
3.8
```
