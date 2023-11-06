---
layout: post
title: "[WIP] 2D Geometric Constraint Solver"
author: "Антон"
preview: "assets/images/posts/2023-10-18-2d-geometric-constraint-solver/preview.gif"
---

{% include vars.html %}
{% include mathjax.html %}

### Содержание
1. [Вступление](#intro)
1. [Постановка задачи](#requirements)
1. [Обозначения](#definitions)
1. [Пример](#example)
1. [Оптимизация](#optimization)

### Вступление <a name="intro"></a>

<!--excerpt-->

Я провожу кучу времени в MCAD программах, таких как SolidWorks, Onshape, Fusion 360 и т.д. Мне всегда было интересно, как они устроены "под капотом", поэтому я решил создать свой собственный 2D-редактор чертежей с ограниченным набором геометрических примитивов (только отрезки и дуги окружностей) и базовыми ограничениями. Главная проблема, которую нужно решить, это [Geometric constraint solving](https://en.wikipedia.org/wiki/Geometric_constraint_solving), и для фана я решил разработать свой метод решения этой проблемы, не изучая предварительно существующие подходы.

Вот что в итоге у меня получилось:

<img src="{{ site.baseurl }}/assets/images/posts/2023-10-18-2d-geometric-constraint-solver/preview.gif"/>

### Постановка задачи <a name="requirements"></a>

Основные функции разрабатываемого 2D редактора, которые должны поддерживаться:

1. Добавление и удаление отрезков и дуг окружностей
2. Передвижение и изменение с помощью мыши существующих отрезков и дуг окружностей, при этом программа должна автоматически следить за тем, чтобы существующие ограничения не нарушались
3. Наложение новых ограничений на линии и дуги окружностей и удаление существующих ограничений

Ограничения, которые должны поддерживаться:

1. **COINCIDENCE**: совпадение точек
2. **PARALLELITY**: параллельность отрезков
3. **PERPENDICULARITY**: перпендикулярность отрезков
4. **EQUAL_LENGTH_OR_RADIUS**: равная длина отрезков или равный радиус дуг
5. **FIXED**: неподвижность точек
6. **HORIZONTALITY**: горизонтальность точек
7. **VERTICALITY**: вертикальность точек
8. **TANGENCY**: касательность дуги и отрезка или касательность двух дуг
9. **CONCENTRICITY**: концентричность дуг

### Обозначения <a name="definitions"></a>

1. $x$ -- скаляр
2. $\vec{y}$ -- вектор
3. $\vec{f}()$ -- [вектор-функция](https://ru.wikipedia.org/wiki/%D0%92%D0%B5%D0%BA%D1%82%D0%BE%D1%80-%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D1%8F)

### Пример <a name="example"></a>

{% include clickableSVG.html path=images_path name="example0.svg" %}
{% include clickableSVG.html path=images_path name="example1.svg" %}

### Оптимизация <a name="optimization"></a>

$$
\begin{array}{rl}
\min _\vec{x} & f(\vec{x}) \\
\text { subject to } & \vec{h}(\vec{x}) \geq 0 \\
& \vec{g}(\vec{x})=0 .
\end{array}
$$

$$
\begin{array}{rl}
\min _\vec{x} & f(\vec{x}) \\
\text { subject to } & \vec{g}(\vec{x})=0.
\end{array}
$$