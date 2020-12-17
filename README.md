## Оглавление
1. [Метрические алгоритмы классификации](#Метрические-алгоритмы-классификации)
    1. [Алгоритм 1NN](#Алгоритм-1NN)
    2. [Алгоритм k ближайших соседей](#Алгоритм-k-ближайших-соседей)
    3. [Алгоритм K взвешенных ближайших соседей](#Алгоритм-K-взвешенных-ближайших-соседей)
    4. [Метод парзеновского окна](#Метод-парзеновского-окна)
    5. [Алгоритм (STOLP)](#Алгоритм-(STOLP))
    6. [Метод потенциальных функций](#Метод-потенциальных-функций)
2. [Байесовские методы классификации](#Байесовские-методы-классификации)
    1. [Линии уровня нормального распределения](#Линии-уровня-нормального-распределения)
    2. [Наивный  байесовский классификатор](#Наивный-байесовский-классификатор)
    3. [Plug-in алгоритм](#Plug-in-алгоритм)
    4. [Линейный дискриминант Фишера (ЛДФ)](#Линейный-дискриминант-Фишера-(ЛДФ))
    
3. [Линейные методы классификации](#Линейные-методы-классификации)

    1.[Метод стохастического градиента](#Метод-стохастического-градиента)
    
    2.[Адаптивный линейный элемент](#Адаптивный-линейный-элемент)
    
    3.[Логистическая регрессия](#Логистическая-регрессия)
  

<center>
<table>
  <tbody>
	   <tr>
      <th>Метод</th>
      <th>Параметры</th>
      <th>Точность</th>
    </tr>
    <tr>
      <td>KWNN</a></td>
      <td>k=9</td>
      <td>0.0333</td>
    </tr>
    <tr>
      <td>KNN</a></td>
      <td>k=6</td>
      <td>0.0333</td>
    </tr>
   <tr>
      <td>Парзеновские окна</a></td>
      <td>h=0,4 (Прямоугольное ядро)</td>
      <td>0.04</td>
    </tr>
    <tr>
      <td>Парзеновские окна</a></td>
      <td>h=0,4(Треугольное ядро)</td>
      <td>0.04</td>
    </tr>
     <tr>
      <td>Парзеновские окна</a></td>
      <td>h=0,4(ядро Епанечникова)</td>
      <td>0.04</td>
    </tr>
    <tr>
      <td>Парзеновские окна</a></td>
      <td>h=0,4(Квартическое ядро)</td>
      <td>0.04</td>
    </tr>
    <tr>
      <td>Парзеновские окна</a></td>
      <td>h=0,1 (Гауссовское ядро)</td>
      <td>0.04</td>
    </tr>
    <tr>
      <td>Метод потенциальных функций(для всех)</a></td>
      <td>h=1 </td>
      <td>0.0467</td>
    </tr>
	  
   </table>



## Алгоритм 1NN

[Оглавление](#Оглавление)

Алгоритм ближайшего соседа - 1NN  относит классифицируемый объект ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула1.png) к тому классу , к которому относится его ближайший сосед.
 ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула2.png) 

### Приемущество: 
 Простота 

### Недостатки: 
  Неустойчивость к погрешностям — выбросам.
 Отсутствие параметров, которые можно было бы настраивать по выборке. Алгоритм полностью зависит от того, насколько удачно выбрана
метрика *ρ*.



 1. Нужно задать метрическую функцию. 
 2. Обучающая выборка сортируется в порядке увеличения расстояния от классифицируемого элемента.  
 3. Классифицируемый элемент относим к классу, к которому принадлежит ближайший элемент(первый в отсортированной выборке). 
  
  
 
  Метрические алгоритмы классификации с обучающей выборкой *Xl* относят объект *u* к тому классу *y*, для которого суммарный вес ближайших обучающих объектов  ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула3.png) максимален: 
  ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула4.png)
  
  
   , где весовая функция *w(i, u)* оценивает степень важности *i*-го соседа для классификации объекта *u*.
   Функция ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула3.png) называется оценкой близости объекта *u* к классу *y*. Выбирая различную весовую функцию *w(i, u)* можно получать различные метрические классификаторы.
  
  ```R
  distances = function(xl, data, k) { # возвращает отсортированный набор данных по метрике для объекта 
 cases = dim(data)[1]
 features = dim(data)[2]-1
 dists = matrix(0, cases, 2) # создаем матрицу расстояний 
   for (i in 1:cases) {
     cost = k(xl, data[i,1:features])
     dists[i,] = c(cost, i)
   }
   idx = order(dists[,1])
   data[dists[idx,2],]
 }
NN = function(xl, data, k=dist) 
 { # находит 1-ближайшего соседа 
 sorted = distances(xl, data, k)
   sorted[1,features+1]
 }
 ```
 
 ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/1.1nn.png)

[Оглавление](#Оглавление)


## Алгоритм k ближайших соседей 

[Оглавление](#Оглавление)

 Алгоритм k ближайших соседей – kNN относит объект *u* к тому классу,
элементов которого больше среди *k* ближайших соседей  ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула6.png)


 Алгоритм работает следующим образом: пусть дан классифицируемый объект *u* и обучающая выборка . Требуется определить класс объекта *u* на основе данных из обучающей выборки. Для этого:

1. Вся выборка  сортируется по возрастанию расстояния от объекта *u* до каждого объекта выборки.
2. Проверяются классы *k* ближайших соседей объекта *u*. Класс, встречаемый наиболее часто, присваивается объекту *u*. Для оценки близости классифицируемого объекта *u* к классу  алгоритм kNN использует следующую функцию: ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула7.png) , где *i* - порядок соседа по расстоянию к классифицируемому объекту *u*.

```R
kNN = function(xl, data, k, m=dist) { # находит k-ближайших соседей  
 sorted = distances(xl, data, m)
   
   n = 10 
   counts = rep(0, times=n)
 features = dim(data)[2]-1
 for (i in 1:k) {
     cls = sorted[i,features+1]
     counts[cls] = counts[cls] + 1
   }
   argmax = 1
   for (i in n) {
     if (counts[argmax] < counts[i]) {
       argmax = i
     }
   }
   
   cls[argmax]
 }
 ```
 
 ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/KNN.png)

Алгоритм kNN выглядит  качествеено. Для того чтобы привести более точное обоснование чем kNN лучше в этом случае, чем 1NN, следует прибегнуть к скользящему контролю.

Для поиска оптимальных параметров для каждого из рассматриваемых ниже метрических алгоритмов используется LOO - leave-one-out (критерий скользящего контроля), который состоит в следующем:

1.Исключать объекты *x(i)* из выборки *Xl* по одному, получится новая выборка без объекта *x(i)*.

2.Запускать алгоритм от объекта *u*, который нужно классифицировать, на выборке *Xl_1*.

3.Завести переменную *Q* (накопитель ошибки, изначально Q = 0) и, когда алгоритм ошибается, Q = Q + 1.

4.Когда все объекты *x(i)* будут перебраны, вычислить LOO = Q / l (l -- количество объектов выборки).

При минимальном значении LOO получим оптимальный параметр алгоритма.

### Преимущества:
 Преимущество LOO состоит в том, что каждый объект ровно один раз участвует в контроле, а длина обучающих подвыборок лишь на единицу меньше длины полной выборки.
 ### Недостатки:
  Недостатком LOO является большая ресурсоёмкость, так как обучаться приходится L раз. Некоторые методы обучения позволяют достаточно быстро перенастраивать внутренние параметры алгоритма при замене одного обучающего объекта другим. В этих случаях вычисление LOO удаётся заметно ускорить. 
```R
LOO = function(xl,class) {
  n = dim(xl)[1];
  loo = rep(0, n-1) 
  
    for(i in 1:n){
      X=xl[-i, 1:3]
      u=xl[i, 1:2]
      orderedXl <- sortObjectsByDist(X, u)
      
      for(k in 1:(n-1)){
        
        test=knn(X,u,k,orderedXl)
        if(colors[test] != colors[class[i]]){
          loo[k] = loo[k]+1;
        
      }
    } 
    }
  
  loo = loo / n
  x = 1:length(loo)
  y = loo
  plot(x, y,main ="LOO for KNN(k)", xlab="k", ylab="LOO", type = "l")

  min = which.min(loo)
  lOOmin=round(loo[min],3)
  points(min, loo[min], pch = 21, col = "red",bg = "red")
  label = paste("   K = ", min, "\n", "   LOO = ", lOOmin, sep = "")
  xmin = 3*min;
  text(xmin, lOOmin, labels = label, pos = 3, col = "red")
  map(min);
}
```

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/LOO.png)

[Оглавление](#Оглавление)

## Алгоритм K взвешенных ближайших соседей

[Оглавление](#Оглавление)

K  взвешенных ближайших соседей - это метрический алгоритм классификации, основанный на оценивании сходства объектов. Классифицируемый объект относится к тому классу, которому принадлежат ближайшие к нему объекты обучающей выборки.

Реализаця метода kwNN

1. В каждом классе выбирается *k* ближайших к *U* объектов, и объект *u* относится к тому классу, для которого среднее расстояние до k ближайших соседей минимально.
2. ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%208.png), где *w(i)* — строго убывающая последовательность вещественных весов, задающая вклад *i*-го соседа при классификации объекта *u*.


 
 ```R
 kwnn <- function(xl, z, k,orderedXl)
{
  
  n <- dim(orderedXl)[2] - 1
  weights = rep(0,3)
  names(weights) <- c("setosa", "versicolor", "virginica")
  classes <- orderedXl[1:k, n+1]
  
  for(i in 1:k)
  {
    weights[classes[i]]<-weightsKWNN(i,k)+weights[classes[i]];
  }
  class <- names(which.max(weights))
  return (class)
}
```
 ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/kwnn1.png)
 Чтобы показать более высокую эффективность kwNN, подберём выборку  так, чтобы kNN ошибся при классификации, а kwNN - нет.
 
 ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/квнн%20и%20кнн.png)
 
 [Оглавление](#Оглавление)
 
   ## Метод парзеновского окна
   
   [Оглавление](#Оглавление)
   
Идея этого алгоритма в том, чтобы присваивать вес каждому объекту выборки не на основе ранга близости i-го объекта к классифицируемому, а на основе расстояния от классифицируемого объекта до данного объекта выборки.
Функция веса  ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула9.png)
Где *i* - номер объекта выборки, *z* - классифицируемый объект, *xi* - *i*-й объект выборки, *h* - ширина окна, *K* - функция ядра.

Функция ядра - произвольная чётная функция, невозрастающая на ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула10.png)


![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула11.png)

где ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/функция12.png)
```R
parzen <- function(xl, h, distances, kernelFunction = kernel.G) {
  
  n <- ncol(xl)
  classes <- xl[1:l, n]
  weights <- table(classes) # Таблица для весов классов
  weights[1:length(weights)] <- 0
  for (i in 1:l) { # Для каждого объекта выборки
    class <- xl[i, n] # Берём его класс
    r <- distances[i] / h
    weights[class] <- weights[class] + kernelFunction(r) # И прибавляем его вес к общему весу его класса
  }
  if (max(weights) != 0) # Если точке присвоились какие-нибудь веса классов (точка попала в окно)
    return (names(which.max(weights))) # Вернуть класс с максимальным весом
  return (0) # Иначе вернуть 0
}
```
Прямоугольное ядро:
![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/пр.png)
Треугольное ядро:
![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/тр.png)
Ядро Епанечникова:
![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/еп.png)
Квадратичное ядро:
![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/кв.png)
Гауссовское ядро:
![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/гс.png)

   [Оглавление](#Оглавление)
   
### Алгоритм (STOLP)

   [Оглавление](#Оглавление)
   

Выделяют несколько выдов объектов обучения:

*Эталонные* — типичные представители классов. Если классифицируемый объект близок к эталону, то, скорее всего, он принадлежит тому же классу.

*Неинформативные* — плотно окружены другими объектами того же класса. Если их удалить из выборки, это практически не отразится на качестве классификации.

*Выбросы* — находятся в окружении объектов чужого класса. Как правило, их удаление только улучшает качество классификации.

Алгоритм СТОЛП (STOLP) — алгоритм отбора эталонных объектов для метрического классификатора. Отступ - величина, показывающая, насколько объект является типичным представителем своего класса. Отступ равен разности между степенью близости объекта к своему классу и суммой близостей объекта ко всем остальным классам.

Идея алгоритма STOLP состоит в том, чтобы уменьшить исходную выборку, выбрав из неё эталонные объекты. Такой подход уменьшит размер выборки, и может улучшить качество классификации. На вход подаётся выборка, допустимый порог ошибок и порог фильтрации выбросов. 

## Алгоритм:

1.Удалить из выборки все выбросы (объекты, отступ которых меньше порога фильтрации выбросов).

2.Пересчитать все отступы заново, взять по одному эталону из каждого класса (объекты с наибольшим положительным отступом), и добавить их в множество эталонов.

3.Проклассифицировать объекты обучающей выборки, взяв в качестве обучающей выборки для этого множество эталонов. Посчитать число ошибок.

4.Если число ошибок меньше заданного порога, то алгоритм завершается.

5.Иначе присоединить ко множеству эталонов объекты с наименьшим отступом из каждого класса из числа классифицированных неправильно.

6.Повторять шаги 3-5 до тех пор, пока множество эталонов и обучающая выборка не совпадут, или не сработает проверка в пункте 4.
Реализация функция для нахождения отступа.
```R
margin = function(points,classes,point,class){
Myclass = points[which(classes==class), ]
OtherClass = points[which(classes!=class), ]
MyMargin = Parzen(Myclass,point[1:2],1,FALSE)
OtherMargin = Parzen(OtherClass,point[1:2],1,FALSE)
return(MyMargin-OtherMargin)
}
```
![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/stolp.png)

Отступ для парзвеновского окна:

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/отступ.png)

```R
 badpoints = which(margins < 0)
  badmargins = rep(0,length(badpoints))
  for (i in 1:length(badpoints)){
    badmargins[i]=margins[badpoints[i]]
  }
  
  badmargins = rev(sort(badmargins))
  print(badmargins)
 for (i in 2:length(badpoints)){
 vubros<-0
 if(abs(badmargins[i]-badmargins[i-1])>1)
    {
      vubros=i
      print(badmargins[vubros])
    }
  }
  ```
  
 [Оглавление](#Оглавление)
  
### Метод потенциальных функций

[Оглавление](#Оглавление)

Пусть дана обучающая выборка *xl* и объект *z*, который требуется классифицировать. 
Тогда:
1. Для каждого объекта выборки задаётся ширина окна *hi* (выбирается из собственных соображений).
2. Для каждого объекта выборки задаётся сила потенциала ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2019.png). 
3. Каждому объекту выборки присваивается вес по формуле ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2020.png), *K(r)*-функция ядра.
4. Суммируются веса объектов одинаковых классов. Класс с самым большим весом присваивается объекту *z*.

Программная реализация метода потенциальных функций:
```R
pF <- function(distances, potentials, h, xl, kernelFunction = kernel.G) {
  l <- nrow(xl)
  n <- ncol(xl)
  classes <- xl[, n]
  weights <- table(classes) # Таблица для весов классов
  weights[1:length(weights)] <- 0 # По умолчанию все веса равны нулю
  for (i in 1:l) { # Для каждого объекта выборки
    class <- xl[i, n] # Берется его класс
    r <- distances[i] / h[i]
    weights[class] <- weights[class] + potentials[i] * kernelFunction(r) 
  }
  if (max(weights) != 0) return (names(which.max(weights))) 
  return ("") # Если точка не проклассифицировалась, то вернуть пустую строку
}
```

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/PF.png)

### *Достоинства алгоритма:*
1. Большое количество параметров для подбора.
2. После настройки силы потенциалов, объекты выборки с нулевыми потенциалами можно не использовать при классификации. Благодаря этому - высокая скорость работы.

### *Недостатки алгоритма:*
1. Слишком грубая настройка параметров *hi* и ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2019.png) .
2. Неопределённое время работы алгоритма подбора gamma_i.
3. При случайном выборе объектов из выборки при подборе ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2019.png), результат работы на одной и той же выборке будет разным.


[Оглавление](#Оглавление)

## Байесовские алгоритмы классификации

[Оглавление](#Оглавление)

Байесовские алгоритмы классификации основаны на идее о том, что обучающая выборка формируется из неизвестного вероятностного распределения, имеющего некоторую плотность.
В основе байесовского подхода лежит теорема о том, что если для классов известны вероятности *P* и плотности распределения *p*, то можно записать решающее правило в виде
![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2014.png), где ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2015.png)- важность класса y,и оно будет оптимальным, т. е. обладать наименьшей вероятностью ошибки. На практике вероятности и плотности распределения для классов обычно неизвестны, и их приходится восстанавливать по выборке. Из за этого решающее правило перестаёт быть оптимальным, так как восстановить эти значения по выборке можно только с некоторой погрешностью. Существуют параметрический и не параметрический подходы к задаче восстановления плотности.
[Оглавление](#Оглавление)

## Нормальный дискриминантный анализ

[Оглавление](#Оглавление)
Это метод классификации, основанный на параметрическом подходе, специальный случай, при котором плотности распределения всех классов полагаются многомерными нормальными.
Функция плотности многомерного нормального распределения выглядит следующим образом: ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2016.png), где 

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2017.png)-объект, состоящий из *n* признаков

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2018.png)-мат. ожидание каждого признака

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2021.png)-матрица ковариации признаков. Симметричная, невырожденная, положительно определённая.

[Оглавление](#Оглавление)

## Линии уровня нормального распределения

[Оглавление](#Оглавление)

1.Если признаки некоррелированы, т. е. матрица ковариации диагональна, то линии уровня имеют форму эллипсоидов, параллельных осям координат, вытянутых относительно признака.   
![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/line1.1.png)

2. Если признаки имеют одинаковые дисперсии, тогда эллипсоиды являются сферами.  ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/line2.1.png)

3.Если признаки коррелированы, то есть матрица ковариации не диагональна, то линии уровня имеют форму эллипсоидов, наклонённых относительно осей координат.  
 ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/line31.png)
 
 [Оглавление](#Оглавление)
 
 ## Наивный байесовский классификатор
 
 [Оглавление](#Оглавление)
 
 Предполагается, что все объекты обучающей выборки X описываются *n* числовыми признаками 
 Все эти признаки - независимые случайные величины. Тогда функции правдоподобия классов представимы в виде: ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2022.png),
 
 где  ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула23.png)- плотность распределения значений *j*-го признака для класса *y*.
 
 Данный алгоритм основан на предположении, что n одномерных плотностей оценить проще, чем одну n-мерную, но на практике это редко получается.

Эмпирическая оценка *n*-мерной плотности находится следующим образом:
![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула24.png)

где x - классифицируемая точка, ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула29.png) точки обучающей выборки, *h* - ширина окна, *m* - кол-во точек выборки, *n* - кол-во признаков, *K* - функция ядра, ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула30.png) - признаки.

Подставив эту оценку в оптимальное байесовское решающее правило, получим "наивный" байесовский классификатор.

В качестве обучающей выборки *X* рассматриваются ирисы Фишера, ядро *K* выбрано Гауссовское.

Имея мат. ожидания и среднеквадратичные отклонения решающее правило можно написать так:
```R
a = function(x, classes, probs, mus, stds) {
  Y = length(classes)
  n = dim(mus)[2]
  scores = rep(0, Y)
  for (i in 1:Y) {
    scores[i] = probs[i]
    for (j in 1:n) {
      scores[i] = scores[i] * N(x[j], mus[i,j], stds[i,j])
    }
  }
  res = which.max(scores)
  factor(classes[res], levels=classes)
}
```
Карта классификации:

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/Naivn.png)

Видно, что граница принятия решения гладкая, что очень хорошо, однако можно видеть неблагоприятные решения отмеченные красным цветом. Наивный байесовский классификатор показал ошибку 4% на 2х признаках ирисов Фишера.

Вывод: Наивный байесовский классификатор это алгоритм, используемый для тестирования точности других алгоритмов. Он прост, чрезвычайно быстр и создает очень гладкие границы принятия решения.

[Оглавление](#Оглавление)

## Plug-in алгоритм

[Оглавление](#Оглавление)

В качестве модели восстанавливаемой плотности рассматривается многомерная нормальная:
![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула25.png)

где ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула26.png)- математическое ожидание , а 𝛴 ∈ ℝ - ковариационная матрица (симметричная, невырожденная, положительно определённая).

Алгоритм заключается в восстановлении параметров нормального распределения ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула31.png)  для каждого класса *y* и подстановке их в формулу оптимального байесовского классификатора. Предполагается, что ковариационные матрицы классов не равны.

Оценка параметров нормального распределения производится на основе принципа максимума правдоподобия:

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2027.png)

