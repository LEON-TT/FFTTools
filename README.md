﻿# Использование БФП для цифровой обработки изображений

## Предисловие

Цифровая фотография или иное растровое изображение представляет собой массив чисел, зафиксированных сенсорами уровней яркости, в двумерной плоскости.
Зная, что с математической точки зрения тонкая линза выполняет преобразование Фурье изображений, размещённых в фокальных плоскостях, можно создать алгоритмы обработки изображений, являющихся аналогами обработки изображений классической оптической системой.
### Формула таких алгоритмов будет выглядеть следующим образом:
1. Z=FFT(X) – прямое двухмерное преобразование Фурье
2. Z′=T(Z) – применение функции или транспаранта к Фурье-образу изображения
3. Y=BFT(Z′) – обратное двухмерное преобразование Фурье
Хотя оптическая система линз осуществляет преобразование Фурье на непрерывном диапазоне аргумента и для непрерывного спектра, но при переходе к цифровой обработке данных формулы преобразования Фурье могут быть заменены на формулы дискретного преобразования Фурье.
 
## Алгоритм размытия изображения

В оптических системах диафрагма, размещённая в фокальной плоскости, представляет собой простое отверстие в экране. В результате прохождения светового потока через диафрагму, волны высоких частот (с более короткими длинами волн) проходят через препятствие, а волны с низких частот (с более длинными длинами волн) отсекаются экраном. Таким образом повышается резкость получаемого изображения.
Если заменить отверстие в экране на препятствие в экране, то в результате будет получено размытое изображение, поскольку оно будет сформировано из частот волн больших длин.
### Алгоритм:
1. Пусть X(N1,N2) – массив яркостей пикселей изображения.
2. Вычислить Px = средняя (среднеквадратичная) яркость пикселей в массиве X
3. Вычислить массив Z=FT(X) – прямое двухмерное дискретное преобразование Фурье
4. Вычислить массив Z′=T(Z), где T – обнуление строк и столбцов, находящихся в заданных внутренних областях матрицы-аргумента, соответствующих высоким частотам (то есть обнуление коэффициентов Фурье-разложения, соответствующих высоким частотам)
6. Вычислить массив Y=RFT(Z′) – обратное двухмерное дискретное преобразование Фурье
7. Вычислить Py = средняя (среднеквадратичная) яркость пикселей в массиве Y
8. Нормировать массив Y(N1,N2) по среднему уровню яркости Px/Py

## Алгоритм повышения резкости изображения

В оптических системах диафрагма, размещённая в фокальной плоскости, представляет собой простое отверстие в экране. В результате прохождения светового потока через диафрагму, волны высоких частот (с более короткими длинами волн) проходят через препятствие, а волны с низких частот (с более длинными длинами волн) отсекаются экраном. Таким образом повышается резкость получаемого изображения.
### Алгоритм:
1. Пусть X(N1,N2) – массив яркостей пикселей изображения.
2. Вычислить Px = средняя (среднеквадратичная) яркость пикселей в массиве X
3. Вычислить массив Z=FT(X) – прямое двухмерное дискретное преобразование Фурье
4. Сохранить значение L=Z(0,0) – соответствующее средней яркости пикселей исходного изображения
5. Вычислить массив Z′=T(Z), где T – обнуление строк и столбцов, находящихся в заданных внешних областях матрицы-аргумента, соответствующих низким частотам (то есть обнуление коэффициентов Фурье-разложения, соответствующих низким частотам)
7. Восстановить значение Z’(0,0)=L – соответствующее средней яркости пикселей исходного изображения
8. Вычислить массив Y=RFT(Z′) – обратное двухмерное дискретное преобразование Фурье
9. Вычислить Py = средняя (среднеквадратичная) яркость пикселей в массиве Y
10. Нормировать массив Y(N1,N2) по среднему уровню яркости Px/Py

## Алгоритм масштабирования изображения

В оптических системах световой поток в фокальной плоскости системы представляет собой Фурье-преобразование исходного изображения. Размер получаемого на выходе оптической системы изображения определяется соотношением фокальных расстояний объектива и окуляра.
### Алгоритм:
1. Пусть X(N1,N2) – массив яркостей пикселей изображения.
2. Вычислить Px = средняя (среднеквадратичная) яркость пикселей в массиве X
3. Вычислить массив Z=FT(X) – прямое двухмерное дискретное преобразование Фурье
4. Вычислить массив Z′=T(Z), где T – либо добавление нулевых строк и столбцов матрицы соответствующих высоким частотам, либо удаление строк и столбцов матрицы соответствующих высоким частотам для получения требуемого размера итогового изображения
5. Вычислить массив Y=RFT(Z′) – обратное двухмерное дискретное преобразование Фурье
6. Вычислить Py = средняя (среднеквадратичная) яркость пикселей в массиве Y
7. Нормировать массив Y(M1,M2) по среднему уровню яркости Px/Py

