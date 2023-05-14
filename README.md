# project_01

Библиотека написана с целью позволить быстро вычислять необходимое приращение скорости
для маневра между изначально некомпланарными эллиптическими орбитами, заданными своими
Кепплеровскими элементами:
a - большая полусь
e - эксцентриситет
i - наклонение
Ω - долгота восходящего узла
ω - аргумент перицентра

Задача нахождения приращения скорости состоит из нескольких этапов:
1) Проделыается конвертация Кепплеровских элементов орбиты в радиус-вектор (r) и вектор
скорости (v).
2) По известным параметрам орбит в  виде радиус-вектора (r) и вектора скорости (v) вос-
станавливается истинная аномалия космического аппарата, находящегося в точке узла.
3) По известным параметрам орбиты и аномалии подсчитывается нужная скорость для первого
маневра, которых позволяет изменить текущую орбиту на орбиту, компланарную с целевой.
4) Далее проделывается второй компланарный маневр уже в одной плоскости, вычисляется не-
щбходимое приращение скорости для такого маневра.

Импульсный маневр будет проводится в точках узлов (точки пересения орбиты и плоскости эк
клиптики.

1.Для дальнейшего вычисления приращения скорости необходимо, в первую очередь, по задан-
ным орбитам конвертировать их в радиус - векторы и векторы скорости аппарата (КА).
Для этого использовался алгоритм ALGORITHM 10: COE2RV из [1]. Функции
Cartesian_orbit rv_solver()
Cartesian_orbit rv_solver_true_anomaly_is_zero() 
выполняют саму конвертацию. Первая для общего случая, когда помимо пяти элементов орбиты
известна еще и истинная аномалия. Вторая функция делает конвертацию в предположении, что 
истинная аномалия равна нулю (КА в перигее).Это необходимо для нахождения радиус-вектора,
направленного в перигей.


2. Зная долготу восходящего узла и параметры исходной и целевой орбит,посчитаем истинную
аномалию  для КА, находящегося в узле. Функция double true_anomaly() по заданной долготе
восходящего узла и найденным ранее радиус-вектором, направленным на перигей, считает ис-
тинную  аномалию для КА в точке узла. Вектор n, объявленный в классе Orbit самой орбиты, 
находится как проекция вектора, направленного на узел, на оси системы координат, связан-
ной с осью эклиптики.

3. Далее, по известным параметрам  исходной и целевой орбит и истинной аномалии КА,нахо-
дящегося в узле на исходной орбите, вычисляется  приращение скорости для первого маневра
с помощью функции.
double first_stage_speed

Формулы для рассчета представлены в [2]. (формулы 10.10, 10.11)

Для 4 пар орбит написаны тесты (maneuver1), выдающие скорости [км/с].

4.  Остается совершить второй маневр для изменения  орбиты уже не выходя из плоскости, в
которой движется КА.Функция double second_stage_speed() внутри меняет параметры исходной 
орбиты (наклоняем исходную орбиту в связи с первым маневром) так, чтобы она была компла-
нарна с целевой. Для нахождения истинных аномалий в точки узлов проделаем ту же операцию
что и ранее. Затем уже  по вычисленной истинной аномалии находятся R1, R2 и далее второе
приращение скорости.
Затем  вычисляет приращение скорости в точке узла и, таким образом, со-
вершается второй переход уже на целевую орбиту.


Источники:
1. Vallado fundamentals of astrodynamics and applications [стр 118]
2. М. Ю. Овчинников Введение в динамику космического полета
3. Заборский, Кириллюк: Оптимальный биэллиптический переходмежду компланарными эллиптически
ми орбитами