Разделяющая поверхность между двумя классами *s* и *t* задаётся следующим образом:

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула32.png)

Прологарифмируем и выразим константу:

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула34.png)

Как известно:

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула35.png)

Логарифмируя плотности каждого класса и подставив в выражение с разностью логарифмов, получим коэффициенты разделяющей кривой:

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула36.png)

Реализация плагин алгоритма:

```R
a = function(x, classes, probs, mus, covs) {
  Y = length(classes)
  n = dim(mus)[2]
  scores = rep(0, Y)
  for (i in 1:Y) {
    scores[i] = probs[i] * N(x, mus[i,], covs[[i]])
  }
  res = which.max(scores)
  factor(classes[res], levels=classes)
}
```

Построим карту классификации:

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/map%20plug.png)


![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/lines%20plug.png)


Посчитать ошибки:

1. На 2х признаках: 2.666%
2. На 4х признаках: 1.333%

Вывод: Этот алгоритм дал хороший результаты, т.к. в природе параметры ирисов распределены нормально.

[Оглавление](#Оглавление)

## Линейный дискриминант Фишера (ЛДФ)

[Оглавление](#Оглавление)

ЛДФ основан на подстановочном алгоритме с предположением, что ковариационные матрицы классов равны. Отсюда следует, что разделяющая поверхность вырождается в прямую. Это условие в plug-in не выполнялось, так как разделяющая поверхность все равно была квадратичной (хоть и приближенной к прямой). Отсюда следует, что ЛДФ должен иметь более высокое качество классификации при одинаковых ковариационных матрицах.

Программно алгоритм отличается от подстановочного пересчетом ковариационной матрицы и поиском разделяющей поверхности.

Так как матрицы равны, можем оценить их все вместе:
![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула28.png)
```R
ldf = function(x, classes, probs, mus, covinv, covdet) {
  Y = length(classes)
  n = dim(mus)[2]
  scores = rep(0, Y)
  for (i in 1:Y) {
    scores[i] = probs[i] * N(x, mus[i,], covinv, covdet)
  }
  res = which.max(scores)
  factor(classes[res], levels=classes)
}
```
Карта классификации: 

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/map.png)

Граница принятия решения кусочно-линейная,  это значит о хорошей обобщающей способности.

Построить разделяющие прямые ЛДФ.

Поскольку ЛДФ создает кусочно-линейную границу, то мы можем её найти аналитически. Построим эти прямые  для каждой пары классов.
Имея мат. ожидания и общую ковариационную матрицу решающее правило можно написать так:

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/ldf.png)

