# Методы одномерного поиска

## Оптимизируемые модели
- "Хорошая" унимодальная функция с явно выраженным минимумом:

${f(x)=0.35 (x-1.05)^2 + 1}$.
- Унимодальная, но несимметричная относительно минимума функция, с участками плато или другими особенностями поведения:

${f(x)=0.4 x^2 + 2\sin(x) + 0.2\cos(2x+2)}$

- Многомодальная функция:

${f(x)=0.5 x^2 + 5\sin(x-4) + 0.5\cos(2x)}$

<img src='readme_img/functions.png' style='width:100%; height:auto;'>

## Оптимизационные методы
### Метод пассивного поиска
Метод предполагает разбиение исходного интервала неопределенности ${\left[a;b\right]}$ на N-точек с последующим вычислением значений минимизируемой функции в этих точках.

$$\begin{equation}
    h=\frac{b-a}{N},
\end{equation}$$

$$\begin{equation}
    x_k=a+k\cdot h,
\end{equation}$$

$$\begin{equation}
    f_k=f(x_k).
\end{equation}$$

Среди N-значений функции выбирается наименьшее значение ${f_k=f_{min}}$ и определяется ${x^\*}$, соответствующее ${f_{min}=f(x^\*)}$.

В данной работе метод пассивного поиска используется в качестве предварительной разведки при минимизации многомодальной функции: локализуется интервал неопределенности длиной порядка ${100 \epsilon}$.

### Метод дихотомии
Метод дихотомии основан на последовательном делении интервала неопределенности пополам с последующим вычислением значений функции в двух точках:

$$\begin{equation}
    \begin{aligned}
        \lambda_k=\cfrac{a_k+b_k}{2} - \epsilon/2, \\
        \mu_k=\cfrac{a_k+b_k}{2} + \epsilon/2.
    \end{aligned}
\end{equation}$$

Новые границы интервала неопределенности выбираются по правилу:

$$\begin{equation}
    \begin{aligned}
        f(\lambda_k) > f(\mu_k) \rightarrow \left[\lambda_k;b_k\right], \\
        f(\lambda_k) < f(\mu_k) \rightarrow \left[a_k;\mu_k\right].
    \end{aligned}
\end{equation}$$

### Метод золотого сечения
Деление интервала неопределенности выполняется в следующих пропорциях:

$$\begin{equation}
    \begin{aligned}
        \lambda_k=b_k - (b_k-a_k)/ \varphi, \\
        \mu_k=a_k + (b_k-a_k)/ \varphi,
    \end{aligned}
\end{equation}$$
где ${\varphi=\frac{1+\sqrt(5)}{2}}$ - константа золотого сечения.

Главным преимуществом метода золотого сечения, в сравнении с методом дихотомии, является в 2 раза меньшее вычисление значений функции: на каждой k-й итерации требуется вычислять всего одно значение функции.

### Метод Фибоначчи
Деление интервала неопределенности выполняется в следующих пропорциях:

$$\begin{equation}
    \begin{aligned}
        \lambda_k=a_k + \cfrac{F_{n-k}}{F_{n-k+2}} (b_k-a_k), \\
        \mu_k=a_k + \cfrac{F_{n-k+1}}{F_{n-k+2}} (b_k-a_k),
    \end{aligned}
\end{equation}$$
либо
$$\begin{equation}
    \begin{aligned}
        \lambda_k=b_k - \cfrac{F_{n-k+1}}{F_{n-k+2}} (b_k-a_k), \\
        \mu_k=b_k - \cfrac{F_{n-k}}{F_{n-k+2}} (b_k-a_k),
    \end{aligned}
\end{equation}$$
где ${F_n}$ - последовательность чисел Фибоначчи (${F_0=1}$).

Реализация метода требует предварительного вычисления K-чисел Фибоначчи:
$$\begin{equation}
    F_K > \cfrac{b-a}{\epsilon}.
\end{equation}$$

### Метод парабол

## Метод парабол

Необходимо определить значения коэффициентов $a$, $b$, $c$ системы уравнений

