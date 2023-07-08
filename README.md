# Проектная работа: Защита данных клиентов страховой компании

## Описание проекта
Необходимо защитить данные клиентов страховой компании «Хоть потоп». Разработать такой метод преобразования данных, чтобы по ним было сложно восстановить персональную информацию. Также обосновать корректность его работы. 

Нужно защитить данные, чтобы при преобразовании качество моделей машинного обучения не ухудшилось. Подбирать наилучшую модель не требуется.

## План по выполнению проекта

 1. Загрузить и изучить данные.
 2. Ответить на вопрос и обосновать решение.
 3. Признаки умножить на обратимую матрицу. Изменится ли качество линейной регрессии? (Её можно обучить заново.)
     1. Изменится. Привести примеры матриц.
     2. Не изменится. Указать, как связаны параметры линейной регрессии в исходной задаче и в преобразованной.
 4. Предложить алгоритм преобразования данных для решения задачи. Обосновать, почему качество линейной регрессии не поменяться.
 5. Запрограммировать этот алгоритм, применив матричные операции. Проверить, что качество линейной регрессии из sklearn не отличается до и после преобразования. Применить метрику R2.

## Описание данных

Набор данных находится в файле `datasets/insurance.csv`.

- **Признаки**: пол, возраст и зарплата застрахованного, количество членов его семьи.
- **Целевой признак:** количество страховых выплат клиенту за последние 5 лет.


# Теория - умножение матриц

<!-- Для отображения LaTex --> 
<!-- <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script> -->
<!-- <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.4/latest.js?config=TeX-AMS_CHTML"></script> -->

Обозначения:

- $X$ — матрица признаков (нулевой столбец состоит из единиц)

- $y$ — вектор целевого признака

- $P$ — матрица, на которую умножаются признаки

- $w$ — вектор весов линейной регрессии (нулевой элемент равен сдвигу)

в Предсказания:

$$
a = Xw
$$

Задача обучения:

$$
w = \arg\min_w MSE(Xw, y)
$$

Формула обучения:

$$
w = (X^T X)^{-1} X^T y
$$

**Вопрос:** Признаки умножают на обратимую матрицу? Изменится ли качество линейной регрессии?

**Ответ:**

Нет, не изменится качество линейной регрессии после умножения на обратную матрицы

**Обоснование:**

Немного об обратимой матрицы.
Квадратные матрицы, для которых можно найти обратные, называются **обратимыми** (англ. invertible matrix). Но не у каждой матрицы есть обратная. Например, у матрицы A, состоящей только из нулей, нет обратной: в результате любого умножения будут только нули. Такая матрица называется **необратимой** (англ. non-invertible matrix).
**Обратная для квадратной матрицы A** (англ. inverse matrix) — матрица A с верхним индексом -1, произведение которой на А равно единичной матрице. Умножение может быть в любом порядке:
$$ AA^{-1} = A^{-1}A = E $$

Пусть F (Features) произвольная матрица признаков, размерность которой равна размерности матрицы признаков X, тогда: $ F \equiv X $ (эквивалентны)

_*Примечание: Матрица признака должна быть квадратичной формой, т.е. длина строк равна длине столбцов*_ $ i = j $

Тогда у нас в этом случае можно рассмотреть так:

$$ F = XP $$

где:
    - $X$ - матрица признаков (нулевой столбец состоит из единиц)
    - $P$ — матрица, на которую умножаются признаки


Формула предсказания будет иметь следующий вид:
$$ a = Fw $$

Вектор весов линейной регрессии:
$$ w = \frac{a}{F} $$

Формула обучения:
$$ w = (F^T  F)^{-1} F^T y $$

Подставляем формулу предсказания в формулу обучения:
$$ \frac{a}{F} = (F^T  F)^{-1} X^T y $$

Умножим признаки F на обе стороны, чтобы перенести признак на правую часть
$$ a = F(F^T  F)^{-1} X^T y $$

Значения $ F = XP $ подставляем в эту формулу $ a = F(F^T  F)^{-1} X^T y $ и получим:

$$ a = XP((XP)^T XP)^{-1} (XP)^T y$$

$$ / (XP)^T = X^T P^T / $$

$$ a = XP(X^T P^T)^{-1} X^{-1} P^{-1} X^T P^T y$$