Вывод: ЛДФ отличный алгоритм, с линейной разделительной поверхностью принятия решений и хорошей обобщающей способностью.

[Оглавление](#Оглавление)

### Линейные алгоритмы

[Оглавление](#Оглавление)

Линейным классификатором называется алгоритм классификации  ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2037.png) вида:

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2038.png).

Гиперплоскость, разделяющая классы в пространстве *R^n*, задаётся уравнением *<w, x> = 0*. Если вектор x находится по одну сторону гиперплоскости с её направляющим вектором *w*, то объект x относится к классу +1, иначе - к классу -1.

Для того, чтобы подобрать оптимальный вектор параметров w, минимизирующий эмпирический риск
![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2039.png)  
применяется ## метод стохастического градиента.

[Оглавление](#Оглавление)

### Метод стохастического градиента 

[Оглавление](#Оглавление)

В этом методе сначала выбирается начальное приближение для *w* (инициализируется небольшими случайными значениями *w = (-1/2n, 1/2n)*, где *n* - число признаков *x*), затем запускается итерационный процесс, на каждом шаге которого вектор w сдвигается в направлении, противоположном направлению вектора градиента ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2040.png).

Веса *w* изменяются следующим образом:
![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2041.png), где  ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2042.png) - темп обучения. Для лучшей сходимости принято полагать  ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2043.png)/