$$\left\{
\begin{aligned}
\bold{a} x_1^2 + \bold{b} x_1 + \bold{c} &= f(x_1), \\
\bold{a} x_2^2 + \bold{b} x_2 + \bold{c} &= f(x_2), \\
\bold{a} x_3^2 + \bold{b} x_3 + \bold{c} &= f(x_3)
\end{aligned}
\right.$$

С этой целью обозначим

$$F = \begin{bmatrix}
f(x_1) \\
f(x_2) \\
f(x_3)
\end{bmatrix}$$,

$$P = \begin{bmatrix}
a \\
b \\
c
\end{bmatrix}$$,

$$M_{sys} = \begin{bmatrix}
x_1^2 & x_1 & 1 \\
x_2^2 & x_2 & 1 \\
x_3^2 & x_3 & 1
\end{bmatrix}$$.

Тогда, исходная система уравнений принимает вид

${M_{sys} \cdot P = F}$.

Новое приближение:

$$\begin{equation}
    x_k = - \cfrac{b}{2a}.
\end{equation}$$

Данный метод склонен к биениям (относится к нестабильным) и неэффективен при минимизации функций с участками плато.

### Комбинированный метод Брента (метод парабол + золотое сечение).

Вычисления начинаются методом парабол. Поиск новых приближений выполняется до тех пор, пока

$$\begin{equation}
    \left| x_k - x_{k-1} \right| < \cfrac{\left| x_{k-1} - x_{k-2} \right|}{2}.
\end{equation}$$

Затем алгоритм переключается на метод золотого сечения.

Такая комбинация позволяет уменьшить число итераций при минимизации функций с участками плато, когда метод парабол сходится медленно.

Расчеты приведены в блокноте [notebook.ipynb](notebook.ipynb)

## Минимизация унимодальной функции
<img src='readme_img/x_optim_f_unimod.png' style='width:100%; height:auto;'>
<img src='readme_img/iter_number_f_unimod.png' style='width:100%; height:auto;'>
<img src='readme_img/func_calc_number_f_unimod.png' style='width:100%; height:auto;'>

### Динамика интервала неопределенности (унимодальная функция)
<img src='readme_img/interval_eps_0.1_f_unimod.png' style='width:100%; height:auto;'>
<img src='readme_img/interval_eps_0.01_f_unimod.png' style='width:100%; height:auto;'>
<img src='readme_img/interval_eps_0.001_f_unimod.png' style='width:100%; height:auto;'>
<img src='readme_img/interval_eps_0.0001_f_unimod.png' style='width:100%; height:auto;'>
<img src='readme_img/interval_eps_1e-05_f_unimod.png' style='width:100%; height:auto;'>
<img src='readme_img/interval_eps_1e-06_f_unimod.png' style='width:100%; height:auto;'>
<img src='readme_img/interval_eps_1e-07_f_unimod.png' style='width:100%; height:auto;'>
<img src='readme_img/interval_eps_1e-08_f_unimod.png' style='width:100%; height:auto;'>

## Минимизация унимодальной, но несимметричной относительно минимума функции
<img src='readme_img/x_optim_f_nonsym.png' style='width:100%; height:auto;'>
<img src='readme_img/iter_number_f_nonsym.png' style='width:100%; height:auto;'>
<img src='readme_img/func_calc_number_f_nonsym.png' style='width:100%; height:auto;'>

### Динамика интервала неопределенности (унимодальная, но несимметричная функция)
<img src='readme_img/interval_eps_0.1_f_nonsym.png' style='width:100%; height:auto;'>
<img src='readme_img/interval_eps_0.01_f_nonsym.png' style='width:100%; height:auto;'>
<img src='readme_img/interval_eps_0.001_f_nonsym.png' style='width:100%; height:auto;'>
<img src='readme_img/interval_eps_0.0001_f_nonsym.png' style='width:100%; height:auto;'>
<img src='readme_img/interval_eps_1e-05_f_nonsym.png' style='width:100%; height:auto;'>
<img src='readme_img/interval_eps_1e-06_f_nonsym.png' style='width:100%; height:auto;'>
<img src='readme_img/interval_eps_1e-07_f_nonsym.png' style='width:100%; height:auto;'>
<img src='readme_img/interval_eps_1e-08_f_nonsym.png' style='width:100%; height:auto;'>

## Минимизация многомодальной функции

С целью грубой оценки начального интервала неопределенности (предварительная разведка) использован метод пассивного поиска. Дальнейшая оптимизация выполнена методом золотого сечения. Полученный результат сравнен с применением этого метода без предварительной разведки.

<img src='readme_img/x_optim_multimode.png' style='width:100%; height:auto;'>

## Приложение с таблицами

<!-- START_X_OPTIM_F_UNIMOD --> 
### Таблица оптимальных значений (унимодальная функция)
|    eps |   dihotom |   golden |   fibonacci |   parabol |   brent |
|-------:|----------:|---------:|------------:|----------:|--------:|
| 0.1    |   1.0499  |  1.05245 |     1.05197 |      1.05 |    1.05 |
| 0.01   |   1.04999 |  1.04965 |     1.04963 |      1.05 |    1.05 |
| 0.001  |   1.05    |  1.04981 |     1.0498  |      1.05 |    1.05 |
| 0.0001 |   1.05    |  1.05001 |     1.05001 |      1.05 |    1.05 |
| 1e-05  |   1.05    |  1.05    |     1.05    |      1.05 |    1.05 |
| 1e-06  |   1.05    |  1.05    |     1.05    |      1.05 |    1.05 |
| 1e-07  |   1.05    |  1.05    |     1.05    |      1.05 |    1.05 |
| 1e-08  |   1.05    |  1.05    |     1.05    |      1.05 |    1.05 |
<!-- END_X_OPTIM_F_UNIMOD -->
<!-- START_ITER_COUNTER_F_UNIMOD --> 
### Таблица количества итераций (унимодальная функция)
|    eps |   dihotom |   golden |   fibonacci |   parabol |   brent |
|-------:|----------:|---------:|------------:|----------:|--------:|
| 0.1    |        16 |       12 |          12 |         2 |       2 |
| 0.01   |        19 |       16 |          16 |         2 |       2 |
| 0.001  |        22 |       21 |          21 |         2 |       2 |
| 0.0001 |        26 |       26 |          26 |         2 |       2 |
| 1e-05  |        29 |       31 |          31 |         2 |       2 |
| 1e-06  |        32 |       35 |          35 |         2 |       2 |
| 1e-07  |        36 |       40 |          40 |         2 |       2 |
| 1e-08  |        39 |       45 |          45 |         2 |       2 |
<!-- END_ITER_COUNTER_F_UNIMOD -->
<!-- START_FUNC_COUNTER_F_UNIMOD --> 
### Таблица количества вычислений функции (унимодальная функция)
|    eps |   dihotom |   golden |   fibonacci |   parabol |   brent |
|-------:|----------:|---------:|------------:|----------:|--------:|
| 0.1    |        32 |       14 |          14 |         5 |       5 |
| 0.01   |        38 |       18 |          18 |         5 |       5 |
| 0.001  |        44 |       23 |          23 |         5 |       5 |
| 0.0001 |        52 |       28 |          28 |         5 |       5 |
| 1e-05  |        58 |       33 |          33 |         5 |       5 |
| 1e-06  |        64 |       37 |          37 |         5 |       5 |
| 1e-07  |        72 |       42 |          42 |         5 |       5 |
| 1e-08  |        78 |       47 |          47 |         5 |       5 |
<!-- END_FUNC_COUNTER_F_UNIMOD -->
<!-- START_X_OPTIM_F_NONSYM --> 
### Таблица оптимальных значений (несимметричная функция)
|    eps |   dihotom |   golden |   fibonacci |   parabol |    brent |
|-------:|----------:|---------:|------------:|----------:|---------:|
| 0.1    |  -1.158   | -1.15295 |    -1.15216 |  -1.15256 | -1.14772 |
| 0.01   |  -1.15812 | -1.15921 |    -1.15917 |  -1.15816 | -1.16046 |
| 0.001  |  -1.15813 | -1.15805 |    -1.15804 |  -1.15813 | -1.15817 |
| 0.0001 |  -1.15813 | -1.15815 |    -1.15815 |  -1.15813 | -1.15813 |
| 1e-05  |  -1.15813 | -1.15813 |    -1.15813 |  -1.15813 | -1.15813 |
| 1e-06  |  -1.15813 | -1.15813 |    -1.15813 |  -1.15813 | -1.15813 |
| 1e-07  |  -1.15813 | -1.15813 |    -1.15813 |  -1.15813 | -1.15813 |
| 1e-08  |  -1.15813 | -1.15813 |    -1.15813 |  -1.15813 | -1.15813 |
<!-- END_X_OPTIM_F_NONSYM -->
<!-- START_ITER_COUNTER_F_NONSYM --> 
### Таблица количества итераций (несимметричная функция)
|    eps |   dihotom |   golden |   fibonacci |   parabol |   brent |
|-------:|----------:|---------:|------------:|----------:|--------:|
| 0.1    |        16 |       12 |          12 |         6 |      11 |
| 0.01   |        19 |       16 |          16 |         7 |      16 |
| 0.001  |        22 |       21 |          21 |         8 |      21 |
| 0.0001 |        26 |       26 |          26 |         8 |      26 |
| 1e-05  |        29 |       31 |          31 |         9 |      30 |
| 1e-06  |        32 |       35 |          35 |        10 |      35 |
| 1e-07  |        36 |       40 |          40 |        10 |      40 |
| 1e-08  |        39 |       45 |          45 |        11 |      45 |
<!-- END_ITER_COUNTER_F_NONSYM -->
<!-- START_FUNC_COUNTER_F_NONSYM --> 
### Таблица количества вычислений функции (несимметричная функция)
|    eps |   dihotom |   golden |   fibonacci |   parabol |   brent |
|-------:|----------:|---------:|------------:|----------:|--------:|
| 0.1    |        32 |       14 |          14 |         9 |      15 |
| 0.01   |        38 |       18 |          18 |        10 |      20 |
| 0.001  |        44 |       23 |          23 |        11 |      25 |
| 0.0001 |        52 |       28 |          28 |        11 |      30 |
| 1e-05  |        58 |       33 |          33 |        12 |      34 |
| 1e-06  |        64 |       37 |          37 |        13 |      39 |
| 1e-07  |        72 |       42 |          42 |        13 |      44 |
| 1e-08  |        78 |       47 |          47 |        14 |      49 |
<!-- END_FUNC_COUNTER_F_NONSYM -->