$$ a = XX^{-1} PP^{-1} (X^T P^T)^{-1}  X^T P^T y$$

$$ / PP^{-1} = E / $$

$$ / (X^T P^T)^{-1} = (X^T)^{-1} (P^T)^{-1} / $$

$$ a = XX^{-1} E (X^T)^{-1} (P^T)^{-1} X^T P^T y$$

$$ a = XX^{-1} E (X^T)^{-1}X^T (P^T)^{-1} P^T y$$

$$ / (P^T)^{-1} P^T = E / $$

$$ a = XX^{-1} E (X^T)^{-1}X^T E y$$

$$ / E*E = E / $$

$$ a = XX^{-1} (X^T)^{-1}X^T y E$$

$$ / X^{-1} (X^T)^{-1} = (XX^T)^{-1} / $$

$$ a = X (XX^T)^{-1} X^T y E$$

$$ По \quad формуле \quad обучения: w = (X^T X)^{-1} X^T y $$

$$ a = X w E = X w $$

Как видим, что после умножение матрицы признаков на обратимую матрицу предсказания a не изменилась. Следовательно, качество линейной регрессии не изменяется, т.к. она не зависит от масштаба признаков. Если хотим улучшить качество линейной регрессии, тогда нужно преобразовать признаки или добавить новые признаки

# Алгоритм преобразования

## Предложите алгоритм преобразования данных для решения задачи. Обоснуйте, почему качество линейной регрессии не поменяется

**Алгоритм**

Нам нужно разработать алгоритм преобразования данных, чтобы по ним было сложно восстановить персональную информацию. Но при этом нужно защитить данные, чтобы при преобразовании качество моделей машинного обучения не ухудшилось.

Тогда здесь алгоритм будет:
1. Произвольно создадим матрицу признаков, размер которой равен размеру матрицы признаков (в этом случае произвольно создадим квадратную матрицу)
2. Умножим эти матрицы
3. Добавить нулевой столбец в полученном матрице и выводить в линейную регрессию

Следовательно:
$$ F = XA $$

где:
    - $X$ - матрица признаков
    - $A$ — произвольная матрица, на которую умножаются признаки

**Обоснование**

Пусть произвольная (A - arbitrary) матрица будет иметь следующий вид:
$$
A =
 \begin{pmatrix}
  a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\
  a_{2,1} & a_{2,2} & \cdots & a_{2,n} \\
  \vdots  & \vdots  & \ddots & \vdots  \\
  a_{n,1} & a_{n,2} & \cdots & a_{n,n}
 \end{pmatrix}
$$

А матрица признаков:

$$
X =
 \begin{pmatrix}
  x_{1,1} & x_{1,2} & \cdots & x_{1,n} \\
  x_{2,1} & x_{2,2} & \cdots & x_{2,n} \\
  \vdots  & \vdots  & \ddots & \vdots  \\
  x_{m,1} & x_{m,2} & \cdots & x_{m,n}
 \end{pmatrix}
$$

_*Примечание: кол-во столбцов матрицы признаков должны быть равен кол-ву строк произвольной матрицы*_

Тогда полученная матрица будет:

$$
F =
 \begin{pmatrix}
  f_{1,1} & f_{1,2} & \cdots & f_{1,n} \\
  f_{2,1} & f_{2,2} & \cdots & f_{2,n} \\
  \vdots  & \vdots  & \ddots & \vdots  \\
  f_{m,1} & f_{m,2} & \cdots & f_{m,n}
 \end{pmatrix}
$$

# Проверка алгоритма

Нам известно, что приблизительное значение R2_score и MSE выдали: 0,42 и 0,12. Теперь преобразуем матрицу и смотрим, сильно ли изменились эти значения?

###### До преобразования

![img.png](image/img.png)

![img_2.png](image/img_2.png)

###### После преобразования

![img_1.png](image/img_1.png)

![img_3.png](image/img_3.png)

Как и видим, что здесь присутствуют постронние числа. Т.е. это условно можно назвать как шифрованием. Теперь проверим на метрику R2_score и MSE

# Вывод

Мы выяснили, что качество линейной регрессии не изменилось после умножения на обратной матрицы. Что и теперь можем защищать информацию, применяя знания линейной алгебры - преобразовав матрицу (шифровка), и при этом можем сохранить значения r2_score и mse