## Фурье-вычисления для сравнения изображений

Традиционная техника “начального уровня”, сравнения текущего изображения с эталоном основывается на рассмотрении изображений как двумерных функций яркости (дискретных двумерных матриц интенсивности). При этом измеряется либо расстояние между изображениями, либо мера их близости.
Как правило, для вычисления расстояний между изображениями используется формула, являющаяся суммой модулей или квадратов разностей интенсивности:

- d(X,Y) = SUM ( X[i,j] — Y[i,j] )^2

Если помимо простого сравнения двух изображений требуется решить задачу обнаружения позиции фрагмента одного изображения в другом, то классический метод “начального уровня”, заключающийся в переборе всех координат и вычисления расстояния по указанной формуле, как правило, терпит неудачу практического использования из-за требуемого большого количества вычислений.
Одним из методов, позволяющих значительно сократить количество вычислений, является применение Фурье преобразований и дискретных Фурье преобразований для расчёта меры совпадения двух изображений при различных смещениях их между собой. Вычисления при этом происходят одновременно для различных комбинаций сдвигов изображений относительно друг друга.
Наличие большого числа библиотек, реализующих Фурье преобразований (во всевозможных вариантах быстрых версий), делает реализацию алгоритмов сравнения изображений не очень сложной задачей для программирования.

### Постановка задачи

Пусть даны два изображения X и Y – изображение и образец, размеров (N1,N2) и (M1,M2) соответственно и Ni > Mi
Требуется найти координаты образца Y в полном изображении X и вычислить оценочную величину — меру близости.

### Корреляция как мера между изображениями

Согласно определению, корреляцией <F,G> двух функций F и G называется величина: 

- <F,G> = SUM F(i)*G(i)

Эта величина хорошо известна из курса математики и геометрии, посвященного линейным пространствам, где носит название скалярного произведения. Будем использовать в качестве меры между изображениями формулу:

- m(X,Y) = SUM ( X[i,j] * Y[i,j] ) / ( SQRT ( SUM X[i,j] ^2 ) * SQRT ( SUM Y[i,j] ^2 ) )

или

- m(X,Y) = <X,Y> / ( SQRT (<X,X> ) * SQRT (<Y,Y> ) )

Данная величина получена из операции скалярного произведения векторов (рассматривая изображения как векторы в многомерном пространстве). И даже более — эта же формула представляет собой и стандартную статистическую формулу критерия для гипотезы о совпадении двух вероятностных распределений.

##### Примечание:

При вычислении корреляции между фрагментами изображений, если одно изображение меньше другого, будем делить только на значение норм у пересекающийся частей.

### Свёртка двух функций

Согласно определению, свёрткой двух функций F и G называется функция FхG:

- FхG(t) = SUM F(i)*G(j)|i+j=t

Пусть G’(t) = G(-t) и F’(t) = F(-t), тогда, очевидна справедливость равенств:

- FхF’(0) = SUM F(i)^2 – скалярное произведение вектора F на самого себя
- GхG’(0) = SUM G(j)^2– скалярное произведение вектора G на самого себя
- FхG’(0) = SUM F(i)*G(i) – скалярное произведение двух векторов F и G

Так же очевидно, что FхG’(t) равна корреляции получаемой в результате сдвига одного вектора, относительно другого на шаг t (это легко проверить явной подстановкой значений в формулу корреляции).

### Преобразование Фурье

Преобразование Фурье (FFT) — операция, сопоставляющая одной функции вещественной переменной другую функцию, также вещественной переменной. Эта новая функция описывает коэффициенты («амплитуды») при разложении исходной функции на элементарные составляющие — гармонические колебания с разными частотами.
Преобразование Фурье функции f вещественной переменной является интегральным и задаётся следующей формулой:

- g[y] = S f[x]*exp(-i*(xy))*dx

Разные источники могут давать определения, отличающиеся от приведённого выше выбором коэффициента перед интегралом, а также знака «−» в показателе экспоненты. Но все свойства будут те же, хотя вид некоторых формул может измениться.
Кроме того, существуют разнообразные обобщения данного понятия.