Функционал аппроксимированного эмпирического риска *Q* оценивается следующим образом:
 ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2044.png) , где   ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2045.png) - ошибка алгоритма на случайном объекте x_i из Xl, ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2046.png)- параметр сглаживания (обычно полагается *1/l*).
 
 Сдвигаемся до тех пор, пока вектор *w* не перестанет изменяться и/или функционал *Q* не стабилизируется. Важно заметить, что градиент вычисляется не на всех объектах обучающей выборки, а на случайном объекте (поэтому "стохастический"). При использовании метода стохастического градиента нужно нормализовать данные:
 ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2047.png)
 
 В зависимости от функции потерь *L* в функционале эмпирического риска *Q* существуют разнообразные линейные алгоритмы классификации.

Для минимизации *Q(w)* применяется метод градиентного спуска.

В начале выбирается некоторое начальное приближение вектора весов *w*. Не существует единого способа инициализации весов. Хорошей практикой считается инициализировать веса случайными малыми значениями: 
![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2048.png)  , где n – количество признаков x.

Далее высчитывается текущая оценка функционала![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2049.png). 
Затем запускается итерационный процесс, на каждом шаге которого вектор w изменяется в сторону наиболее быстрого убывания *Q*. Это направление противоположно вектору градиента 
![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2050.png). 
Соответственно веса меняются по правилу:

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2051.png)

