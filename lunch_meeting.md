---
marp: true
theme: default
paginate: true
math: mathjax
backgroundColor: #ffffff
style: |
  h1, h2, h3 { color: #1e293b; }
  h1 { font-size: 2.5em; text-align: center; }
  blockquote {
    background-color: #f1f5f9;
    padding: 20px;
    border-radius: 8px;
    border-left: 6px solid #4f46e5;
    margin: 20px 0;
  }
---

# martingale
## 2026/06/25 lunch meeting

---

## カジノの「マルチンゲール戦略」

勝率50%、配当2倍のゲーム（コイントスなど）における有名なベッティングシステムです。

> **ルール**
> 1. 最初に1ドル賭ける。
> 2. 負けたら、**賭け金を倍**にする（1 → 2 → 4 → 8...）。
> 3. 勝ったら、賭け金を最初の1ドルに戻す。

- **特徴:** 何連敗しても、**1回勝てば必ず最初の1ドル分の利益**が出ます。
- **現実の壁:** 必勝法に見えますが、連敗すると賭け金が指数関数的に増大するため、「無限の資金」と「カジノの賭け金上限なし」という非現実的な条件がない限り、いつか破綻します。

確率論における「マルチンゲール」は、このような**「公平なギャンブルにおける所持金の推移」**を数学的に一般化して名付けられました。

---

# 1. martingaleの定義

---

## 条件付き期待値 $E(X | \mathcal{F})$ の定義

簡単に言うと、「持っている情報をもとにしたベストな予測値」を表す新しい確率変数のことです。

> **定義**
> 確率空間$(\Omega,\mathcal F_o,P)$、$\sigma$-field $\mathcal F\subset \mathcal F_o$ 、確率変数$X\in \mathcal F_o$ で$E|X|< \infty$ を満たすとする。このとき、条件付き期待値 $E(X | \mathcal{F})$ を以下の2条件を満たす任意の確率変数$Y$として定義する。
> 
> 1. $Y \in \mathcal F$ すなわち、手元の情報 $\mathcal{F}$ だけで $Y$ の値が完全に決定できる。
> 2. 任意の事象$A \in \mathcal F$ に対し、$\int_A Y dP = \int_A X dP$ すなわち、情報 $\mathcal{F}$ に含まれるどんな事象に限定して平均をとっても、元の $X$ の平均と同じになる。

---
## martingaleの定義

確率変数列 $X_0,X_1,\dots$ が以下の3条件を満たすとき、filtration (i.e. $\mathcal F_0 \subset \mathcal F_1 \subset \cdots$) $\mathcal F_n$ に関する **martingale** と呼びます。

1. **可積分性:** $E|X_n| < \infty$
2. **適合性:** $X_n \in \mathcal{F}_n$ （時刻 $n$ までの情報で値が決まる）
3. **公平性:** $E(X_{n+1} | \mathcal{F}_n) = X_n$

> **直感的な意味：**
> 「現在の情報をすべて知っているとき、将来（次の一歩）の期待値は現在の値に等しい」

---

## super/submartingale

ゲームの「有利・不利」によって3つに分類されます。

- **supermartingale** 
  $$E(X_{n+1} | \mathcal{F}_n) \le X_n$$
  期待値が減少していく過程。ギャンブラーにとって「不利」なゲーム。

- **martingale** 
  $$E(X_{n+1} | \mathcal{F}_n) = X_n$$
  期待値が一定。「公平」なゲーム。

- **submartingale** 
  $$E(X_{n+1} | \mathcal{F}_n) \ge X_n$$
  期待値が増加していく過程。ギャンブラーにとって「有利」なゲーム。

---

# 2. predictableとstopping time
戦略によって「公平なゲーム」を打破できるか？

---

## 予測可能過程 (Predictable Process) と賭け金

- $H_n$ を時刻 $n$ での「賭け金」とします。
- 重要なのは、$H_n$ が **時刻 $n-1$ までの情報（$\mathcal{F}_{n-1}$）で決まる** ことです。（＝predictable）

時刻 $n$ までの累計の利益は以下のように書けます：
$$(H \cdot X)_n = \sum_{m=1}^n H_m(X_m - X_{m-1})$$

> **定理：**
> $X_n$ がsupermartingaleで $H_n \ge 0$ なら、$(H \cdot X)_n$ もまたsupermartingaleである。
> つまり、**どんな賭け方（戦略）をしても、不利なゲームを有利にはできない。**

---

## 引き際と停止時刻

いつゲームをやめるかという「引き際」の戦略はどうでしょうか？

- $N$ をStopping time とします。($\{N=n\} \in\mathcal F_n$)
- **停止された過程** を $X_{N \wedge n}$ と書きます。（時間 $n$ か $N$ の早い方で止まる過程）

> **定理 4.2.9：**
> $X_n$ がmartingale（またはsub/super）なら、停止された過程 $X_{N \wedge n}$ も同じ性質を受け継ぐ。

ゲームを「いつやめるか」という戦略も、ゲームの公平性自体を変えることはできません。

---

# 3. upcrossing inequalityとmartingale convergence theorem


---

## upcrossing inequality

区間 $[a, b]$ を下から上へ横切る回数を $U_n$ とします。(figure 4.2参照)

$$(b-a)EU_n \le E(X_n - a)^+ - E(X_0 - a)^+$$

**直感：**
  ゲームが有利（submartingale）であったとしても、一定の幅を勝ち越す回数（upcrossing）の期待値は、最終的な期待利益によって上から抑えられる。
この不等式が、極限で値が振動し続けないことの証明の鍵になります。

---

## martingale convergence theorem

> **定理：**
> $X_n$ がsubmartingaleで、$\sup_n E X_n^+ < \infty$ を満たすならば，
> 1. $X_n \to X \text{ a.s.}$
> 2. $E|X| < \infty$

**「有界なsubmartingaleは、いつかは落ち着く」** という非常に強力な主張です。
特に、$X_n$ が非負のsupermartingaleならば、$X_n \to X \text{ a.s.}$ となります。

---

# 4. martingale convergence theoremの応用例

---

## 応用例1：Pólya's Urn

壺の中に最初に赤玉が $r$ 個、緑玉が $g$ 個入っているとします。

> **ルール**
> 1. 壺からランダムに玉を1個取り出し、色を確認して元に戻す。
> 2. さらに、**それと同じ色の玉を $c$ 個新しく追加**する。
> 3. これを繰り返し、時刻 $n$ における**緑玉の割合**を $X_n$ とする。

すると，$X_n$ が**martingale**になります。さらに $X_n$は非負なので，martingale convergence theoremより $X_n \to X \text{ a.s.}$（極限 $X$ はベータ分布に従います）。

---

## 応用例2：Branching Process

生物の個体数増加に関するモデル（Galton-Watson process）を考えます。

> **モデルの基本：**
> 1. 時刻 $n$ における個体数を $Z_n$ とする（初期値 $Z_0 = 1$）。
> 2. 各個体は、独立に平均 $\mu$ の子孫を残す。

すると，$E(Z_{n+1} | \mathcal{F}_n) = \mu Z_n$ となるため、このままではmartingale
になりません。よって、期待される増殖率 $\mu^n$ で割って正規化した値、$X_n=Z_n/\mu^n$
を考えると、これはmartingaleになります。さらに $Z_n \ge 0$ より $X_n \ge 0$ であるため、martingale convergence theoremから $X_n \to X \text{ a.s.}$となります。

---

## branching processの極限が意味すること

極限値 $X$ は、システムの運命を教えてくれます。

- **$\mu \le 1$ のとき**
  個体数 $Z_n$ はいつか必ず 0 になります（確率 1 で絶滅）。したがって、極限は $X = 0$ になります。

- **$\mu > 1$ のとき：**
  一定の確率で絶滅を免れ、個体数が爆発的に増加します。このとき、極限値 $X$ は正の値をとることがあります。
  $$Z_n \approx X \cdot \mu^n$$
  これは、爆発する際の**個体数の増加スピードが、$\mu^n$ のオーダーである**ことを示しています。