### Многомерное преобразование Фурье

##### Преобразование Фурье функций, заданных на пространстве R^n, определяется формулой:

- g[y1,y2,...,yn] ~ S f[x1,x2,...,xn]*exp(-i*<x,y>)*dx1*dx2*...*dxn

##### Обратное преобразование в этом случае задается формулой:

- f[x1,x2,...,xn] ~ S g[y1,y2,...,yn]*exp(i*<x,y>)*dy1*dy2*...*dyn

Как и прежде, в разных источниках определения многомерного преобразования Фурье могут отличаться выбором константы перед интегралом.

#### Дискретное преобразование Фурье

Дискретное преобразование Фурье (в англоязычной литературе DFT, Discrete Fourier Transform) — это одно из преобразований Фурье, широко применяемых в алгоритмах цифровой обработки сигналов (его модификации применяются в сжатии звука в MP3, сжатии изображений в JPEG и др.), а также в других областях, связанных с анализом частот в дискретном (к примеру, оцифрованном аналоговом) сигнале. Дискретное преобразование Фурье требует в качестве входа дискретную функцию. Такие функции часто создаются путём дискретизации (выборки значений из непрерывных функций). Дискретные преобразования Фурье помогают решать дифференциальные уравнения в частных производных и выполнять такие операции, как свёртки. Дискретные преобразования Фурье также активно используются в статистике, при анализе временных рядов. Существуют многомерные дискретные преобразования Фурье.

#### Формулы дискретных преобразований

##### Прямое преобразование:

- g[y1,y2,...,yn] ~ SUM f[x1,x2,...,xn]*exp(-i*<j,k>/n)

##### Обратное преобразование:

- f[x1,x2,...,xn] ~ SUM g[y1,y2,...,yn]*exp(i*<j,k>/n)

Дискретное преобразование Фурье является линейным преобразованием, которое переводит вектор временных отсчётов в вектор спектральных отсчётов той же длины. Таким образом преобразование может быть реализовано как умножение симметричной квадратной матрицы на вектор:

### Фурье-преобразования для вычисления свёртки

Одним из замечательных свойств преобразований Фурье является возможность быстрого вычисления корреляции двух функций определённых, либо на действительном аргументе (при использовании классической формулы), либо на конечном кольце (при использовании дискретных преобразований).
И хотя подобные свойства присущи многим линейным преобразованиям, для практического применения, для вычисления операции свёртки, согласно данному нами определению, используется формула

- FхG = BFT ( FFT(F)*FFT(G) )

Где

- FFT – операция прямого преобразования Фурье
- BFT – операция обратного преобразования Фурье

Проверить правильность равенства довольно легко – явно подставив в формулы Фурье-преобразований и сократив получившиеся формулы 

### Фурье-преобразования для вычисления корреляции

Пусть <F,G>(t) равна корреляции получаемой в результате сдвига одного вектора, относительно другого на шаг t
Тогда, как уже показано ранее, выполняется 

- <F,G>(t) = FхG’(t) = BFT ( FFT(F)*FFT(G’) )

Если используются реализации алгоритма трансформации Фурье через комплексные числа, то такие преобразования обладают ещё одним замечательным свойством:

- FFT(G’) = CONJUGATE ( FFT(G) )

Где 

- CONJUGATE ( FFT(G) ) – матрица, составленная из сопряжённых элементов матрицы FFT(G)

Таким образом, получаем

- <F,G>(t) = BFT ( FFT(F)*CONJUGATE ( FFT(G) ))

### Фурье-преобразования для решения задачи фрагмента в полном изображении

При использовании формулы для оценки расстояния между изображениями при сдвиге (i,j) относительно друг друга
- m(X,Y) (i,j) = <X,Y>(i,j) / ( |X|(i,j) ) * |Y|(i,j) ),

получаем, что

- <X,Y> = XxY’ = BFT ( FFT(X) * CONJUGATE ( FFT(Y) ) )
- |X|^2 = <X,X> = XxX’xE’ = BFT ( FFT(X) * CONJUGATE ( FFT(X) ) * CONJUGATE ( FFT(E) ) ) = BFT ( SQUAREMAGNITUDE( FFT(X) ) * CONJUGATE ( FFT(E) ) )
- |Y|^2 = <Y,Y> = YxY’xE’ = BFT ( FFT(Y) * CONJUGATE ( FFT(Y) ) * CONJUGATE ( FFT(E) ) )