или

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2052.png)

где ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2042.png)  – темп обучения. Чтобы не проскочить локальный минимум темп обучания принято полагать небольшим. Однако, при слишком маленьком его значении алгоритм будет медленно сходится. Хорошей практикой считается его постепенное уменьшение по ходу итераций. Мы же будем полагать его равным 
![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2053.png)

## Алгоритм:
 Вход: обучающая выборка, темп обучения, параметр сглаживания.

 Выход: Веса w1,...,wn

1. Инициализировать веса w1,...,wn

2. Вычислить начальное значение функционала эмпирического риска

3. повторять

4. выбрать объект из обучающей выборки

5. вычислить ошибку алгоритма на функции потерь

6. сделать шаг градиентного спуска

7. получить новое значение функционала эмпирического риска

8. пока значение функционала эмпирического риска не стабилизируются или веса не перестанут изменяться.
```R
minimize = function(f,g,y,x,w,maxiter=100,nu=0.01,callback=callback) {
  l = dim(x)[1]
  for (iter in 1:maxiter) {
    w = w - nu * g(y,x,w,sample(1:l,1))
    print(sprintf("%d %f", iter, f(y,x,w)))
    callback(w)
  }
  w
}
```
[Оглавление](#Оглавление)

### Адаптивный линейный элемент

[Оглавление](#Оглавление)

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2062.png) - это квадратичная функция потерь.
![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2063.png)  - правило обновления весов на каждом шаге итерации метода стохастического градиента. Данное правило получено путем дифференцирования квадратичной функции.

Пусть дана обучающая выборка: множество входных значений *X* и множество выходящих значений *Y*, такие что каждому входу *xj* соответствует *yj* - выход, *j = 1...m*. Необходимо по этим данным построить ADALINE, которая допускает наименьшее количество ошибок на этой обучающей выборке. Обучение ADALINE заключается в подборе "наилучших" значений вектора весов *w*. Какие значение весов лучше определяет функционал потерь.
В ADALINE применяется квадратичная функция потерь, которая представлена выше.

```R
  repeat {

      # случайным образом выбираем индекс из объектов ошибки
      i <- sample(1:l, 1)
      iterCount <- iterCount + 1
      xi <- xl[i, 1:n]
      yi <- xl[i, n + 1]
      
      wx <- crossprod(w, xi)
      margin <- wx * yi
      ex <- lossQuad(margin)
      w <- w - eta * (wx - yi) * xi ##расчитываем градиентный шаг для итерации
      
      Qprev <- Q
      Q <- (1 - lambda) * Q + lambda * ex ##обновляем Q
      abline(a = w[3] / w[2], b = -w[1] / w[2],  col = "black")
 
      if(abs(Qprev - Q) < 1e-6) { 
      break
    }
  }
  return(w) 
}
```

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/adaline.png)