Где

- <X,Y>(i,j) – скалярное произведение двух изображений, получаемых при сдвиге (i,j) относительно друг друга изображений X и Y
- E – изображение размера равному минимальным размерам X и Y, и заполненное единичными значениями (то есть “кадр” в котором сравниваются X и Y)
- |X|(i,j) – норма общей части изображения X при сдвиге (i,j)
- |Y|(i,j) – норма общей части изображения Y при сдвиге (i,j)
- FFT – операция прямого двухмерного дискретного преобразования Фурье
- BFT – операция обратного двухмерного дискретного преобразования Фурье
- CONJUGATE – операция вычисления матрицы из сопряжённых элементов
- SQUAREMAGNITUDE– операция вычисления матрицы квадратов амплитуд элементов

### Упрощение формул для решения поставленной задачи

При решении задачи для поиска одного образца, дополнительное нормирование образца является излишним, а также вычисление нормы у общей части может быть заменено на сумму яркостей пикселей в этой общей части или на сумму квадратов яркостей в этой общей части 
При использовании формулы для оценки расстояния между изображениями при сдвиге (i,j) относительно друг друга

- m(X,Y) (i,j) = <X,Y>(i,j) / |X|^2(i,j),

получаем, что

- <X,Y> = BFT ( FFT(X) * CONJUGATE ( FFT(Y) ) )
- <X,X> = BFT ( SQUAREMAGNITUDE( FFT(X) ) * CONJUGATE ( FFT(E) ) )

Где

- <X,Y>(i,j) – скалярное произведение двух изображений, получаемых при сдвиге (i,j) относительно друг друга изображений X и Y
- E – изображение размера равному минимальным размерам X и Y, и заполненное единичными значениями (то есть “кадр” в котором сравниваются X и Y)
- <X,X>(i,j) – норма (сумма яркостей пикселей) общей части изображения X при сдвиге (i,j)
- FFT – операция прямого двухмерного дискретного преобразования Фурье
- BFT – операция обратного двухмерного дискретного преобразования Фурье
- CONJUGATE – операция вычисления матрицы из сопряжённых элементов
- SQUAREMAGNITUDE– операция вычисления матрицы квадратов амплитуд элементов


### Алгоритм поиска фрагмента в полном изображении

Пусть даны два изображения X и Y – изображение и образец, размеров (N1,N2) и (M1,M2) соответственно и Ni > Mi
Требуется найти координаты образца Y в полном изображении X и вычислить оценочную величину — меру близости.

- Расширить изображение Y до размера (N1,N2), дополнив его нулями
- Сформировать изображение E из единиц размера (M1,M2) и расширить до размера (N1,N2), дополнив его нулями
- Вычислить <X,Y> = BFT ( FFT(X) * CONJUGATE ( FFT(Y) ) )
- Вычислить <X,X> = BFT ( SQUAREMAGNITUDE( FFT(X) ) * CONJUGATE ( FFT(E) ) )
- Вычислить M[i,j] = (f + <X,Y> [i,j])/(f + <X,X> [i,j])
- В матрице M найти элемент с максимальным значением – координаты этого элемента и являются искомой позицией образца в полном изображении, а значение равно оценке меры сравнения.

##### Примечание:

При использовании дискретного преобразования Фурье, матрица M содержит также элементы от циклического сдвига изображений между собой. Поэтому, если не требуется анализировать циклический сдвиг кадров, то поиск максимального элемента в матрице M нужно ограничить областью (0,0)-(N1-M1, N2-M2).

## Примеры реализации

Реализованные алгоритмы являются частью библиотеки с открытым исходным кодом FFTTools. Интернет-адрес: github.com/dprotopopov/FFTTools

## Используемое программное обеспечение

- Microsoft Visual Studio 2015 C# - среда и язык программирования
- EmguCV/OpenCV – C#/C++ библиотека структур и алгоритмов для обработки изображений
- FFTWSharp/FFTW – C#/C++ библиотека реализующая алгоритмы быстрого дискретного преобразования Фурье

## Литература
- А.Л. Дмитриев. Оптические методы обработки информации. Учебное пособие. СПб. СПюГУИТМО 2005. 46 с.  
- А.А.Акаев, С.А.Майоров «Оптические методы обработки информации» М.:1988  
- Дж.Гудмен «Введение в Фурье-оптику» М.:Мир 1970