## Правило Хебба 
Правило Хебба Имеет кусочно-линейную функцию потерь ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2064.png)  и правило Хебба для обновления весов  ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2065.png)


![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/habbs.png)

 [Оглавление](#Оглавление)
 
 ### Логистическая регрессия
 
 [Оглавление](#Оглавление)
 
Является-оптимальный байесовским классификатором.

Имеет логистическую функцию потерь ![screenshot of sample]( https://github.com/ZaraL3/ML1/blob/master/image/формула%2054.png)  и логистическое правило обновления весов
![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2055.png)  , где ![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2056.png)  – сигмоидная функция.

Испульзуем метод градиентного спуска, написанный ранее. Используем “Bias Trick” для обучения параметра w0.

Напомним что количество очков, даваемое классу, определяется как:

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2057.png)

Тогда создадим вектор

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2058.png)

И обучим с решающим правилом:

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2059.png)

Тогда в итоге получим:

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/формула%2060.png)

## Достоинства:

Логистическая регрессия дает лучшие результаты по сравнению с линейным дискриминантом Фишера (поскольку она основана на менее жестких гипотезах), а также по сравнению с адалайном и правилом Хэбба (поскольку она использует более правильную функцию потерь).

Возможность оценивать апостериорные вероятности и риски.

## Недостатки:

Градиентный метод обучения логистической регрессии наследует все недостатки метода стохастического градиента. Практичная реализация должна предусматривать стандартизацию данных, отсев выбросов, регуляризацию (сокращение весов), отбор признаков и т.п..

```R
# функция потерь
lossLog <- function(x)
{
 return (log2(1 + exp(-x)))
}
# сигмоидная функция
sigmoidFunction <- function(z)
{
 return (1 / (1 + exp(-z)))
}
```

Пример работы алгоритма:

![screenshot of sample](https://github.com/ZaraL3/ML1/blob/master/image/reg.png)

[Оглавление](#Оглавление)


